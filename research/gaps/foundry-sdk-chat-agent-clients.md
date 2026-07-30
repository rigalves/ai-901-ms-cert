# Foundry SDK chat and agent clients — Exam-readiness gaps

Verified: **July 30, 2026**

## Official scope

Current AI-901 objectives covered by this cluster:

- Create a lightweight chat client application by using the Foundry SDK.
- Create a lightweight client application for an agent.

The study guide also states that candidates need Python coding syntax and programming-technique knowledge and should be familiar with REST APIs, SDKs, and CLIs.

Study guide: https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-901

## Official material reviewed

- AI-901 study guide: https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-901
- AI-901 exam page: https://learn.microsoft.com/en-us/credentials/certifications/exams/ai-901/
- Using a generative AI model: https://learn.microsoft.com/en-us/training/modules/get-started-with-generative-ai-and-agents/3-using-generative-ai-models
- Creating an agent: https://learn.microsoft.com/en-us/training/modules/get-started-with-generative-ai-and-agents/4-creating-an-agent
- Microsoft Foundry SDKs and endpoints: https://learn.microsoft.com/en-us/azure/foundry/how-to/develop/sdk-overview
- Microsoft Foundry SDK quickstart: https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code
- Agents, conversations, and responses: https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/runtime-components
- Azure AI Projects client library for Python: https://learn.microsoft.com/en-us/python/api/overview/azure/ai-projects-readme
- Current exam-format clarification: https://learn.microsoft.com/en-us/answers/questions/5924630/does-the-ai-901-exam-include-a-lab-%28performance-ba

## Coverage assessment

The official objectives are explicit, but the primary Learn units devote only a small section to each client. The documentation contains enough current material to build the clients, but a learner must combine several pages and recognize which examples belong to the new Foundry 2.x experience.

The durable implementation pattern is consistent across the current documentation:

`project endpoint + credential → project client → model or agent response client → input → response → output`

The difficulty is not general programming. It is recognizing the correct endpoint, client layer, model or agent reference, state mechanism, and response property while Microsoft terminology and SDK shapes continue to evolve.

## Gaps

### 1. The two client objectives are underexplained in the primary Learn module

- **Type:** Implementation gap
- **Evidence:** The chat-client and agent-client sections are short, while the detailed setup, package, endpoint, authentication, state, and response patterns live in separate SDK and Agent Service documentation.
- **Confidence:** High
- **Why it matters:** The implementation domain is 55–60% of AI-901, and these are separate objective bullets rather than optional examples.
- **Study material needed:** One compact end-to-end workflow for each client, with side-by-side Python and C# mappings.

### 2. “Foundry SDK” involves two cooperating client layers

- **Type:** Underexplained
- **Evidence:** Current documentation distinguishes the Foundry project client from the OpenAI-compatible client. The project client handles Foundry-native operations; the OpenAI-compatible client handles Responses API calls, models, and agents.
- **Confidence:** High
- **Why it matters:** A code question can be answered incorrectly if `AIProjectClient` is mistaken for the object that directly returns generated text, or if a direct OpenAI endpoint is confused with a Foundry project endpoint.
- **Study material needed:** A client-responsibility table and a request-flow diagram.

### 3. New Foundry 2.x and classic 1.x examples must not be mixed

- **Type:** Terminology gap
- **Evidence:** Official documentation lists separate new-Foundry and classic package generations and explicitly warns that the examples are incompatible. Current .NET guidance also warns against installing overlapping OpenAI extension packages that define ambiguous types.
- **Confidence:** High
- **Why it matters:** Package names, object types, endpoint forms, and agent invocation patterns differ. A learner can understand each snippet separately and still assemble an invalid hybrid.
- **Study material needed:** A version guardrail that teaches one current 2.x path and treats classic code as recognition-only legacy material.

### 4. The Learn agent unit contains transitional terminology

- **Type:** Terminology gap
- **Evidence:** The unit says an `agent-id` is required, but its code retrieves an agent by name. Current Agent Service documentation identifies persisted agents by name and version and notes that they no longer use a GUID called `AgentID`.
- **Confidence:** High
- **Why it matters:** Memorizing one temporary identifier shape is less useful than understanding the durable reference: a persisted agent definition is invoked by the client using its current service reference.
- **Study material needed:** A warning to prefer the current name/version or agent-reference pattern and to avoid treating old `agent-id` wording as universal.

