# Security Notes

During repository inspection, the workflow export appeared sanitized for credentials, but a few references should be reviewed before production use or wider publication.

## Potential Risks Found

- The Google Sheets node contains a spreadsheet URL or document identifier that appears environment-specific.
- The Google Sheets node contains a sheet/tab identifier and cached sheet name that appear environment-specific.
- Workflow metadata includes an n8n instance identifier.
- Pinned data for the document analysis node includes invoice-like sample output, including supplier-style text, invoice number, amount, tax, category, and suggested filename.
- Some credential placeholders reuse a generic placeholder value where n8n normally stores credential IDs. This is safer than real credentials, but should still be replaced only inside n8n after import.

## Recommended Remediation

- Replace environment-specific Google Sheets references with safe placeholders in a future workflow sanitization pass.
- Remove pinned data from exported workflows before sharing externally.
- Keep real IDs, folder references, and sheet URLs in n8n credentials/configuration rather than public JSON exports.
- Re-export the workflow after sanitization and verify it contains no real execution data.

No real values are repeated in this note.
