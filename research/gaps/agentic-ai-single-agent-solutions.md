# Agentic AI and single-agent solutions — Exam-readiness gaps

Research date: **July 30, 2026**

## Official scope

Current AI-901 objectives relevant to this cluster:

- Identify scenarios for common AI workloads, including generative and agentic AI.
- Create and test a single-agent solution in the Foundry portal.

The separate objective **Create a lightweight client application for an agent** is acknowledged here only to preserve the end-to-end boundary. Detailed packages, endpoint shapes, SDK calls, and Python-to-C# code recognition belong to the later **Foundry SDK chat and agent clients** gap cluster.

## Official material reviewed

- AI-901 study guide: https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-901
- AI agents concept unit: https://learn.microsoft.com/en-us/training/modules/fundamentals-generative-ai/7-agents
- Creating an agent unit: https://learn.microsoft.com/en-us/training/modules/get-started-with-generative-ai-and-agents/4-creating-an-agent
- Get started with generative AI and agents module: https://learn.microsoft.com/en-us/training/modules/get-started-with-generative-ai-and-agents/
- Microsoft Foundry Agent Service overview: https://learn.microsoft.com/en-us/azure/foundry/agents/overview
- Prompt-agent quickstart: https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/prompt-agent
- Agent tools overview: https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/tool-catalog
- Agents, conversations, and responses: https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/runtime-components

## Coverage assessment

The Learn material establishes the correct core model: an agent combines a model, instructions, and tools; it can use knowledge, maintain conversational context, choose actions, and be created and tested in the Foundry portal. It also shows a lightweight Project API client, but that client code is intentionally deferred to the mapped SDK-client research cluster.

The main weakness is compression. The concept unit is approximately five minutes and the implementation unit is approximately four minutes, yet the blueprint expects candidates to recognize agentic scenarios and create and test a reusable single-agent solution. The material does not provide one durable comparison between a model-only chat, a single agent, a deterministic workflow, and a multi-agent system. It also gives only a thin testing method and does not clearly separate conversation history, retrieved knowledge, tools, and persistent memory.

Current product documentation adds another source of confusion: the new generally available Foundry Agent Service uses prompt agents, versioned agent assets, conversations, responses, and the Foundry Projects 2.x API, while older Foundry Agent Service classic documentation and 1.x SDK patterns are deprecated. Durable study material should teach the concepts and portal flow first, then treat exact SDK syntax as version-sensitive.

## Gaps

### A model-only chatbot and an agent are easy to confuse

- **Type:** Underexplained conceptual distinction
- **Evidence:** Learn defines the agent as model + instructions + tools, but does not provide a stable side-by-side comparison with a normal model request or an application-controlled workflow.
- **Confidence:** High
- **Why it matters:** A model can generate an answer without being an agent. An agent packages reusable behavior and can decide to use tools or retrieved context across a task.
- **Study material needed:** A comparison of model-only generation, single agents, deterministic workflows, and multi-agent systems.

### The agent execution loop is implied rather than taught

- **Type:** Underexplained workflow gap
- **Evidence:** Official documentation explains that the model can inspect tools, choose one, receive the result, and continue, but the Learn unit does not show the complete loop as one sequence.
- **Confidence:** High
- **Why it matters:** Candidates should recognize that tool use is conditional and iterative, not a fixed call made for every request.
- **Study material needed:** A compact request → decide → tool call → result → continue → response flow.

### Tools, knowledge, context, and memory need explicit boundaries

- **Type:** Terminology gap
- **Evidence:** The Learn unit uses the useful shorthand “Tools = actions” and “Knowledge = context.” Current Agent Service documentation also exposes retrieval capabilities such as File Search through the tool catalog, while conversations preserve multi-turn history.
- **Confidence:** High
- **Why it matters:** Retrieved knowledge does not train the model, a conversation is not automatically long-term memory, and a tool can provide either information or an action.
- **Study material needed:** A table separating instructions, tools, knowledge/retrieval, conversation history, and persistent memory.

### Tool selection needs scenario practice

- **Type:** Practice gap
- **Evidence:** Microsoft lists web search, code interpreter, file search, function calling, MCP, and OpenAPI-style capabilities, but the Learn unit provides only examples rather than decision practice.
- **Confidence:** High
- **Why it matters:** The exam can ask whether a scenario needs current public information, private documents, calculation/code execution, or an external business action.
- **Study material needed:** A scenario-to-tool decision table and original questions.

### The portal objective needs one end-to-end workflow

- **Type:** Implementation gap
- **Evidence:** Learn distributes the steps across model choice, instructions, tools, knowledge, saving the agent, and playground testing.
- **Confidence:** High
- **Why it matters:** “Create and test” is an implementation verb. Candidates should recognize the order and purpose of each portal step even when exact UI labels evolve.
- **Study material needed:** A portal workflow from project and model deployment through agent configuration, testing, refinement, and versioning.

### “Test the agent” is too close to a single happy-path prompt

- **Type:** Practice and reliability gap
- **Evidence:** The module says to continue testing and refining in the playground, but does not provide a compact test matrix.
- **Confidence:** High
- **Why it matters:** A valid test checks expected answers, tool selection, arguments, grounding, refusal or confirmation behavior, multi-turn context, missing data, and tool failures.
- **Study material needed:** A small testing matrix for happy path, tool-required, tool-not-needed, ambiguous, unsafe, failure, and multi-turn cases.

