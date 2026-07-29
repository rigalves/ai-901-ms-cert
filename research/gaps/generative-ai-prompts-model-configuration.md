# Generative AI, prompts, and model configuration — Exam-readiness gaps

Research date: **July 29, 2026**

## Official scope

Current AI-901 objectives relevant to this cluster:

- Identify an appropriate AI model, based on capabilities.
- Identify appropriate model deployment options and configuration parameters.
- Create effective system and user prompts for generative AI models.
- Deploy a model and interact with it in the Foundry portal.

How generative models work is covered in `responsible-ai-model-concepts.md`. Foundry hierarchy, projects, endpoints, and authentication are covered in `foundry-resources-projects-endpoints-authentication.md`. Chat clients, agents, and Python SDK recognition belong to later mapped clusters and are intentionally outside this file.

## Official material reviewed

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

## Coverage assessment

The Learn modules cover the correct workflow: discover and compare a model, deploy it, experiment in a playground, adjust prompts and parameters, and then use the working configuration from code. They also introduce the model catalog, task-oriented model selection, benchmark comparison, deployment types, temperature, maximum output tokens, and system instructions.

The main weakness is compression. Several different decisions are presented close together without a stable distinction between the **model artifact**, its **deployment**, deployment capacity and data-processing choices, and per-request generation settings. The material also names current model families, but those names change faster than the exam objective. A durable study asset should emphasize capability categories and decision criteria first, using current model names only as examples.

The prompt material correctly introduces system and user prompts, context, examples, structure, conversation history, and RAG. However, it needs a reusable prompt anatomy and explicit warnings that prompting does not permanently train the model, does not guarantee factual accuracy, and does not replace grounding, evaluation, or application controls.

## Gaps

### Current model names can distract from the capability-selection objective

- **Type:** Volatility and selection gap
- **Evidence:** The current Learn unit lists specific model families while the study guide asks candidates to identify an appropriate model based on capabilities.
- **Confidence:** High
- **Why it matters:** Memorizing a temporary leaderboard or brand list is brittle. The exam-ready skill is selecting by modality, task, quality, context, latency, throughput, cost, safety, availability, licensing, and support requirements.
- **Study material needed:** A stable model-category decision table with scenario cues and a reminder to verify current model cards.

### Benchmarks need to be connected to representative workload evaluation

- **Type:** Underexplained decision gap
- **Evidence:** Foundry exposes quality, safety, cost, latency, and throughput benchmarks, and the Learn unit also supports testing with your own data.
- **Confidence:** High
- **Why it matters:** A model that leads one public benchmark may still be unsuitable for the application's prompts, language, data, latency target, budget, safety requirements, or deployment region.
- **Study material needed:** A selection flow that uses catalog filters and benchmarks to shortlist models, then validates candidates with representative prompts and metrics.

### “Deployment option” and “deployment type” describe different decision layers

- **Type:** Terminology gap
- **Evidence:** Foundry documentation distinguishes serverless API deployments from managed compute. For serverless deployments, it then distinguishes global, data-zone, regional, standard, provisioned, and batch types. Instant access is also available as a current Preview path for supported models.
- **Confidence:** High
- **Why it matters:** A candidate can confuse where and how a model is hosted with the billing, capacity, latency, and data-processing characteristics of a serverless deployment.
- **Study material needed:** A two-layer deployment map and short scenario table.

### Deployment-time configuration and request-time generation parameters are easy to mix

- **Type:** Configuration terminology gap
- **Evidence:** The model unit discusses deployment type, model version, and TPM allocation. The playground unit discusses temperature, maximum output tokens, and system instructions. The Responses API exposes additional request settings such as `top_p`.
- **Confidence:** High
- **Why it matters:** TPM controls service capacity and throttling; maximum output tokens caps one response. Deployment geography affects data processing; temperature affects token sampling. These are not interchangeable knobs.
- **Study material needed:** A table separating deployment settings, request parameters, and prompt/application context.

### Temperature and `top_p` require practical distinctions and caveats

- **Type:** Underexplained parameter gap
- **Evidence:** Microsoft documentation says lower temperature produces more focused output and higher temperature increases randomness. `top_p` also controls sampling randomness, and Microsoft recommends changing one of the two at a time.
- **Confidence:** High
- **Why it matters:** Lower randomness is useful for extraction, classification, and repeatable business output; higher randomness can help brainstorming. Neither parameter guarantees truth, safety, or identical responses.
- **Study material needed:** Scenario-based parameter guidance and explicit false-assumption checks.

### System prompts and user prompts need a reusable construction pattern

