# Repository Guidelines

## Project Overview

This repository is a public, redacted reference implementation for a SYE6602 course-revision workflow built on Make.com. It documents scenario design, prompt contracts, deployment checks, and QA. Make.com is the runtime; no local application service is included.

## Architecture & Data Flow

The workflow follows:

`Custom Webhook` → deterministic length validation → DeepSeek intent classification → JSON parsing → intent Router.

- `answer`: Knowledge search → DeepSeek answer → Webhook response.
- `quiz`: Knowledge search → DeepSeek generates five four-option questions → Data Store writes `current_quiz` → response.
- `submit`: read `current_quiz` → DeepSeek judges one answer → Data Store writes `r_{question_id}` → response.
- `unrelated`: course-related fallback response.
- Module-level error handlers return a fallback and end with `Commit`.

## Key Directories

- `docs/`: architecture, prompts, Make guidance, and verified gotchas.
- `examples/`: redacted blueprint examples only.
- Course files, credentials, local runtime state, and live resource identifiers are intentionally excluded.

## Development Commands

No local build, package manager, lint, typecheck, or test command exists. Work happens in Make.com.

Typical lifecycle:

```text
Discover modules → configure → validate modules → validate blueprint
→ deploy private test scenario → real webhook request → inspect execution and state
```

## Code Conventions & Common Patterns

- Model workflows as Make modules, routers, filters, mappings, and error-handler routes.
- Put deterministic validation before model calls.
- Use mutually exclusive route filters or an explicit fallback.
- Discover module names; never invent identifiers.
- Run a module before mapping its output.
- Use strict JSON contracts and `json:CreateJSON` for arbitrary text entering an HTTP body.
- Attach errors to the module that can fail.
- Generate question IDs in Make, not in the model.
- Check actual response data and Data Store state, not only execution status.

## Runtime/Tooling Preferences

- Runtime: hosted Make.com.
- Integration: Make MCP tools and Make-specific skills.
- Package manager: none.
- Use private test resources and your own credentials.
- Never commit `.env`, `.mcp.json`, `.rivet/`, tokens, API keys, private keys, passwords, webhook URLs, or live Make resource IDs.

## Testing & QA

QA is manual end-to-end testing through a real Webhook request. Cover:

1. Normal course question.
2. Invalid boundary input.
3. Unrelated question.
4. DeepSeek failure or malformed JSON.
5. Quiz generation followed by a later-run answer submission.

For each case, inspect HTTP status, response body, executed modules and filters, and persisted Data Store state. Reset stateful fixtures and keep one request in flight.