### Agent tools increase the security and governance surface

- **Type:** Responsible-AI implementation bridge
- **Evidence:** Agent Service tools can search data, execute code, and call external APIs. Official documentation emphasizes identity, RBAC, managed authentication, tracing, and evaluation.
- **Confidence:** High
- **Why it matters:** Instructions alone are not an authorization boundary. Action tools should use least privilege, controlled inputs, confirmation for consequential actions, and observable execution.
- **Study material needed:** Fundamentals-level safety rules for permissions, secrets, validation, human approval, and monitoring.

### New Agent Service and classic examples can be mixed accidentally

- **Type:** Terminology and versioning gap
- **Evidence:** Current Microsoft documentation marks Agent Service classic as deprecated and directs users to the new generally available service. The current prompt-agent quickstart uses the Foundry Projects 2.x API and warns that it is incompatible with 1.x examples.
- **Confidence:** High
- **Why it matters:** Package names, object models, endpoint examples, and agent lifecycle concepts differ. Combining snippets can produce code that looks plausible but cannot run.
- **Study material needed:** A “new versus classic” warning and a rule to follow one current documentation generation end to end.

### Advanced agent types can distract from the single-agent objective

- **Type:** Scope-control gap
- **Evidence:** Current Agent Service documentation includes prompt agents, hosted agents, multi-agent patterns, publishing, tracing, evaluation, and many tools. The exam objective is narrower: create and test a single-agent solution in the portal.
- **Confidence:** High
- **Why it matters:** Hosted runtimes and multi-agent orchestration are useful recognition topics but should not displace the portal prompt-agent workflow.
- **Study material needed:** A scope table that marks prompt-agent portal work as core and hosted/multi-agent architecture as recognition only.

### The agent-client objective requires a separate version-sensitive asset

- **Type:** Deferred implementation gap
- **Evidence:** The Learn unit includes a Project API sample, while current product documentation has newer versioned-agent and Responses API patterns.
- **Confidence:** High
- **Why it matters:** Client code is explicitly in the blueprint, but mixing it into this concept-and-portal file would blur the stable workflow with fast-changing SDK syntax.
- **Study material needed:** The later Foundry SDK chat and agent clients asset should map current Python and C# flows and record package/API versions.

## Study-session assets created

Created **July 30, 2026**:

- [`../../docs/topics/agentic-ai-single-agent-solutions.md`](../../docs/topics/agentic-ai-single-agent-solutions.md)
  - agent versus model-only chat, deterministic workflow, and multi-agent comparison
  - model, instructions, tools, knowledge, conversation, and memory distinctions
  - agent execution loop
  - scenario-to-tool selection table
  - single-agent Foundry portal workflow
  - agent testing matrix
  - responsible-AI and security checklist
  - new-versus-classic terminology warning
  - sixteen original scenario and recognition questions with answer key
  - future study-session checkpoint and guided portal exercise

**Research status:** Complete. The missing study assets now exist and are linked. Learner understanding is intentionally not evaluated during a gap-research session.

## Claims not accepted

- Every application that sends a prompt to a model is an agent.
- An agent must always have several tools.
- An available tool is invoked for every user request.
- Knowledge files permanently train or fine-tune the model.
- Conversation history is the same thing as durable cross-session memory.
- An agent is automatically more accurate than a model-only application.
- System instructions are a complete security or authorization boundary.
- Giving an agent a write-capable tool makes its actions automatically safe.
- Multi-agent architecture is required to satisfy the single-agent objective.
- Hosted agents are the default scope of the AI-901 portal objective.
- Agent Service classic and current Foundry Projects 2.x code can be freely combined.
- A first-hand exam report proves the frequency of agent questions.
- Any dump-derived claim about real exam questions.

## Suggested future study checkpoint

Use the prepared checkpoint in `docs/topics/agentic-ai-single-agent-solutions.md` during a later concept, implementation, or review session. That session may evaluate whether the learner can:

1. Explain why an agent is more than a single model request.
2. Identify the roles of the model, instructions, tools, knowledge, and conversation.
3. Trace the conditional tool-calling loop.
4. Select a suitable tool from a scenario.
5. Create a prompt agent in the Foundry portal in the correct order.
6. Test tool use, grounding, ambiguity, failures, and unsafe requests.
7. Distinguish conversation history from persistent memory.
8. Apply least privilege and human confirmation to action tools.
9. Distinguish prompt agents, hosted agents, and multi-agent systems at recognition level.
10. Avoid mixing current and classic SDK/API generations.

This checkpoint is stored here as a handoff. It is not administered during gap research.

## External evidence reviewed

Two dated first-hand beta exam reports were reviewed:

- https://www.linkedin.com/posts/alanro_ai901-ai103-ai900-activity-7452955327089672192-JoGp
- https://carlarjenkins.medium.com/my-ai-901-azure-ai-fundamentals-beta-exam-experience-6a320227b602

They support the broader conclusion that AI-901 is more implementation- and code-oriented than AI-900. They do not provide sufficiently specific, independently corroborated evidence about agent-question frequency, so no agent-specific exam-distribution claim was accepted.

Sources that advertised real, exact, verified, or dump-style exam questions were rejected and not used.