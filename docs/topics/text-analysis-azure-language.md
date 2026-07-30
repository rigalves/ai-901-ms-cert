# Text analysis and Azure Language

Verified: **July 30, 2026**

> This document was prepared by a gap-research session for use during a later study session. The questions, checkpoint, and guided exercise are stored here; they are not administered during research.

## Official exam objectives

Current AI-901 objectives covered here:

- Identify scenarios for common AI workloads, including text analysis.
- Describe common text analysis techniques, including keyword extraction, entity detection, sentiment analysis, and summarization.
- Build a lightweight application that includes text analysis.

Study guide: https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-901

# Part 1 — From raw text to useful meaning

## Durable mental model

Natural language processing converts unstructured human language into representations and results an application can use.

Compact flow:

`text corpus → tokens → normalized features or embeddings → model/algorithm → task result`

- A **corpus** is the collection of text being analyzed.
- A **token** is a unit of text, often a word, subword, punctuation mark, or symbol.
- A **feature** is a measurable representation used by a statistical or machine-learning model.
- An **embedding** is a numeric vector that represents semantic meaning and relationships.

## Common preprocessing terms

| Term | Meaning | Recognition cue |
|---|---|---|
| **Tokenization** | Splits text into tokens | Break the input into processable units |
| **Normalization** | Converts equivalent forms into a consistent representation | Lowercase text or standardize forms |
| **Stop-word removal** | Removes extremely common words that may add little analytical value | Remove words such as “the” or “and” when appropriate |
| **N-gram** | A sequence of `n` adjacent tokens | Preserve phrases such as “credit card” rather than isolated words |
| **Stemming** | Heuristically removes prefixes or suffixes | Fast but may produce a non-word root |
| **Lemmatization** | Maps a word to its dictionary base form | Uses linguistic knowledge; `running` → `run` |
| **Part-of-speech tagging** | Labels grammatical roles | Noun, verb, adjective, and so on |

These steps are techniques, not a mandatory fixed pipeline. Modern semantic models can process raw text with their own tokenizers and learned representations.

## Statistical text analysis

| Technique | Core idea | Typical use |
|---|---|---|
| **Frequency analysis** | Frequent normalized terms may reveal the main subject | Quick topic clues inside one document |
| **TF-IDF** | A term matters more when frequent in one document but uncommon across the corpus | Distinguish documents by their characteristic terms |
| **Bag of words** | Represent text by token counts or occurrences while ignoring word order | Traditional classification, such as spam detection |
| **TextRank** | Rank connected words or sentences in a graph | Keyword extraction or extractive summarization |

No formula memorization is required here. Recognize the decision:

- frequency asks, “What appears often?”
- TF-IDF asks, “What is unusually important in this document?”

## Semantic language models

Statistical counts do not fully capture context. Semantic models represent tokens as vectors called **embeddings**. Similar meanings tend to have nearby vector directions.

Modern models also use **attention** to calculate how surrounding tokens affect a token’s meaning. This produces contextual embeddings: the representation of a word can change according to its sentence.

Example:

- `bank` in “river bank”
- `bank` in “bank account”

The token is the same, but the context is different.

## .NET mental mapping — not an exact equivalence

Think of the NLP pipeline as an adapter layer that turns an unstructured `string` into a typed result the rest of the application can consume.

- preprocessing resembles input normalization;
- a feature vector resembles a numeric DTO consumed by a model;
- a task-specific analyzer resembles a service client returning a documented result contract;
- a generative model resembles a flexible interpreter whose output contract depends heavily on the prompt and model configuration.

# Part 2 — The four named exam techniques

## Core comparison

| Technique | Main question | Typical output | What it does not necessarily provide |
|---|---|---|---|
| **Keyword or key phrase extraction** | What are the main concepts? | List of important words or phrases | A coherent summary |
| **Entity detection / NER** | Which named things occur, and what categories are they? | Entity text, category, position, confidence | A canonical external identity |
| **Sentiment analysis** | What emotional polarity is expressed? | Positive, neutral, negative, mixed, and confidence values | The exact target of every opinion unless opinion mining is used |
| **Summarization** | What is the condensed meaning of the source? | Shorter text or selected source sentences | A list limited to named entities or keywords |

## Keyword extraction versus summarization

**Keyword extraction** returns concepts, not prose.

Input:

> The hotel room was quiet, the staff was friendly, and the breakfast buffet had excellent local fruit.

Possible keywords:

- quiet hotel room
- friendly staff
- breakfast buffet
- local fruit

A **summary** would instead return a sentence such as:

> The guest praised the quiet room, helpful staff, and breakfast.

