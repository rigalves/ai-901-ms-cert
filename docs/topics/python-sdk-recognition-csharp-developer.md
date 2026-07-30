# Python SDK recognition for a C# developer

Verified: **July 30, 2026**

> This document was prepared by a gap-research session for use during later implementation and review sessions. The questions are stored here; they are not administered during research.

## Official exam relevance

The current AI-901 study guide says candidates need knowledge of Python coding syntax and programming techniques and should be familiar with REST APIs, SDKs, and CLIs.

This asset supports the lightweight-application objectives for:

- Foundry model chat and agents;
- text analysis;
- Azure Speech;
- computer vision and image generation; and
- Content Understanding information extraction.

Study guide: https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-901

Related topic assets:

- [`foundry-sdk-chat-agent-clients.md`](foundry-sdk-chat-agent-clients.md)
- [`text-analysis-azure-language.md`](text-analysis-azure-language.md)
- [`speech.md`](speech.md)
- [`computer-vision.md`](computer-vision.md)
- [`information-extraction.md`](information-extraction.md)

# Part 1 — The bounded Python target

## What you need

For AI-901, learn to:

1. read a short Python SDK snippet;
2. identify configuration, authentication, client, request, and result;
3. recognize the operation that matches the scenario;
4. trace collections and nested result properties;
5. choose a missing method or value;
6. spot a client, endpoint, state, or completion mismatch; and
7. make a small edit such as changing input, deployment, analyzer, or output handling.

## What you do not need from this asset

- general Python application architecture;
- algorithms or data-structure exercises;
- object-oriented Python design;
- decorators, metaclasses, packaging, or framework development;
- advanced concurrency;
- memorization of exact preview package versions; or
- reproduction of complete SDK samples from memory.

Decision rule:

> Read Python as a compact description of an Azure client workflow, not as a new software-engineering career path.

# Part 2 — The universal SDK-reading flow

When a snippet looks unfamiliar, locate these seven items in order:

| Step | Question | Common Python cue |
|---:|---|---|
| 1 | Which package or SDK is used? | `import` or `from ... import ...` |
| 2 | Where does configuration come from? | `os.environ[...]`, `os.getenv(...)`, `.env` |
| 3 | How does authentication work? | key wrapper or `DefaultAzureCredential()` |
| 4 | Which client is constructed? | `ClientName(endpoint=..., credential=...)` |
| 5 | Which remote operation is called? | `responses.create`, `detect_language`, `begin_analyze`, and so on |
| 6 | Is state or delayed completion involved? | conversation ID, callback, poller, `.get()`, `await` |
| 7 | Where is the useful result? | `output_text`, nested properties, event argument, `poller.result()` |

Durable mental model:

`configuration → credential → client → request or operation → result → output`

The SDK family changes. The skeleton usually does not.

# Part 3 — Core Python-to-C# map

These are mental mappings, not exact syntax equivalences.

## Imports, variables, and calls

| Python | C# mental mapping |
|---|---|
| `import os` | `using System;` plus access to an installed package/module |
| `from azure.identity import DefaultAzureCredential` | `using Azure.Identity;` and a NuGet dependency |
| `import azure.cognitiveservices.speech as speechsdk` | namespace alias such as `using SpeechSdk = ...;` |
| `client = OpenAI(...)` | `var client = new OpenAIClient(...);` |
| `endpoint=endpoint` | named constructor argument |
| `response = client.responses.create(...)` | inferred local variable receiving a service response |
| `response.output_text` | property or helper on a response DTO |

Python variables do not declare their type in the usual assignment. Infer the type from the right side and later member access.

```python
response = client.responses.create(...)
print(response.output_text)
```

Read as:

```csharp
var response = client.CreateResponse(...);
Console.WriteLine(response.OutputText);
```

## Blocks and control flow

| Python | C# mental mapping |
|---|---|
| indentation | braces define block scope |
| `if condition:` | `if (condition) { ... }` |
| `elif condition:` | `else if (condition) { ... }` |
| `else:` | `else { ... }` |
| `for item in items:` | `foreach (var item in items)` |
| `def handler(evt):` | local method or callback function |
| `return value` | same intent as C# `return value;` |

