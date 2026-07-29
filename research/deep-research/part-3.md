# AI-901 Deep Research Report — Part 3 of 4

> Preserved from the July 2026 deep-research session. Embedded ChatGPT citation markers are session-local; verify claims against current live sources before reuse.

## Technical coverage and hands-on checklist

### The technical topics you should actually cover

For **Foundry and model work**, you should understand what a Foundry resource is, how Foundry unifies models, agents, and tools, how to choose a model based on capabilities, what deployment names and endpoints are doing, and the difference between the older Chat Completions flow and the newer Responses API direction. Microsoft’s SDK quickstarts and deployment docs make this concrete: generate a model response, create an agent, create a multi-turn conversation, deploy a model through the portal or CLI, and call the endpoint with either API keys or Entra-based auth. citeturn30view1turn30view2turn30view4turn27search2turn33view5

For **prompt engineering and agents**, study prompts as controllable system behavior, not as generic AI advice. Microsoft’s prompt-engineering guidance says prompt construction requires practice, and the skills outline explicitly requires effective system and user prompts, single-agent creation/testing, and lightweight agent clients. Foundry Agent Service also distinguishes between **prompt agents** and **hosted agents**, which is the kind of product differentiation that commonly becomes exam fodder. citeturn33view0turn8view0turn33view2turn33view3

For **language and text analysis**, you should know the canonical text-analysis tasks—keyword extraction, entity detection, sentiment analysis, summarization, PII detection—and how Azure Language is now used through Foundry, REST APIs, and client libraries. Microsoft’s text-analysis module is especially aligned because it literally teaches you to build a lightweight Python client application and to use Azure Language with an agent. Language also matters historically because Microsoft is continuing to move authoring/testing from Language Studio into Foundry, so older Language Studio-only prep can become misleading. citeturn8view0turn32view3turn35search2turn35search3turn35search13turn35search7

For **speech**, study the difference between speech-to-text, text-to-speech, translation, and live voice scenarios, and know the operational split between real-time, fast, and batch transcription. Candidates repeatedly mention speech as disproportionately visible, and Microsoft’s current docs are detailed enough to make this a likely question area. citeturn23view2turn32view0turn32view1

For **vision and image generation**, focus on what Azure Vision does, what OCR/Read does, when a general multimodal model is sufficient, and when a specific vision capability is better. The skills outline also explicitly includes image-generation models and interpreting visual input in prompts. citeturn8view0turn32view2turn27search7turn27search4

For **information extraction**, treat Azure Content Understanding as a first-class topic. The exam outline names it repeatedly, and Microsoft positions it as a multimodal extraction solution spanning documents, images, audio, and video. The “choose the right document-processing tool” guidance is also worth learning because it sharpens the boundary between Content Understanding, Document Intelligence, and generic LLM prompting—exactly the kind of “best fit” distinction certification exams like to test. citeturn8view0turn32view4turn32view5turn9search10

For **resource provisioning, security, and compliance basics**, you should know how to create a Foundry resource, what RBAC roles are relevant, how to authenticate with Azure CLI and Entra, and the basic privacy/security guarantees Microsoft publishes for Foundry Models and Agent Service. Microsoft’s security/privacy docs are especially useful because AI-901 includes responsible AI and practical implementation, so you should be able to reason about prompts, outputs, abuse monitoring, data handling, and role access at a basic level. citeturn30view3turn31view3turn33view5turn30view5turn31view0turn31view1turn31view2

### Recommended hands-on build checklist

