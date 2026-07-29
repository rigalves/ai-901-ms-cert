# Generative AI, prompts, and model configuration

Verified: **July 29, 2026**

> This document was prepared by a gap-research session for use during a later study session. The questions and exit check are stored here; they are not administered during research.

## Official exam objectives

Current AI-901 objectives covered here:

- Identify an appropriate AI model, based on capabilities.
- Identify appropriate model deployment options and configuration parameters.
- Create effective system and user prompts for generative AI models.
- Deploy a model and interact with it in the Foundry portal.

Study guide: https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-901

Model mechanics are covered in [`responsible-ai-model-concepts.md`](responsible-ai-model-concepts.md). Foundry hierarchy, endpoints, and authentication are covered in [`foundry-foundation.md`](foundry-foundation.md). SDK chat clients and agents are intentionally left for later mapped clusters.

# Part 1 — Select an appropriate model

## The durable rule

Do not start with a brand name. Start with the workload requirements, shortlist models by capability, and then test the candidates with representative data.

Flow:

`workload and constraints → model category → catalog filters and model cards → benchmark shortlist → representative evaluation → selected model`

## Model-selection dimensions

| Dimension | Questions to ask | Why it changes the choice |
|---|---|---|
| **Task** | Chat, reasoning, coding, summarization, embeddings, image generation, or another inference task? | Different model categories are optimized for different outputs |
| **Modality** | Text only, or must the model accept images, audio, video, or files? | A text-only model cannot satisfy a multimodal requirement |
| **Quality and reasoning** | Is ordinary language generation enough, or is complex multi-step reasoning required? | Higher reasoning capability can improve difficult tasks but often increases latency and cost |
| **Context and output limits** | How much input and conversation history must fit, and how large can the answer be? | Context window and maximum output vary by model |
| **Latency and throughput** | Interactive response, high request volume, or offline batch processing? | Small/fast models and deployment capacity choices can matter more than maximum quality |
| **Cost** | What is the acceptable input, output, reasoning, hosting, or capacity cost? | A less expensive model that meets the target is often the better engineering choice |
| **Safety and evaluation** | Which harmful-output, groundedness, fairness, or quality metrics matter? | Model cards and evaluators reveal limitations and tradeoffs |
| **Availability and governance** | Required region, deployment type, SLA, support, licensing, lifecycle, or provider terms? | A capable model is unusable if it cannot be deployed or governed as required |

## Model categories and scenario cues

| Scenario cue | Appropriate category | Important tradeoff |
|---|---|---|
| Interactive assistant, drafting, ordinary summarization | General chat/instruction model | Balance response quality, latency, and cost |
| Complex planning, difficult logic, or multi-step code analysis | Reasoning-capable model | Usually more latency and token use |
| High-volume or constrained-cost application with a narrow task | Small language model | Validate whether lower cost/latency still meets quality requirements |
| Text plus images, audio, video, or other media | Multimodal model | Verify the exact supported input and output modalities |
| Semantic search or vector retrieval | Embedding model | Produces vectors rather than a conversational answer |
| New images from text or image instructions | Image-generation model | Check image-specific safety, format, quality, and cost |
| Specialized industry or domain task | Domain-specialized model or Foundry tool | Validate domain performance, terms, support, and compliance |

Current model family names are examples, not durable exam facts. Use the Foundry model catalog and model cards to verify capabilities, regions, lifecycle, context limits, supported features, licensing, and deployment options.

## Benchmarks: useful, but not the final answer

Foundry can compare models using metrics such as:

- quality or task accuracy;
- safety;
- latency and throughput;
- cost;
- groundedness, relevance, coherence, and fluency for suitable evaluations.

Use benchmarks to create a shortlist. Then test representative prompts and data from the intended workload.

A benchmark winner can still lose for your application because of unsupported modality, context size, latency, cost, language, domain behavior, region, licensing, deployment restrictions, safety, or groundedness requirements.

# Part 2 — Model, deployment, endpoint, and request

## Core objects

| Term | Meaning | .NET mental mapping—not an exact equivalence |
|---|---|---|
| **Model** | The trained AI artifact and its capabilities | A reusable implementation artifact, not yet your running service |
| **Model version** | A particular released version of the model | A package/runtime version with its own lifecycle |
| **Deployment** | A named configured instance that makes a model callable | A hosted service deployment with configuration and capacity |
| **Endpoint** | The URL through which a deployed model is invoked | The service base address or route |
| **Deployment name** | The application-facing name assigned to the deployment | A configured service identifier; Azure OpenAI-style requests commonly pass it in the `model` field |
| **Request parameters** | Per-call options that influence one generated response | An options object supplied with one API request |

Compact flow:

`model in catalog → configured deployment → endpoint + deployment name → request with prompts and parameters → response`