### 5. Conversation state has two valid patterns that are easy to confuse

- **Type:** Underexplained
- **Evidence:** Current samples use either a persisted conversation ID or `previous_response_id` to continue context. A one-off model request requires neither.
- **Confidence:** High
- **Why it matters:** The learner must recognize when history is absent, explicitly chained, or stored in a conversation.
- **Study material needed:** A decision table for one-shot input, previous-response chaining, and conversation-based multi-turn chat.

### 6. Code-recognition practice is thin

- **Type:** Practice gap
- **Evidence:** The official objectives require lightweight clients and the exam page explicitly calls for SDK and Python familiarity, but the primary module supplies little original code-completion or troubleshooting practice. A current Microsoft Q&A answer also states that fundamentals exams do not use labs and that coding items may ask the learner to select a missing method from a list.
- **Confidence:** Medium
- **Why it matters:** The likely preparation need is reading and completing short snippets, not building a production application from memory.
- **Study material needed:** Original method-selection, object-order, state, endpoint, and output-property questions with an answer key.

### 7. Python needs to be mapped, not taught as a separate course

- **Type:** Practice gap
- **Evidence:** Official examples are Python-first, while the learner is an experienced C# developer. The client samples use a small repeated subset of Python: imports, environment variables, constructor keyword arguments, dictionaries, context managers, and property access.
- **Confidence:** High
- **Why it matters:** General Python study would consume time without improving AI-901 readiness proportionally.
- **Study material needed:** Client-specific Python-to-C# mappings. The broader cross-cutting Python cluster remains separate and pending.

## Study-session assets created

Created [`../../docs/topics/foundry-sdk-chat-agent-clients.md`](../../docs/topics/foundry-sdk-chat-agent-clients.md), containing:

- the durable chat-client and agent-client workflows;
- project-client versus OpenAI-compatible-client responsibilities;
- current Foundry 2.x Python and C# examples;
- endpoint, authentication, model, agent, response, and state distinctions;
- focused Python-to-C# mappings;
- code-recognition traps;
- original practice questions and answer key;
- a minimal future implementation exercise.

## Claims not accepted

- **“The exam requires a hands-on lab.”** Rejected. Current Microsoft guidance says fundamentals exams do not include labs.
- **“A specific SDK class or patch version will definitely be memorized.”** Rejected. The objective requires client implementation recognition, but exact SDK surfaces change.
- **“The agent is always identified by a GUID called `agent-id`.”** Rejected. Current documentation uses agent name/version or an agent reference.
- **“Only API-key authentication matters.”** Rejected. Current Foundry project samples emphasize Microsoft Entra ID with `DefaultAzureCredential`; key-based examples may still appear for supported endpoints.
- **“Conversation and previous-response chaining are the same object.”** Rejected. They are alternative state patterns with different identifiers.
- **Any dump-derived claims about exact questions or distribution.** Rejected by repository policy.

## Suggested future study checkpoint

A later implementation or review session should verify that the learner can:

1. Reconstruct the client flow from configuration to displayed output.
2. Explain why the project client and OpenAI-compatible client both appear.
3. Select the model-client path versus the persisted-agent path.
4. Identify the project endpoint, model deployment name, and agent name in a snippet.
5. Choose between one-shot input, `previous_response_id`, and a conversation ID.
6. Read the Python sample without needing general Python instruction.
7. Detect a mixed 1.x/2.x or project-endpoint/resource-endpoint example.

## Sources

### Official Microsoft

- https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-901
- https://learn.microsoft.com/en-us/credentials/certifications/exams/ai-901/
- https://learn.microsoft.com/en-us/training/modules/get-started-with-generative-ai-and-agents/3-using-generative-ai-models
- https://learn.microsoft.com/en-us/training/modules/get-started-with-generative-ai-and-agents/4-creating-an-agent
- https://learn.microsoft.com/en-us/azure/foundry/how-to/develop/sdk-overview
- https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code
- https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/runtime-components
- https://learn.microsoft.com/en-us/python/api/overview/azure/ai-projects-readme

### Current community/support evidence

- https://learn.microsoft.com/en-us/answers/questions/5924630/does-the-ai-901-exam-include-a-lab-%28performance-ba

The support evidence is used only to shape practice format. It is not treated as proof of exact exam questions or frequency.
