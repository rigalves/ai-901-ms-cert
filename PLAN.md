# AI-901 Study Plan

## Goal

Prepare to pass AI-901 with genuine working knowledge of the official objectives by approximately **August 31, 2026**.

## Constraints and preferences

- Study target: **5 hours per week**.
- Typical session: **1 focused hour**.
- Sessions may occur on any day; there is no fixed weekday schedule.
- The user is a senior C# developer with very limited Python experience.
- Python work should be limited to reading, running, and slightly modifying exam-relevant SDK examples.
- The user transcribes AI-generated summaries by hand. Summaries should therefore be compact, structured, and useful rather than exhaustive.
- The AI recommends the next topic; the user chooses whether to follow the recommendation.
- Use a personal Azure subscription and Microsoft Learn sandboxes for hands-on work.
- Prefer free official resources. Paid practice material is not part of the default plan.

## Evidence-driven priorities

The official blueprint is centered on two domains:

1. Identify AI concepts and capabilities: 40–45%.
2. Implement AI solutions by using Microsoft Foundry: 55–60%.

Preparation must therefore move beyond conceptual definitions into portal, workflow, SDK, prompt, agent, speech, vision, text-analysis, and information-extraction recognition.

## Study phases

### Phase 1: Finish the concepts learning path

Complete the remaining concept modules using current Microsoft Learn pages.

For each topic:

1. Verify the official objective and current Learn page.
2. Use the prepared concise official-source summary.
3. Review the existing exam-readiness gap file and its prepared study assets, when available.
4. Complete the Microsoft module knowledge check.
5. Use a topic-appropriate exit check when useful.

After the concepts path is complete, take the official AI-901 Practice Assessment as a baseline diagnostic.

### Phase 2: Complete the Foundry implementation learning path

This is the higher-weighted phase.

For each module:

1. Preview the official objective.
2. Complete the current Microsoft Learn exercise.
3. Inspect the prepared Python SDK example.
4. Map the workflow to familiar C# concepts when useful.
5. Explain the end-to-end flow without relying on copied text.
6. Keep a minimal working lab only when it adds exam value.

Core workflow to recognize:

`resource → deployment/tool → authentication → client → request → response`

### Phase 3: Diagnose and repair

Use the official Practice Assessment and trustworthy original practice questions.

For each missed item, identify whether the problem was:

- missing knowledge
- confusion between similar Microsoft services or capabilities
- unfamiliar portal or SDK workflow
- misreading the scenario or code

Repair the matching official objective, then retest with new questions.

### Phase 4: Final readiness

Use mixed scenario questions, service comparisons, short code-reading exercises, and focused review of weak areas.

A practical readiness signal is:

- all official objectives can be explained with an example
- implementation flows are recognizable
- short Python SDK snippets no longer cause confusion
- recent mixed assessments are consistently strong without answer memorization
- no major unresolved exam-readiness gaps remain

Any score threshold used for readiness is a study heuristic, not a conversion of Microsoft’s scaled passing score.

## Session types

Do not force every session into one generic flow.

- **Concept study session:** prepared official summary → distinctions/scenarios → knowledge check.
- **Implementation study session:** objective preview → exercise/lab → code reading → workflow explanation.
- **Gap-research session:** independently research gaps and produce or update reusable repository material for later study. Do not quiz the user, wait for answers, or assess mastery.
- **Assessment session:** timed questions → error analysis → targeted repair.
- **Review session:** mixed recall, scenario selection, and weak-area drills.

Gap-research sessions prepare future study sessions and do not count as completed Learn modules or demonstrated learner progress.

## Labs

Keep only a small set of exam-relevant labs in this repository. Likely candidates include:

- Foundry chat client
- prompt or single-agent solution
- text analysis
- speech
- vision or OCR
- Content Understanding

Each lab should contain the smallest useful code, a short README, the official objective it supports, and any important Python-to-C# mapping. Avoid turning the repository into a portfolio project.
