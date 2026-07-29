# Exam-Readiness Gap Research Plan

## Purpose

Create short, topic-specific research files that identify where official Microsoft learning material may be too brief, too conceptual, difficult to translate into exam performance, or missing useful practice.

Each gap-research session must also create or update the reusable documents needed by a later study session. The research session prepares the lesson; it does not conduct the lesson.

Normal study sessions should use this prepared material rather than launching broad web research.

## Session boundary

A gap-research session is a non-interactive repository-production session.

It should:

- research current evidence
- classify accepted and rejected gaps
- write or update the matching gap file
- create or update the topic summary, comparison, practice set, checkpoint, or lab instructions needed for later study
- include answer keys when it creates questions
- link the resulting materials together

It should not:

- quiz the user
- ask the user to explain the topic
- wait for answers
- assign work for completion inside the research session
- assess learner mastery
- update confirmed Learn progress
- silently turn into a concept, implementation, review, or assessment session

## Boundary of scope

The current official AI-901 blueprint defines the scope. Gap research may improve depth, examples, practice, terminology awareness, or implementation readiness, but it must not expand into unrelated Azure AI topics.

## Source hierarchy

1. Current official AI-901 study guide and exam page.
2. Current Microsoft Learn modules and exercises.
3. Current Microsoft product documentation, SDK quickstarts, REST documentation, and official samples.
4. Credible first-hand candidate experiences with identifiable dates and outcomes.
5. Reputable trainers or original practice providers that clearly avoid dumps.

Reject or ignore:

- brain dumps and leaked questions
- vendors claiming real or verified exam questions
- unsupported promotional pass-rate claims
- undated or clearly stale AI-900 advice presented as AI-901 guidance
- one-off anecdotes treated as universal exam distribution

## Planned topic files

Create one gap file for each meaningful official objective cluster, not necessarily every Learn unit.

Initial list:

1. Responsible AI and model concepts
2. Generative AI, prompts, and model configuration
3. Agentic AI and single-agent solutions
4. Text analysis and Azure Language
5. Speech recognition and synthesis
6. Computer vision, OCR, multimodal input, and image generation
7. Information extraction and Content Understanding
8. Foundry resources, projects, deployments, endpoints, and authentication
9. Foundry SDK chat and agent clients
10. Python SDK recognition for a C# developer

Merge or split topics only when the official blueprint or current learning-path structure makes that useful.

## Research session workflow

### 1. Establish official scope

- Open the current AI-901 study guide.
- Copy the exact relevant objective bullets into working notes.
- Open the matching current Microsoft Learn module and exercise.
- Record the verified official URLs.

### 2. Evaluate the official teaching coverage

Ask internally:

- Does Learn explain only what the capability is, or also how to use it?
- Are important implementation steps hidden in an exercise or linked quickstart?
- Does the module use current Foundry terminology?
- Does it show enough portal, SDK, authentication, request, and response flow for the official verbs?
- Are commonly confused services or capabilities compared clearly?
- Is practice provided for scenario selection or code recognition?

These are research prompts for the agent, not questions for the user.

### 3. Research external evidence

Search recent and trustworthy sources for:

- first-hand pass and failure experiences
- repeated reports of unexpected implementation or code emphasis
- portal, SDK, REST, CLI, prompt, agent, speech, vision, or extraction patterns
- differences between official practice and reported exam difficulty
- current trainer explanations or original practice material

Prefer independent corroboration. One candidate report is evidence of possibility, not frequency.

### 4. Classify each gap

Use one of these labels:

- **Missing:** relevant official objective with no meaningful teaching coverage found.
- **Underexplained:** present, but too brief for reliable understanding.
- **Implementation gap:** concept is taught, but practical flow is weak.
- **Terminology gap:** old and new Microsoft names or architectures may cause confusion.
- **Practice gap:** teaching exists, but realistic scenario/code practice is missing.
- **Reported exam emphasis:** several credible candidates report stronger emphasis than Learn suggests.
- **Unverified anecdote:** plausible but supported by only weak or isolated evidence.

### 5. Assign confidence

- **High:** official evidence plus multiple independent, current reports.
- **Medium:** multiple credible reports or strong official inference, but incomplete confirmation.
- **Low:** one credible report, beta-era evidence, or uncertain current relevance.
- **Rejected:** dump-derived, promotional, unverifiable, or outside official scope.

### 6. Produce the study-session assets

Every accepted gap must produce a concrete repository response, such as:

- a concise explanation based on current official sources
- a service or capability decision table
- a portal-to-client workflow
- a short Python snippet with C# mental mapping
- original scenario or code-recognition questions with an answer key
- a small checkpoint for a future study session
- minimal lab instructions tied to an official objective

Create the asset during the research session whenever practical. Do not merely tell the user to create or complete it.

Questions, exercises, and exit checks are content for a later study session. They must be stored, not administered, during gap research.

### 7. Complete the repository handoff

Before ending the gap-research session:

- save the gap analysis under `research/gaps/`
- create or update the relevant file under `docs/topics/`
- add supporting files only when they improve later study
- link the gap file and study material to each other
- update verified official links when applicable
- verify that answer keys match any generated questions
- mark research/material production as complete without making a learner-mastery claim
- report the changed files and commits

## Gap file format

```markdown
# Topic name — Exam-readiness gaps

## Official scope

## Official material reviewed

## Coverage assessment

## Gaps

### Gap title
- Type:
- Evidence:
- Confidence:
- Why it matters:
- Study material needed:

## Study-session assets created

## Claims not accepted

## Suggested future study checkpoint

## Sources
```

## Relationship to normal study sessions

- Gap research prepares the documents used by normal study sessions.
- A later study session reads the matching topic and gap files, then teaches, practices, or evaluates the material.
- Missing gap research should produce a warning, not block the official study session.
- The official summary remains based only on current Microsoft sources.
- The exam-readiness section may summarize accepted findings from the gap file.
- Broad external research should not occur during a normal one-hour study session.
- Only a study, review, or assessment session may ask the user questions or evaluate understanding.

## Relationship to the weekly exam-watch task

The weekly task is radar, not the research author.

It should identify:

- official blueprint or Learn changes
- important new candidate patterns
- relevant new videos or resources
- topics whose gap files may need review

A separate gap-research session decides whether evidence is strong enough to change the repository and produces the required study-session assets.
