# Agentic AI and single-agent solutions

Verified: **July 30, 2026**

> This document was prepared by a gap-research session for use during a later study session. The questions, checkpoint, and guided exercise are stored here; they are not administered during research.

## Official exam objectives

Current AI-901 objectives covered here:

- Identify scenarios for common AI workloads, including generative and agentic AI.
- Create and test a single-agent solution in the Foundry portal.

Study guide: https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-901

The separate objective **Create a lightweight client application for an agent** is only outlined at recognition level here. Detailed SDK and Python-to-C# work belongs to the later Foundry SDK chat and agent clients asset.

# Part 1 — What makes an AI application agentic?

## Durable definition

An AI agent is a reusable AI application that uses a generative model to interpret a goal, follow instructions, decide whether to use available tools, and continue until it can return a result or request help.

Core mental model:

`agent = model + instructions + tools`

A practical runtime also commonly uses:

`conversation context + retrieved knowledge + safety and application controls`

## Agent versus nearby concepts

| Pattern | Who controls the next step? | Typical capability | Best recognition cue |
|---|---|---|---|
| **Model-only request** | The application sends one request; the model generates one response | Drafting, summarizing, answering from supplied context | No reusable agent definition or autonomous tool choice is required |
| **Single agent** | The model can choose among allowed tools while following reusable instructions | Search, analyze, retrieve, or take an action to complete one goal | One agent packages model, instructions, and tools |
| **Deterministic workflow** | Application code explicitly controls every step and branch | Fixed business process with predictable sequencing | Code, rules, or an orchestrator decides what runs next |
| **Multi-agent system** | Several specialized agents collaborate or delegate | Complex work split among specialized roles | More than one agent is involved; not required for the AI-901 single-agent objective |

A normal chat application can use a generative model without being an agent. The agentic distinction is not “it talks naturally”; it is reusable behavior plus the ability to reason over a goal and use capabilities beyond plain text generation.

## .NET mental mapping — not an exact equivalence

A prompt agent resembles a configured service component that has:

- a selected implementation engine: the model;
- configuration and policy: instructions;
- injected capabilities: tools;
- request state: conversation context.

The important difference is that an LLM can choose tools probabilistically from natural-language instructions. This is not the same as deterministic C# dispatch or a hard-coded workflow.

# Part 2 — Agent components and boundaries

## Core components

| Component | Purpose | What it is not |
|---|---|---|
| **Model** | Understands language, reasons, and generates outputs | The complete agent by itself |
| **Instructions** | Define role, goals, constraints, style, and decision guidance | A complete authorization or security boundary |
| **Tools** | Let the agent retrieve information, run code, or call external capabilities | A guarantee that the action is correct or safe |
| **Knowledge/retrieval** | Supplies relevant external content, often through search or file-retrieval capabilities | Permanent model training or fine-tuning |
| **Conversation** | Preserves interaction history across turns | Automatically durable memory across unrelated conversations |
| **Agent definition/version** | Stores a named, reusable snapshot of model, instructions, and tools | A user conversation or one generated response |

## Tools versus knowledge

The Learn unit gives a helpful shorthand:

- **Tools = actions**
- **Knowledge = context**

Use that for exam recognition, but remember the product boundary is slightly more flexible. Retrieval features such as File Search appear in the Agent Service tool catalog because the agent invokes them as capabilities. Their result is knowledge that grounds the response.

The durable distinction is:

- **Knowledge answers “what information should the agent use?”**
- **Action tools answer “what can the agent do?”**

Some tools retrieve knowledge; others change an external system.

## Conversation versus persistent memory

- A **conversation** carries the current interaction history so follow-up prompts make sense.
- **Persistent memory** stores or retrieves information across longer periods or separate sessions when explicitly configured.
- Neither guarantees perfect recall.
- Both consume or retrieve context that the model uses during inference; they do not rewrite the model’s learned weights.

# Part 3 — The agent execution loop

## End-to-end flow

1. The user or application provides a goal.
2. The model interprets the request using the agent instructions and available context.
3. The model decides whether a tool is needed.
4. If needed, the model produces a tool call with arguments.
5. The application or Agent Service executes the tool.
6. The tool result returns to the agent conversation.
7. The model may call another tool, ask for clarification, or produce the final response.
8. The application presents the output and records traces or evaluation data when configured.

