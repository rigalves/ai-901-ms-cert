# Responsible AI and model concepts

Verified: **July 29, 2026**

> This document was prepared by a gap-research session for use during a later study session. The questions and exit check are stored here; they are not administered during research.

## Official exam objectives

Current AI-901 objectives covered here:

- Describe considerations for fairness in an AI solution.
- Describe considerations for reliability and safety in an AI solution.
- Describe considerations for privacy and security in an AI solution.
- Describe considerations for inclusiveness in an AI solution.
- Describe considerations for transparency in an AI solution.
- Describe considerations for accountability in an AI solution.
- Describe how generative AI models work.

Study guide: https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-901

Model selection, deployment options, configuration parameters, prompts, and agents are covered by later mapped gap clusters.

# Part 1 — Responsible AI

## Official summary

Responsible AI means designing, building, deploying, and operating AI systems in ways that reduce harm and keep people and organizations responsible for the results.

Microsoft identifies six principles:

| Principle | Core question | Typical design response | Common confusion |
|---|---|---|---|
| **Fairness** | Are people or groups treated equitably? | Use representative data, compare outcomes across groups, test for bias, and mitigate unjustified differences | **Inclusiveness:** fairness is about treatment and outcomes; inclusiveness is about who can participate and use the system |
| **Reliability and safety** | Does the system work dependably and avoid unsafe behavior? | Test expected and unexpected conditions, use confidence thresholds or safe fallbacks, monitor failures, and add human review for consequential actions | **Privacy and security:** safety concerns harmful behavior or failure; privacy/security concerns protection of data and access |
| **Privacy and security** | Is sensitive data protected from misuse, exposure, or unauthorized access? | Minimize collected data, restrict access, protect stored and transmitted data, define retention, and prevent leakage | **Transparency:** disclosure does not replace access control or data protection |
| **Inclusiveness** | Can people with different abilities, languages, and backgrounds use the solution? | Provide accessible interaction modes such as captions, keyboard support, readable interfaces, and appropriate language support | **Fairness:** accessibility can improve inclusion even when outcome fairness is not the main issue |
| **Transparency** | Do users understand that AI is involved, what it can do, and where its limits are? | Disclose AI use, explain important limitations, communicate confidence or uncertainty appropriately, and provide understandable reasons when needed | **Accountability:** transparency explains the system; accountability identifies who owns and governs it |
| **Accountability** | Which people or organization are answerable and in control? | Assign owners, define governance and approval, maintain auditability, monitor operation, and prepare incident-response or rollback plans | **Transparency:** a system can be explained but still lack clear ownership and oversight |

### Fast selection cues

- Different outcomes across demographic or user groups → **Fairness**
- Unsafe behavior, unreliable performance, or failure under unusual conditions → **Reliability and safety**
- Sensitive data, unauthorized access, retention, or leakage → **Privacy and security**
- Accessibility or participation barriers → **Inclusiveness**
- User disclosure, explainability, capabilities, or limitations → **Transparency**
- Ownership, governance, approvals, oversight, or human control → **Accountability**

A realistic solution can involve several principles. For exam-style scenarios, identify the **most direct** principle named by the requirement.

## Commonly confused pairs

### Fairness versus inclusiveness

- **Fairness:** compare treatment or outcomes between people or groups.
- **Inclusiveness:** remove barriers so diverse people can use and benefit from the system.

Example:

- A speech model has significantly worse recognition accuracy for one accent group → primarily **fairness**.
- A speech application adds captions for users who cannot hear the audio → primarily **inclusiveness**.

### Transparency versus accountability

- **Transparency:** users and stakeholders can understand the AI role, capabilities, limitations, and important reasons.
- **Accountability:** named humans or organizations own decisions, governance, monitoring, and remediation.

Example:

- The application tells users that responses are AI-generated and may contain errors → **transparency**.
- A review board must approve deployment, and an owner can suspend the system after an incident → **accountability**.

### Reliability and safety versus privacy and security

- **Reliability and safety:** prevent harmful behavior or unsafe failure.
- **Privacy and security:** protect data, credentials, access, and confidentiality.

Example:

- A robot refuses to move when object-detection confidence is too low → **reliability and safety**.
- Temporary facial images are deleted and access is limited to authorized staff → **privacy and security**.

