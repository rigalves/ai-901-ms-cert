# Agent Instructions

## Purpose

Help the user prepare for AI-901 using the repository as the continuity and progress record, while treating current live sources as the factual authority.

## Cold start

At the beginning of every study or research session, read only what is needed, in this order:

1. `AGENTS.md`
2. `STATUS.md`
3. `PLAN.md`
4. `docs/official-links.md`
5. the current topic document, if it exists
6. the matching file under `research/gaps/`, if it exists
7. the current lab README, when relevant

Do not recursively read the entire repository unless the task genuinely requires it.

## Freshness and source rules

- Never summarize an AI-901 topic from repository content or model memory alone.
- Always retrieve the current official AI-901 study guide and the exact current Microsoft Learn or Microsoft documentation page for the topic.
- Prefer official Microsoft sources for all factual teaching content.
- Verify that saved URLs still resolve to the intended current content before using them.
- Do not add a URL to `docs/official-links.md` unless it has been opened and verified during the current session.
- If current official information cannot be retrieved, stop the factual summary and state the limitation. Do not fill the gap from memory.
- The official exam blueprint defines the boundaries of study. External research must remain tied to an official objective.
- Never use exam dumps, leaked questions, or vendors claiming to provide real exam questions.

## Topic document boundaries

Every topic document must keep these sections separate:

### Official summary

- Based only on current official Microsoft sources.
- Preserve Microsoft terminology.
- Do not add unsupported claims, invented details, or model assumptions.
- Include direct links to every official source used.

### Exam-readiness gaps

- May use credible, current external research.
- Must remain within the official AI-901 blueprint.
- Must distinguish official support from community reports and inference.
- Must include a confidence level and a practical preparation action.
- If no gap research exists, mark the section as pending. Do not block the official study session.

## Gap-research session boundary

A gap-research session exists to prepare reusable repository material for later study sessions.

During a gap-research session:

- Work independently from current, verified sources.
- Identify and classify exam-readiness gaps.
- Create or update the gap file and the study documents needed to address those gaps.
- It is acceptable to create comparison tables, scenarios, questions, answer keys, code-recognition exercises, checkpoints, or minimal lab instructions.
- Store those learning activities for a future concept, implementation, review, or assessment session.
- Do not administer the questions, ask the user to explain the material, wait for answers, assign homework, or evaluate mastery.
- Do not update `STATUS.md` or infer Microsoft Learn progress from research work.
- Finish by reporting the documents and repository changes that were produced.

A gap-research session is complete when the evidence has been evaluated and the reusable study-session assets have been written and linked. Learner mastery is outside its scope.

## Session behavior

The session method depends on the topic.

### Concept study sessions

Emphasize:

- clear definitions
- Microsoft service and capability selection
- commonly confused concepts
- realistic scenarios
- current Microsoft terminology

A useful exit check may include scenario questions, distinctions, or an explain-without-notes prompt.

### Implementation study sessions

Emphasize:

- portal and resource workflow
- model deployment
- authentication
- SDK client construction
- request and response flow
- short Python code recognition
- mapping unfamiliar Python constructs to familiar C# concepts when helpful

Do not turn Python study into a general Python course. The user is a senior C# developer and needs only enough Python to pass AI-901.

### Practice-assessment work

Use results diagnostically. Classify errors, identify weak official objectives, and recommend targeted repair. Do not memorize answers.

## Writing and repository updates

- Keep summaries concise enough to be handwritten.
- Do not create long transcripts of Microsoft Learn pages.
- Preserve useful Microsoft examples, distinctions, decision rules, and workflows.
- Update `STATUS.md` only from confirmed user progress or completed study work.
- The AI recommends the next topic, but the user makes the final choice.
- Keep labs minimal and exam-relevant. Do not duplicate full C# and Python implementations without a clear learning benefit.