## Two deployment decision layers

### Layer 1: hosting option

| Option | What it means | Best recognition cue |
|---|---|---|
| **Serverless API deployment** | Microsoft hosts the model and exposes an inference API; billing is typically based on input/output usage or reserved throughput | Simplified managed inference without sizing model-hosting virtual machines |
| **Managed compute** | Model weights run on managed virtual-machine compute associated with a suitable Foundry/classic project | Specialized or open models that require hosted compute; VM/core-hour and infrastructure considerations |
| **Instant access (Preview)** | Supported models can be called without first creating a deployment | Prototyping or quickly trying a model; availability and Preview status must be checked |

### Layer 2: common serverless deployment types

| Requirement | Likely deployment type | Key tradeoff |
|---|---|---|
| General or bursty workload; broad availability and quota | **Global Standard** | Pay per token; inference can be processed globally; latency can vary |
| Processing restricted to US, EU, or APAC data zone | **Data Zone Standard** | Pay per token; processing stays within the selected zone |
| Processing must remain in one Azure region | **Standard / regional** | Regional availability and quota can be more limited |
| Consistent high-volume workload with predictable throughput and lower latency variance | **Provisioned**: global, data-zone, or regional | Reserved capacity measured in PTUs rather than ordinary pay-per-token only |
| Large offline job that is not time-sensitive | **Batch**: global or data-zone | Lower cost and separate quota; asynchronous rather than interactive |

### Fast deployment cues

- **Bursty and cost follows usage** → standard.
- **Predictable high volume and stable throughput** → provisioned.
- **Large asynchronous workload** → batch.
- **No inference-location restriction** → global may be acceptable.
- **US/EU/APAC boundary** → data zone.
- **Exactly one Azure region** → regional.

Not every model supports every deployment type or region.

## Model version and upgrade policy

A model version and an API version are different:

- **Model version:** the released model artifact used by the deployment.
- **API version:** the contract used to call the service.

Model versions have lifecycles and retirement dates. Depending on deployment configuration, a version can remain fixed until retirement or upgrade automatically according to the selected policy. Production systems should monitor retirement notifications and retest after upgrades.

For AI-901, recognize the distinction and why model lifecycle belongs in deployment planning; do not memorize retirement dates.

# Part 3 — Configuration parameters

## Separate the configuration layers

| Layer | Examples | What it controls |
|---|---|---|
| **Deployment-time** | Deployment type, processing geography, model/version, upgrade policy, TPM allocation, PTUs, safety configuration | Hosting, lifecycle, throughput, data processing, and governance |
| **Request-time** | System instructions, user input, temperature, `top_p`, maximum output tokens | The behavior and limits of one model call |
| **Application context** | Conversation history, retrieved grounding data, examples, user preferences, validation rules | What information and workflow surround the request |

## Essential parameter distinctions

| Setting | Meaning | Good scenario cue | Common confusion |
|---|---|---|---|
| **Temperature** | Adjusts sampling randomness; lower values are more focused, higher values more varied | Low for extraction/classification; higher for brainstorming | Low temperature does not guarantee truth or identical output |
| **`top_p`** | Restricts sampling to a probability mass of likely tokens | Alternative way to tune randomness | Microsoft recommends changing temperature or `top_p`, not both at once, while evaluating |
| **Maximum output tokens** | Caps the generated response length | Control response size and per-request token usage | It does not set deployment capacity |
| **TPM** | Tokens-per-minute quota/rate allocation for a deployment | Scale and throttling | It does not cap one answer's length |
| **PTU** | Provisioned throughput unit representing reserved model-processing capacity | Predictable high-throughput provisioned deployment | It is not model training or fine-tuning |
| **System instructions** | Per-request or conversation-level behavioral guidance | Role, tone, boundaries, format, uncertainty handling | It is not a complete security or factual guarantee |

Parameter support varies by model and API. Always verify the selected model's documentation and playground controls.

# Part 4 — Create effective prompts

## System prompt versus user prompt

| Prompt type | Usually supplied by | Purpose |
|---|---|---|
| **System prompt/instructions** | The application | Defines role, behavioral boundaries, tone, priorities, response format, and handling rules |
| **User prompt** | The end user or application on the user's behalf | States the task, question, input, context, and desired result for the current request |

The system prompt guides the overall behavior. The user prompt requests the specific work. The application may include both, plus history, retrieved evidence, or examples.

## Reusable prompt anatomy

### System instructions

1. **Role:** What kind of assistant is this?
2. **Scope and boundaries:** What should it do or refuse?
3. **Behavior:** How should it handle uncertainty, evidence, and missing data?
4. **Style and output rules:** Tone, length, format, citations, or schema.

### User prompt

