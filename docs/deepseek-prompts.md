# DeepSeek Prompt Contracts

Use your own API connection and credentials. Never commit them.

## Intent classification

Input: the user's original message.

Output only JSON:

```json
{"intent":"answer|quiz|submit|unrelated","question":"normalized question or topic"}
```

Rules:

- `answer`: course knowledge question.
- `quiz`: request for questions; use a non-empty fallback topic when no topic is supplied.
- `submit`: answer submission.
- `unrelated`: outside the course scope.

## Answer

Provide retrieved course context and the normalized question. Instruct the model to answer only from supplied context. If context is insufficient, say so instead of inventing facts.

## Quiz

Provide retrieved course context. Require five single-choice questions, four options per question, one correct answer, and strict JSON without Markdown fences.

```json
{"questions":[{"question":"...","options":["A","B","C","D"],"answer":"B"}]}
```

Make generates identifiers and stores the answer key separately from display text.

## Judge

Provide the question, options, correct answer, and one submitted option. Require strict JSON:

```json
{"is_correct":true,"explanation":"..."}
```

Validate model output before using it. A syntactically valid but semantically wrong label is not caught by JSON parsing.
