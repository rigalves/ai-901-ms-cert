# Foundry SDK chat and agent clients

Verified: **July 30, 2026**

> This document was prepared by a gap-research session for use during a later study session. The questions and implementation exercise are stored here; they are not administered during research.

## Official exam objectives

Current AI-901 objectives covered here:

- Create a lightweight chat client application by using the Foundry SDK.
- Create a lightweight client application for an agent.

Study guide: https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-901

Related material:

- Foundry hierarchy, endpoints, and authentication: [`foundry-foundation.md`](foundry-foundation.md)
- Model deployment and prompts: [`generative-ai-prompts-model-configuration.md`](generative-ai-prompts-model-configuration.md)
- Agent concepts and portal workflow: [`agentic-ai-single-agent-solutions.md`](agentic-ai-single-agent-solutions.md)

# Part 1 — The durable client pattern

Both objectives reduce to the same engineering skeleton:

`configuration → credential → project client → model or agent response client → input → response → output`

The remote model or agent does the AI work. The lightweight client mainly:

1. reads configuration;
2. authenticates;
3. creates the correct SDK client;
4. sends input;
5. preserves state when needed;
6. displays or processes the result.

## Core objects

| Object or value | Purpose | .NET mental mapping—not an exact equivalence |
|---|---|---|
| **Project endpoint** | Routes calls to one Foundry project | A configured service base URI scoped to a project |
| **Credential** | Acquires an identity token or supplies a supported key | An authentication provider injected into an SDK client |
| **`AIProjectClient`** | Entry point for Foundry-native project operations and related clients | A root SDK client/factory for the project |
| **OpenAI-compatible client** | Sends Responses API calls to models and agents | A generated-service client for inference operations |
| **Model deployment name** | Selects the deployed model used for a request | A configured deployment identifier—not the catalog display name alone |
| **Agent name/version or reference** | Selects a persisted agent definition | A reference to a reusable configured component |
| **Response** | Contains output text plus possible tool calls and metadata | A response DTO with typed output items |
| **Conversation ID** | Preserves server-side multi-turn history | A durable conversation/session identifier |
| **Previous response ID** | Chains the next request to one earlier response | An explicit continuation token/reference |

# Part 2 — Which client path should you use?

| Scenario | Use | Required request identity |
|---|---|---|
| One request to a deployed model | Model response client | Model deployment name |
| Multi-turn chat directly with a model | Model response client plus state | Model deployment name + conversation or previous response |
| Reusable behavior with instructions and tools | Persisted agent client path | Agent name/version or agent reference |
| Project administration, connections, agent creation, tracing | Project client | Project endpoint and credential |

Decision rule:

- **Model request:** the application supplies the prompt and model selection.
- **Agent request:** the persisted agent already packages model, instructions, and tools; the application supplies user input and state.

# Part 3 — Lightweight model chat client

## Flow

`project endpoint → DefaultAzureCredential → AIProjectClient → OpenAI-compatible response client for model → input → output text`

## Python — current Foundry project pattern

```python
import os
from azure.identity import DefaultAzureCredential
from azure.ai.projects import AIProjectClient

project = AIProjectClient(
    endpoint=os.environ["FOUNDRY_PROJECT_ENDPOINT"],
    credential=DefaultAzureCredential(),
)

openai = project.get_openai_client()

response = openai.responses.create(
    model=os.environ["FOUNDRY_MODEL"],
    input="Explain dependency injection in one sentence.",
)

print(response.output_text)
```

## C# — equivalent mental flow

```csharp
using Azure.AI.Projects;
using Azure.AI.Extensions.OpenAI;
using Azure.Identity;

string endpoint = Environment.GetEnvironmentVariable("FOUNDRY_PROJECT_ENDPOINT")
    ?? throw new InvalidOperationException("FOUNDRY_PROJECT_ENDPOINT is missing.");

string model = Environment.GetEnvironmentVariable("FOUNDRY_MODEL")
    ?? throw new InvalidOperationException("FOUNDRY_MODEL is missing.");

AIProjectClient projectClient = new(
    endpoint: new Uri(endpoint),
    tokenProvider: new DefaultAzureCredential());

var responsesClient =
    projectClient.ProjectOpenAIClient.GetProjectResponsesClientForModel(model);

var response = await responsesClient.CreateResponseAsync(
    "Explain dependency injection in one sentence.");

Console.WriteLine(response.GetOutputText());
```