Python indentation is syntax. A line indented under an `if`, `for`, `def`, `with`, or `async with` belongs to that block.

## Null-like and boolean checks

| Python | C# mental mapping |
|---|---|
| `None` | `null` |
| `value is None` | `value is null` |
| `value is not None` | `value is not null` |
| `if value:` | non-null/non-empty/truthy check |
| `if not value:` | null, empty, zero, or false-style check depending on type |
| `a and b` | `a && b` |
| `a or b` | `a || b` or fallback-style selection depending on context |
| `not condition` | `!condition` |

Do not translate `if value:` mechanically without checking the value’s type. For a collection, it commonly means “not null and not empty.”

## Strings

| Python | C# mental mapping |
|---|---|
| `f"Name: {name}"` | `$"Name: {name}"` |
| `"Name: {}".format(name)` | `string.Format("Name: {0}", name)` |
| `input()` | `Console.ReadLine()` |
| `print(value)` | `Console.WriteLine(value)` |

# Part 4 — Environment variables

## Required configuration

```python
endpoint = os.environ["FOUNDRY_PROJECT_ENDPOINT"]
```

`os.environ[...]` uses dictionary-style access. If the variable is missing, access fails rather than quietly returning no value.

C# mental mapping:

```csharp
string endpoint = Environment.GetEnvironmentVariable("FOUNDRY_PROJECT_ENDPOINT")
    ?? throw new InvalidOperationException("Missing endpoint");
```

## Optional or nullable lookup

```python
key = os.getenv("CONTENTUNDERSTANDING_KEY")
```

`os.getenv(...)` returns the value when present and otherwise returns `None` unless a default is supplied.

C# mental mapping:

```csharp
string? key = Environment.GetEnvironmentVariable("CONTENTUNDERSTANDING_KEY");
```

## Recognition rule

- `os.environ["NAME"]`: treat the setting as required.
- `os.getenv("NAME")`: expect a possibly missing value or fallback decision.

Example:

```python
credential = AzureKeyCredential(key) if key else DefaultAzureCredential()
```

Read as:

```csharp
TokenCredentialOrKey credential = key is not null
    ? new AzureKeyCredential(key)
    : new DefaultAzureCredential();
```

The exact common C# base type varies by SDK; the decision is what matters.

# Part 5 — Lists, indexing, dictionaries, and typed results

## List construction versus indexing

```python
texts = [text]
result = client.detect_language(texts)[0]
```

- `[text]` creates a one-item list.
- `[0]` selects the first returned result.

C# mental mapping:

```csharp
var texts = new[] { text };
var result = client.DetectLanguage(texts)[0];
```

Azure Language uses a batch-shaped request even when there is only one document.

## Slicing

```python
for phrase in transcript_phrases[:2]:
```

`[:2]` means take elements from the beginning up to, but not including, index 2.

C# mental mapping:

```csharp
foreach (var phrase in transcriptPhrases.Take(2))
```

## Dictionaries as JSON-like request payloads

```python
input=[
    {
        "role": "user",
        "content": [
            {"type": "input_text", "text": "Describe the image"},
            {"type": "input_image", "image_url": image_url},
        ],
    }
]
```

Read from the outside inward:

1. `input` is a list of messages.
2. Each message is a dictionary with `role` and `content`.
3. `content` is another list.
4. Each content item is a dictionary describing one typed part.

C# mental mapping: a nested request DTO graph or JSON object initializer.

## Dictionary access versus typed property access

```python
field = content.fields.get("CustomerName")
print(field.value)
```

- `.get("CustomerName")` is safe dictionary lookup and can return `None`.
- `.value` is property access on the returned SDK object.

C# mental mapping:

```csharp
if (content.Fields.TryGetValue("CustomerName", out var field))
{
    Console.WriteLine(field.Value);
}
```

## Type checks and member checks

| Python | C# mental mapping |
|---|---|
| `isinstance(value, ObjectField)` | `value is ObjectField` |
| `hasattr(value, "value")` | reflection/dynamic member check; often a guard around a union-like result |
| `result: AnalysisResult = ...` | local variable type annotation; similar intent to an explicit variable type |