Compact form:

`goal → reason → optionally call tool → receive result → continue → final response`

## Important decision rule

An available tool is not automatically invoked. The model selects tools from their descriptions, instructions, and the current request.

This means testing must verify both directions:

- the agent calls the right tool when it should;
- the agent does not call a tool when it should not.

# Part 4 — Select a suitable tool

## Scenario-to-tool table

| Scenario | Likely capability | Why |
|---|---|---|
| Answer from the model’s general capability and supplied prompt | **No tool** | A tool adds no value when the task is self-contained |
| Answer from private PDFs, manuals, or internal policy documents | **File Search, Azure AI Search, or another retrieval source** | Grounds the response in organization-controlled content |
| Answer with current public-web information | **Web search or web-grounding tool** | The base model may not contain current facts |
| Analyze a spreadsheet, execute calculations, or create a chart | **Code Interpreter** | Runs code in a controlled environment |
| Apply an exact business rule or call application code | **Function calling** | The application owns the deterministic implementation |
| Call an existing HTTP service described by an API contract | **OpenAPI tool** | Connects the agent to an API through its specification |
| Connect to reusable tools exposed by another system or team | **MCP tool/server** | Standard protocol for discovering and invoking external tools |
| Create or update a support ticket, order, calendar event, or record | **Action tool with controlled permissions** | The agent must interact with an external system |

Tool availability, region support, authentication method, and Preview status can change. For the exam, recognize the purpose and safe selection rule; do not memorize a temporary catalog list.

## Deterministic rule versus agent judgment

Prefer a deterministic function or application workflow when:

- the rule must be exact;
- the allowed sequence is fixed;
- failure must be handled predictably;
- the action is high impact;
- compliance requires explicit control.

Use an agent when natural-language interpretation, flexible planning, tool selection, or unstructured input materially improves the solution.

# Part 5 — Create a single agent in the Foundry portal

## Core portal workflow

Exact labels can change, but recognize this sequence:

1. **Open the correct Foundry project.** The project is the workspace boundary for agent assets and connected resources.
2. **Confirm a suitable model is available or deployed.** The model must support the intended agent scenario and tools.
3. **Open the agent-development area.** In the current Foundry experience, start with a prompt agent for the portal-first single-agent objective.
4. **Name the agent.** Use a purpose-oriented name that identifies the reusable component.
5. **Select the model.** Match reasoning quality, modality, latency, cost, tool support, and availability to the scenario.
6. **Write instructions.** Define role, task boundaries, tool-use guidance, output format, uncertainty behavior, and safety rules.
7. **Add only the required tools or knowledge.** More tools increase cost, ambiguity, permissions, and testing surface.
8. **Save or create the agent version.** Current Agent Service stores named, versioned agent assets.
9. **Test in the agents playground.** Use representative prompts rather than one demo prompt.
10. **Inspect behavior.** Check answer quality, selected tools, tool arguments, citations or grounding, conversation behavior, and failure handling.
11. **Refine and retest.** Change instructions, model, knowledge, tool descriptions, or permissions, then repeat the same tests.

Flow:

`project → model → agent name → instructions → tools/knowledge → save version → playground tests → inspect → refine → retest`

## Prompt-agent scope

For AI-901, a **prompt agent** is the durable center of the portal objective:

- configured in the portal or through declarative APIs;
- defined by model, instructions, and tools;
- run by the managed Agent Service;
- tested in the playground;
- callable later from a client application.

## Recognition-only advanced types

| Type | What to recognize | AI-901 depth |
|---|---|---|
| **Prompt agent** | Declarative model + instructions + tools, managed by Foundry | Core for the portal single-agent objective |
| **Hosted agent** | Custom agent code or framework packaged and run by Foundry | Recognition only unless the blueprint changes |
| **Multi-agent system** | Several agents collaborate or delegate | Concept recognition; not required for the single-agent workflow |

# Part 6 — Write usable agent instructions

## Compact instruction anatomy

1. **Role and goal:** What outcome should the agent achieve?
2. **Scope:** Which tasks are allowed and disallowed?
3. **Tool guidance:** When should each tool be used or avoided?
4. **Evidence rules:** Which sources should ground the response?
5. **Uncertainty behavior:** When should the agent ask a question or say it cannot verify?
6. **Action controls:** Which actions require confirmation?
7. **Output rules:** Format, length, fields, citations, or structure.

