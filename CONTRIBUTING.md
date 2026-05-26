# Contributing

## Principles

- Do not alter workflow logic in documentation-only changes.
- Do not add real credentials, tokens, IDs, webhooks, or secrets.
- Do not remove security placeholders.
- Keep examples fictional and safe to publish.
- Document operational behavior based on `workflow/workflow.sanitized.json`.

## Local Workflow

1. Create a branch from `main`.
2. Make focused changes.
3. Review the diff for sensitive data.
4. Confirm `workflow/workflow.sanitized.json` was not modified unless the task explicitly requires it.
5. Open a Pull Request with a clear summary and security checklist.

## Documentation Guidelines

- Keep `README.md` short and use it as the multilingual hub.
- Use `docs/pt-br/README.md` as the most complete documentation source.
- Keep `docs/en`, `docs/fr`, and `docs/es` aligned as professional translations.
- Use `templates/env.example` for placeholders only.
- Put screenshots in `assets/screenshots/` only after checking that no private data is visible.
- Put diagrams in `assets/diagrams/`.

## Pull Request Checklist

- [ ] Workflow logic is unchanged.
- [ ] No real credentials or secrets were added.
- [ ] No sensitive data was added to examples, docs, screenshots, or diagrams.
- [ ] Documentation matches the sanitized workflow.
- [ ] Security notes were updated if new risks were found.