Recognition rule:

`keywords = important concepts; summary = condensed message`

## Entity detection, entity linking, and PII detection

| Capability | Purpose | Example output |
|---|---|---|
| **Named entity recognition (NER)** | Finds and categorizes entities | `Seattle → Location` |
| **Entity linking** | Disambiguates a known entity and links it to a knowledge-base record | `Seattle → Wikipedia identity` |
| **PII detection** | Finds privacy-sensitive entities and can redact them | `555-0100 → PhoneNumber`, redacted text |

Entity detection is the broad exam concept. The implementation scenario determines whether the application needs ordinary NER, external linking, or privacy-focused PII detection.

Recognition rule:

- category only: **NER**
- canonical identity or external link: **entity linking**
- sensitive-data detection or redaction: **PII detection**

## Sentiment analysis versus opinion mining

**Sentiment analysis** assigns polarity at document or sentence level.

> “The phone is disappointing.” → negative

**Opinion mining** is aspect-based sentiment analysis. It connects sentiment to a specific target or attribute.

> “The screen is excellent, but the battery is terrible.”

- screen → positive
- battery → negative

Recognition rule:

`sentiment = overall polarity; opinion mining = polarity tied to an aspect`

## Extractive versus abstractive summarization

| Approach | How it works | Output relationship to source | Main caution |
|---|---|---|---|
| **Extractive** | Selects important source sentences or phrases | Reuses original wording | May feel less fluent or omit connecting context |
| **Abstractive** | Generates new concise wording | May not copy source sentences | Generated wording must be checked for unsupported claims |

TextRank is a classic example of an extractive technique when it ranks sentences and selects the highest-ranked ones.

Recognition rule:

- selected original sentences: **extractive**
- newly generated wording: **abstractive**

# Part 3 — Choose the implementation approach

## Foundry offers two broad approaches

| Decision dimension | General-purpose model | Azure Language purpose-built analyzer |
|---|---|---|
| **Instruction style** | Natural-language prompt | Specific SDK or API operation |
| **Task range** | Broad and flexible | Narrow and task-specific |
| **Output shape** | Prompt-dependent text or requested structure | Documented structured result |
| **Follow-up conversation** | Natural fit | Not the core pattern |
| **Consistency** | Probabilistic wording and formatting | More predictable operation and schema |
| **Typical result fields** | Generated answer | Labels, categories, confidence scores, offsets, redacted text |
| **Best fit** | Flexible analysis, combined tasks, explanation, conversation | Automated pipelines needing stable task-specific fields |
| **Client shown in Learn** | OpenAI Python client and Responses API | `TextAnalyticsClient` from `azure-ai-textanalytics` |
| **Agent integration** | Model is the agent’s reasoning engine | Can be exposed as an MCP tool |

## Durable selection rule

Choose a **general-purpose model** when the requirement is prompt-defined and flexible:

- combine several analytical tasks in one request;
- explain the result in natural language;
- support follow-up questions;
- adapt output depth or format through instructions;
- analyze a scenario that does not match one fixed operation.

Choose **Azure Language** when the application needs a purpose-built operation and structured fields:

- language code and confidence;
- named or PII entity categories;
- redacted text;
- stable per-document result objects;
- predictable integration into an automated pipeline.

A solution may combine both:

`Azure Language structured result → application rules or agent → model-generated explanation`

## What Microsoft means by “deterministic” here

The Learn module describes purpose-built analyzers as returning structured, deterministic results. For exam recognition, interpret this as:

- a fixed task-specific operation;
- a documented response schema;
- more predictable fields than free-form generated text.

Do not interpret it as “the machine-learning result can never be wrong or change.” Confidence values, supported-language behavior, service versions, and ambiguous input still matter.

# Part 4 — Foundry portal workflow

## Model-driven text analysis

1. Create or open a Foundry resource.
2. Create or open a Foundry project.
3. Browse the model catalog and deploy a suitable general-purpose model.
4. Open the chat playground.
5. Write a precise text-analysis prompt.
6. Test representative input and inspect the result.
7. Refine the prompt or model choice.
8. Use the deployment endpoint and client SDK in the application.

Flow:

`Foundry resource → project → model deployment → chat playground → prompt tests → client application`

## Azure Language analysis

1. Create or open a Foundry resource and project.
2. Open the Build area.
3. Navigate to Models and the AI services tab.
4. Select an Azure Language capability.
5. Test representative text and inspect structured fields.
6. Use the Language endpoint and client SDK in the application.

Flow:

`Foundry resource → project → AI services → Language capability → structured test result → client application`