Example:

```text
You are an internal IT support agent.
Answer only from the approved support knowledge source.
Use the ticket tool only after the user confirms the issue summary and priority.
Never expose secrets or internal identifiers.
If the knowledge source does not support an answer, say so and ask one clarifying question.
Return: diagnosis, supporting source, proposed next step, and whether confirmation is required.
```

Instructions guide behavior. Tool permissions and application controls enforce what is actually allowed.

# Part 7 — Test a single-agent solution

## Minimum testing matrix

| Test type | Example purpose | What to inspect |
|---|---|---|
| **Happy path** | Normal supported request | Correct result and clear output |
| **Tool required** | Request needs current or private data | Correct tool, correct arguments, grounded answer |
| **Tool not required** | Simple self-contained request | No unnecessary tool call |
| **Missing or ambiguous input** | Important field is absent | Clarification instead of guessing |
| **Unsupported request** | Outside instructions or data | Safe limitation or refusal |
| **Consequential action** | Create, delete, purchase, send, or update | Confirmation and least-privilege behavior |
| **Tool failure** | Timeout, permission error, empty result | Honest error handling and safe fallback |
| **Multi-turn** | Follow-up depends on earlier context | Correct conversation continuity without invented memory |
| **Adversarial input** | Prompt injection or attempt to override rules | Instructions, permissions, and application controls hold |

## What “good” looks like

A successful test verifies more than fluent wording:

- the right capability was selected;
- tool arguments were sensible and bounded;
- retrieved evidence supports the answer;
- citations appear when the configured knowledge source supports them;
- the agent did not claim an action that did not occur;
- unsafe or high-impact actions require the intended control;
- failures are visible rather than hidden;
- repeated tests remain acceptably consistent.

# Part 8 — Responsible and secure agent behavior

## Fundamentals-level rules

1. **Least privilege:** Give tools only the permissions needed for the task.
2. **Human confirmation:** Require approval before consequential or irreversible actions.
3. **Validate inputs and outputs:** Treat model-generated tool arguments as untrusted application input.
4. **Protect secrets:** Do not place credentials in prompts, instructions, or source-controlled configuration.
5. **Control data flow:** Know which tool, service, or third party receives the request data.
6. **Ground important claims:** Use approved knowledge sources when correctness and traceability matter.
7. **Test prompt injection and misuse:** Retrieved content and user prompts can attempt to redirect the agent.
8. **Trace and evaluate:** Observe model calls, tool calls, failures, latency, and quality when the platform supports it.
9. **Provide safe fallback:** The agent should ask for help, refuse, or hand off instead of inventing success.

Rule:

`instructions guide; identities and permissions authorize; validation checks; monitoring reveals`

# Part 9 — Current terminology warning

## New Agent Service versus classic

Current Microsoft documentation distinguishes the new generally available Foundry Agent Service from **Agent Service classic**.

- Follow one current documentation generation end to end.
- Do not combine Foundry Projects 2.x packages and concepts with 1.x or classic samples.
- Expect package names, endpoint examples, and object types to evolve faster than the exam’s conceptual objective.
- For AI-901, prioritize the durable flow: project → agent definition → model/instructions/tools → test → client invocation.

The exact retirement date of classic is not a memorization target. The important exam-readiness rule is to recognize that classic examples are legacy and version-sensitive.

# Part 10 — Agent client boundary

Detailed code belongs to the later SDK-client cluster. For now, recognize the durable client flow:

`project endpoint + credential → locate/reference agent → optional conversation → send response request → inspect output`

The agent definition contains reusable behavior. The client supplies user input, conversation state when needed, and application-specific presentation or controls.

# Prepared practice

Use these questions during a future study or review session. Answer before opening the key.