A Python type annotation helps readers and tooling. It does not turn Python into statically enforced C#.

# Part 6 — Four completion patterns

## Pattern A: Immediate synchronous result

```python
response = client.responses.create(...)
print(response.output_text)
```

The call returns the completed response directly.

Mental mapping: a normal synchronous method returning a DTO.

## Pattern B: Long-running poller, synchronous client

```python
poller = client.begin_analyze(...)
result = poller.result()
```

- `begin_analyze` starts a long-running operation and returns a poller.
- `poller.result()` waits for completion and returns the final analysis result.

Mental mapping:

```csharp
var operation = client.Analyze(..., WaitUntil.Started);
var completed = operation.WaitForCompletion();
```

The exact C# API shape differs by SDK. Preserve the two-stage idea: start operation, then obtain completed result.

## Pattern C: True Python coroutine

```python
async with ContentUnderstandingClient(...) as client:
    poller = await client.begin_analyze(...)
    result = await poller.result()
```

Recognition cues:

- imported from an `.aio` package;
- enclosing function uses `async def`;
- calls use `await`;
- `async with` manages asynchronous cleanup.

C# mental mapping: `await using` plus awaited asynchronous operations.

## Pattern D: SDK future with `.get()`

```python
result = speech_synthesizer.speak_text_async(text).get()
```

This current Speech SDK sample does **not** use Python `await`. The SDK returns its own future-like object, and `.get()` waits for the result.

Mental mapping: blocking on an SDK-specific future or task result—not recommended as a general C# async pattern, but correct recognition for this sample.

## Completion decision table

| Cue | Returned first | How final result is obtained |
|---|---|---|
| ordinary method | response | use response directly |
| `begin_...` sync client | poller | `poller.result()` |
| `.aio`, `async def`, `await` | coroutine/poller | `await ...` and `await poller.result()` |
| Speech method ending `_async` | Speech SDK future | `.get()` in the shown sample |

Never choose `await` solely because a method name contains `_async`.

# Part 7 — Callbacks and events in Speech

Continuous recognition commonly uses callback functions:

```python
def recognized(evt):
    print(evt.result.text)

speech_recognizer.recognized.connect(recognized)
speech_recognizer.start_continuous_recognition()
```

C# mental mapping:

```csharp
recognizer.Recognized += (_, evt) =>
    Console.WriteLine(evt.Result.Text);

await recognizer.StartContinuousRecognitionAsync();
```

Recognition rules:

- `def recognized(evt)` declares the callback.
- `.connect(recognized)` subscribes the callback to the event.
- recognized text arrives through `evt.result.text`.
- starting continuous recognition does not itself contain every transcript result.

# Part 8 — Context managers and generator expressions

## Context manager

```python
with open("generated.png", "wb") as file:
    file.write(image_bytes)
```

`with` ensures the file is cleaned up when the block exits.

- `"wb"` means write binary.
- `file` is scoped to the block by convention.

C# mental mapping:

```csharp
using var file = File.OpenWrite("generated.png");
file.Write(imageBytes);
```

## Generator expression with `next`

```python
image_base64 = next(
    item.result
    for item in response.output
    if item.type == "image_generation_call"
)
```

Read as:

1. iterate over `response.output`;
2. keep items whose type is `image_generation_call`;
3. project each matching item to `item.result`;
4. return the first projected value.

C# mental mapping:

```csharp
var imageBase64 = response.Output
    .Where(item => item.Type == "image_generation_call")
    .Select(item => item.Result)
    .First();
```

# Part 9 — Service-family recognition signatures

