# Computer vision and image generation

Verified: **July 29, 2026**

> This document was prepared by a gap-research session for use during a later study session. The questions and exit check are stored here; they are not administered during research.

## Official exam objectives

Current AI-901 objectives relevant to this topic:

- Identify scenarios for common AI workloads, including computer vision.
- Identify features and capabilities of computer vision and image-generation models.
- Interpret visual input in prompts by using a deployed multimodal model.
- Create new visual outputs by using generative models.
- Build a lightweight application that includes vision capabilities.

Study guide: https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-901

## Official summary

- **Image classification:** predicts the main class or label for an image.
- **Object detection:** identifies multiple objects and returns a class plus a bounding box for each object.
- **Semantic segmentation:** assigns a class to individual pixels, producing precise masks instead of rectangular boxes.
- **Contextual image analysis:** interprets relationships, activities, diagrams, screenshots, or scenes and produces a natural-language response.
- **OCR:** extracts readable text from visual content. Layout-aware services may also return positions, lines, tables, or document structure.
- **Image generation:** creates or edits visual output from a text and/or image prompt, commonly through a diffusion-based process.

### How the main model families fit

- **CNN:** applies learned convolution filters to extract feature maps, then uses those features to predict classes or support more complex vision tasks.
- **Vision transformer (ViT):** divides an image into patches, encodes them as vectors, and uses attention to model relationships among visual features.
- **Multimodal model:** combines visual and language representations so one prompt can contain both images and text.
- **Image-generation model:** maps prompt concepts to visual features and iteratively generates or edits an image.

## Important distinctions

### Select by the required output

| Need | Best-fit capability | Typical output | Common trap |
|---|---|---|---|
| Identify the main subject of one image | Image classification | One label with a score | Does not locate multiple objects |
| Find and locate several objects | Object detection | Labels, scores, bounding boxes | Boxes are not pixel-accurate masks |
| Mark the exact pixels belonging to classes | Semantic segmentation | Per-pixel classes or masks | More precise than object detection |
| Ask an open-ended question about a photo, chart, screenshot, or diagram | Deployed multimodal model | Natural-language answer | Not a deterministic OCR or extraction result |
| Read text or document layout | OCR / Read / Layout capability | Text, lines, positions, layout | Reading text is not the same as reasoning over the scene |
| Extract named fields from documents or mixed media | Document Intelligence or Content Understanding | Structured fields and schema-aligned output | This belongs primarily to the information-extraction objective |
| Create or edit a visual asset | Image-generation model | Image bytes or base64 data | Input interpretation and output generation are opposite directions |

### Current Azure route selection

| Scenario | Current preparation route |
|---|---|
| Flexible visual question answering or image interpretation | Deploy a vision-capable multimodal model in Microsoft Foundry and send text plus image input through the Responses API |
| Generate a new image | Deploy a supported image-generation model and call the image-generation tool/API; decode the returned image data |
| OCR or layout extraction only | Use current Content Understanding Read/Layout or Document Intelligence guidance, depending on the workload |
| Standard invoices, receipts, IDs, or other common structured forms | Prefer a Document Intelligence prebuilt model |
| Extract a schema from images, documents, audio, or video for the AI-901 information-extraction objective | Use Content Understanding |
| Existing Azure Vision Image Analysis solution | Recognize it as a legacy API that is deprecated and scheduled for retirement on September 25, 2028; follow Microsoft migration guidance |

### Fast selection rules

- Need **one label**? Classification.
- Need **objects plus locations**? Detection.
- Need **pixel masks**? Segmentation.
- Need **a natural-language interpretation** of visual input? Multimodal model.
- Need **text/layout/fields**? OCR or information-extraction service.
- Need **new pixels as output**? Image-generation model.

### Video generation: recognition level only

The current implementation module includes video generation. Preserve only this flow unless later evidence increases its priority:

`deployed video model → create asynchronous job → poll status → download completed video`

The explicit computer vision objective bullets remain the higher-priority study target.

