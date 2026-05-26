# n8n Invoice Document Inclusion Automation

## Overview

This repository documents an n8n workflow exported as `workflow/workflow.sanitized.json`. The automation processes Gmail attachments, analyzes invoice documents with Google Gemini, uploads files to Google Drive, and appends structured records to Google Sheets.

The workflow file was not modified during this organization pass.

## Workflow Goal

Automate invoice intake from email attachments by detecting Gmail messages with files, extracting structured invoice data, storing the original file in Google Drive, and writing the extracted data to Google Sheets.

## Problem Solved

The workflow reduces manual work such as opening emails, downloading files, reading invoices, renaming documents, uploading files, and filling spreadsheets by hand.

## Architecture

The workflow follows this sequence:

1. `Gmail Trigger` monitors Gmail and downloads attachments.
2. `If` checks whether an attachment exists.
3. `Split Out` separates binary attachments.
4. `Loop Over Items` processes each attachment.
5. `Analyze document` uses Google Gemini to extract JSON.
6. `Edit Fields` normalizes the returned object.
7. `If1` checks whether the document is corporate.
8. `Merge` and `Merge1` combine structured data and binary data.
9. `Upload file` and `Upload file1` upload the file to Google Drive.
10. `Append row in sheet` writes the structured data to Google Sheets.

See [assets/diagrams/workflow-architecture.mmd](../../assets/diagrams/workflow-architecture.mmd).

## n8n Nodes Used

- `n8n-nodes-base.gmailTrigger`
- `n8n-nodes-base.if`
- `n8n-nodes-base.splitOut`
- `n8n-nodes-base.splitInBatches`
- `@n8n/n8n-nodes-langchain.googleGemini`
- `n8n-nodes-base.noOp`
- `n8n-nodes-base.set`
- `n8n-nodes-base.merge`
- `n8n-nodes-base.googleDrive`
- `n8n-nodes-base.googleSheets`

## Required Integrations

- Gmail OAuth2
- Google Gemini or Google PaLM API
- Google Drive OAuth2
- Google Sheets OAuth2

## Expected Configuration

Use `templates/env.example` as a placeholder reference. Real values must be configured only in n8n or a secure secret manager.

Expected configuration includes Gmail, Gemini/PaLM, Drive, and Sheets credentials, plus the target spreadsheet, sheet, and Drive folders.

## Import Steps

1. Open n8n.
2. Import `workflow/workflow.sanitized.json`.
3. Review all nodes before activation.
4. Reconnect local credentials for Gmail, Gemini/PaLM, Drive, and Sheets.
5. Replace spreadsheet, sheet, and folder references with environment-specific values.
6. Test with fictional attachments.
7. Activate only after validating outputs and logs.

## Credential Setup

Create credentials directly in n8n. Do not edit the sanitized JSON to insert secrets. Test each credential before enabling the workflow.

## Examples

- [Sample input](../../examples/sample-input.md)
- [Sample output](../../examples/sample-output.md)

## Limitations

- Output quality depends on document quality and model behavior.
- The prompt expects pure JSON; invalid model output can break downstream steps.
- Environment-specific spreadsheet or folder references must be reviewed after import.
- See [SECURITY_NOTES.md](../../SECURITY_NOTES.md) for potential sensitive references found during inspection.

## Next Steps

- Replace environment-specific references with safe placeholders.
- Add updated screenshots.
- Test with fictional corporate and personal invoices.
- Improve error handling for AI, Drive, and Sheets failures.