1. **Task:** Use a clear action verb.
2. **Context and audience:** Explain why and for whom.
3. **Primary input:** Include the text, data, or question, preferably with clear delimiters.
4. **Examples:** Add input/output examples when the desired behavior is hard to describe.
5. **Output structure:** Bullets, table, JSON schema, numbered steps, and so on.
6. **Constraints:** Length, allowed evidence, language, exclusions, or required fields.

Compact template:

```text
System:
You are [role].
Follow these boundaries: [scope/rules].
When evidence is missing: [uncertainty behavior].
Return: [style/format].

User:
Task: [specific action].
Audience/context: [who/why].
Input:
---
[content]
---
Output requirements: [structure and limits].
```

## Prompt techniques to recognize

| Technique | What it does | What it does not do |
|---|---|---|
| **Zero-shot** | Gives instructions without examples | Does not show the model an exact desired pattern |
| **One-shot/few-shot** | Includes one or several example input/output pairs for the current inference | Does not permanently train or change model weights |
| **Conversation history** | Supplies earlier turns so the current request has continuity | Is not unlimited memory; it consumes context and may be summarized or truncated |
| **Grounding/RAG** | Retrieves relevant source information and includes it with the request | Does not fine-tune the model; retrieved evidence can still be incomplete or misused |
| **Structured output request** | Specifies a table, bullets, fields, or JSON-like schema | Does not guarantee valid or truthful output without validation |

## Prompt quality checklist

A good prompt is usually clear, specific, contextual, explicit about audience and structure, illustrated with examples when useful, grounded when reliable facts matter, tested with representative and edge cases, and paired with validation and application controls.

### Important rule

`prompting guides behavior; grounding supplies evidence; evaluation measures results; application controls enforce requirements`

A system prompt alone is not a security boundary. Prompt engineering does not guarantee factual accuracy, safety, or universal generalization.

# Part 5 — Foundry portal workflow

Recognize this end-to-end sequence:

1. **Define the workload:** task, modality, quality, context, latency, throughput, cost, residency, safety, and support requirements.
2. **Open the model catalog:** filter by inference task, supported features, provider/collection, region, deployment option, lifecycle, and other requirements.
3. **Inspect model cards:** capabilities, limits, versions, deployment availability, benchmarks, licensing, risks, and caveats.
4. **Compare candidates:** use leaderboards and benchmark metrics to create a shortlist.
5. **Test representative data:** use your own prompts or evaluation set; do not select only from aggregate public scores.
6. **Deploy the selected model:** choose deployment name, type, geography, model version, and capacity/quota settings.
7. **Open the playground:** configure system instructions, send user prompts, and adjust request parameters.
8. **Evaluate outputs:** quality, safety, groundedness, latency, token use, and failure cases.
9. **Capture the working configuration:** save prompts/settings or use generated code later in the separate client-implementation workflow.

Flow:

`requirements → catalog → model card and benchmarks → representative evaluation → deployment → playground → prompts and parameters → validate → capture configuration`

# Prepared practice

Use these questions during a future study or review session. Answer before opening the key.

1. A mobile application must perform a narrow language task with low latency and low cost. Which model category should be considered first?
2. An application must interpret a product photo and the user's text question in the same request. Which capability is required?
3. A semantic-search system needs vectors for documents and queries. Which model category should it use?
4. A model has the highest public reasoning score, but it exceeds the application's latency target. Is it automatically the correct model?
5. A general application has variable traffic and no geographic inference restriction. Which deployment type is a common starting point?
6. Prompts and responses must be processed only within the European Union data zone. Which deployment family matches the requirement?
7. A regulated workload requires processing in one specific Azure region. Should you choose global, data-zone, or regional processing?
8. A high-volume service needs reserved capacity and predictable latency. Standard, provisioned, or batch?
9. Millions of documents can be summarized asynchronously overnight. Standard interactive or batch?
10. Which setting caps the length of one generated response: TPM or maximum output tokens?
11. Which setting represents deployment token-processing quota/rate: TPM or temperature?
12. Which parameter would generally be lowered for focused extraction output: temperature or PTU?
13. Microsoft recommends changing both temperature and `top_p` simultaneously for every experiment. True or false?
14. Which prompt type should define the assistant's role, boundaries, tone, and default format?
15. Which prompt type contains the user's current task and input?
16. Including three labeled input/output examples is zero-shot, few-shot, or fine-tuning?
17. Retrieving policy text and adding it to the prompt is RAG or model retraining?
18. What is the correct portal sequence: deploy first and define requirements later, or requirements → compare/evaluate → deploy → playground test?

<details>
<summary><strong>Answer key</strong></summary>

