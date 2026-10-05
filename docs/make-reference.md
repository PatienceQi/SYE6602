# Make Reference

## Core concepts

- **Scenario**: a visual workflow.
- **Module**: one operation in a workflow.
- **Bundle**: data passed between modules.
- **Mapping**: an upstream value selected for a downstream field.
- **Router**: independent routes selected by filters.
- **Data Store**: cross-run key-value state.
- **Webhook**: an HTTP trigger.

## Build workflow

1. Clarify trigger, systems, data, and expected outputs.
2. Discover applications and module identifiers through Make's tools.
3. Configure each module from left to right.
4. Run modules before mapping their output fields.
5. Validate each module.
6. Validate the complete blueprint.
7. Deploy to a private scenario.
8. Activate and run a real webhook request.
9. Inspect execution details, response data, and persisted state.

## Safety rules

- Do not place credentials in text fields, blueprints, screenshots, or documentation.
- Treat webhook URLs as bearer credentials.
- Do not use live production data for demonstrations.
- Keep a working scenario copy before risky changes.
- Do not treat HTTP 200 or Make success status as proof of correct business output.
- Use `json:CreateJSON` when arbitrary retrieved text enters an HTTP request body.
- Put error handlers on the failing module, not on an unrelated downstream module.

## Sharing

Before sharing a scenario or repository:

1. Delete test data and personal information.
2. Replace live IDs and URLs with placeholders.
3. Confirm every connection belongs to the recipient's own account.
4. Test the redacted copy with safe sample data.
