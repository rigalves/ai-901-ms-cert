# Information extraction and Content Understanding

Verified: **July 29, 2026**

> This document was prepared by a gap-research session for use during a later study session. The questions and exit check are stored here; they are not administered during research.

## Official exam objectives

Current AI-901 objectives relevant to this topic:

- Identify scenarios for common AI workloads, including information extraction.
- Identify techniques to extract information from text, images, audio, and videos.
- Extract information from documents and forms by using Azure Content Understanding in Foundry Tools.
- Extract information from images by using Content Understanding.
- Extract information from audio and video by using Content Understanding.
- Build a lightweight application with information extraction capabilities by using Content Understanding.

Study guide: https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-901

## Official summary

- **Information extraction:** transforms unstructured content into structured, usable information.
- **OCR:** detects visual text and returns machine-readable text, often with positions and layout. OCR does not by itself determine business meaning.
- **Field extraction:** maps content to named business fields such as `InvoiceNumber`, `Total`, or `RequestedActions`.
- **Schema:** defines the fields, types, collections, and nested structure expected in the result.
- **Analyzer:** the reusable Content Understanding configuration that applies extraction settings and a schema to incoming content.
- **Structured output:** commonly includes Markdown for readable/searchable content and JSON-like fields for automation.
- **Confidence:** estimates the reliability of an extracted field.
- **Grounding:** identifies the source region or evidence from which a value was obtained.

Content Understanding supports **documents, images, audio, and video**. It can use prebuilt analyzers or custom analyzers.

## Core mental model

`unstructured input → analyzer applies extraction settings and schema → structured result`

The analyzer is not the input and it is not the result:

| Component | Role |
|---|---|
| Input | Document, image, audio, or video to analyze |
| Schema | Definition of the fields and structure to return |
| Analyzer | Reusable processing configuration that applies extraction and the schema |
| Result | Markdown, fields, confidence, grounding, transcript, keyframes, or other structured output |

## Important distinctions

### Select by the required output

| Need | Best-fit capability | Typical output | Common trap |
|---|---|---|---|
| Read visible text and layout | OCR / Read / Layout | Text, lines, positions, tables, layout | OCR does not map values to business fields |
| Extract known fields into a schema | Content Understanding or Document Intelligence | Named, typed fields and nested structures | Schema extraction is more than text recognition |
| Ask an open-ended question about a visual scene | Deployed multimodal model | Natural-language answer | Not deterministic schema extraction |
| Transcribe spoken content only | Speech recognition | Transcript | Does not automatically produce schema-aligned summaries or actions |
| Extract transcript plus summaries, actions, speakers, or other fields | Content Understanding audio analyzer | Transcript and structured insights | Broader than speech-to-text alone |
| Extract chapters, keyframes, transcript, and structured video insights | Content Understanding video analyzer | Multimodal structured result | Not merely frame classification |

### Content Understanding or Document Intelligence?

| Scenario | Current preparation route |
|---|---|
| OCR or layout only | Content Understanding `prebuilt-read` or `prebuilt-layout` |
| Standard invoices, receipts, IDs, tax forms, or other common structured documents | Document Intelligence prebuilt model is often the deterministic, low-latency fit |
| Custom extraction from highly structured forms with labeled examples | Document Intelligence custom model |
| Varied or unstructured documents, inferred fields, or zero-shot schema extraction | Content Understanding custom analyzer |
| Images, audio, or video | Content Understanding |
| Full control over custom models, prompts, preprocessing, and orchestration | Build directly with Foundry models only when managed extraction does not fit |

For AI-901 implementation practice, keep the main flow centered on **Content Understanding**, because the official objective names it directly.

## Modality recognition

| Input | Current prebuilt example | Useful outputs to recognize |
|---|---|---|
| Document | `prebuilt-invoice` | Markdown, typed invoice fields, confidence, grounding |
| Image | `prebuilt-imageSearch` | Image description and searchable visual information |
| Audio | `prebuilt-audioSearch` | Transcript, summary, speaker information, structured insights |
| Video | `prebuilt-videoSearch` | Keyframes, transcript, chapters, structured insights |

A custom analyzer can define a schema for fields that are specific to the application.

## Current lightweight application flow

### Setup sequence

