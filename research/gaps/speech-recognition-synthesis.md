# Speech recognition and synthesis — Exam-readiness gaps

Research date: **July 29, 2026**

## Official scope

Current AI-901 objectives relevant to this topic:

- Identify scenarios for common AI workloads, including speech.
- Identify features and capabilities of speech recognition and speech synthesis.
- Respond to spoken prompts by using a deployed multimodal model.
- Build a lightweight application by using Azure Speech in Foundry Tools.

## Official material reviewed

- AI-901 study guide: https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-901
- Concepts learning path: https://learn.microsoft.com/en-us/training/paths/ai-concepts/
- Introduction to AI speech concepts: https://learn.microsoft.com/en-us/training/modules/introduction-ai-speech/
- Speech recognition concept unit: https://learn.microsoft.com/en-us/training/modules/introduction-ai-speech/3-speech-recognition
- Speech synthesis concept unit: https://learn.microsoft.com/en-us/training/modules/introduction-ai-speech/4-speech-synthesis
- Concept exercise: https://learn.microsoft.com/en-us/training/modules/introduction-ai-speech/5-exercise-speech
- Implementation learning path: https://learn.microsoft.com/en-us/training/paths/introduction-to-ai-on-azure/
- Get started with speech in Azure: https://learn.microsoft.com/en-us/training/modules/get-started-speech-azure/
- Speech recognition implementation unit: https://learn.microsoft.com/en-us/training/modules/get-started-speech-azure/2-speech-recognition
- Speech synthesis implementation unit: https://learn.microsoft.com/en-us/training/modules/get-started-speech-azure/3-speech-synthesis
- Voice Live unit: https://learn.microsoft.com/en-us/training/modules/get-started-speech-azure/4-voice-live
- Implementation exercise: https://learn.microsoft.com/en-us/training/modules/get-started-speech-azure/5-exercise
- Azure Speech documentation: https://learn.microsoft.com/en-us/azure/ai-services/speech-service/
- Speech-to-text quickstart: https://learn.microsoft.com/en-us/azure/ai-services/speech-service/get-started-speech-to-text
- Text-to-speech quickstart: https://learn.microsoft.com/en-us/azure/ai-services/speech-service/get-started-text-to-speech
- GPT Realtime audio documentation: https://learn.microsoft.com/en-us/azure/foundry/openai/how-to/realtime-audio
- Audio-enabled model quickstart: https://learn.microsoft.com/en-us/azure/foundry/openai/audio-completions-quickstart

## Coverage assessment

The concept module gives strong coverage of what speech recognition and synthesis do and explains their internal processing pipelines in unusual depth for a fundamentals course.

The implementation module covers Foundry portal exploration, Azure Speech resource configuration, Python SDK installation, speech recognition and synthesis client flows, real-time versus batch transcription, and Voice Live. It includes useful Python examples for `SpeechConfig`, `AudioConfig`, `SpeechRecognizer`, and `SpeechSynthesizer`.

The main weakness is alignment across the separate implementation objectives. The speech exercise focuses on a Voice Live agent, while the blueprint also expects recognition of a standalone Azure Speech client flow and a deployed audio-capable multimodal model flow.

## Gaps

### Azure Speech, Voice Live, and multimodal audio models are easy to blur together

- **Type:** Terminology gap and implementation gap
- **Evidence:** The blueprint separately names a deployed multimodal model and Azure Speech in Foundry Tools. The speech module then introduces Voice Live as another speech-capable agent route.
- **Confidence:** High
- **Why it matters:** These approaches overlap in user experience but use different resources, clients, and request flows. Treating them as synonyms can cause service-selection and workflow errors.
- **Recommended preparation:** Create a one-page comparison with three rows:
  1. Azure Speech speech-to-text and text-to-speech
  2. Azure Speech Voice Live
  3. deployed GPT audio or realtime multimodal model

For each row, identify the input, output, Azure resource or deployment, portal playground, client package, and best-fit scenario.

### The official exercise does not practice the standalone Speech SDK flow

