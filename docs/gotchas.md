# Verified Make Gotchas

- Run a module before mapping its output.
- Re-select mapping tokens after replacing an upstream module.
- HTTP response fields may be nested under `data`; inspect the actual output bundle.
- Make arrays use one-based mapping indexes in common expressions.
- Router routes are not automatically mutually exclusive; filters must enforce exclusivity.
- A fallback route catches an unmatched case; it is not the same as an explicit false condition.
- Error handlers catch failures from the module they are attached to.
- JSON parsing catches syntax errors, not semantically wrong model labels.
- Use explicit advanced HTTP settings required by the module schema.
- Do not embed stored JSON containing quotes directly into another JSON string.
- Use a JSON builder for arbitrary knowledge text with quotes or newlines.
- Search modules can emit many bundles and multiply operations.
- A successful webhook receipt may occur before downstream modules finish.
- Test webhook input through the real endpoint, not only through a scenario runner.
- Stateful tests require reset fixtures and deliberate handling of queued requests.
- Verify Data Store contents directly; response text alone does not prove persistence.
