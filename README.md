# LocalProspects.ai n8n Workflow

This repo contains a public n8n workflow template for finding local businesses with the LocalProspects.ai API, polling the enrichment job, turning the returned results into one item per lead, and passing each lead to an OpenAI step for personalized SEO outreach email copy.

## Import

1. Open n8n.
2. Choose **Import from File**.
3. Select `workflows/localprospects-lead-email-generator.json`.
4. Configure the credentials and placeholders listed below.

## Required Setup

Before running the workflow, update these values inside n8n:

- `x-api-key` header in the LocalProspects HTTP nodes: add your LocalProspects.ai API key.
- OpenAI credentials on the `Write personalized email` node.
- The final placeholder node: replace it with your own n8n Data Table, Google Sheet, CRM, email tool, or database step.

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
4. If the job is still running, the workflow waits and checks again.
5. If the job fails, the workflow stops with a clear error instead of polling forever.
6. Once complete, n8n fetches enriched results.
7. A Code node normalizes the API response and returns one n8n item per lead.
8. OpenAI drafts one personalized SEO outreach email per lead using the full lead JSON.
9. The final node is a placeholder for saving or sending the lead and email draft.

The OpenAI prompt is intentionally generic and safe for a public template. It offers a practical local SEO tune-up covering Google Business Profile cleanup, service/location pages, technical SEO fixes, schema, reviews, and conversion improvements. It also tells the model not to invent facts and to use only fields present in the scraped lead data.

## Notes

This template intentionally does not include private API keys, n8n webhook IDs, n8n instance IDs, OpenAI credential IDs, or personal data-table IDs.