| Service family | Client/request signature | Result signature | Main syntax traps |
|---|---|---|---|
| Foundry model | project or OpenAI-compatible client → `responses.create(model=..., input=...)` | `response.output_text` | named args, deployment name, nested prompt list |
| Foundry agent | project client → agent-bound client → optional conversation → `responses.create` | response output plus reused conversation | agent name versus model deployment; state ID |
| Azure Language | `TextAnalyticsClient` → named task method with `[text]` | one typed result per input document | one-item list, `[0]`, nested properties, loops |
| Azure Speech | `SpeechConfig` + audio config + recognizer/synthesizer | direct result, SDK future, or event argument | callbacks, enums, `_async().get()` |
| Vision model | OpenAI-compatible client → nested text/image content | generated text | nested lists/dictionaries, deployment name |
| Image generation | Responses API with image-generation tool | matching output item containing Base64 | generator expression, binary file context |
| Content Understanding | client → `begin_analyze(analyzer_id=..., inputs=[...])` | poller → result → contents/fields | poller, type annotations, `.get`, type checks, async client |

Fast identification rule:

> Find the client type and the operation name before reading every line of syntax.

# Part 10 — Common traps

## Trap 1: Required versus optional environment lookup

```python
endpoint = os.getenv("ENDPOINT")
client = Client(endpoint=endpoint)
```

Potential issue: `endpoint` can be `None`. Confirm whether the sample validates it or expects a required lookup.

## Trap 2: Passing one string instead of a batch

```python
result = client.detect_language(text)[0]
```

Potential issue: the Language method expects a list of documents in the shown pattern. Use `[text]`.

## Trap 3: Treating a poller as the final result

```python
result = client.begin_analyze(...)
print(result.contents[0])
```

Potential issue: `begin_analyze` returns a poller. Obtain `poller.result()` first.

## Trap 4: Awaiting the Speech SDK sample blindly

```python
result = await synthesizer.speak_text_async(text)
```

Potential issue: the shown Python Speech SDK pattern uses `.get()` on its own future-like return object.

## Trap 5: Reading generated text from the client

```python
print(client.output_text)
```

Potential issue: output belongs to the response, not the service client.

## Trap 6: Confusing a dictionary key with a property

```python
print(content.fields.CustomerName)
```

Potential issue: `fields` is dictionary-like in the shown Content Understanding sample. Use `.get("CustomerName")` or indexed access as appropriate.

## Trap 7: Losing multi-turn state

```python
conversation = openai.conversations.create()
first = openai.responses.create(conversation=conversation.id, input="Hello")
second = openai.responses.create(input="What did I say?")
```

Potential issue: the second request does not reuse the conversation or a previous response reference.

## Trap 8: Forgetting binary decoding

```python
file.write(image_base64)
```

Potential issue: Base64 text must be decoded to bytes before writing a binary image file.

# Prepared practice

Use these questions during a future implementation or review session. Answer before opening the key.

1. In `from azure.identity import DefaultAzureCredential`, what is imported?
2. What is the C# mental mapping for `client = TextAnalyticsClient(endpoint=endpoint, credential=credential)`?
3. What practical difference matters between `os.environ["NAME"]` and `os.getenv("NAME")`?
4. In `detect_language([text])[0]`, what does `[text]` do?
5. What does the final `[0]` do?
6. In a nested multimodal request, are `{...}` blocks usually service clients or JSON-like request objects?
7. What is the difference between `fields.get("Total")` and `field.value`?
8. What does `if not result.contents:` usually guard against?
9. What is the C# mental mapping for `for entity in result.entities:`?
10. What does `result: AnalysisResult = poller.result()` add before the assignment operator?
11. Which object is returned by `begin_analyze(...)`: final analysis or a poller?
12. Which line obtains the final result from a synchronous Content Understanding poller?
13. Which cues indicate the true asynchronous Content Understanding client?
14. Why is `speak_text_async(text).get()` not proof that the code should use Python `await`?
15. In continuous speech recognition, where does recognized text commonly arrive?
16. What does `with open(path, "wb") as file:` manage?
17. What does `next(item.result for item in response.output if ...)` select?
18. Which value selects a deployed model in `responses.create(...)`?
19. Which value selects the Content Understanding extraction configuration?
20. A snippet creates a credential and client correctly but prints `client.output_text`. What should it likely print instead?
21. A Language request passes `text` instead of `[text]`. What category of mistake is this?
22. A Content Understanding snippet calls `begin_analyze` and immediately accesses `.contents`. What line is missing?
23. A second agent request should remember the first turn. Name one valid state mechanism.
24. What is the universal flow you should reconstruct before worrying about incidental Python syntax?

