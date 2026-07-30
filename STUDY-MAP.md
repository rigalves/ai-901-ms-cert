# AI-901 Study Map

Verified against the live Microsoft study guide and Microsoft Learn paths on **July 30, 2026**.

## Purpose

This is the operational curriculum for the repository.

It connects:

`official domain → official topic → official subtopic/objective → current Microsoft Learn material → gap research → prepared study asset → learner status`

Use this file to decide what to study and which repository material to open. Use [`STATUS.md`](STATUS.md) for confirmed learner progress and the exact current position, [`PLAN.md`](PLAN.md) for strategy, [`docs/official-links.md`](docs/official-links.md) for the full verified source inventory, and [`ASSESSMENT-LOG.md`](ASSESSMENT-LOG.md) for diagnostic repair work.

## Authority and scope

- Exam page: https://learn.microsoft.com/en-us/credentials/certifications/exams/ai-901/
- Study guide: https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-901
- Concepts path: https://learn.microsoft.com/en-us/training/paths/ai-concepts/
- Implementation path: https://learn.microsoft.com/en-us/training/paths/get-started-ai-apps-agents/

The current blueprint contains two domains and **29 objective bullets**:

1. **Identify AI concepts and capabilities — 40–45%**
2. **Implement AI solutions by using Microsoft Foundry — 55–60%**

The objective bullets are illustrative rather than exhaustive. Related topics and commonly used Preview features may appear, but study expansion must remain tied to an official objective.

## Status legend

- **Completed:** learner progress confirmed in `STATUS.md`.
- **In progress:** current learner module confirmed in `STATUS.md`.
- **Not started:** learner progress not yet confirmed.
- **Gap complete:** gap research and reusable study material exist.
- **Gap pending:** the planned gap-research session has not produced its repository assets yet.
- **Cross-cutting:** supports several objectives rather than one Learn module.

## Current route