## What to recognize

1. `FOUNDRY_PROJECT_ENDPOINT` is the project-scoped endpoint.
2. `DefaultAzureCredential` supplies Microsoft Entra ID authentication.
3. `AIProjectClient` is created first.
4. The OpenAI-compatible response client performs inference.
5. The request identifies the model deployment.
6. The generated text is read from `output_text` or the corresponding SDK helper.

# Part 4 — Lightweight client for a persisted agent

The agent must already exist in the Foundry project. It may have been created in the portal or with the SDK.

## Flow

`project endpoint → credential → project client → response client bound to agent → optional conversation → user input → agent response`

## Python — conversation-based multi-turn agent client

```python
import os
from azure.identity import DefaultAzureCredential
from azure.ai.projects import AIProjectClient

project = AIProjectClient(
    endpoint=os.environ["FOUNDRY_PROJECT_ENDPOINT"],
    credential=DefaultAzureCredential(),
)

openai = project.get_openai_client(
    agent_name=os.environ["FOUNDRY_AGENT_NAME"]
)

conversation = openai.conversations.create()

first = openai.responses.create(
    conversation=conversation.id,
    input="What can you help me with?",
)
print(first.output_text)

follow_up = openai.responses.create(
    conversation=conversation.id,
    input="Give me one example.",
)
print(follow_up.output_text)
```

## C# — equivalent mental flow

```csharp
using Azure.AI.Projects;
using Azure.AI.Extensions.OpenAI;
using Azure.Identity;

string endpoint = Environment.GetEnvironmentVariable("FOUNDRY_PROJECT_ENDPOINT")
    ?? throw new InvalidOperationException("FOUNDRY_PROJECT_ENDPOINT is missing.");

string agentName = Environment.GetEnvironmentVariable("FOUNDRY_AGENT_NAME")
    ?? throw new InvalidOperationException("FOUNDRY_AGENT_NAME is missing.");

AIProjectClient projectClient = new(
    endpoint: new Uri(endpoint),
    tokenProvider: new DefaultAzureCredential());

var conversation = projectClient.ProjectOpenAIClient
    .GetProjectConversationsClient()
    .CreateProjectConversation();

var responsesClient = projectClient.ProjectOpenAIClient
    .GetProjectResponsesClientForAgent(
        defaultAgent: agentName,
        defaultConversationId: conversation.Id);

var first = await responsesClient.CreateResponseAsync(
    "What can you help me with?");
Console.WriteLine(first.GetOutputText());

var followUp = await responsesClient.CreateResponseAsync(
    "Give me one example.");
Console.WriteLine(followUp.GetOutputText());
```

## What changed from the model client?

| Model client | Agent client |
|---|---|
| Selects a model deployment | Selects a persisted agent |
| Application usually supplies instructions and prompt | Agent already stores reusable instructions and tools |
| Tools must be configured in the request or surrounding app | Agent definition can already contain tools |
| State is optional | State is commonly useful for agent conversations |

# Part 5 — State choices

| Need | Pattern | Recognition cue |
|---|---|---|
| One independent request | No state ID | Only `input` plus model or agent reference |
| Continue from one prior response | `previous_response_id` | The new request explicitly points to the earlier response |
| Preserve a named multi-turn interaction | Conversation ID | Several requests reuse the same conversation |

These patterns are alternatives. Do not pass a random response ID where a conversation ID is expected.

## Previous-response pattern

```python
first = openai.responses.create(
    model=model_name,
    input="What is the largest city in France?",
)

follow_up = openai.responses.create(
    model=model_name,
    previous_response_id=first.id,
    input="What is its population?",
)
```

