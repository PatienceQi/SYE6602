# SYE6602 Course Assistant

A Make.com workflow for course revision: answer course questions, generate quizzes, and judge submitted answers.

## What is included

- Architecture and data-flow notes.
- DeepSeek prompt contracts.
- Make configuration and verification gotchas.
- A redacted historical blueprint example.

## What is not included

Course PDFs, extracted lecture text, credentials, webhook URLs, local MCP state, and live Make resource identifiers are intentionally excluded. Upload only material that you are licensed to use in your own Make Knowledge space.

## Architecture

```text
Custom Webhook
  → deterministic length validation
  → DeepSeek intent classification
  → JSON parsing
  → intent Router
       ├─ answer  → Knowledge search → DeepSeek answer → Webhook response
       ├─ quiz    → Knowledge search → DeepSeek questions → Data Store → response
       ├─ submit  → Data Store → DeepSeek judge → Data Store → response
       └─ unrelated → course-related fallback
```

Canonical intent labels:

```text
answer | quiz | submit | unrelated
```

Example Data Store keys:

```text
current_quiz
r_{question_id}
```

## Deployment outline

1. Create or select your own Make connections, Knowledge files, Data Store, and Data Structures.
2. Discover module identifiers with Make's module-listing tools.
3. Configure and validate each module.
4. Validate the complete blueprint.
5. Deploy to a private test scenario.
6. Run a real JSON `POST` through your own Webhook URL.
7. Inspect module execution, filters, HTTP response, and Data Store state.

Do not paste credentials or bearer URLs into blueprints, screenshots, issues, or commits.

## Verification checklist

- Normal course question.
- Empty, too-short, and overlong message.
- Unrelated question.
- DeepSeek failure or malformed JSON.
- Quiz generation followed by answer submission in a later run.

HTTP 200 or a successful Make execution is not enough. Check actual output and persisted state.

## Repository guidance

See [`AGENTS.md`](AGENTS.md) for AI-assistant-oriented repository conventions.

## License

MIT. Course materials and third-party content are not included and remain subject to their original licenses.
