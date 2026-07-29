# Computer vision and image generation — Exam-readiness gaps

Research date: **July 29, 2026**

## Official scope

Current AI-901 objectives relevant to this topic:

- Identify scenarios for common AI workloads, including computer vision.
- Identify features and capabilities of computer vision and image-generation models.
- Interpret visual input in prompts by using a deployed multimodal model.
- Create new visual outputs by using generative models.
- Build a lightweight application that includes vision capabilities.

Information extraction from images is a separate official objective. It is considered here only where service-selection boundaries affect computer vision readiness.

## Official material reviewed

- AI-901 study guide: https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-901
- Concepts learning path: https://learn.microsoft.com/en-us/training/paths/ai-concepts/
- Introduction to computer vision concepts: https://learn.microsoft.com/en-us/training/modules/introduction-computer-vision/
- Computer vision tasks and techniques: https://learn.microsoft.com/en-us/training/modules/introduction-computer-vision/2-overview
- Images and image processing: https://learn.microsoft.com/en-us/training/modules/introduction-computer-vision/3-understand-image-processing
- Convolutional neural networks: https://learn.microsoft.com/en-us/training/modules/introduction-computer-vision/4-computer-vision-models
- Vision transformers and multimodal models: https://learn.microsoft.com/en-us/training/modules/introduction-computer-vision/5-modern-vision-models
- Image generation concepts: https://learn.microsoft.com/en-us/training/modules/introduction-computer-vision/5a-generate-images
- Concept exercise: https://learn.microsoft.com/en-us/training/modules/introduction-computer-vision/5b-exercise
- Implementation learning path: https://learn.microsoft.com/en-us/training/paths/introduction-to-ai-on-azure/
- Get started with computer vision in Azure: https://learn.microsoft.com/en-us/training/modules/get-started-vision-azure/
- Multimodal image analysis: https://learn.microsoft.com/en-us/training/modules/get-started-vision-azure/2-vision-enabled-models
- Image generation models: https://learn.microsoft.com/en-us/training/modules/get-started-vision-azure/3-image-generation
- Video generation models: https://learn.microsoft.com/en-us/training/modules/get-started-vision-azure/4-video-generation
- Implementation exercise: https://learn.microsoft.com/en-us/training/modules/get-started-vision-azure/5-exercise
- Responses API: https://learn.microsoft.com/en-us/azure/foundry/openai/how-to/responses
- OCR for images: https://learn.microsoft.com/en-us/azure/ai-services/computer-vision/concept-ocr
- Image Analysis overview: https://learn.microsoft.com/en-us/azure/ai-services/computer-vision/overview-image-analysis
- Image Analysis migration options: https://learn.microsoft.com/en-us/azure/ai-services/computer-vision/migration-options
- Document and content-processing selection: https://learn.microsoft.com/en-us/azure/ai-services/content-understanding/choosing-right-ai-tool

## Coverage assessment

The concept module covers classic computer vision tasks, pixel representation, convolution filters, CNNs, vision transformers, multimodal models, and diffusion-based image generation. It is strong conceptual coverage for a fundamentals exam.

The implementation module is substantially more modern than older AI-900-era computer vision material. Its primary application flow is a deployed vision-capable model in Foundry, an OpenAI client, the Responses API, a multipart prompt containing text and image input, and a natural-language response. Image generation uses a deployed image model, an image-generation tool call, and a base64 result that the client decodes and saves.

The largest readiness risk is not missing content. It is failing to separate overlapping solution categories and current implementation paths from older Azure Vision APIs that remain searchable.

## Gaps

### Classic vision tasks and multimodal reasoning are easy to treat as interchangeable

- **Type:** Terminology gap and practice gap
- **Evidence:** The concepts module teaches image classification, object detection, semantic segmentation, and contextual image analysis in sequence. The implementation module then centers on a general-purpose multimodal model that accepts an image plus a natural-language instruction.
- **Confidence:** High
- **Why it matters:** These tasks produce different outputs. A class label, bounding boxes, pixel masks, extracted text, and a natural-language answer are not equivalent results.
- **Study material needed:** A compact task-selection table showing the input, expected output, and best-fit scenario for classification, detection, segmentation, OCR, structured extraction, multimodal interpretation, and image generation.

### Current Foundry vision implementation differs from older Azure Vision Image Analysis examples

- **Type:** Terminology gap and implementation gap
- **Evidence:** The current AI-901 implementation module teaches a deployed multimodal model and the OpenAI Responses API. Azure Vision Image Analysis documentation is still available, but Microsoft marks Image Analysis 4.0 as deprecated and schedules retirement for September 25, 2028.
- **Confidence:** High
- **Why it matters:** Search results can lead a learner into memorizing a legacy `ImageAnalysisClient` flow that is not the implementation path emphasized by the current AI-901 Learn module.
- **Study material needed:** Preserve the current exam flow as:

`Foundry resource → vision-capable deployment → authentication/client → text + image prompt → Responses API → text result`

Keep legacy Image Analysis awareness to one note; do not create a new lab around the retiring API.

### OCR, document extraction, Content Understanding, and multimodal image interpretation overlap in appearance