1. Create a Microsoft Foundry resource in a supported region.
2. Configure the required default model deployments for Content Understanding.
3. Obtain the Foundry resource endpoint and a credential.
4. Install `azure-ai-contentunderstanding`.
5. Create `ContentUnderstandingClient`.
6. Select a prebuilt analyzer or create a custom analyzer.
7. Submit one or more inputs with `begin_analyze`.
8. Wait for the long-running operation to complete.
9. Read `result.contents`, including Markdown and structured fields.

Flow:

`Foundry resource → model defaults → endpoint + credential → ContentUnderstandingClient → analyzer ID + inputs → begin_analyze → poller.result → result.contents`

### Minimal current Python example

```python
import os

from azure.ai.contentunderstanding import ContentUnderstandingClient
from azure.ai.contentunderstanding.models import AnalysisInput
from azure.core.credentials import AzureKeyCredential

client = ContentUnderstandingClient(
    endpoint=os.environ["CONTENTUNDERSTANDING_ENDPOINT"],
    credential=AzureKeyCredential(
        os.environ["CONTENTUNDERSTANDING_KEY"]
    ),
)

invoice_url = (
    "https://raw.githubusercontent.com/"
    "Azure-Samples/azure-ai-content-understanding-assets/"
    "main/document/invoice.pdf"
)

poller = client.begin_analyze(
    analyzer_id="prebuilt-invoice",
    inputs=[AnalysisInput(url=invoice_url)],
)

result = poller.result()

for content in result.contents:
    print(content.markdown)
    print(content.fields)
```

### What the code means

- `ContentUnderstandingClient` is the service client.
- `analyzer_id` selects the reusable extraction behavior.
- `AnalysisInput` identifies the content to process.
- `begin_analyze` starts an asynchronous long-running operation.
- `poller.result()` waits while the SDK handles polling.
- `result.contents` contains one result for each analyzed input.
- `content.markdown` is useful for readable or searchable content.
- `content.fields` contains schema-aligned structured values.

Microsoft Entra authentication with `DefaultAzureCredential` is also supported. The exam-level idea is **endpoint + credential + client**, not memorizing only one credential class.

### REST recognition

The REST flow uses the same logic:

`POST analyzer:analyze → receive Operation-Location → poll analyzerResults → status Succeeded → read result.contents`

Current GA API version: **`2025-11-01`**.

The Learn unit still displays `2025-05-01-preview` inside one sample response. That preview version was retired on July 15, 2026. Treat it as stale sample output and do not memorize it.

## C# mental mapping

- `ContentUnderstandingClient(...)` ≈ constructing an Azure SDK service client.
- `AnalysisInput(url=...)` ≈ creating a request DTO.
- `inputs=[...]` ≈ a `List<AnalysisInput>` or array.
- `begin_analyze(...)` ≈ starting an Azure long-running operation.
- `poller.result()` ≈ awaiting an `Operation<T>` until completion.
- `result.contents` ≈ a collection of per-input analysis results.
- `content.fields` ≈ a dictionary/object graph of typed extracted fields.
- `content.markdown` ≈ a normalized readable representation of the input.

## Prepared scenario practice

Use these questions during a future study or review session. Answer before opening the key.

1. A system must convert scanned pages into searchable text while preserving lines, tables, and layout. It does not need named business fields. Which capability best fits?
2. A maintenance app receives photographs of equipment labels in many layouts and must return `SerialNumber`, `Model`, and `ManufactureDate` as structured fields. Which route best fits?
3. A company processes a very high volume of standardized invoices and prioritizes deterministic extraction and low latency. Which service family is the strongest fit?
4. A voicemail-processing app must return the transcript, caller, summary, requested actions, and callback number. Which service best fits?
5. A recorded meeting must produce keyframes, transcript, chapters, and a structured list of action items. Which service and modality best fit?

## Prepared Python code-recognition practice

6. Which class belongs in the blank?

```python
inputs = [___________(url=invoice_url)]
```

7. Which method starts the asynchronous analysis?

```python
poller = client.____________(
    analyzer_id="prebuilt-invoice",
    inputs=inputs,
)
```

8. Why is the following call needed?

```python
result = poller.result()
```

- It waits for the long-running operation and returns the completed result.
- It creates the schema.
- It authenticates the client.

9. Which collection contains the per-input analysis results?