## Current multimodal image-input flow

The goal is to recognize the objects and their order—not memorize every line.

```python
import os
from openai import OpenAI

client = OpenAI(
    api_key=os.environ["FOUNDRY_KEY"],
    base_url=os.environ["ENDPOINT"],
)

response = client.responses.create(
    model=os.environ["MODEL_NAME"],  # Foundry deployment name
    input=[
        {
            "role": "user",
            "content": [
                {
                    "type": "input_text",
                    "text": "Describe the safety issue in this photo.",
                },
                {
                    "type": "input_image",
                    "image_url": "https://example.com/photo.jpg",
                },
            ],
        }
    ],
)

print(response.output_text)
```

Flow:

`Foundry resource → vision-capable deployment → OpenAI client → multipart text + image input → responses.create → output_text`

### Supplying a local image

A local binary image can be base64 encoded and included as a data URL:

```python
import base64

with open("photo.jpg", "rb") as image_file:
    encoded = base64.b64encode(image_file.read()).decode("utf-8")

image_item = {
    "type": "input_image",
    "image_url": f"data:image/jpeg;base64,{encoded}",
}
```

The request structure stays the same; only the image source changes.

## Current image-generation flow

```python
import base64
import os
from openai import OpenAI

client = OpenAI(
    api_key=os.environ["FOUNDRY_KEY"],
    base_url=os.environ["ENDPOINT"],
)

response = client.responses.create(
    model=os.environ["MODEL_NAME"],
    input="Create a clean flat illustration of a robot watering a plant.",
    tools=[{"type": "image_generation"}],
)

image_base64 = next(
    item.result
    for item in response.output
    if item.type == "image_generation_call"
)

with open("generated.png", "wb") as output_file:
    output_file.write(base64.b64decode(image_base64))
```

Flow:

`Foundry resource → image-capable deployment → OpenAI client → prompt + image-generation tool → image_generation_call → base64 decode → image file`

Do not memorize a specific model version. Recognize the deployment, tool, output-item type, and binary decoding steps.

### C# mental mapping

- `OpenAI(...)` ≈ constructing a service client with endpoint and credentials.
- Python dictionaries and lists in `input` ≈ nested request DTOs and collections.
- `MODEL_NAME` is the **deployment name**, similar to selecting a named deployed backend.
- `response.output_text` ≈ a convenience property over the text response payload.
- `next(item.result for item in ... if ...)` ≈ `response.Output.First(x => x.Type == ...).Result`.
- `base64.b64decode(...)` ≈ `Convert.FromBase64String(...)`.
- `with open(..., "wb")` ≈ opening a binary file stream in a `using` block.

## Prepared scenario practice

Use these questions during a future study or review session. Answer before opening the key.

1. A grocery checkout receives one photo containing one fruit and must return only `apple`, `banana`, or `orange`. Which computer vision task best fits?
2. A traffic system must identify every vehicle in a frame and return the rectangular location of each one. Which task best fits?
3. A user uploads a chart and asks, “Which quarter had the largest revenue increase, and why?” Which solution route best fits?
4. A claims system must extract the policy number, claimant, and total from many insurance forms into a known schema. Which family of services best fits?
5. A marketing app must create a new product illustration from a written description. Which capability best fits?

## Prepared Python code-recognition practice

6. Which content item belongs in the blank when the request must include an image?

```python
content = [
    {"type": "input_text", "text": "What is shown?"},
    {"type": "___________", "image_url": image_url},
]
```

7. What value should `model` receive?

```python
response = client.responses.create(
    model=____________________,
    input=message,
)
```

- The model's marketing-family name from memory
- The deployment name configured in Foundry
- The Azure subscription ID

8. Which property contains the convenient natural-language result from a multimodal Responses API call?

```python
print(response.___________)
```

9. Which request element explicitly asks the Responses API to produce an image?

```python
tools=[{"type": "________________"}]
```

10. Why does the image-generation example call `base64.b64decode(image_base64)`?