Exact portal labels can change. Preserve the resource, project, capability, test, and client sequence.

# Part 5 — Lightweight client applications

## Client A: general-purpose model through the Responses API

Install:

```bash
pip install openai python-dotenv
```

Configuration shape:

```text
AZURE_OPENAI_ENDPOINT=https://<resource>.openai.azure.com/openai/v1/
MODEL_DEPLOYMENT_NAME=<deployment-name>
API_KEY=<foundry-key>
```

Recognition sample based on the current Learn unit:

```python
import os
from dotenv import load_dotenv
from openai import OpenAI

load_dotenv()
endpoint = os.getenv("AZURE_OPENAI_ENDPOINT")
api_key = os.getenv("API_KEY")
deployment_name = os.getenv("MODEL_DEPLOYMENT_NAME")

client = OpenAI(
    base_url=endpoint,
    api_key=api_key,
)

response = client.responses.create(
    model=deployment_name,
    input="Classify the sentiment and explain the reason: The setup was easy, but the app crashes frequently.",
)

print(response.output[0])
```

What to recognize:

- `OpenAI(...)` creates the authenticated service client.
- `base_url` points to the deployed-model API surface.
- `model` is the **deployment name**, not merely the catalog model family.
- `input` contains the prompt and source text.
- the response is generated and can vary in wording or formatting.

Durable flow:

`model endpoint + key + deployment → OpenAI client → responses.create → generated result`

## Client B: Azure Language through `TextAnalyticsClient`

Install:

```bash
pip install azure-ai-textanalytics python-dotenv
```

Configuration shape:

```text
AZURE_LANGUAGE_ENDPOINT=https://<resource>.cognitiveservices.azure.com/
API_KEY=<foundry-key>
```

Language-detection sample from the current Learn pattern:

```python
import os
from dotenv import load_dotenv
from azure.core.credentials import AzureKeyCredential
from azure.ai.textanalytics import TextAnalyticsClient

load_dotenv()
endpoint = os.getenv("AZURE_LANGUAGE_ENDPOINT")
key = os.getenv("API_KEY")

client = TextAnalyticsClient(
    endpoint=endpoint,
    credential=AzureKeyCredential(key),
)

text = "¡Hola! Me llamo Josefina y vivo en Madrid, España."
result = client.detect_language([text])[0]

print(result.primary_language.name)
print(result.primary_language.iso6391_name)
print(result.primary_language.confidence_score)
```

PII-recognition shape:

```python
text = "Maria Garcia called from 020 7946 0958."
result = client.recognize_pii_entities([text])[0]

print(result.redacted_text)

for entity in result.entities:
    print(entity.text, entity.category, entity.confidence_score)
```

What to recognize:

- `AzureKeyCredential(key)` wraps the key in the credential type expected by the Azure SDK client.
- the endpoint uses the Azure AI services domain, not the model endpoint.
- `[text]` sends a collection containing one document.
- the SDK returns one result per submitted document.
- `[0]` selects the first document result.
- nested properties expose typed fields such as language, ISO code, category, confidence, and redacted text.

Durable flow:

`Language endpoint + credential → TextAnalyticsClient → task method(documents) → per-document structured results`

## Python-to-C# mental mapping — not exact syntax equivalence

| Python pattern | C# mental mapping |
|---|---|
| `import ...` / `from ... import ...` | `using` plus a NuGet package reference |
| `os.getenv("NAME")` | configuration or `Environment.GetEnvironmentVariable("NAME")` |
| `client = TextAnalyticsClient(...)` | construct a typed Azure SDK service client |
| `[text]` | `new[] { text }` or another collection |
| `results[0]` | `results[0]` or `results.First()` |
| `result.primary_language.name` | nested DTO/property access |
| `for entity in result.entities` | `foreach (var entity in result.Entities)` |
| `f"{value}"` | `$"{value}"` string interpolation |
| indentation | block scope normally expressed with braces in C# |

The exam goal is to recognize client construction, request input, operation selection, and result handling—not to become a general Python programmer.

## Common code-reading traps

1. **Endpoint mismatch:** `openai.azure.com/openai/v1/` and `cognitiveservices.azure.com/` are not interchangeable.
2. **Model versus operation:** a Responses API request supplies a deployment and prompt; an Azure Language request calls a named operation.
3. **Single item wrapped in a list:** `[text]` is one document inside a batch-shaped request.
4. **First result indexing:** `[0]` selects the matching first response; it is not part of the text itself.
5. **Generated versus structured result:** free-form output and typed properties require different validation and parsing strategies.
6. **Secret handling:** configuration values belong outside source code; a `.env` file used locally should not be committed with real keys.