- **Type:** Practice gap
- **Evidence:** The Learn unit defines system prompts as application-provided behavior and constraint instructions, and user prompts as task requests. The detailed prompt guidance adds task instructions, primary content, supporting context, examples, cues, structure, and grounding.
- **Confidence:** High
- **Why it matters:** Knowing definitions is not enough to create an effective prompt under an implementation objective.
- **Study material needed:** A compact prompt anatomy: role and boundaries → task → context/input → examples when useful → output format → constraints and uncertainty behavior.

### Few-shot prompting, conversation history, and RAG can be mistaken for model training

- **Type:** Conceptual distinction gap
- **Evidence:** Microsoft describes few-shot examples, previous turns, and retrieved content as context supplied during inference.
- **Confidence:** High
- **Why it matters:** These techniques influence the current request but do not permanently change model weights. RAG grounds a response with retrieved information; it is not fine-tuning.
- **Study material needed:** A comparison of zero-shot, few-shot, conversation history, RAG, and fine-tuning at recognition level.

### Prompt engineering can be mistaken for a factual or security guarantee

- **Type:** Reliability and safety bridge gap
- **Evidence:** Microsoft prompt guidance explicitly says generated responses still require validation and recommends grounding when reliable, current information matters.
- **Confidence:** High
- **Why it matters:** A strong system prompt can guide behavior, but application design still needs representative evaluation, source grounding when appropriate, output validation, content-safety controls, and human oversight for consequential uses.
- **Study material needed:** A concise “prompting helps; controls verify” rule.

### The portal workflow needs one end-to-end recognition map

- **Type:** Workflow practice gap
- **Evidence:** The Learn units distribute the steps across model discovery, benchmark comparison, deployment, playground configuration, prompt testing, and code generation.
- **Confidence:** High
- **Why it matters:** The objective explicitly includes deploying and interacting with a model in the Foundry portal.
- **Study material needed:** One sequence: define workload → filter/compare → inspect model card → deploy → open playground → set system/user prompts and request parameters → test representative cases → save or export working configuration.

## Study-session assets created

Created **July 29, 2026**:

- [`../../docs/topics/generative-ai-prompts-model-configuration.md`](../../docs/topics/generative-ai-prompts-model-configuration.md)
  - stable model-selection decision framework
  - model category and scenario table
  - two-layer deployment map
  - deployment-time versus request-time configuration table
  - temperature, `top_p`, maximum-output-token, TPM, and PTU distinctions
  - reusable system/user prompt anatomy
  - zero-shot, few-shot, conversation-history, RAG, and fine-tuning distinctions
  - Foundry portal workflow
  - eighteen original scenario and recognition questions with answer key
  - future study-session exit check

**Research status:** Complete. The missing study assets now exist and are linked. Learner understanding is intentionally not evaluated during a gap-research session.

## Claims not accepted

- The newest, largest, or highest-scoring model is automatically the correct choice.
- A public benchmark proves how a model will perform on every application's data and prompts.
- Global deployment keeps inference processing in the resource's Azure region.
- Provisioned throughput is the same thing as a fine-tuned or privately trained model.
- TPM controls the length of one response.
- Maximum output tokens controls deployment throughput.
- A temperature of zero guarantees factual accuracy or byte-for-byte identical output.
- Temperature and `top_p` should always be changed together.
- A system prompt is a complete security boundary.
- Few-shot examples or conversation history permanently retrain the model.
- RAG changes the model's learned weights.
- Every model supports the same parameters, context size, deployment types, regions, or API features.
- The client always sends the underlying catalog model ID; Azure OpenAI-style calls commonly use the deployment name in the `model` field.
- Agents and SDK client implementation are complete because portal prompting is covered.
- Any dump-derived claim about real exam questions.

## Suggested future study checkpoint

Use the prepared checkpoint in `docs/topics/generative-ai-prompts-model-configuration.md` during a later study or review session. That session may evaluate whether the learner can:

1. Choose a model category from workload requirements without relying on brand memorization.
2. Explain why benchmarks shortlist models but representative evaluation selects one.
3. Distinguish serverless versus managed compute and standard versus provisioned versus batch.
4. Select global, data-zone, or regional processing from residency requirements.
5. Separate deployment capacity settings from per-request generation parameters.
6. Explain temperature, `top_p`, maximum output tokens, TPM, and PTU.
7. Build effective system and user prompts with context, structure, examples, and constraints.
8. Distinguish few-shot context, conversation history, RAG, and fine-tuning.
9. Trace the Foundry portal workflow from model discovery through playground testing.
10. Explain why prompting improves behavior but does not eliminate validation and safety controls.

This checkpoint is stored here as a handoff. It is not administered during gap research.

## External evidence reviewed

No unofficial exam reports or question-distribution claims were needed for this cluster. The accepted gaps are supported by the official objective structure, Microsoft Learn modules, and current Microsoft Foundry documentation.