1. Continue **Introduction to computer vision concepts** in the official module order, using [`docs/topics/computer-vision.md`](docs/topics/computer-vision.md).
2. Study **Introduction to AI-powered information extraction concepts** using [`docs/topics/information-extraction.md`](docs/topics/information-extraction.md).
3. Take the [official Practice Assessment](https://aiskillsnavigator.microsoft.com/credentials/cert-83587e0a0754cfee561ade3e27d9fa1cdaf15ae03be52d2413b2b858d1b4eda4) as the first baseline diagnostic after completing the concepts path, and record results in [`ASSESSMENT-LOG.md`](ASSESSMENT-LOG.md).
4. Complete the six-module Foundry implementation path in order, using the mapped gap and topic assets as they become available.
5. Revisit completed topics when the user requests review or diagnostic evidence identifies a weakness.

The learner follows the official Microsoft Learn sequence by default. Completed topics have handwritten notes and may be revisited without changing their completion status. Gap-research production does not change learner status.

## Official learning sequence

### Phase 1 — Concepts path

| Order | Official module | Main coverage | Repository material | Gap status | Learner status |
|---:|---|---|---|---|---|
| 1 | [Introduction to AI concepts](https://learn.microsoft.com/en-us/training/modules/get-started-ai-fundamentals/) | AI workload overview and responsible AI | [`docs/topics/responsible-ai-model-concepts.md`](docs/topics/responsible-ai-model-concepts.md) | [Gap complete](research/gaps/responsible-ai-model-concepts.md) | **Completed** |
| 2 | [Introduction to generative AI and agents](https://learn.microsoft.com/en-us/training/modules/fundamentals-generative-ai/) | LLMs, prompts, agents | [`docs/topics/responsible-ai-model-concepts.md`](docs/topics/responsible-ai-model-concepts.md) for LLM mechanics; [`docs/topics/generative-ai-prompts-model-configuration.md`](docs/topics/generative-ai-prompts-model-configuration.md) for prompt and model-selection review; [`docs/topics/agentic-ai-single-agent-solutions.md`](docs/topics/agentic-ai-single-agent-solutions.md) for agent concepts | [Model-mechanics gap complete](research/gaps/responsible-ai-model-concepts.md); [generative/prompt gap complete](research/gaps/generative-ai-prompts-model-configuration.md); [agent gap complete](research/gaps/agentic-ai-single-agent-solutions.md) | **Completed** |
| 3 | [Introduction to natural language processing concepts](https://learn.microsoft.com/en-us/training/modules/introduction-language/) | Tokenization, statistical analysis, semantic models | [`docs/topics/text-analysis-azure-language.md`](docs/topics/text-analysis-azure-language.md) | [Gap complete](research/gaps/text-analysis-azure-language.md) | **Completed** |
| 4 | [Introduction to AI speech concepts](https://learn.microsoft.com/en-us/training/modules/introduction-ai-speech/) | Recognition, synthesis, speech-enabled solutions | [`docs/topics/speech.md`](docs/topics/speech.md) | [Gap complete](research/gaps/speech-recognition-synthesis.md) | **Completed** |
| 5 | [Introduction to computer vision concepts](https://learn.microsoft.com/en-us/training/modules/introduction-computer-vision/) | Vision tasks, CNNs, ViTs, multimodal models, image generation | [`docs/topics/computer-vision.md`](docs/topics/computer-vision.md) | [Gap complete](research/gaps/computer-vision-image-generation.md) | **In progress** |
| 6 | [Introduction to AI-powered information extraction concepts](https://learn.microsoft.com/en-us/training/modules/introduction-information-extraction/) | OCR, fields, mapping, extraction approaches | [`docs/topics/information-extraction.md`](docs/topics/information-extraction.md) | [Gap complete](research/gaps/information-extraction-content-understanding.md) | **Not started** |

### Phase 2 — Foundry implementation path

| Order | Official module | Main coverage | Repository material | Gap status | Learner status |
|---:|---|---|---|---|---|
| 1 | [Get started with AI in Azure](https://learn.microsoft.com/en-us/training/modules/get-started-with-ai-in-azure/) | Azure hierarchy, Foundry resources/projects, endpoints, authentication | [`docs/topics/foundry-foundation.md`](docs/topics/foundry-foundation.md) | [Gap complete](research/gaps/foundry-resources-projects-endpoints-authentication.md) | **Not started** |
| 2 | [Get started with generative AI and agents in Azure](https://learn.microsoft.com/en-us/training/modules/get-started-with-generative-ai-and-agents/) | Model selection/deployment, prompts, chat client, agent portal/client | [`docs/topics/generative-ai-prompts-model-configuration.md`](docs/topics/generative-ai-prompts-model-configuration.md) for model selection, deployment/configuration, prompts, and portal workflow; [`docs/topics/agentic-ai-single-agent-solutions.md`](docs/topics/agentic-ai-single-agent-solutions.md) for agent concepts, portal workflow, and testing; detailed client assets pending | [Generative/prompt gap complete](research/gaps/generative-ai-prompts-model-configuration.md); [agent gap complete](research/gaps/agentic-ai-single-agent-solutions.md); Foundry SDK and Python gaps pending | **Not started** |
| 3 | [Get started with text analysis in Azure](https://learn.microsoft.com/en-us/training/modules/get-started-text-analysis-azure/) | Foundry text analysis, Azure Language, Python client, agent tool | [`docs/topics/text-analysis-azure-language.md`](docs/topics/text-analysis-azure-language.md) | [Text-analysis gap complete](research/gaps/text-analysis-azure-language.md); cross-cutting Python gap pending | **Not started** |
| 4 | [Get started with speech in Azure](https://learn.microsoft.com/en-us/training/modules/get-started-speech-azure/) | Speech SDK, recognition, synthesis, Voice Live | [`docs/topics/speech.md`](docs/topics/speech.md) | [Gap complete](research/gaps/speech-recognition-synthesis.md) | **Not started** |
| 5 | [Get started with computer vision in Azure](https://learn.microsoft.com/en-us/training/modules/get-started-vision-azure/) | Multimodal visual input, image/video generation | [`docs/topics/computer-vision.md`](docs/topics/computer-vision.md) | [Gap complete](research/gaps/computer-vision-image-generation.md) | **Not started** |
| 6 | [Get started with AI-powered information extraction in Azure](https://learn.microsoft.com/en-us/training/modules/get-started-information-extraction/) | Content Understanding for documents, images, audio, and video | [`docs/topics/information-extraction.md`](docs/topics/information-extraction.md) | [Gap complete](research/gaps/information-extraction-content-understanding.md) | **Not started** |

# Objective-by-objective map

## Domain 1 — Identify AI concepts and capabilities (40–45%)

### 1. Describe principles of responsible AI

Primary module: [Introduction to AI concepts](https://learn.microsoft.com/en-us/training/modules/get-started-ai-fundamentals/)

Primary unit: [Responsible AI](https://learn.microsoft.com/en-us/training/modules/get-started-ai-fundamentals/7-responsible-ai)

Gap cluster: [**Responsible AI and model concepts — complete**](research/gaps/responsible-ai-model-concepts.md)

| Official subtopic | Official study link | Prepared asset |
|---|---|---|
| Fairness | [Responsible AI](https://learn.microsoft.com/en-us/training/modules/get-started-ai-fundamentals/7-responsible-ai) | [`docs/topics/responsible-ai-model-concepts.md`](docs/topics/responsible-ai-model-concepts.md) |
| Reliability and safety | [Responsible AI](https://learn.microsoft.com/en-us/training/modules/get-started-ai-fundamentals/7-responsible-ai) | [`docs/topics/responsible-ai-model-concepts.md`](docs/topics/responsible-ai-model-concepts.md) |
| Privacy and security | [Responsible AI](https://learn.microsoft.com/en-us/training/modules/get-started-ai-fundamentals/7-responsible-ai) | [`docs/topics/responsible-ai-model-concepts.md`](docs/topics/responsible-ai-model-concepts.md) |
| Inclusiveness | [Responsible AI](https://learn.microsoft.com/en-us/training/modules/get-started-ai-fundamentals/7-responsible-ai) | [`docs/topics/responsible-ai-model-concepts.md`](docs/topics/responsible-ai-model-concepts.md) |
| Transparency | [Responsible AI](https://learn.microsoft.com/en-us/training/modules/get-started-ai-fundamentals/7-responsible-ai) | [`docs/topics/responsible-ai-model-concepts.md`](docs/topics/responsible-ai-model-concepts.md) |
| Accountability | [Responsible AI](https://learn.microsoft.com/en-us/training/modules/get-started-ai-fundamentals/7-responsible-ai) | [`docs/topics/responsible-ai-model-concepts.md`](docs/topics/responsible-ai-model-concepts.md) |

### 2. Identify AI model components and configurations

Primary concept module: [Introduction to generative AI and agents](https://learn.microsoft.com/en-us/training/modules/fundamentals-generative-ai/)

Primary implementation module: [Get started with generative AI and agents in Azure](https://learn.microsoft.com/en-us/training/modules/get-started-with-generative-ai-and-agents/)

Gap clusters:

- [**Responsible AI and model concepts — complete**](research/gaps/responsible-ai-model-concepts.md)
- [**Generative AI, prompts, and model configuration — complete**](research/gaps/generative-ai-prompts-model-configuration.md)
- [**Foundry resources, projects, deployments, endpoints, and authentication — complete**](research/gaps/foundry-resources-projects-endpoints-authentication.md)

| Official subtopic | Official study links | Prepared asset |
|---|---|---|
| How generative AI models work | [Large language models](https://learn.microsoft.com/en-us/training/modules/fundamentals-generative-ai/3-language-models) | [`docs/topics/responsible-ai-model-concepts.md`](docs/topics/responsible-ai-model-concepts.md) |
| Select an appropriate model based on capabilities | [Generative AI models](https://learn.microsoft.com/en-us/training/modules/get-started-with-generative-ai-and-agents/2-generative-ai-models) | [`docs/topics/generative-ai-prompts-model-configuration.md`](docs/topics/generative-ai-prompts-model-configuration.md) |
| Select deployment options and configuration parameters | [Generative AI models](https://learn.microsoft.com/en-us/training/modules/get-started-with-generative-ai-and-agents/2-generative-ai-models) · [Using a generative AI model](https://learn.microsoft.com/en-us/training/modules/get-started-with-generative-ai-and-agents/3-using-generative-ai-models) | [`docs/topics/foundry-foundation.md`](docs/topics/foundry-foundation.md) covers deployment foundations; [`docs/topics/generative-ai-prompts-model-configuration.md`](docs/topics/generative-ai-prompts-model-configuration.md) covers model selection, deployment types, configuration layers, and parameters |

### 3. Identify AI workloads

Overview module: [Introduction to AI concepts](https://learn.microsoft.com/en-us/training/modules/get-started-ai-fundamentals/)

Overview exercise: [Explore AI workloads](https://learn.microsoft.com/en-us/training/modules/get-started-ai-fundamentals/7b-exercise)

| Official subtopic | Official study links | Gap and prepared assets |
|---|---|---|
| Scenarios for generative and agentic AI, text analysis, speech, computer vision, and information extraction | [Generative AI and agents](https://learn.microsoft.com/en-us/training/modules/get-started-ai-fundamentals/2-generative-ai) · [Text and natural language](https://learn.microsoft.com/en-us/training/modules/get-started-ai-fundamentals/5-natural-language-processing) · [Speech](https://learn.microsoft.com/en-us/training/modules/get-started-ai-fundamentals/4-speech) · [Computer vision](https://learn.microsoft.com/en-us/training/modules/get-started-ai-fundamentals/3-computer-vision) · [Information extraction](https://learn.microsoft.com/en-us/training/modules/get-started-ai-fundamentals/6-extract-insights) | Generative scenarios in [`docs/topics/generative-ai-prompts-model-configuration.md`](docs/topics/generative-ai-prompts-model-configuration.md); agent scenarios in [`docs/topics/agentic-ai-single-agent-solutions.md`](docs/topics/agentic-ai-single-agent-solutions.md); text scenarios in [`docs/topics/text-analysis-azure-language.md`](docs/topics/text-analysis-azure-language.md); speech, vision, and extraction assets complete |
| Keyword extraction, entity detection, sentiment analysis, and summarization | [Statistical text analysis](https://learn.microsoft.com/en-us/training/modules/introduction-language/3-statistical-techniques) · [Semantic language models](https://learn.microsoft.com/en-us/training/modules/introduction-language/4-semantic-models) | [`docs/topics/text-analysis-azure-language.md`](docs/topics/text-analysis-azure-language.md); [gap complete](research/gaps/text-analysis-azure-language.md) |
| Speech recognition and speech synthesis | [Speech-enabled solutions](https://learn.microsoft.com/en-us/training/modules/introduction-ai-speech/2-speech-enabled-solutions) · [Speech recognition](https://learn.microsoft.com/en-us/training/modules/introduction-ai-speech/3-speech-recognition) · [Speech synthesis](https://learn.microsoft.com/en-us/training/modules/introduction-ai-speech/4-speech-synthesis) | [`docs/topics/speech.md`](docs/topics/speech.md); gap complete |
| Computer vision and image-generation models | [Vision tasks](https://learn.microsoft.com/en-us/training/modules/introduction-computer-vision/2-overview) · [CNNs](https://learn.microsoft.com/en-us/training/modules/introduction-computer-vision/4-computer-vision-models) · [ViTs and multimodal models](https://learn.microsoft.com/en-us/training/modules/introduction-computer-vision/5-modern-vision-models) · [Image generation](https://learn.microsoft.com/en-us/training/modules/introduction-computer-vision/5a-generate-images) | [`docs/topics/computer-vision.md`](docs/topics/computer-vision.md); gap complete |
| Extract information from text, images, audio, and video | [Information extraction overview](https://learn.microsoft.com/en-us/training/modules/introduction-information-extraction/2-overview) · [OCR](https://learn.microsoft.com/en-us/training/modules/introduction-information-extraction/3-vision-extraction) · [Field extraction and mapping](https://learn.microsoft.com/en-us/training/modules/introduction-information-extraction/4-form-extraction) | [`docs/topics/information-extraction.md`](docs/topics/information-extraction.md); gap complete |

## Domain 2 — Implement AI solutions by using Microsoft Foundry (55–60%)

### 4. Implement generative AI apps and agents by using Foundry

Primary module: [Get started with generative AI and agents in Azure](https://learn.microsoft.com/en-us/training/modules/get-started-with-generative-ai-and-agents/)

Primary exercise: [Get started with generative AI and agents in Microsoft Foundry](https://learn.microsoft.com/en-us/training/modules/get-started-with-generative-ai-and-agents/6-exercise)

Gap clusters:

- [**Generative AI, prompts, and model configuration — complete**](research/gaps/generative-ai-prompts-model-configuration.md)
- [**Agentic AI and single-agent solutions — complete**](research/gaps/agentic-ai-single-agent-solutions.md)
- [**Foundry resources, projects, deployments, endpoints, and authentication — complete**](research/gaps/foundry-resources-projects-endpoints-authentication.md)
- **Foundry SDK chat and agent clients — pending**
- **Python SDK recognition for a C# developer — pending and cross-cutting**

| Official subtopic | Official study links | Prepared asset |
|---|---|---|
| Create effective system and user prompts | [Prompts](https://learn.microsoft.com/en-us/training/modules/fundamentals-generative-ai/6-writing-prompts) · [Using a generative AI model](https://learn.microsoft.com/en-us/training/modules/get-started-with-generative-ai-and-agents/3-using-generative-ai-models) | [`docs/topics/generative-ai-prompts-model-configuration.md`](docs/topics/generative-ai-prompts-model-configuration.md) |
| Deploy a model and interact with it in the Foundry portal | [Generative AI models](https://learn.microsoft.com/en-us/training/modules/get-started-with-generative-ai-and-agents/2-generative-ai-models) · [Using a generative AI model](https://learn.microsoft.com/en-us/training/modules/get-started-with-generative-ai-and-agents/3-using-generative-ai-models) | [`docs/topics/foundry-foundation.md`](docs/topics/foundry-foundation.md) covers hierarchy, endpoint, and authentication foundations; [`docs/topics/generative-ai-prompts-model-configuration.md`](docs/topics/generative-ai-prompts-model-configuration.md) covers model discovery, deployment decisions, playground configuration, and testing workflow |
| Create a lightweight chat client with the Foundry SDK | [Using a generative AI model](https://learn.microsoft.com/en-us/training/modules/get-started-with-generative-ai-and-agents/3-using-generative-ai-models) | Pending Python/C# mapped client asset |
| Create and test a single-agent solution in the Foundry portal | [AI agents](https://learn.microsoft.com/en-us/training/modules/fundamentals-generative-ai/7-agents) · [Creating an agent](https://learn.microsoft.com/en-us/training/modules/get-started-with-generative-ai-and-agents/4-creating-an-agent) | [`docs/topics/agentic-ai-single-agent-solutions.md`](docs/topics/agentic-ai-single-agent-solutions.md) includes the portal workflow, tool selection, testing matrix, safety rules, and guided exercise |
| Create a lightweight client application for an agent | [Creating an agent](https://learn.microsoft.com/en-us/training/modules/get-started-with-generative-ai-and-agents/4-creating-an-agent) | Pending Foundry SDK chat and agent client asset |

### 5. Implement AI solutions for text and speech by using Foundry

#### Text analysis

Primary module: [Get started with text analysis in Azure](https://learn.microsoft.com/en-us/training/modules/get-started-text-analysis-azure/)

Gap clusters:

- [**Text analysis and Azure Language — complete**](research/gaps/text-analysis-azure-language.md)
- **Python SDK recognition for a C# developer — pending and cross-cutting**

| Official subtopic | Official study links | Prepared asset |
|---|---|---|
| Build a lightweight application that includes text analysis | [Understand text analysis in Foundry](https://learn.microsoft.com/en-us/training/modules/get-started-text-analysis-azure/2-azure-language) · [Create a text-analysis client](https://learn.microsoft.com/en-us/training/modules/get-started-text-analysis-azure/3-language-sdk) · [Azure Language with an agent](https://learn.microsoft.com/en-us/training/modules/get-started-text-analysis-azure/4-language-mcp) · [Exercise](https://learn.microsoft.com/en-us/training/modules/get-started-text-analysis-azure/5-exercise) | [`docs/topics/text-analysis-azure-language.md`](docs/topics/text-analysis-azure-language.md) includes technique distinctions, model-versus-Language selection, portal flow, minimal Python recognition, C# mappings, MCP boundaries, practice, and a guided exercise |

#### Speech

Primary module: [Get started with speech in Azure](https://learn.microsoft.com/en-us/training/modules/get-started-speech-azure/)

Gap cluster: [**Speech recognition and synthesis — complete**](research/gaps/speech-recognition-synthesis.md)

| Official subtopic | Official study links | Prepared asset |
|---|---|---|
| Respond to spoken prompts with a deployed multimodal model | [Voice Live](https://learn.microsoft.com/en-us/training/modules/get-started-speech-azure/4-voice-live) | [`docs/topics/speech.md`](docs/topics/speech.md) separates Azure Speech, Voice Live, and deployed audio-capable models |
| Build a lightweight application with Azure Speech in Foundry Tools | [Speech recognition](https://learn.microsoft.com/en-us/training/modules/get-started-speech-azure/2-speech-recognition) · [Speech synthesis](https://learn.microsoft.com/en-us/training/modules/get-started-speech-azure/3-speech-synthesis) · [Exercise](https://learn.microsoft.com/en-us/training/modules/get-started-speech-azure/5-exercise) | [`docs/topics/speech.md`](docs/topics/speech.md) includes minimal Python flows and C# mappings |

### 6. Implement computer vision and image generation by using Foundry

Primary module: [Get started with computer vision in Azure](https://learn.microsoft.com/en-us/training/modules/get-started-vision-azure/)

Gap cluster: [**Computer vision, OCR, multimodal input, and image generation — complete**](research/gaps/computer-vision-image-generation.md)

| Official subtopic | Official study links | Prepared asset |
|---|---|---|
| Interpret visual input with a deployed multimodal model | [Multimodal image analysis](https://learn.microsoft.com/en-us/training/modules/get-started-vision-azure/2-vision-enabled-models) | [`docs/topics/computer-vision.md`](docs/topics/computer-vision.md) |
| Create visual output with generative models | [Image generation models](https://learn.microsoft.com/en-us/training/modules/get-started-vision-azure/3-image-generation) | [`docs/topics/computer-vision.md`](docs/topics/computer-vision.md) |
| Build a lightweight application with vision capabilities | [Exercise](https://learn.microsoft.com/en-us/training/modules/get-started-vision-azure/5-exercise) | [`docs/topics/computer-vision.md`](docs/topics/computer-vision.md) includes image-input and generation client flows |

Recognition-only related unit: [Video generation models](https://learn.microsoft.com/en-us/training/modules/get-started-vision-azure/4-video-generation). It appears in the official Learn module but is not a separate objective bullet.

### 7. Implement information extraction by using Foundry

Primary module: [Get started with AI-powered information extraction in Azure](https://learn.microsoft.com/en-us/training/modules/get-started-information-extraction/)

Gap cluster: [**Information extraction and Content Understanding — complete**](research/gaps/information-extraction-content-understanding.md)

| Official subtopic | Official study links | Prepared asset |
|---|---|---|
| Extract information from documents and forms with Content Understanding | [Extract information from documents](https://learn.microsoft.com/en-us/training/modules/get-started-information-extraction/2-documents) | [`docs/topics/information-extraction.md`](docs/topics/information-extraction.md) |
| Extract information from images with Content Understanding | [Extract information from documents](https://learn.microsoft.com/en-us/training/modules/get-started-information-extraction/2-documents) | [`docs/topics/information-extraction.md`](docs/topics/information-extraction.md) includes image analyzer selection |
| Extract information from audio and video with Content Understanding | [Extract information from audio and video](https://learn.microsoft.com/en-us/training/modules/get-started-information-extraction/3-audio-video) | [`docs/topics/information-extraction.md`](docs/topics/information-extraction.md) |
| Build a lightweight Content Understanding application | [Exercise](https://learn.microsoft.com/en-us/training/modules/get-started-information-extraction/4-exercise) | [`docs/topics/information-extraction.md`](docs/topics/information-extraction.md) includes the asynchronous SDK flow |

# Cross-cutting Foundry foundation

These units support multiple implementation objectives and should not be treated as a separate exam domain.

Gap cluster: [**Foundry resources, projects, deployments, endpoints, and authentication — complete**](research/gaps/foundry-resources-projects-endpoints-authentication.md)

| Subtopic | Official study link | Prepared asset and remaining work |
|---|---|---|
| Azure tenants, subscriptions, resource groups, and resources | [Understand Azure](https://learn.microsoft.com/en-us/training/modules/get-started-with-ai-in-azure/2-what-is-azure) | [`docs/topics/foundry-foundation.md`](docs/topics/foundry-foundation.md) |
| AI application infrastructure, security, hosting, and data | [Developing AI apps on Azure](https://learn.microsoft.com/en-us/training/modules/get-started-with-ai-in-azure/3-develop-ai-apps) | [`docs/topics/foundry-foundation.md`](docs/topics/foundry-foundation.md) covers identity and secret safety at recognition level |
| Foundry resources, projects, models, tools, agents, and knowledge | [Microsoft Foundry for AI](https://learn.microsoft.com/en-us/training/modules/get-started-with-ai-in-azure/4-microsoft-foundry) | [`docs/topics/foundry-foundation.md`](docs/topics/foundry-foundation.md) |
| Endpoints, keys, Entra ID, and clients | [Using Microsoft Foundry endpoints](https://learn.microsoft.com/en-us/training/modules/get-started-with-ai-in-azure/5-endpoints) | [`docs/topics/foundry-foundation.md`](docs/topics/foundry-foundation.md); detailed chat and agent client gap pending |
| End-to-end portal exercise | [Get started with Microsoft Foundry](https://learn.microsoft.com/en-us/training/modules/get-started-with-ai-in-azure/6-exercise) | Foundation scenario and code-recognition practice complete; official exercise remains part of the later study module |

# Gap-research production map

The official map defines what must be covered. Gap research strengthens the official material without expanding beyond it.

| # | Gap cluster | Objective coverage | Status | Repository output |
|---:|---|---|---|---|
| 1 | Responsible AI and model concepts | Responsible AI; model mechanics | **Complete** | [`research/gaps/responsible-ai-model-concepts.md`](research/gaps/responsible-ai-model-concepts.md) · [`docs/topics/responsible-ai-model-concepts.md`](docs/topics/responsible-ai-model-concepts.md) |
| 2 | Generative AI, prompts, and model configuration | Model selection, deployment/configuration, prompts | **Complete** | [`research/gaps/generative-ai-prompts-model-configuration.md`](research/gaps/generative-ai-prompts-model-configuration.md) · [`docs/topics/generative-ai-prompts-model-configuration.md`](docs/topics/generative-ai-prompts-model-configuration.md) |
| 3 | Agentic AI and single-agent solutions | Agent concepts and portal single-agent workflow | **Complete** | [`research/gaps/agentic-ai-single-agent-solutions.md`](research/gaps/agentic-ai-single-agent-solutions.md) · [`docs/topics/agentic-ai-single-agent-solutions.md`](docs/topics/agentic-ai-single-agent-solutions.md) |
| 4 | Text analysis and Azure Language | NLP techniques and lightweight text-analysis app | **Complete** | [`research/gaps/text-analysis-azure-language.md`](research/gaps/text-analysis-azure-language.md) · [`docs/topics/text-analysis-azure-language.md`](docs/topics/text-analysis-azure-language.md) |
| 5 | Speech recognition and synthesis | Speech concepts and implementation | **Complete** | [`research/gaps/speech-recognition-synthesis.md`](research/gaps/speech-recognition-synthesis.md) · [`docs/topics/speech.md`](docs/topics/speech.md) |
| 6 | Computer vision, OCR, multimodal input, and image generation | Vision concepts and implementation | **Complete** | [`research/gaps/computer-vision-image-generation.md`](research/gaps/computer-vision-image-generation.md) · [`docs/topics/computer-vision.md`](docs/topics/computer-vision.md) |
| 7 | Information extraction and Content Understanding | Extraction concepts and implementation | **Complete** | [`research/gaps/information-extraction-content-understanding.md`](research/gaps/information-extraction-content-understanding.md) · [`docs/topics/information-extraction.md`](docs/topics/information-extraction.md) |
| 8 | Foundry resources, projects, deployments, endpoints, and authentication | Cross-cutting Foundry foundation | **Complete** | [`research/gaps/foundry-resources-projects-endpoints-authentication.md`](research/gaps/foundry-resources-projects-endpoints-authentication.md) · [`docs/topics/foundry-foundation.md`](docs/topics/foundry-foundation.md) |
| 9 | Foundry SDK chat and agent clients | Chat client and agent client | **Pending** | Cross-topic client asset not yet created |
| 10 | Python SDK recognition for a C# developer | Cross-cutting code recognition | **Pending** | Cross-topic asset not yet created |

# Maintenance rules

1. The live study guide remains the authority for scope.
2. `STUDY-MAP.md` is the navigation source of truth and must contain every active objective bullet.
3. `docs/official-links.md` is the detailed verified-link inventory.
4. `STATUS.md` alone owns confirmed learner progress and the exact current study position.
5. `ASSESSMENT-LOG.md` owns assessment diagnostics, repair actions, and retest status.
6. Creating gap or topic material must not mark a Learn module complete.
7. When Microsoft changes the blueprint, a module, or a canonical URL, update the study map and official-link inventory together.
8. A new gap file must be linked from both its objective rows and the gap-production table.
9. A normal study session should start from the current route or a user-selected row in this map, not from improvised topic selection.