## Answer key

1. The `DefaultAzureCredential` class from the `azure.identity` module/package.
2. Construct a typed Azure SDK client using named endpoint and credential arguments.
3. `os.environ[...]` treats the variable as required and fails when missing; `os.getenv(...)` can return `None` or a supplied default.
4. It creates a one-item list so the text is sent through a batch-shaped API.
5. It selects the first per-document result.
6. JSON-like request objects represented by Python dictionaries.
7. `.get("Total")` looks up a dictionary entry; `.value` reads a property from the returned SDK field object.
8. A missing or empty contents collection.
9. `foreach (var entity in result.Entities)`.
10. A Python type annotation saying the local is expected to be an `AnalysisResult`.
11. A poller for a long-running operation.
12. `result = poller.result()`.
13. Imports from an `.aio` package plus `async def`, `async with`, and `await`.
14. The Speech SDK returns its own future-like object, and the shown pattern waits by calling `.get()`.
15. In the subscribed event callback, commonly through `evt.result.text`.
16. Opening and reliably closing a binary output file around the indented block.
17. The first `result` value from an output item that matches the filter condition.
18. The model deployment name supplied to `model=`.
19. The analyzer ID supplied to `analyzer_id=`.
20. A property on the response, such as `response.output_text`.
21. A collection-shape or batch-input mistake.
22. A line that waits for and retrieves the final result, such as `result = poller.result()`.
23. Reuse a conversation ID or explicitly chain with a previous response ID when supported by that client pattern.
24. `configuration → credential → client → request or operation → result → output`.

# Future study checkpoint

A later session can consider this asset successful when the learner can, without a general Python lesson:

1. read imports and identify the SDK family;
2. distinguish required and optional configuration access;
3. identify credential and client construction;
4. explain list construction, indexing, slicing, and nested dictionaries;
5. follow typed properties and dictionary fields in a result;
6. map conditions, loops, callbacks, and context managers to C# concepts;
7. distinguish immediate responses, pollers, true coroutines, and Speech SDK futures;
8. identify model deployment, agent, analyzer, input, state, and output values;
9. complete short missing-method or missing-property snippets; and
10. summarize the Azure workflow while ignoring incidental syntax.

# Official sources

- https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-901
- https://learn.microsoft.com/en-us/training/modules/get-started-with-generative-ai-and-agents/3-using-generative-ai-models
- https://learn.microsoft.com/en-us/training/modules/get-started-with-generative-ai-and-agents/4-creating-an-agent
- https://learn.microsoft.com/en-us/training/modules/get-started-text-analysis-azure/3-language-sdk
- https://learn.microsoft.com/en-us/training/modules/get-started-speech-azure/2-speech-recognition
- https://learn.microsoft.com/en-us/training/modules/get-started-speech-azure/3-speech-synthesis
- https://learn.microsoft.com/en-us/training/modules/get-started-vision-azure/2-vision-enabled-models
- https://learn.microsoft.com/en-us/training/modules/get-started-vision-azure/3-image-generation
- https://learn.microsoft.com/en-us/training/modules/get-started-information-extraction/4-exercise
- https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code
- https://learn.microsoft.com/en-us/python/api/overview/azure/ai-projects-readme
- https://learn.microsoft.com/en-us/python/api/overview/azure/ai-contentunderstanding-readme?view=azure-python
- https://learn.microsoft.com/en-us/azure/ai-services/content-understanding/quickstart/use-rest-api
- https://docs.python.org/3/tutorial/controlflow.html
- https://docs.python.org/3/tutorial/datastructures.html
- https://docs.python.org/3/tutorial/inputoutput.html
- https://docs.python.org/3/reference/compound_stmts.html
- https://docs.python.org/3/library/os.html
- https://docs.python.org/3/library/functions.html

## Related gap research

- [`../../research/gaps/python-sdk-recognition-csharp-developer.md`](../../research/gaps/python-sdk-recognition-csharp-developer.md)