# Speech recognition and synthesis

Verified: **July 29, 2026**

> This document was prepared by a gap-research session for use during a later study session. The questions and exit check are stored here; they are not administered during research.

## Official exam objectives

Current AI-901 objectives relevant to this topic:

- Identify scenarios for common AI workloads, including speech.
- Identify features and capabilities of speech recognition and speech synthesis.
- Respond to spoken prompts by using a deployed multimodal model.
- Build a lightweight application by using Azure Speech in Foundry Tools.

Study guide: https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-901

## Official summary

- **Speech recognition (speech-to-text):** converts spoken audio into text. Typical uses include captions, transcripts, dictation, and processing recorded audio.
- **Speech synthesis (text-to-speech):** converts text into audible speech. The application selects text, a voice, and an audio destination such as a speaker or file.
- **Real-time transcription:** processes an incoming microphone or audio stream while speech is happening.
- **Batch transcription:** asynchronously processes stored recordings, often from Azure Storage or another file location.
- **Speech-to-speech:** accepts spoken audio, performs processing or reasoning, and returns spoken audio.

## Important distinctions

### Three speech-capable solution paths

| Path | Input and output | Azure setup | Portal or playground | Client or API | Best-fit scenario | Common exam trap |
|---|---|---|---|---|---|---|
| **Azure Speech STT/TTS** | Audio → text, or text → audio | Azure Speech through a Foundry/Speech resource; authenticate with a key or supported Microsoft Entra ID flow | Azure Speech speech-to-text or text-to-speech playground in Foundry | `azure-cognitiveservices-speech`; `SpeechRecognizer` or `SpeechSynthesizer` | Transcription, captions, dictation, reading text aloud, generating audio files | It performs specialized speech conversion; it does not by itself reason about what was said |
| **Azure Speech Voice Live** | Live audio → live audio, with agent processing between them | Voice Live plus a selected generative AI model; optionally integrated into a Foundry agent | Voice Live playground or Voice mode for a Foundry agent | `azure-ai-voicelive` and a real-time audio session | Natural conversational agents, interruptions, hands-free assistants, voice customer support | It is a managed speech-agent pipeline, not the same client flow as `SpeechRecognizer`/`SpeechSynthesizer` |
| **Deployed audio-capable multimodal model** | Audio and/or text → text and/or audio | Deploy an audio-capable model in Microsoft Foundry/Azure OpenAI | Model deployment and its supported Foundry test experience | OpenAI-compatible chat completions for audio files, or Realtime API over WebRTC/WebSocket/SIP for live audio | Understanding or reasoning over a spoken prompt, multimodal conversations, model-generated spoken responses | A model deployment and prompt/message flow are required; this is not simply Azure Speech transcription or synthesis |

### Fast selection rule

- Need **accurate conversion** between speech and text? Choose **Azure Speech STT/TTS**.
- Need a **managed real-time voice agent experience**? Choose **Voice Live**.
- Need the model to **understand, reason, or respond to a spoken prompt**? Choose a **deployed audio-capable multimodal model**.

### Real-time versus batch recognition

| Need | Choose |
|---|---|
| Captions, live dictation, or transcription while someone speaks | Real-time speech recognition |
| Many stored recordings processed asynchronously | Batch transcription |
| One short utterance in a simple client | `recognize_once_async()` |
| A longer live session with partial and final results | Continuous recognition with events and explicit start/stop |

## Minimal Python SDK flows

The goal is to recognize the objects and their order—not memorize every line.

### Speech recognition

```python
import azure.cognitiveservices.speech as speechsdk

speech_config = speechsdk.SpeechConfig(
    subscription="SPEECH_KEY",
    endpoint="SPEECH_ENDPOINT",
)

audio_config = speechsdk.audio.AudioConfig(use_default_microphone=True)

recognizer = speechsdk.SpeechRecognizer(
    speech_config=speech_config,
    audio_config=audio_config,
)

result = recognizer.recognize_once_async().get()
print(result.text)
```

Flow:

`credentials/endpoint → SpeechConfig → AudioConfig input → SpeechRecognizer → recognition operation → text result`

To read a WAV file instead of the microphone:

```python
audio_config = speechsdk.audio.AudioConfig(filename="message.wav")
```

### Speech synthesis

```python
import azure.cognitiveservices.speech as speechsdk

speech_config = speechsdk.SpeechConfig(
    subscription="SPEECH_KEY",
    endpoint="SPEECH_ENDPOINT",
)
speech_config.speech_synthesis_voice_name = "en-US-Ava:DragonHDLatestNeural"

audio_config = speechsdk.audio.AudioOutputConfig(use_default_speaker=True)

synthesizer = speechsdk.SpeechSynthesizer(
    speech_config=speech_config,
    audio_config=audio_config,
)

result = synthesizer.speak_text_async("The deployment succeeded.").get()
```

Flow:

`credentials/endpoint → SpeechConfig + voice → AudioOutputConfig → SpeechSynthesizer → synthesis operation → audio result`

To save a WAV file instead of using the speaker:

```python
audio_config = speechsdk.audio.AudioOutputConfig(filename="output.wav")
```

### C# mental mapping