| Build | What you should practice | Why it belongs on your checklist |
|---|---|---|
| **Provision a Foundry resource from the CLI** citeturn30view3 | Use `az login`, create a resource group, create an `AIServices` Foundry resource, and identify the required RBAC permission. citeturn30view3 | Validates provisioning concepts, Azure CLI familiarity, and resource terminology exactly where Microsoft documents them. |
| **Deploy one model in Foundry** citeturn30view4turn33view4 | Add a model deployment and note the deployment name, endpoint, and required permissions. | Model deployment options and configuration are explicitly on the blueprint. |
| **Build a minimal Python chat client** citeturn33view5turn27search2 | Send messages with system and user roles, receive a response, and keep a multi-turn conversation. | This directly matches the exam’s “lightweight chat client” objective. |
| **Create and test a prompt agent** citeturn33view2turn33view3 | Build a prompt agent in the portal, attach instructions/tools, and test it in a conversation. | Multiple candidates reported agent/prompt questions, and Microsoft lists single-agent work explicitly. |
| **Call a model with Entra auth, not only an API key** citeturn33view5 | Use Azure CLI plus `DefaultAzureCredential` or equivalent auth flow. | Helps with security/compliance basics and with recognizing real code samples. |
| **Text-analysis mini-app** citeturn32view3turn35search3 | Run sentiment, entity/PII, and summarization-style tasks; compare Azure Language vs general-purpose model usage. | Covers a cluster of objective verbs and common “which service fits” questions. |
| **Speech mini-app** citeturn32view0turn32view1 | Do one speech-to-text run and one text-to-speech run; be able to explain real-time vs fast vs batch transcription. | Speech is both official blueprint content and a repeated candidate callout. |
| **Vision + OCR mini-lab** citeturn32view2turn27search7 | Analyze an image, extract text, and send an image to a vision-capable model. | Helps distinguish classic vision features from multimodal prompting. |
| **Image-generation lab** citeturn27search4 | Deploy an image-generation model and generate one output from Foundry. | The blueprint explicitly includes computer vision and image-generation capabilities. |
| **Content Understanding lab** citeturn32view4turn32view5 | Run extraction against one document, one image, and one audio or video sample if possible. | This topic is too visible in the blueprint to leave theoretical. |
| **Privacy / governance review lab** citeturn30view5turn31view0turn31view2 | Read Microsoft’s privacy/security guarantees for Foundry Models and Agent Service, then write a one-page note on what is and is not used for training. | Responsible AI questions often turn into operational privacy/governance questions. |

A practical rule for “what is worth writing on paper” is this: write down only the things that help you **differentiate**, **sequence**, or **execute**. In other words, write contrasts like *speech-to-text vs text-to-speech*, *Content Understanding vs Document Intelligence*, *prompt agent vs hosted agent*, *model vs deployment vs endpoint*, and write short execution flows like *resource → deployment → auth → prompt → response*. Do **not** copy whole docs. That recommendation is partly an inference from the blueprint’s implementation verbs and partly from note-taking research suggesting that longhand is most useful when it forces deeper processing rather than transcription. citeturn8view0turn16search30turn16search22

## Study methodology for a paper-note learner

### The core method

The highest-yield study method for AI-901 is a five-part loop: **map the blueprint, learn a topic, build a tiny lab, retrieve from memory on paper, then test and analyze errors**. This is not just generic study advice. Microsoft’s official objectives are action-oriented, the Practice Assessment gives per-question rationales and further-reading links, and high-performing candidates repeatedly stress exercises and code examples. Learning-science reviews also rate **practice testing** and **distributed practice** among the highest-utility strategies, while retrieval-practice experiments show that actively reconstructing knowledge beats passive review for long-term retention. citeturn8view0turn17view0turn34view1turn16search1turn16search13turn16search28

For someone who learns by writing on paper, the goal is not to fill notebooks. The goal is to turn paper into a **retrieval device**. The classic longhand note-taking study found an advantage for conceptual learning over laptop transcription, although later replication work found less consistent differences; taken together, the safest conclusion is that paper helps most when you summarize, compare, and reconstruct, not when you copy. For AI-901, that means one-page sheets, contrast tables, and memory dumps are useful; copying module text is not. citeturn16search30turn16search22

### The recommended workflow

```mermaid
flowchart TD
    A[Read official AI-901 skill objective] --> B[Study one module or doc]
    B --> C[Build one tiny lab]
    C --> D[Close the docs]
    D --> E[Write from memory on paper]
    E --> F[Self-quiz or practice assessment block]
    F --> G[Error log]
    G --> H[Targeted re-study]
    H --> C
    G --> I[Weekly cumulative review]
