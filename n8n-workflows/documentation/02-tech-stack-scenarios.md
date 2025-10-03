# Tech Stack Scenarios

Different companies use different tool combinations. Here are the two most common scenarios with specific n8n workflow implementations.

---

## Scenario A: Enterprise Stack

**Tools**: SalesNav + Apollo + Salesforce + Outreach + LinkedHelper

### Workflow Sequence

**Step 1**: Manual - Build list in SalesNav
- Human strategic decision on targeting
- Export to CSV OR push to Apollo

**Step 2**: 🤖 n8n Workflow #1 - Multi-Source Enrichment
- Input: CSV with names + company domains
- Process:
  - Apollo API enrichment (emails, phones, firmographic data)
  - Hunter.io email validation (fallback if Apollo fails)
  - NeverBounce verification (confidence scoring)
  - Deduplication check against existing CRM data
- Output: Validated, enriched CSV with confidence scores
- Time saved: 2-3 hours per 500 contacts

**Step 3**: 🤖 n8n Workflow #2 - Salesforce Import
- Input: Enriched CSV from Workflow #1
- Process:
  - Check Salesforce for existing leads/contacts (by email + LinkedIn URL)
  - Create new leads if not found
  - Update existing records if found (append new data)
  - Assign to appropriate campaigns
  - Apply lead scoring rules
  - Set ownership (round-robin or territory-based)
- Output: Clean CRM data ready for sequences
- Time saved: 1-2 hours per import

**Step 4**: 🤖 n8n Workflow #3 - Outreach Auto-Enrollment
- Input: Salesforce campaign membership (new leads)
- Trigger: New lead added to campaign OR status changed to "Ready for Outreach"
- Process:
  - Map Salesforce fields → Outreach custom fields
  - Enroll in specified sequence
  - Set send schedule (timezone-aware)
  - Apply personalization tokens
- Output: Leads automatically in email sequences
- Time saved: 30-45 minutes per campaign

**Step 5**: 🤖 n8n Workflow #4 - LinkedHelper CSV Optimizer
- Input: Salesforce campaign members
- Process:
  - Export relevant fields
  - Format for LinkedHelper requirements:
    - LinkedIn URL (required)
    - First Name, Last Name
    - Custom Field 1, Custom Field 2 (for personalization)
    - Company Name, Title
  - Save to Dropbox/Google Drive designated folder
  - Send Slack notification: "CSV ready for LinkedHelper - 247 contacts"
- Output: Ready-to-upload CSV
- Time saved: 20-30 minutes per campaign

**Step 6**: Manual - LinkedHelper CSV Upload
- Human action: Download CSV, upload to LinkedHelper VM, start campaign
- Why manual: LinkedHelper has no API

**Step 7**: 🤖 n8n Workflow #5 - LinkedIn/Email Sequence Sync (CRITICAL)
- Input: 
  - Option A: Watch Dropbox/Drive folder for LinkedHelper daily activity CSVs
  - Option B: Poll Salesforce for manual status updates
- Process:
  - Parse LinkedHelper CSV for activities:
    - "Reply Received"
    - "Connection Accepted + Message Exchanged"
    - "Meeting Scheduled"
  - Match LinkedIn URL to Salesforce lead
  - Update Salesforce status field
  - Trigger Outreach API: Pause sequence for this lead
  - Send Slack alert to SDR: "LinkedIn activity detected for [Name] - Outreach paused"
- Output: Synchronized multi-channel outreach
- Impact: **Prevents embarrassing double-touches**
- Time saved: 1-2 hours per week checking manually

**Step 8**: 🤖 n8n Workflow #6 - Meeting Handoff Automation
- Input: Calendly webhook (meeting booked)
- Process:
  - Update Salesforce: Meeting date/time, mark as "Meeting Scheduled"
  - Create Opportunity (if qualified)
  - Assign to AE (based on territory or round-robin)
  - Send AE Slack notification with prospect context
  - Send prospect confirmation email with prep instructions
  - Add to AE's Google Calendar
