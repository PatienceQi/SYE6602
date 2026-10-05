# Course Assistant Architecture

## Purpose

The assistant helps students revise course material through a single Make.com scenario:

- answer a course question;
- generate five multiple-choice questions;
- judge one submitted answer and save the result.

Course documents belong in the operator's own Make Knowledge space. This repository does not redistribute course material.

## Flow

```text
Webhook(message)
  → Router #1: length validation
  → DeepSeek: classify intent
  → Parse JSON
  → Router #2: answer / quiz / submit / unrelated
```

### Answer

```text
Knowledge search(question)
  → DeepSeek answer(context + question)
  → Webhook response
```

### Quiz

```text
Knowledge search(topic)
  → DeepSeek generates five questions
  → Data Store: current_quiz
  → Webhook response
```

The model returns question content, options, and answer keys. Make owns question identifiers.

### Submit

```text
Data Store: get current_quiz
  ├─ missing → 404 response
  └─ found → DeepSeek judge → Data Store: r_{question_id} → response
```

Use a clean answer string for judging. Do not embed stored question JSON directly into a new request body.

## Contracts

Intent labels:

```json
{"intent":"answer|quiz|submit|unrelated","question":"normalized question or topic"}
```

Quiz payload:

```json
{
  "questions": [
    {
      "question": "Question text",
      "options": ["Option A", "Option B", "Option C", "Option D"],
      "answer": "B"
    }
  ]
}
```

Judge result:

```json
{"is_correct":true,"explanation":"Short explanation"}
```

## Operational constraints

Search modules can emit multiple bundles and multiply downstream operations. If the design needs only the first relevant result, stop after the response with Make's commit module. Validate actual outputs and stored records after every deployment.