- To convert the returned text representation into binary image bytes
- To authenticate the client
- To classify the generated image

<details>
<summary><strong>Answer key</strong></summary>

1. **Image classification.** The output is one label for the main subject.
2. **Object detection.** The system needs multiple labels and bounding boxes.
3. **A deployed multimodal model.** The prompt combines a visual chart with an open-ended reasoning question.
4. **Document Intelligence or Content Understanding, depending on the document variability and extraction design.** The goal is schema-aligned field extraction, not open-ended visual description.
5. **An image-generation model.** The output must be newly generated visual content.
6. **`input_image`**
7. **The deployment name configured in Foundry.** The request targets a deployed model endpoint.
8. **`output_text`**
9. **`image_generation`**
10. **To convert the returned text representation into binary image bytes.** The bytes can then be written to a `.png` or other supported output file.

</details>

## Prepared exit check

Use this only during a future study or review session. Without notes, explain all five statements:

1. Classification, detection, and segmentation differ mainly in the granularity and shape of their outputs.
2. A multimodal model combines text instructions and image input to return a contextual response.
3. OCR or structured extraction should not automatically be replaced by open-ended multimodal reasoning.
4. The current lightweight vision app flow uses a Foundry deployment, OpenAI client, multipart input, and the Responses API.
5. Image generation returns image data that the client may need to decode and save.

**Suggested future study-session heuristic:** 5/5 correct explanations plus at least 8/10 practice answers. This is not an official Microsoft passing threshold.

## Exam-readiness gaps

Matching research: [`../../research/gaps/computer-vision-image-generation.md`](../../research/gaps/computer-vision-image-generation.md)

Accepted gaps addressed by this topic file:

- **Task-selection and terminology gap — High confidence:** classic vision, OCR/extraction, multimodal interpretation, and generation are separated by expected output.
- **Current implementation gap — High confidence:** the deployed-model and Responses API flow is preserved instead of centering a retiring Image Analysis API.
- **Practice gap — High confidence:** minimal image-input and image-generation flows plus ten original questions.
- **Video-generation scope ambiguity — Medium confidence:** recognition-level async flow without disproportionate study depth.

The research-session deliverables are complete. These exercises are reserved for a future study session; no learner status is inferred from their creation.

## Official sources

- AI-901 study guide: https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-901
- Computer vision concepts module: https://learn.microsoft.com/en-us/training/modules/introduction-computer-vision/
- Computer vision tasks: https://learn.microsoft.com/en-us/training/modules/introduction-computer-vision/2-overview
- CNN concepts: https://learn.microsoft.com/en-us/training/modules/introduction-computer-vision/4-computer-vision-models
- Vision transformers and multimodal models: https://learn.microsoft.com/en-us/training/modules/introduction-computer-vision/5-modern-vision-models
- Image generation concepts: https://learn.microsoft.com/en-us/training/modules/introduction-computer-vision/5a-generate-images
- Computer vision implementation module: https://learn.microsoft.com/en-us/training/modules/get-started-vision-azure/
- Multimodal image analysis: https://learn.microsoft.com/en-us/training/modules/get-started-vision-azure/2-vision-enabled-models
- Image generation implementation: https://learn.microsoft.com/en-us/training/modules/get-started-vision-azure/3-image-generation
- Responses API: https://learn.microsoft.com/en-us/azure/foundry/openai/how-to/responses
- OCR guidance: https://learn.microsoft.com/en-us/azure/ai-services/computer-vision/concept-ocr
- Image Analysis migration guidance: https://learn.microsoft.com/en-us/azure/ai-services/computer-vision/migration-options
- Content-processing selection guidance: https://learn.microsoft.com/en-us/azure/ai-services/content-understanding/choosing-right-ai-tool

## External gap-research sources

See the matching gap-research file. External candidate evidence is not used as official teaching content here.

## Metadata

- Verified on: July 29, 2026
- Official blueprint checked on: July 29, 2026
- Research material status: Complete
- Study-session status: Not started