1. **A small language model**, provided representative testing confirms it meets quality requirements.
2. **A multimodal model** that supports image and text input.
3. **An embedding model.** It produces semantic vectors rather than ordinary chat text.
4. **No.** Selection must also satisfy latency, cost, availability, safety, and workload-specific quality requirements.
5. **Global Standard** is a common starting point for bursty general workloads when global processing is acceptable.
6. **Data Zone Standard or Data Zone Provisioned**, depending on whether pay-per-token flexibility or reserved predictable capacity is needed.
7. **Regional processing.** Global can process in any Azure region; data zone limits processing to a zone rather than one exact region.
8. **Provisioned.** It reserves capacity using PTUs and targets predictable throughput/latency.
9. **Batch.** The workload is large, asynchronous, and not latency-sensitive.
10. **Maximum output tokens.** TPM is deployment throughput/quota.
11. **TPM.** Temperature affects sampling randomness.
12. **Temperature.** Lower values generally make output more focused.
13. **False.** Evaluate by changing one sampling control at a time.
14. **System prompt/instructions.**
15. **User prompt.**
16. **Few-shot prompting.** The examples condition the current inference; they do not permanently change weights.
17. **RAG/grounding.** Retrieved context is supplied during inference.
18. **Requirements → compare/evaluate → deploy → playground test.**

</details>

## Prepared exit check

Use this only during a future study or review session. Without notes, explain all eight statements:

1. Model selection starts with workload capabilities and constraints, not the newest model name.
2. Benchmarks shortlist candidates; representative evaluation selects one.
3. A model artifact, model version, deployment, deployment name, endpoint, and request parameter are different concepts.
4. Serverless versus managed compute is a different layer from standard versus provisioned versus batch.
5. Global, data-zone, and regional deployments differ in inference processing location.
6. Maximum output tokens, TPM, PTU, temperature, and `top_p` control different things.
7. System prompts guide overall behavior; user prompts request the current task.
8. Few-shot examples, conversation history, and RAG add inference context; they do not permanently retrain the model.

**Suggested future study-session heuristic:** all eight explanations correct plus at least 15 of 18 practice answers. This is not an official Microsoft passing threshold.

## Exam-readiness gaps

Matching research: [`../../research/gaps/generative-ai-prompts-model-configuration.md`](../../research/gaps/generative-ai-prompts-model-configuration.md)

Accepted gaps addressed by this topic file:

- **Capability-first model selection — High confidence:** stable categories and decision dimensions instead of volatile brand memorization.
- **Representative evaluation — High confidence:** benchmarks shortlist; workload-specific testing decides.
- **Deployment terminology — High confidence:** hosting option, deployment type, geography, capacity, and version are separated.
- **Configuration layers — High confidence:** deployment-time, request-time, and application context are explicitly distinguished.
- **Prompt construction practice — High confidence:** reusable system/user anatomy, examples, structure, constraints, and grounding.
- **Inference-context distinctions — High confidence:** zero-shot, few-shot, history, RAG, and fine-tuning are not conflated.
- **Portal workflow — High confidence:** discovery through playground testing is represented as one end-to-end flow.

The research-session deliverables are complete. These exercises are reserved for a future study session; no learner status is inferred from their creation.

## Official sources

- AI-901 study guide: https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-901
- Prompts unit: https://learn.microsoft.com/en-us/training/modules/fundamentals-generative-ai/6-writing-prompts
- Generative AI models unit: https://learn.microsoft.com/en-us/training/modules/get-started-with-generative-ai-and-agents/2-generative-ai-models
- Using a generative AI model unit: https://learn.microsoft.com/en-us/training/modules/get-started-with-generative-ai-and-agents/3-using-generative-ai-models
- Microsoft Foundry Models overview: https://learn.microsoft.com/en-us/azure/foundry/concepts/foundry-models-overview
- Model benchmarks and leaderboards: https://learn.microsoft.com/en-us/azure/foundry/concepts/model-benchmarks
- Deployment types for Foundry Models: https://learn.microsoft.com/en-us/azure/foundry/foundry-models/concepts/deployment-types
- Model versioning: https://learn.microsoft.com/en-us/azure/foundry/foundry-models/concepts/model-versions
- Foundry model endpoints: https://learn.microsoft.com/en-us/azure/foundry/foundry-models/concepts/endpoints
- Prompt engineering techniques: https://learn.microsoft.com/en-us/azure/foundry/openai/concepts/prompt-engineering
- System message design: https://learn.microsoft.com/en-us/azure/foundry/openai/concepts/advanced-prompt-engineering
- Responses API reference: https://learn.microsoft.com/en-us/rest/api/microsoft-foundry/azureopenai/responses

## External gap-research sources

None. Official Microsoft sources were sufficient for this cluster.

## Metadata

- Verified on: July 29, 2026
- Official blueprint checked on: July 29, 2026
- Research material status: Complete
- Study-session status: Completed previously at the concept-module level; prepared review and implementation asset not yet used