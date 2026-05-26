# Security Policy

## Scope

This policy applies to this repository, the n8n workflow export, documentation, examples, templates, screenshots, and diagrams.

## Secret Handling

Do not commit or publish:

- Real OAuth credentials.
- API keys, tokens, webhook URLs, client secrets, refresh tokens, or passwords.
- Real Gmail inbox data, sender addresses, document contents, invoices, customer information, or financial records.
- Real Google Drive folder IDs or Google Sheets document IDs unless intentionally public and approved.
- Unsanitized n8n workflow exports.

Use placeholders in documentation and templates. Configure real values only inside n8n, a secret manager, or a secure deployment environment.

## Workflow Safety

- Keep `workflow/workflow.sanitized.json` sanitized.
- Do not remove security placeholders.
- Do not add real credential IDs to exported workflow files.
- Review workflow exports before committing because n8n files can contain pinned data, credential references, document IDs, folder IDs, and sample execution outputs.

## Reporting Security Issues

Report sensitive data exposure or automation security issues privately to the repository maintainer. Include:

- File and section where the issue appears.
- Type of risk, such as credential, document ID, customer data, or execution output.
- Suggested remediation, if known.

Do not open public issues containing secrets or private document identifiers.

## Review Checklist

Before merging changes:

- [ ] No real credentials were added.
- [ ] No tokens, IDs, webhooks, or secrets were invented.
- [ ] Placeholders remain in place.
- [ ] Workflow logic was not changed unless explicitly required.
- [ ] Examples use fictional data only.
- [ ] Screenshots do not expose inboxes, file names, document contents, IDs, or account details.