- `SpeechConfig` ≈ service/client configuration.
- `AudioConfig` or `AudioOutputConfig` ≈ input/output adapter.
- `SpeechRecognizer` or `SpeechSynthesizer` ≈ specialized service client.
- `.get()` waits for the SDK's asynchronous result, similar in purpose to awaiting a `Task`.

## Prepared scenario practice

Use these questions during a future study or review session. Answer before opening the key.

1. A conference app must create captions while each presenter is speaking. Which capability should it use?
2. A company has 12,000 call recordings in Azure Storage and wants searchable transcripts without processing them live. Which approach should it use?
3. An accessibility feature must read newly received messages aloud using a selected neural voice. Which capability should it use?
4. A customer-service assistant must hold a low-latency spoken conversation, handle interruptions, and invoke agent tools. Which solution path best fits?
5. A user uploads a spoken product review and asks an AI model to judge its sentiment, explain the reasoning, and answer with speech. Which solution path best fits?

## Prepared Python code-recognition practice

6. Which class belongs in the blank?

```python
audio_config = speechsdk.audio.AudioConfig(use_default_microphone=True)
client = speechsdk.__________(speech_config=speech_config, audio_config=audio_config)
```

7. What should replace the blank to recognize one short utterance?

```python
result = recognizer.________________().get()
```

8. Which configuration switches recognition from the default microphone to a WAV file?

```python
# A
speechsdk.audio.AudioConfig(filename="message.wav")

# B
speechsdk.audio.AudioOutputConfig(filename="message.wav")
```

9. Which class should replace the blank to write synthesized audio to a file?

```python
audio_config = speechsdk.audio.________________(filename="output.wav")
```

10. This request includes `type: "input_audio"`, `modalities: ["text", "audio"]`, and a deployed model name. Which solution path is it using?

- Azure Speech `SpeechRecognizer`
- Voice Live
- An audio-capable multimodal model

<details>
<summary><strong>Answer key</strong></summary>

1. **Azure Speech real-time speech recognition.** Captions are needed while the speaker is talking.
2. **Batch transcription.** The recordings already exist and can be processed asynchronously.
3. **Azure Speech text-to-speech using `SpeechSynthesizer`.** Text is converted to spoken audio with a chosen voice.
4. **Azure Speech Voice Live.** The scenario emphasizes a managed, real-time conversational agent with interruptions and tools.
5. **A deployed audio-capable multimodal model.** The model must understand and reason over the spoken content, not merely transcribe it.
6. **`SpeechRecognizer`**
7. **`recognize_once_async`**
8. **A — `AudioConfig(filename="message.wav")`**. `AudioOutputConfig` is for synthesized output.
9. **`AudioOutputConfig`**
10. **An audio-capable multimodal model.** The request uses model modalities and an audio message item rather than Speech SDK recognizer classes.

</details>

## Prepared exit check

Use this only during a future study or review session. Without notes, explain all five statements:

1. Recognition and synthesis are opposite conversion directions.
2. Real-time and batch transcription solve different timing and storage scenarios.
3. `SpeechRecognizer` and `SpeechSynthesizer` have different audio configuration roles.
4. Azure Speech STT/TTS, Voice Live, and a deployed multimodal model are not interchangeable.
5. A spoken prompt that requires model reasoning belongs to the multimodal-model objective, not merely speech-to-text.

**Suggested future study-session heuristic:** 5/5 correct explanations plus at least 8/10 practice answers. This is not an official Microsoft passing threshold.

## Exam-readiness gaps

Matching research: [`../../research/gaps/speech-recognition-synthesis.md`](../../research/gaps/speech-recognition-synthesis.md)

Accepted gaps addressed by this topic file:

- **Terminology and implementation gap — High confidence:** three-way comparison of Azure Speech, Voice Live, and deployed audio-capable models.
- **Practice gap — High confidence:** minimal recognition/synthesis flows and ten original questions.
- **Constructor/authentication variation — Medium confidence:** focus on the role of `SpeechConfig`, not one memorized constructor shape.

The research-session deliverables are complete. These exercises are reserved for a future study session; no learner status is inferred from their creation.

## Official sources

- AI-901 study guide: https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-901
- Speech recognition module: https://learn.microsoft.com/en-us/training/modules/get-started-speech-azure/2-speech-recognition
- Speech synthesis module: https://learn.microsoft.com/en-us/training/modules/get-started-speech-azure/3-speech-synthesis
- Voice Live module: https://learn.microsoft.com/en-us/training/modules/get-started-speech-azure/4-voice-live
- Speech-to-text quickstart: https://learn.microsoft.com/en-us/azure/ai-services/speech-service/get-started-speech-to-text
- Text-to-speech quickstart: https://learn.microsoft.com/en-us/azure/ai-services/speech-service/get-started-text-to-speech
- GPT Realtime audio documentation: https://learn.microsoft.com/en-us/azure/foundry/openai/how-to/realtime-audio
- Audio-enabled model quickstart: https://learn.microsoft.com/en-us/azure/foundry/openai/audio-completions-quickstart

## External gap-research sources

See the matching gap-research file. External candidate evidence is not used as official teaching content here.

## Metadata

- Verified on: July 29, 2026
- Official blueprint checked on: July 29, 2026
- Research material status: Complete
- Study-session status: Not started