# Part 6 — Direct SDK versus an MCP tool

## Direct application call

The application explicitly chooses and invokes the Language operation.

`application → TextAnalyticsClient → Language operation → structured result → application logic`

Use this when ordinary application code owns the workflow.

## Agent tool call

The agent receives a goal, decides whether the Azure Language capability is needed, and invokes it through the Azure Language MCP server.

`user goal → agent reasoning → optional Azure Language MCP tool call → structured tool result → agent response`

MCP roles:

- **MCP client:** the agent or host application.
- **MCP server:** the service that exposes tools and returns structured results.

Recognition rule:

- fixed code path chooses the operation: **direct SDK/API**
- agent discovers and conditionally invokes the capability: **MCP tool**

Adding an MCP server does not make the Language result generative. It lets an agent use the structured analyzer as a tool.

# Part 7 — Quality, safety, and result handling

## Confidence is evidence, not certainty

A confidence score expresses model confidence for a result. It is not a guarantee of correctness.

Applications should consider:

- thresholds appropriate to the scenario;
- low-confidence review or fallback;
- language and domain support;
- ambiguous wording, sarcasm, mixed sentiment, and unusual names;
- privacy controls before storing or displaying sensitive text;
- validation of generated or analyzed output before consequential action.

## PII-specific rule

Detecting PII does not automatically make a workflow private. The application must also control:

- where raw text is sent;
- who can access input and results;
- whether logs contain sensitive data;
- how redacted and original text are stored;
- retention and deletion behavior.

## Generative-result rule

When a model creates a summary, classification explanation, or structured-looking response:

- request the required format clearly;
- validate the output before downstream use;
- check important claims against the source;
- do not treat fluent wording as proof of accuracy.

# Part 8 — Current lifecycle warning

The current AI-901 blueprint explicitly requires the concepts of keyword extraction, entity detection, sentiment analysis, and summarization.

Current Azure Language documentation also distinguishes:

- **core capabilities for new development**, including language detection, PII, prebuilt/custom NER, and Text Analytics for health; and
- several **legacy capabilities supported for existing implementations**, including key phrase extraction, sentiment and opinion mining, summarization, and entity linking.

Microsoft has published future retirement dates for several of those Azure Language implementations. This creates two separate decisions:

1. **Exam decision:** understand the named text-analysis technique and recognize its output.
2. **Architecture decision:** verify current product guidance before choosing the service for a new production implementation.

Do not discard an NLP concept because one implementation is retiring. Do not recommend a legacy implementation for a new system without checking current Microsoft guidance.

Exact retirement dates and beta package names are maintenance details, not primary AI-901 memorization targets.

# Prepared practice

Use these questions during a future study or review session. Answer before opening the key.

1. A system must return the five main concepts from each support ticket as a list. Which text-analysis technique is the direct match?
2. A system must produce a three-sentence explanation of a long report. Why is keyword extraction alone insufficient?
3. What is the difference between named entity recognition and entity linking?
4. A compliance pipeline must remove phone numbers and email addresses before storage. Which capability is the strongest direct match?
5. A review says, “The screen is beautiful, but the battery is awful.” What extra detail does opinion mining provide beyond overall sentiment?
6. A summary consists entirely of sentences copied from the original document. Is it extractive or abstractive?
7. Why can TF-IDF distinguish documents better than raw frequency when the corpus contains many common words?
8. A user wants conversational follow-up questions and a combined translation, summary, and explanation. Which approach is the natural first choice?
9. A pipeline requires an ISO language code, a confidence score, and a stable response schema. Which approach is the natural first choice?
10. In `client.detect_language([text])[0]`, why is `text` inside brackets?
11. What does the final `[0]` represent?
12. Which endpoint shape belongs to the model path: `openai.azure.com/openai/v1/` or `cognitiveservices.azure.com/`?
13. What is the durable difference between `client.responses.create(...)` and `client.detect_language(...)`?
14. An agent should call Azure Language only when a request needs PII redaction. What integration pattern lets the agent discover and invoke that capability as a tool?
15. Does Microsoft’s “deterministic” description mean the analyzer can never be wrong? Explain the exam-safe interpretation.
16. If Azure Language key phrase extraction is marked as a legacy capability, does the current AI-901 keyword-extraction objective disappear?

## Answer key