```python
for content in result.________:
    print(content.markdown)
```

10. Which property should be read for schema-aligned extracted values?

```python
print(content.________)
```

<details>
<summary><strong>Answer key</strong></summary>

1. **OCR / Read / Layout.** The requirement is text and structure, not semantic field mapping.
2. **A Content Understanding image analyzer with a schema.** The layouts vary and the output must be structured named fields.
3. **Document Intelligence, typically a prebuilt invoice model.** Standardized documents, deterministic extraction, high volume, and low latency favor it.
4. **Content Understanding with an audio analyzer.** It can combine transcription with schema-aligned summaries and actions.
5. **Content Understanding with a video analyzer.** Video analyzers can combine keyframes, transcript, chapters, and structured insights.
6. **`AnalysisInput`**
7. **`begin_analyze`**
8. **It waits for the long-running operation and returns the completed result.**
9. **`contents`**
10. **`fields`**

</details>

## Prepared exit check

Use this only during a future study or review session. Without notes, explain all six statements:

1. OCR returns recognized text and layout; field extraction maps content to business meaning.
2. A schema defines the expected fields, while an analyzer applies the schema and extraction settings.
3. Content Understanding supports documents, images, audio, and video.
4. Document Intelligence remains a strong fit for deterministic extraction from standardized documents.
5. Content Understanding analysis is asynchronous: start, poll or wait, then read the structured result.
6. Current GA guidance uses API version `2025-11-01`, not the retired preview version shown in one Learn response example.

**Suggested future study-session heuristic:** 6/6 correct explanations plus at least 8/10 practice answers. This is not an official Microsoft passing threshold.

## Exam-readiness gaps

Matching research: [`../../research/gaps/information-extraction-content-understanding.md`](../../research/gaps/information-extraction-content-understanding.md)

Accepted gaps addressed by this topic file:

- **OCR versus schema extraction gap — High confidence:** outputs and solution routes are separated.
- **Analyzer/schema terminology gap — High confidence:** each component has a distinct role.
- **Asynchronous SDK gap — High confidence:** the LRO pattern is explicit and practiced.
- **Modality-selection gap — High confidence:** documents, images, audio, and video are compared.
- **Content Understanding versus Document Intelligence gap — High confidence:** current service-selection guidance is preserved.
- **API freshness gap — High confidence:** current GA guidance replaces the retired preview version.
- **Confidence and grounding awareness — Medium confidence:** preserved at recognition level.

The research-session deliverables are complete. These exercises are reserved for a future study session; no learner status is inferred from their creation.

## Official sources

- AI-901 study guide: https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-901
- Information extraction concepts module: https://learn.microsoft.com/en-us/training/modules/introduction-information-extraction/
- Information extraction overview: https://learn.microsoft.com/en-us/training/modules/introduction-information-extraction/2-overview
- OCR concepts: https://learn.microsoft.com/en-us/training/modules/introduction-information-extraction/3-vision-extraction
- Field extraction and mapping: https://learn.microsoft.com/en-us/training/modules/introduction-information-extraction/4-form-extraction
- Information extraction implementation module: https://learn.microsoft.com/en-us/training/modules/get-started-information-extraction/
- Documents implementation: https://learn.microsoft.com/en-us/training/modules/get-started-information-extraction/2-documents
- Audio and video implementation: https://learn.microsoft.com/en-us/training/modules/get-started-information-extraction/3-audio-video
- Content Understanding overview: https://learn.microsoft.com/en-us/azure/ai-services/content-understanding/overview
- Content Understanding quickstart: https://learn.microsoft.com/en-us/azure/ai-services/content-understanding/quickstart/use-rest-api
- Python SDK overview: https://learn.microsoft.com/en-us/python/api/overview/azure/ai-contentunderstanding-readme?view=azure-python
- Tool-selection guidance: https://learn.microsoft.com/en-us/azure/ai-services/content-understanding/choosing-right-ai-tool
- Document Intelligence overview: https://learn.microsoft.com/en-us/azure/ai-services/document-intelligence/overview

## External gap-research sources

See the matching gap-research file. External candidate evidence is not used as official teaching content here.

## Metadata

- Verified on: July 29, 2026
- Official blueprint checked on: July 29, 2026
- Research material status: Complete
- Study-session status: Not started