Mental mapping:

- `previous_response_id=first.id` resembles explicitly passing a continuation reference.
- Reusing a conversation resembles sending requests through the same server-managed session.

# Part 6 — Project client versus OpenAI-compatible client

| Client | Main responsibility | Typical operations |
|---|---|---|
| **Project client** | Foundry-native project access | Project properties, connections, agent administration, tracing, obtaining related clients |
| **OpenAI-compatible client** | Responses-style model and agent invocation | `responses.create`, conversations, output items, tool-call results |

Common mistake:

> “I created `AIProjectClient`, so I should call a text-generation method directly on it.”

Correction:

The project client normally gives access to the model/agent response client. The response client performs the inference request.

# Part 7 — Authentication and configuration

## Preferred current recognition path

1. Store the project endpoint, model deployment name, and agent name in configuration or environment variables.
2. Use `DefaultAzureCredential` for Microsoft Entra ID authentication.
3. Ensure the signed-in user or managed identity has the required Foundry RBAC role.
4. Never place credentials in prompts or source-controlled code.

## Endpoint distinction

| Endpoint | Recognition |
|---|---|
| `https://<resource>.services.ai.azure.com/api/projects/<project>` | Foundry project endpoint; supports project-scoped models, agents, tools, and governance |
| `https://<resource>.openai.azure.com/openai/v1` | Resource-level OpenAI-compatible endpoint for supported OpenAI operations |

Do not combine a project-endpoint client pattern with configuration names copied blindly from a resource-level API-key sample.

# Part 8 — Current-version guardrails

Verified current documentation distinguishes:

- **Foundry 2.x:** new Foundry project experience.
- **Foundry 1.x/classic:** legacy-compatible experience with different examples.

Rules:

1. Follow one documentation generation end to end.
2. Do not combine 1.x object types with 2.x methods.
3. In .NET, do not install overlapping preview and GA OpenAI extension packages that define the same types.
4. Prefer current agent name/version or agent-reference terminology.
5. Treat exact package patch versions and temporary preview names as lookup details, not durable memorization targets.

# Part 9 — Focused Python-to-C# mappings

| Python | C# mental mapping |
|---|---|
| `import os` | `using System;` plus `Environment` access |
| `os.environ["NAME"]` | `Environment.GetEnvironmentVariable("NAME")` |
| `AIProjectClient(endpoint=..., credential=...)` | Constructor with named arguments |
| `DefaultAzureCredential()` | `new DefaultAzureCredential()` |
| `project.get_openai_client()` | Get a related response client from the project client |
| `response = ...` | Local variable with inferred type using `var` |
| `response.output_text` | Response DTO property/helper such as `GetOutputText()` |
| `conversation.id` | `conversation.Id` |
| `previous_response_id=first.id` | Options property assigned from `first.Id` |
| Python dictionary for `agent_reference` | An options object or SDK-specific agent-bound client |

Only recognize the repeated client syntax. General Python study belongs to the separate cross-cutting Python cluster.

# Part 10 — Code-recognition traps

## Trap 1: Model name versus agent name

```python
response = openai.responses.create(
    model=AGENT_NAME,
    input="Hello",
)
```

Likely problem: the `model` field expects a model deployment, not a persisted agent name.

## Trap 2: Missing credential

```python
project = AIProjectClient(endpoint=PROJECT_ENDPOINT)
```

Likely problem: the current Entra ID pattern also supplies a credential.

## Trap 3: Wrong output object

```python
print(project.output_text)
```

Likely problem: generated output belongs to the response, not the project client.

## Trap 4: Lost conversation

```python
conversation = openai.conversations.create()
first = openai.responses.create(conversation=conversation.id, input="Hello")
second = openai.responses.create(input="What did I just say?")
```

Likely problem: the second request does not reuse the conversation or previous response.

## Trap 5: Mixed generations

A snippet imports classic agent types but calls current 2.x methods.

Likely problem: SDK generations are incompatible even when names look related.

# Prepared practice

Use these questions during a future study or review session. Answer before opening the key.