## Responsible AI lifecycle

Current Microsoft guidance organizes responsible AI work into four recurring stages:

1. **Identify** potential harms for the specific model, application, users, and deployment scenario.
2. **Measure** the frequency and severity of those harms with defined metrics and test sets.
3. **Mitigate** the harms with layered controls, then measure again to verify effectiveness.
4. **Operate** with monitoring, feedback, human oversight, incident response, and rollback readiness.

Flow:

`identify harms → measure them → mitigate them → operate and monitor → repeat`

### Important rule

A content filter or guardrail is **one mitigation**, not the complete Responsible AI process. It does not replace fairness testing, privacy controls, accessible design, transparency, governance, monitoring, or human accountability.

# Part 2 — How generative AI models work

## One-sentence mental model

A generative language model learns statistical relationships between tokens from large amounts of text, then generates a response by repeatedly predicting a likely next token from the prompt and the tokens already produced.

## Core terms

| Term | Meaning | .NET mental mapping—not an exact equivalence |
|---|---|---|
| **Token** | A unit of text such as a word, subword, punctuation mark, or character sequence | A parsed input unit represented internally by an integer ID |
| **Token ID** | The numeric identifier assigned to a token in the model vocabulary | Similar to an integer key used to look up a known item |
| **Vector** | An array of numeric values used to represent features | Similar in shape to a `float[]`, but its dimensions are learned features rather than fields chosen by a developer |
| **Embedding** | A vector representation that captures contextual or semantic relationships | A dense learned feature representation, not a DTO and not the original text |
| **Attention** | A mechanism that weights which surrounding tokens matter most for the current context | Similar to dynamically assigning relevance scores, not a hard-coded rule engine |
| **Learned weights or parameters** | Numeric values adjusted during training so the model improves its predictions | The model's learned state; unlike application configuration, it is produced by training rather than manually authored |
| **Prompt** | The input sequence that starts or guides generation | The request payload sent to the model |
| **Completion** | The generated continuation of the prompt | The response produced token by token |
| **Inference** | Using a trained model to generate or evaluate an output | Calling the deployed model; training is not happening during the normal request |

## Training flow

The detailed mathematics are outside AI-901. Recognize this conceptual sequence:

1. Training text is broken into **tokens**.
2. Tokens receive numeric IDs and initial vector representations.
3. Positional information indicates token order.
4. Transformer attention processes tokens in context and learns useful relationships.
5. The model predicts a next token, compares the prediction with the known training text, and adjusts learned weights to reduce error.
6. Repeating this over large datasets produces a trained model that captures statistical linguistic and semantic patterns.

Flow:

`training text → tokens and positions → contextual processing with attention → prediction error → adjusted learned weights → trained model`

## Inference flow

When an application sends a prompt:

1. The prompt is tokenized.
2. The trained model evaluates the tokens and their context.
3. It calculates probabilities for possible next tokens.
4. A token is selected according to the generation method and configuration.
5. The selected token is appended to the sequence.
6. The process repeats until the response ends or reaches a limit.

Flow:

`prompt → tokens and context → next-token probabilities → selected token → append → repeat → completion`

## Key distinctions

### Training versus inference

| Training | Inference |
|---|---|
| Learns or adjusts model weights | Uses already learned weights |
| Requires large datasets and substantial compute | Handles a prompt or application request |
| Compares predictions with known examples | Produces an output for new input |
| Creates or updates the model artifact | Calls the deployed model |

### Token versus embedding versus learned weight

- A **token** is a unit of input or output text.
- An **embedding** is a numeric representation of meaning or context.
- A **learned weight** is part of the model state that influences its calculations.

They are related, but they are not interchangeable.

### Probable completion versus verified fact

The model generates a statistically plausible continuation. Fluency does not prove factual accuracy.

Engineering consequences:

- test outputs for the intended scenario;
- measure important failure modes;
- use grounding or validation when accuracy matters;
- communicate limitations;
- keep appropriate human oversight for consequential decisions.

This is where model mechanics connect to **reliability and safety**, **transparency**, and **accountability**.

## Prepared scenario and concept practice

Use these questions during a future study or review session. Answer before opening the key.