- Output: Smooth SDR → AE handoff
- Time saved: 15-20 minutes per meeting

### Human-in-Loop Touchpoints
- List building strategy (Step 1)
- LinkedHelper CSV upload (Step 6)
- Response quality review (ongoing)
- Meeting qualification (Step 8)

### Total Automation Coverage: ~65%

---

## Scenario B: SMB Stack

**Tools**: Apollo + HubSpot + Lemlist + LinkedHelper

### Workflow Sequence

**Step 1**: Manual - Build list in Apollo
- Simpler than SalesNav, faster filtering
- Export to CSV

**Step 2**: 🤖 n8n Workflow #1 - Email Validation
- Input: Apollo CSV (already has emails)
- Process:
  - Hunter.io validation (confidence scoring)
  - NeverBounce verification
  - Phone number formatting (international standards)
- Output: Validated CSV
- Note: Simpler than Enterprise because Apollo already enriched
- Time saved: 1 hour per 500 contacts

**Step 3**: 🤖 n8n Workflow #2 - HubSpot Import
- Input: Validated CSV
- Process:
  - Check HubSpot for existing contacts (by email)
  - Create new contacts if not found
  - Create associated companies
  - Add to static lists
  - Apply contact properties (persona, industry, etc.)
- Output: Organized HubSpot contacts
- Time saved: 1 hour per import

**Step 4**: 🤖 n8n Workflow #3 - Lemlist Auto-Enrollment
- Input: HubSpot list membership
- Process:
  - Map HubSpot properties → Lemlist variables
  - Enroll in Lemlist campaign
  - Apply personalization (image personalization if configured)
- Output: Contacts in email sequences
- Note: Lemlist API simpler than Outreach
- Time saved: 20-30 minutes per campaign

**Step 5**: 🤖 n8n Workflow #4 - LinkedHelper CSV Export
- Input: HubSpot list
- Process:
  - Export with LinkedHelper-specific formatting
  - Save to designated folder
  - Slack notification
- Output: Ready-to-upload CSV
- Time saved: 15-20 minutes per campaign

**Step 6**: Manual - LinkedHelper Upload
- Same as Enterprise scenario

**Step 7**: 🤖 n8n Workflow #5 - Sequence Sync via HubSpot
- Input: 
  - LinkedHelper CSV export (daily)
  - OR HubSpot workflow triggers (manual property updates)
- Process:
  - Detect LinkedIn activity
  - Update HubSpot contact property: "LinkedIn Status"
  - Trigger Lemlist API: Remove from campaign
  - Slack alert to SDR
- Output: Synced multi-channel outreach
- Note: HubSpot workflows can help automate property updates
- Time saved: 1 hour per week

**Step 8**: 🤖 n8n Workflow #6 - Calendly → HubSpot Deal Creation
- Input: Calendly webhook
- Process:
  - Update contact: Lifecycle stage → "Opportunity"
  - Create deal in HubSpot
  - Assign owner
  - Send notification
- Output: Meeting booked, deal created
- Time saved: 10-15 minutes per meeting

### Key Differences from Enterprise
- Apollo replaces SalesNav (faster but less sophisticated)
- HubSpot workflows assist automation (built-in triggers)
- Lemlist API is simpler (less features, easier integration)
- Overall lower complexity

### Total Automation Coverage: ~60%

### Implementation Time
- Enterprise Stack: 3-4 weeks
- SMB Stack: 1-2 weeks

---

## Choosing Your Scenario

**Choose Enterprise Stack if**:
- Selling to Fortune 500/enterprise buyers
- Complex org charts (multi-threading required)
- Long sales cycles (6+ months)
- Need advanced SalesNav filtering
- Team size: 10+ SDRs

**Choose SMB Stack if**:
- Selling to SMB/Mid-market
- Simpler buyer journey
- Shorter sales cycles (1-3 months)
- Team size: 1-5 SDRs
- Budget-conscious

---

**Next**: See `03-implementation-roadmap.md` for build sequence.