- **Type:** Service-selection and terminology gap
- **Evidence:** The AI-901 blueprint separates computer vision from information extraction. Current Microsoft guidance distinguishes OCR/layout extraction, structured-document models, multimodal content analyzers, and custom model-driven visual reasoning.
- **Confidence:** High
- **Why it matters:** A screenshot containing text could be handled differently depending on whether the goal is raw text, layout, predefined fields, a schema across varied content, or an open-ended answer.
- **Study material needed:** A decision table that distinguishes:
  - open-ended visual question answering → deployed multimodal model
  - OCR or layout extraction → Content Understanding Read/Layout or Document Intelligence
  - standard structured forms → Document Intelligence prebuilt model
  - AI-901 extraction from images and mixed media → Content Understanding
  - retired Image Analysis API → legacy recognition only

### The image-input request shape deserves explicit code-recognition practice

- **Type:** Implementation gap and practice gap
- **Evidence:** The official implementation unit shows a multipart message with `input_text` and `input_image`. Images can be supplied by URL or as base64 data, and the deployed model name is passed to `responses.create`.
- **Confidence:** High
- **Why it matters:** The object graph is straightforward for an experienced developer, but unfamiliar Python dictionaries and lists can hide the actual request flow.
- **Study material needed:** One minimal Python example, a C# mental mapping, and short questions covering the client, deployment name, multipart content, image URL/base64 input, and `response.output_text`.

### The image-generation response flow is underpracticed

- **Type:** Implementation gap and practice gap
- **Evidence:** The official unit provides a concise example, but the important distinction is embedded in code: `tools=[{"type": "image_generation"}]`, an output item of type `image_generation_call`, a base64 result, and binary file writing.
- **Confidence:** High
- **Why it matters:** A learner may understand text-to-image conceptually but fail to recognize the client-side output handling.
- **Study material needed:** One minimal image-generation flow plus short code-recognition questions. Focus on the request/tool/result sequence rather than memorizing a specific model version.

### Video generation is in the Learn module but not an explicit vision objective bullet

- **Type:** Scope and terminology gap
- **Evidence:** The implementation module includes video-generation models and an asynchronous create-job, poll-status, download-result flow. The current study guide explicitly names visual input, visual output, and vision-capable applications, but not a separate video-generation implementation bullet. Microsoft also notes that related topics may be covered.
- **Confidence:** Medium
- **Why it matters:** Ignoring the unit entirely is risky, but deep study of model-specific video APIs would displace higher-confidence objectives.
- **Study material needed:** Recognition-level coverage only: video generation uses a deployed video model and an asynchronous job flow of create → poll → download.

### External reports support SDK recognition, not a reliable vision question count

- **Type:** Reported exam emphasis and unverified anecdote
- **Evidence:** Current first-hand and community discussion supports preparing for Python syntax, SDK/API recognition, and Foundry workflows. No sufficiently strong, independent evidence was found to estimate how many vision or OCR questions appear.
- **Confidence:** Medium for implementation-style preparation; Low for topic frequency
- **Why it matters:** The official blueprint already justifies code recognition. Candidate anecdotes should not be used to invent topic weighting.
- **Study material needed:** No separate asset beyond the prepared scenario and code-recognition questions.

## Study-session assets created

Created **July 29, 2026**:

- [`../../docs/topics/computer-vision.md`](../../docs/topics/computer-vision.md)
  - classic and modern vision task-selection tables
  - OCR and information-extraction boundary guidance
  - current Foundry multimodal image-input flow
  - current image-generation output flow
  - C# mental mappings
  - five original scenario questions
  - five original Python code-recognition questions
  - answer key and future study-session exit check

**Research status:** Complete. The identified study material now exists and is linked. Learner understanding is intentionally not evaluated during a gap-research session.

## Claims not accepted

- Every visual workload should use one general-purpose multimodal model.
- OCR, object detection, and contextual image analysis produce interchangeable results.
- The retiring Azure Vision Image Analysis API is the default current AI-901 implementation route.
- A model that can read visible text is automatically the best choice for reliable OCR or structured document extraction.
- Video-generation API details deserve the same study depth as the three explicit computer vision implementation bullets.
- A candidate anecdote proves the number or distribution of computer vision questions.
- Any dump-derived claim about real exam questions.

## Suggested future study checkpoint

Use the prepared checkpoint in `docs/topics/computer-vision.md` during a later study or review session. That session may evaluate whether the learner can:

1. Select classification, detection, segmentation, OCR, extraction, multimodal interpretation, or generation for a scenario.
2. Explain CNN versus ViT at a fundamentals level.
3. Trace the current image-input Responses API flow.
4. Trace the image-generation request and base64 result flow.
5. Avoid selecting a retiring Image Analysis implementation for a new current-AI-901 solution.

This checkpoint is stored as a handoff. It is not administered during gap research.

## External evidence reviewed

- First-hand AI-901 beta account emphasizing Python, SDK, and Foundry familiarity, April 29, 2026: https://www.linkedin.com/pulse/career-trajectory-issue-165-free-certified-program-20-carla-qb2zc
- Microsoft Q&A discussion of implementation-style and code-completion preparation, June 19, 2026: https://learn.microsoft.com/en-us/answers/questions/5924630/does-the-ai-901-exam-include-a-lab-performance-bas
- Official exam page and study guide remain the authority for scope and weighting.