1. **Keyword or key phrase extraction.** It returns the main concepts as a list.
2. Keywords are isolated concepts; the requirement asks for coherent condensed prose, which is summarization.
3. NER finds and categorizes an entity; entity linking disambiguates it and associates it with a canonical external identity or link.
4. **PII detection and redaction.** It is designed for privacy-sensitive entity detection and can return redacted text.
5. It links positive or negative sentiment to the specific targets: screen and battery.
6. **Extractive summarization.** The output reuses source sentences.
7. TF-IDF lowers the importance of words common across the corpus and raises terms unusually important in one document.
8. A **general-purpose model** through prompts, because the task is flexible, combined, and conversational.
9. A **purpose-built Azure Language operation**, because the application needs stable structured fields.
10. Azure Language methods accept a collection of documents; `[text]` is a one-item list.
11. It selects the first per-document result returned for the first submitted document.
12. The model path uses `openai.azure.com/openai/v1/`.
13. `responses.create` sends a prompt to a deployed model and returns generated output; `detect_language` invokes one task-specific operation and returns a structured language result.
14. The **Azure Language MCP server** connected as an agent tool.
15. No. For this objective, read it as a fixed task and schema-stable structured result that is more predictable than free-form generation, not infallibility.
16. No. Product lifecycle and exam concept scope are separate. The technique remains explicitly named in the current blueprint; new architecture decisions must follow current product guidance.

# Future study checkpoint

A later study, implementation, review, or assessment session can evaluate whether the learner can complete this checkpoint without opening the answer key:

1. Explain tokenization, normalization, TF-IDF, embeddings, and attention in one concise flow.
2. Select keyword extraction, NER, sentiment, or summarization from a scenario.
3. Distinguish NER, entity linking, and PII detection.
4. Distinguish sentiment analysis and opinion mining.
5. Distinguish extractive and abstractive summarization.
6. Choose a general-purpose model or Azure Language and justify the tradeoff.
7. Trace both Python client flows from configuration through result handling.
8. Explain `[text]`, `[0]`, nested properties, and iteration using C# mental mappings.
9. Explain direct SDK invocation versus an MCP agent tool.
10. Apply the current lifecycle warning correctly.

# Guided implementation exercise for a later session

This exercise is stored for a future implementation session. It is not performed during gap research.

## Goal

Compare flexible model-driven analysis with a structured Azure Language result for the same support-ticket workflow.

## Suggested steps

1. In a Foundry project, deploy a small general-purpose language model.
2. In the chat playground, ask the model to:
   - detect the language;
   - classify sentiment;
   - extract important entities;
   - summarize the issue in one sentence.
3. Repeat the prompt and compare wording and formatting.
4. In the AI services area, test Azure Language detection or PII detection on the same text.
5. Record the fields returned by the structured analyzer.
6. Run or inspect the two minimal Python client patterns in this document.
7. Explain which result is easier to use for:
   - a human-facing explanation;
   - an automated routing rule;
   - PII redaction before persistence.
8. Explain where an agent plus MCP would add value and where it would be unnecessary.

## Exit evidence

The later session is successful when the learner can explain:

`flexible prompt-driven analysis versus task-specific structured analysis`

and can recognize:

`endpoint + credential + client + request + result`

for both approaches.

# Official sources

- https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-901
- https://learn.microsoft.com/en-us/training/modules/introduction-language/2-how-it-works
- https://learn.microsoft.com/en-us/training/modules/introduction-language/3-statistical-techniques
- https://learn.microsoft.com/en-us/training/modules/introduction-language/4-semantic-models
- https://learn.microsoft.com/en-us/training/modules/get-started-text-analysis-azure/2-azure-language
- https://learn.microsoft.com/en-us/training/modules/get-started-text-analysis-azure/3-language-sdk
- https://learn.microsoft.com/en-us/training/modules/get-started-text-analysis-azure/4-language-mcp
- https://learn.microsoft.com/en-us/azure/ai-services/language-service/overview
- https://learn.microsoft.com/en-us/azure/ai-services/language-service/language-detection/overview
- https://learn.microsoft.com/en-us/azure/ai-services/language-service/named-entity-recognition/overview
- https://learn.microsoft.com/en-us/azure/ai-services/language-service/personally-identifiable-information/overview
- https://learn.microsoft.com/en-us/azure/ai-services/language-service/key-phrase-extraction/overview
- https://learn.microsoft.com/en-us/azure/ai-services/language-service/sentiment-opinion-mining/overview
- https://learn.microsoft.com/en-us/azure/ai-services/language-service/summarization/overview
- https://learn.microsoft.com/en-us/azure/ai-services/language-service/entity-linking/overview

## Related gap research

- [`../../research/gaps/text-analysis-azure-language.md`](../../research/gaps/text-analysis-azure-language.md)