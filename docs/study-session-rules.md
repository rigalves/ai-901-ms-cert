# Study Session Rules

These rules protect the study sequence and keep assessments fair.

## Viewed-material boundary

- Questions may cover only material the learner has already viewed.
- Confirmed completed units and topics are always eligible for review.
- Material from the current unit becomes eligible only after it has been explicitly taught, read, or reviewed during the current study session.
- A later unit, module, learning path, or implementation topic that has not been viewed must not appear in a question.
- Unviewed material must not appear indirectly in the question stem, answer choices, distractors, hints, follow-up explanations, or corrective feedback.
- Prepared summaries, gap-research documents, labs, question banks, and repository assets do not mean the learner has viewed the topic.
- Use `STATUS.md` and the current session record to determine the allowed question scope.
- When it is unclear whether a topic has been viewed, treat it as unviewed and do not ask about it.

## Quiz behavior

Before asking a knowledge-check, checkpoint, review, or assessment question:

1. Identify the learner's last confirmed completed unit in `STATUS.md`.
2. Add only concepts explicitly covered so far in the current session.
3. Exclude every later concept, even when it is related or appears in a plausible distractor.
4. Verify that the explanation after the answer also stays inside the viewed-material boundary.

The agent may preview the name of the next topic when recommending the study route, but it must not test the learner on that topic before the learner studies it.