- **Type:** Practice gap
- **Evidence:** The implementation lesson contains Python examples for recognition and synthesis, but its exercise is centered on creating a Voice Live speech-capable agent.
- **Confidence:** High
- **Why it matters:** The blueprint explicitly includes building a lightweight application with Azure Speech. Reading example code is weaker preparation than tracing or running the complete client flow.
- **Recommended preparation:** Run or carefully trace two minimal scripts:
  - microphone or audio file → `SpeechRecognizer` → text
  - text → `SpeechSynthesizer` → speaker or audio file

Be able to explain the flow as `resource credentials → SpeechConfig → audio configuration → recognizer or synthesizer → async operation → result`.

### Authentication and constructor examples use more than one shape

- **Type:** Implementation gap
- **Evidence:** Current material shows endpoint-and-key configuration, while Speech documentation and quickstarts also commonly use key-and-region or Microsoft Entra ID.
- **Confidence:** Medium
- **Why it matters:** A code question may use a valid setup that looks different from the one example memorized from Learn.
- **Recommended preparation:** Recognize the purpose of `SpeechConfig` rather than memorizing one constructor. Know that the client needs a Speech resource identity and location or endpoint, supplied through a key, token, or supported credential flow.

### Scenario and code-recognition practice is thinner than the teaching content

- **Type:** Practice gap
- **Evidence:** The official modules explain the features and show code, but provide little visible practice distinguishing recognition, synthesis, real-time transcription, batch transcription, Voice Live, and audio-capable model scenarios.
- **Confidence:** High
- **Why it matters:** AI-901 requires Python familiarity and SDK recognition, and the exam does not provide Microsoft Learn access during a fundamentals exam.
- **Recommended preparation:** Add five original scenario questions and five short code-completion questions. Focus on selecting the correct class, audio configuration, operation, and service route—not on memorizing long scripts.

### External reports are too limited to estimate speech question frequency

- **Type:** Unverified anecdote
- **Evidence:** One dated beta-exam account reported needing to distinguish recognition from synthesis, audio from speech, and understand Python SDK usage. No strong independent corroboration was found for the frequency or exact form of speech questions.
- **Confidence:** Low
- **Why it matters:** The report supports the official preparation direction, but it cannot justify claims such as “speech is heavily tested.”
- **Recommended preparation:** Use the report only as a reason to practice official objectives, not to change topic weighting.

## Preparation assets created

Created **July 29, 2026**:

- [`../../docs/topics/speech.md`](../../docs/topics/speech.md)
  - three-way comparison of Azure Speech STT/TTS, Voice Live, and deployed audio-capable models
  - minimal Python recognition and synthesis flows
  - five original scenario questions
  - five original Python code-recognition questions
  - answer key and no-notes exit check

**Remediation status:** The missing study assets now exist. The gap is not considered mastered or closed until the user completes the practice and exit check.

## Claims not accepted

- Voice Live alone covers every speech implementation objective.
- Azure Speech and an audio-capable multimodal model are interchangeable.
- One beta candidate report proves the number or distribution of speech questions.
- Deep study of custom voice, advanced SSML, telephony, or speech translation is required for AI-901 without further assessment evidence.
- Any dump-derived claim about real exam questions.

## Practical exit criteria

This gap is adequately addressed when the user can:

1. Explain recognition versus synthesis without notes.
2. Select real-time versus batch transcription for a scenario.
3. Trace the minimal Python SDK flow for both recognition and synthesis.
4. Distinguish Azure Speech, Voice Live, and a deployed multimodal audio model.
5. Answer short scenario and code-recognition questions without relying on copied scripts.

## External evidence reviewed

- First-hand AI-901 beta account, April 29, 2026: https://www.linkedin.com/pulse/career-trajectory-issue-165-free-certified-program-20-carla-qb2zc
- Microsoft Q&A discussion of AI-901 implementation-style preparation, June 19, 2026: https://learn.microsoft.com/en-us/answers/questions/5924630/does-the-ai-901-exam-include-a-lab-performance-bas
- Official exam experience page: https://learn.microsoft.com/en-us/credentials/support/exam-duration-exam-experience