1. A hiring model advances qualified applicants from one demographic group much less often than equally qualified applicants from another group. Which principle is most directly involved?
2. A warehouse robot refuses to move an object when its vision confidence falls below a safe threshold. Which principle is most directly involved?
3. An airport access system deletes temporary face images after use and restricts them to authorized security staff. Which principle is most directly involved?
4. A speech-based assistant adds synchronized text captions so users with hearing impairments can use it. Which principle is most directly involved?
5. A bank tells applicants that AI assists with decisions and explains the main factors and known limitations. Which principle is most directly involved?
6. An organization assigns a named system owner, requires a governance review before release, and maintains an incident rollback plan. Which principle is most directly involved?
7. A speech recognizer works well overall but has consistently lower accuracy for speakers with a particular accent. Is the primary issue fairness or inclusiveness?
8. Which model component is a unit such as a word, subword, or punctuation mark?
9. Which term means a dense numeric representation that captures contextual or semantic relationships?
10. During which phase are learned weights adjusted by comparing predictions with known training examples: training or inference?
11. What mechanism dynamically weighs which surrounding tokens are most relevant in context?
12. A model produces a fluent, confident answer. Does next-token prediction guarantee that the answer is factually correct?

<details>
<summary><strong>Answer key</strong></summary>

1. **Fairness.** The cue is an unjustified difference in outcomes between groups.
2. **Reliability and safety.** The system uses a safe fallback when confidence is inadequate.
3. **Privacy and security.** The requirements concern retention and authorized access to personal data.
4. **Inclusiveness.** Captions remove an accessibility barrier.
5. **Transparency.** The users are informed about AI involvement, decision factors, and limitations.
6. **Accountability.** Ownership, governance review, and rollback responsibility are explicit.
7. **Fairness.** Different model performance across groups is an outcome disparity. Adding accessibility modes would be inclusiveness.
8. **Token**
9. **Embedding**
10. **Training.** Inference uses the learned weights to process new input.
11. **Attention**
12. **No.** The model generates a probable continuation; factual accuracy must be evaluated or validated for the scenario.

</details>

## Prepared exit check

Use this only during a future study or review session. Without notes, explain all six statements:

1. Fairness concerns equitable treatment and outcomes; inclusiveness concerns access and participation.
2. Transparency explains the AI system; accountability establishes human ownership and control.
3. Content filters are one mitigation, not the entire Responsible AI lifecycle.
4. Training adjusts learned weights; inference uses them.
5. Tokens, embeddings, attention, and learned weights perform different roles.
6. A plausible next-token completion is not automatically a verified fact.

**Suggested future study-session heuristic:** all six explanations correct plus at least 10 of 12 practice answers. This is not an official Microsoft passing threshold.

## Exam-readiness gaps

Matching research: [`../../research/gaps/responsible-ai-model-concepts.md`](../../research/gaps/responsible-ai-model-concepts.md)

Accepted gaps addressed by this topic file:

- **Principle differentiation and scenario practice — High confidence:** comparison cues, confused pairs, and seven original Responsible AI scenarios.
- **Lifecycle context — High confidence:** identify, measure, mitigate, and operate, including the limits of content filtering alone.
- **Model-mechanics compression — High confidence:** connected training and inference flows instead of an isolated vocabulary list.
- **Probabilistic-output bridge — High confidence:** connection from next-token generation to reliability, transparency, and accountability.
- **Reported scenario emphasis — Low confidence:** supports practice format only, not an unofficial weighting claim.

The research-session deliverables are complete. These exercises are reserved for a future study session; no learner status is inferred from their creation.

## Official sources

- AI-901 study guide: https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-901
- Responsible AI unit: https://learn.microsoft.com/en-us/training/modules/get-started-ai-fundamentals/7-responsible-ai
- Large language models unit: https://learn.microsoft.com/en-us/training/modules/fundamentals-generative-ai/3-language-models
- Microsoft Responsible AI principles and approach: https://www.microsoft.com/en-us/ai/principles-and-approach/
- Responsible AI practices for Azure OpenAI in Foundry Models: https://learn.microsoft.com/en-us/azure/foundry/responsible-ai/openai/overview

## External gap-research sources

See the matching gap-research file. External candidate evidence is not used as official teaching content here.

## Metadata

- Verified on: July 29, 2026
- Official blueprint checked on: July 29, 2026
- Research material status: Complete
- Study-session status: Completed previously; review asset not yet used