1. A service only needs to rewrite supplied text in a requested tone. No external data or action is required. Must it use an agent?
2. What three components form the core official mental model of an agent?
3. An assistant must answer from private policy PDFs. Which capability should be considered first?
4. An assistant must report today’s public exchange-rate news. Why is a model-only request risky?
5. An agent can access a ticket-creation tool. Should it call the tool for every support question?
6. A payroll rule must calculate an exact statutory deduction. Should the rule primarily depend on free-form model reasoning or deterministic code?
7. What is the durable difference between knowledge and an action tool?
8. Does uploading knowledge permanently train the underlying model?
9. Does a conversation automatically create durable memory across unrelated sessions?
10. Which portal step comes first: testing the agent or defining its model, instructions, and tools?
11. Why should a test include a prompt that does not require a tool?
12. An agent is about to delete a production record. What control is appropriate before the action?
13. A tool returns a permission error. Should the agent claim that the requested action succeeded?
14. Which agent type is the core portal-first focus for the AI-901 single-agent objective: prompt agent or hosted agent?
15. Is a multi-agent architecture required to satisfy the single-agent objective?
16. A code sample uses classic 1.x agent types, while another uses current Foundry Projects 2.x types. Should the snippets be combined casually?

## Answer key

1. **No.** A model-only request is sufficient when the task is self-contained and needs no reusable tool-driven behavior.
2. **Model, instructions, and tools.**
3. **A retrieval capability such as File Search or Azure AI Search.** It grounds the answer in approved private content.
4. **The base model may not contain current information.** A current web-search or grounding capability is more appropriate.
5. **No.** The model should call it only when the request and instructions require an action.
6. **Deterministic code or a controlled function.** Exact business rules should not rely primarily on probabilistic reasoning.
7. **Knowledge supplies information; an action tool changes or invokes something.** Some retrieval tools provide knowledge, but they do not perform the same role as write-capable actions.
8. **No.** Retrieval supplies context during inference; it does not change model weights.
9. **No.** A conversation preserves turn history, while cross-session memory requires separate configuration.
10. **Define and save the agent first, then test it.** The usual order is model → instructions → tools/knowledge → save/version → playground tests.
11. **To verify the agent avoids unnecessary tool calls.** Correct non-use is part of tool-selection quality.
12. **Human confirmation plus least-privilege authorization.** Consequential actions need explicit control.
13. **No.** It should report the failure honestly and offer a safe next step.
14. **Prompt agent.** Hosted agents are advanced recognition material for this exam scope.
15. **No.** The objective explicitly focuses on a single-agent solution.
16. **No.** Follow one current SDK/API generation end to end because classic and 2.x concepts are not interchangeable.

# Guided portal exercise for a future implementation session

Build a small internal support prompt agent:

1. Use a Foundry project and suitable deployed model.
2. Create one prompt agent named for the support scenario.
3. Write instructions that limit answers to an approved knowledge source.
4. Add the knowledge/retrieval capability.
5. Add a ticket action only if the exercise environment supports safe test data.
6. Require confirmation before ticket creation.
7. Test one supported question, one unsupported question, one tool-required request, one request that should not call the tool, one ambiguous request, and one failed-action case.
8. Record which tests passed and which instruction or tool description needed refinement.

This exercise is intentionally portal-first. The later SDK-client session should call the finished agent from code.

# Future study-session exit check

A later study or review session may evaluate whether the learner can, without notes:

1. Define agentic AI and distinguish it from a model-only request.
2. Explain model, instructions, tools, knowledge, and conversation.
3. Trace a conditional tool-calling loop.
4. Select an appropriate tool from a scenario.
5. Reconstruct the Foundry portal single-agent workflow.
6. Design a useful agent test matrix.
7. Explain why permissions, validation, and confirmation cannot be replaced by a system prompt.
8. Distinguish prompt agents, hosted agents, and multi-agent systems at recognition level.
9. Explain why current and classic SDK samples must not be mixed.

# Official sources

- AI-901 study guide: https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-901
- AI agents concept unit: https://learn.microsoft.com/en-us/training/modules/fundamentals-generative-ai/7-agents
- Creating an agent unit: https://learn.microsoft.com/en-us/training/modules/get-started-with-generative-ai-and-agents/4-creating-an-agent
- Generative AI and agents implementation module: https://learn.microsoft.com/en-us/training/modules/get-started-with-generative-ai-and-agents/
- Foundry Agent Service overview: https://learn.microsoft.com/en-us/azure/foundry/agents/overview
- Prompt-agent quickstart: https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/prompt-agent
- Agent tools overview: https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/tool-catalog
- Agents, conversations, and responses: https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/runtime-components

# Exam-readiness research

See [`../../research/gaps/agentic-ai-single-agent-solutions.md`](../../research/gaps/agentic-ai-single-agent-solutions.md).