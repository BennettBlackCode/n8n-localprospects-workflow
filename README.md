# LocalProspects.ai n8n Workflow

This repo contains a public n8n workflow template for finding local businesses with the LocalProspects.ai API, polling the enrichment job, splitting the returned leads, and passing each lead to an OpenAI step for personalized email copy.

## Import

1. Open n8n.
2. Choose **Import from File**.
3. Select `workflows/localprospects-lead-email-generator.json`.
4. Configure the credentials and placeholders listed below.

## Required Setup

Before running the workflow, update these values inside n8n:

- `x-api-key` header in the LocalProspects HTTP nodes: add your LocalProspects.ai API key.
- OpenAI credentials on the `Write personalized email` node.
- The final storage node: connect it to your own n8n Data Table, Google Sheet, CRM, or database.

The workflow form asks for:

- `keyword`, for example `electrician`.
- `location_code`, for example `1012938` for Denver, Colorado.

You can find location codes with:

```bash
curl "https://localprospects.ai/api/v1/locations?q=denver" \
  -H "x-api-key: YOUR_LOCALPROSPECTS_API_KEY"
```

## Workflow Shape

1. Form submission collects a niche keyword and location code.
2. LocalProspects.ai starts the search.
3. n8n waits, then polls the job status.
4. If the job is not complete, the workflow waits and checks again.
5. Once complete, n8n fetches enriched results.
6. Each result is split into its own item.
7. OpenAI drafts a personalized outreach email.
8. The final node is a placeholder for saving the lead and email draft.

## Notes

This template intentionally does not include private API keys, n8n webhook IDs, n8n instance IDs, OpenAI credential IDs, or personal data-table IDs.