1. Which value scopes a current Foundry SDK client to one project?
2. What does `DefaultAzureCredential` provide?
3. Which client is the root entry point for Foundry-native project operations?
4. Which client layer sends Responses API inference calls?
5. What identifies a model request: model deployment name or agent name?
6. What identifies a persisted-agent request?
7. Where is generated text read from: the project client or the response?
8. Which state mechanism explicitly chains one new request to one earlier response?
9. Which state mechanism is designed for several turns in the same interaction?
10. Does a one-shot request require a conversation?
11. A snippet uses `response.output_text`. What is the likely C# equivalent idea?
12. Why should a current 2.x sample not be combined with a classic 1.x sample?
13. A Learn page says `agent-id`, while current docs use agent name/version. What should be memorized?
14. Should secrets be embedded in the system prompt?
15. A snippet calls `project.output_text`. What is wrong?
16. What configuration value should be supplied to `model=`?
17. If the second request omits both the conversation ID and previous response ID, will the service automatically know the first turn?
18. Which approach is better preparation for AI-901: memorizing an entire production app or recognizing the short client flow and missing methods?

## Answer key

1. **The Foundry project endpoint.**
2. **An Azure identity credential/token source for Microsoft Entra ID authentication.**
3. **`AIProjectClient`.**
4. **The OpenAI-compatible responses client obtained through the project client.**
5. **The model deployment name.**
6. **The current agent name/version or SDK agent reference.**
7. **The response object.**
8. **`previous_response_id`.**
9. **A conversation ID.**
10. **No.** It is optional when no history is needed.
11. **Read the generated text from the response DTO, such as with `GetOutputText()`.**
12. **Their packages, endpoint assumptions, object types, and methods can differ.**
13. **The durable idea that the client references a persisted agent using the current service identifier; verify the current SDK shape.**
14. **No.** Use configuration, identity, and secret-management mechanisms.
15. **Output belongs to the inference response, not the project client.**
16. **The deployed model name.**
17. **No.** The new request is independent unless state is supplied.
18. **Recognizing and completing the short client flow.** AI-901 is a fundamentals exam, not a production-coding assessment.

# Minimal future implementation exercise

During a later implementation session:

1. Create one console application or short script.
2. Read the project endpoint and model deployment name from environment variables.
3. Authenticate with `DefaultAzureCredential`.
4. Send one model request and print the result.
5. Call one existing prompt agent.
6. Send a follow-up using either the same conversation or `previous_response_id`.
7. Intentionally remove one required line and identify the resulting failure.
8. Explain the flow in this order:

`endpoint → credential → project client → response client → model/agent reference → input/state → response output`

The purpose is recognition and workflow understanding, not a portfolio-quality application.

# Future study-session exit check

A later study or review session may evaluate whether the learner can, without notes:

1. Reconstruct both lightweight-client flows.
2. Explain the responsibility of each client layer.
3. Distinguish model deployment, persisted agent, conversation, and response.
4. Map the Python snippets to familiar C# concepts.
5. Detect missing credentials, incorrect identifiers, lost state, and mixed SDK generations.

# Official sources

- AI-901 study guide: https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-901
- Using a generative AI model: https://learn.microsoft.com/en-us/training/modules/get-started-with-generative-ai-and-agents/3-using-generative-ai-models
- Creating an agent: https://learn.microsoft.com/en-us/training/modules/get-started-with-generative-ai-and-agents/4-creating-an-agent
- Microsoft Foundry SDKs and endpoints: https://learn.microsoft.com/en-us/azure/foundry/how-to/develop/sdk-overview
- Microsoft Foundry SDK quickstart: https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code
- Agents, conversations, and responses: https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/runtime-components
- Azure AI Projects client library for Python: https://learn.microsoft.com/en-us/python/api/overview/azure/ai-projects-readme

# Exam-readiness research

See [`../../research/gaps/foundry-sdk-chat-agent-clients.md`](../../research/gaps/foundry-sdk-chat-agent-clients.md).
