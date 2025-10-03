# n8n Workflow Templates

This folder will contain exportable n8n workflow JSON files.

## Workflow Naming Convention

`[Number]-[Name]-[TechStack].json`

Examples:
- `01-enrichment-pipeline-salesforce.json`
- `01-enrichment-pipeline-hubspot.json`
- `05-linkedin-email-sync-outreach.json`
- `05-linkedin-email-sync-lemlist.json`

## Status

🚧 Workflows will be added during implementation phase

## Planned Workflows

### Phase 1: Foundation
- [ ] 01-multi-source-enrichment.json
- [ ] 02-crm-import-salesforce.json
- [ ] 02-crm-import-hubspot.json

### Phase 2: Outreach
- [ ] 03-outreach-auto-enroll.json
- [ ] 03-lemlist-auto-enroll.json

### Phase 3: LinkedIn Integration
- [ ] 04-linkedhelper-csv-optimizer.json
- [ ] 05-linkedin-email-sync-outreach.json
- [ ] 05-linkedin-email-sync-lemlist.json

### Phase 4: Intelligence
- [ ] 06-meeting-handoff-automation.json
- [ ] 07-daily-activity-report.json

## Usage

1. Download desired workflow JSON
2. Import into your n8n instance
3. Configure credentials (API keys, webhooks)
4. Adjust field mappings for your CRM schema
5. Test with small data set
6. Deploy to production

## Contributing

If you build improved versions:
1. Export from n8n
2. Remove any sensitive credentials
3. Add to this folder with descriptive name
4. Update this README with description