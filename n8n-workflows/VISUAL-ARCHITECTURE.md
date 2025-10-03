# Visual Workflow Architecture

## Complete System Overview

```
┌─────────────────────────────────────────────────────────────────────┐
│                    B2B SDR AUTOMATION SYSTEM                         │
│                         (n8n Powered)                                │
└─────────────────────────────────────────────────────────────────────┘

                          [HUMAN: Strategic Decisions]
                                     │
                                     ▼
┌────────────────────────────────────────────────────────────────────┐
│ STAGE 1: LEAD LIST BUILDING                                        │
│ Tools: SalesNav / Apollo                                           │
│ Action: Manual targeting, filters, export to CSV                   │
└───────────────────────────┬────────────────────────────────────────┘
                            │
                            ▼
┌────────────────────────────────────────────────────────────────────┐
│ 🤖 WORKFLOW #1: MULTI-SOURCE ENRICHMENT                            │
│ ├─ Input: CSV with names + domains                                 │
│ ├─ Process:                                                         │
│ │  ├─ Apollo API (emails, phones, firmographics)                   │
│ │  ├─ Hunter.io fallback (email validation)                        │
│ │  ├─ NeverBounce (confidence scoring)                             │
│ │  └─ Deduplication check                                          │
│ └─ Output: Enriched + validated CSV                                │
│ ⏱️  Time saved: 2-3 hours per 500 contacts                         │
└───────────────────────────┬────────────────────────────────────────┘
                            │
                            ▼
┌────────────────────────────────────────────────────────────────────┐
│ 🤖 WORKFLOW #2: CRM IMPORT & DEDUPLICATION                         │
│ ├─ Input: Enriched CSV                                             │
│ ├─ Process:                                                         │
│ │  ├─ Check Salesforce/HubSpot for duplicates                      │
│ │  ├─ Create new leads OR update existing                          │
│ │  ├─ Campaign assignment                                          │
│ │  ├─ Lead scoring                                                 │
│ │  └─ Territory/owner assignment                                   │
│ └─ Output: Clean CRM data                                          │
│ ⏱️  Time saved: 1-2 hours per import                               │
└───────────────────────────┬────────────────────────────────────────┘
                            │
                            ▼
        ┌───────────────────┴────────────────────┐
        │                                        │
        ▼                                        ▼
┌────────────────────────┐         ┌─────────────────────────────┐
│ 🤖 WORKFLOW #3:        │         │ 🤖 WORKFLOW #4:             │
│ EMAIL AUTO-ENROLL      │         │ LINKEDHELPER CSV OPTIMIZER  │
│                        │         │                             │
│ ├─ Trigger: New lead   │         │ ├─ Input: CRM campaign      │
│ │  in CRM campaign     │         │ ├─ Process:                 │
│ ├─ Process:            │         │ │  ├─ Export leads          │
│ │  ├─ Map CRM fields   │         │ │  ├─ Format for LH         │
│ │  ├─ Enroll in        │         │ │  ├─ Save to Dropbox       │
│ │  │  Outreach/Lemlist │         │ │  └─ Slack notification    │
│ │  ├─ Personalization  │         │ └─ Output: Ready CSV        │
│ │  └─ Schedule sends   │         │ ⏱️  Time saved: 20-30 min    │
│ └─ Output: Active      │         └──────────┬──────────────────┘
│    email sequences     │                    │
│ ⏱️  Time saved: 30-45m  │                    │ [MANUAL: Human uploads
└────────┬───────────────┘                    │  CSV to LinkedHelper VM]
         │                                     │
         │                                     ▼
         │                        ┌────────────────────────────┐
         │                        │   LINKEDHELPER VM          │
         │                        │   (LinkedIn Automation)    │
         │                        │                            │
         │                        │ ├─ Connection requests     │
         │                        │ ├─ Message sequences       │
         │                        │ ├─ Activity tracking       │
         │                        │ └─ Daily CSV export        │
         │                        └────────┬───────────────────┘
         │                                 │
         │                                 │ [Automated export]
         │                                 │
         │                                 ▼
         │                        ┌────────────────────────────┐
         │                        │  LinkedIn Activity CSV     │
         │                        │  (Daily at 11:00 PM)       │
         │                        └────────┬───────────────────┘
         │                                 │
         │                                 ▼
         │                        ┌────────────────────────────┐
         │                        │ 🤖 WORKFLOW #5:            │
         │                        │ LINKEDIN/EMAIL SYNC        │
         │                        │                            │
         │                        │ ├─ Watch for new CSV       │
         │                        │ ├─ Parse activities:       │
         │                        │ │  • Reply Received        │
         │                        │ │  • Meeting Scheduled     │
         │                        │ ├─ Match LinkedIn URL      │
         └────────────────────────┤   to CRM lead              │
                                  │ ├─ Update CRM status       │
                                  │ ├─ Pause email sequence ◄──┘
                                  │ └─ Slack alert to SDR      │
                                  │ ⏱️  Prevents double-touches │
                                  └────────┬───────────────────┘
                                           │
                                           ▼
                                  ┌────────────────────────────┐
                                  │  CRM: Status = "LinkedIn   │
                                  │  Reply" or "Meeting        │
                                  │  Scheduled"                │
                                  └────────┬───────────────────┘
                                           │
                                           ▼
                                  ┌────────────────────────────┐
                                  │  Calendly/Meeting Booked   │
                                  └────────┬───────────────────┘
                                           │
                                           ▼
                                  ┌────────────────────────────┐
                                  │ 🤖 WORKFLOW #6:            │
                                  │ MEETING HANDOFF            │
                                  │                            │
                                  │ ├─ Trigger: Calendly       │
                                  │ │  webhook                 │
                                  │ ├─ Process:                │
                                  │ │  ├─ Update CRM           │
                                  │ │  ├─ Create opportunity   │
                                  │ │  ├─ Assign to AE         │
                                  │ │  ├─ Slack notification   │
                                  │ │  ├─ Confirmation email   │
                                  │ │  └─ Calendar sync        │
                                  │ └─ Output: AE ready        │
                                  │ ⏱️  Time saved: 15-20 min   │
                                  └────────┬───────────────────┘
                                           │
                                           ▼
                              ┌────────────────────────────────┐
                              │ 🤖 WORKFLOW #7:                │
                              │ DAILY ACTIVITY REPORT          │
                              │                                │
                              │ ├─ Sources:                    │
                              │ │  ├─ Salesforce activities    │
                              │ │  ├─ Outreach metrics         │
                              │ │  └─ LinkedHelper CSVs        │
                              │ ├─ Process:                    │
                              │ │  ├─ Aggregate metrics        │
                              │ │  ├─ Calculate rates          │
                              │ │  └─ Generate dashboard       │
                              │ └─ Output: Google Sheets       │
                              │    + Email/Slack summary       │
                              │ ⏱️  Replaces 1 hour manual work │
                              └────────────────────────────────┘
```

---

## Data Flow Diagram

```
┌──────────────┐
│  Lead List   │ (SalesNav/Apollo)
│  (Manual)    │
└──────┬───────┘
       │
       ▼
┌──────────────┐
│ Enrichment   │ 🤖 Workflow #1
│  + Validation│
└──────┬───────┘
       │
       ▼
┌──────────────┐
│   CRM Import │ 🤖 Workflow #2
│      +       │
│  Dedup Check │
└──────┬───────┘
       │
       ├──────────────────────────────────────────┐
       │                                          │
       ▼                                          ▼
┌──────────────┐                          ┌──────────────┐
│ Email Seq    │ 🤖 Workflow #3           │ LH CSV Prep  │ 🤖 Workflow #4
│  Enrollment  │                          │  + Upload    │
└──────┬───────┘                          └──────┬───────┘
       │                                          │
       │                                          │ (Manual upload)
       │                                          ▼
       │                                   ┌──────────────┐
       │                                   │ LinkedHelper │
       │                                   │ Automation   │
       │                                   └──────┬───────┘
       │                                          │
       │                                          │ (Daily export)
       │                                          ▼
       │                                   ┌──────────────┐
       │                                   │ Activity CSV │
       │                                   └──────┬───────┘
       │                                          │
       │  ┌───────────────────────────────────────┘
       │  │
       │  │ 🤖 Workflow #5: Status Sync
       │  │
       ▼  ▼
┌────────────────┐
│ CRM Status     │
│ Updated        │
│ +              │
│ Email Paused   │ ◄── Prevents double-touch!
└────────┬───────┘
         │
         ▼
┌────────────────┐
│ Meeting Booked │
└────────┬───────┘
         │
         ▼
┌────────────────┐
│ AE Handoff     │ 🤖 Workflow #6
│  Automation    │
└────────┬───────┘
         │
         ▼
┌────────────────┐
│ Deal Pipeline  │
└────────────────┘
```

---

## Automation Coverage by Stage

| Stage | Task | Automation | Tool |
|-------|------|------------|------|
| 1 | ICP/Targeting | 0% Manual | Human Strategy |
| 2 | List Building | 20% Tool | SalesNav/Apollo |
| 3 | Data Export | 0% Manual | SalesNav/Apollo |
| 4 | Enrichment | **100% 🤖** | n8n Workflow #1 |
| 5 | Validation | **100% 🤖** | n8n Workflow #1 |
| 6 | CRM Import | **95% 🤖** | n8n Workflow #2 |
| 7a | Email Enroll | **100% 🤖** | n8n Workflow #3 |
| 7b | LinkedIn Enroll | 80% 🤖 | n8n Workflow #4 + Manual |
| 8 | Response Sync | **90% 🤖** | n8n Workflow #5 |
| 9 | Meeting Handoff | **90% 🤖** | n8n Workflow #6 |
| 10 | Reporting | **95% 🤖** | n8n Workflow #7 |

**Overall Automation: ~65%**

---

## Critical Integration Points

### 1. LinkedIn URL as Primary Key
```
SalesNav → Apollo → Salesforce → LinkedHelper → n8n
              ↓          ↓            ↓           ↓
         linkedin_url (MUST be consistent across all systems)
```

**Why**: Only field that reliably exists in all tools

### 2. Status Field in CRM
```
┌─────────────────────────────────────────┐
│ Salesforce Custom Field:                │
│ LinkedIn_Status__c (Picklist)           │
│                                         │
│ Values:                                 │
│  • Not Started                          │
│  • In Progress                          │
│  • Connection Accepted                  │
│  • Replied                              │
│  • Meeting Scheduled                    │
│  • Closed Won                           │
│  • Closed Lost                          │
└─────────────────────────────────────────┘
         │
         ├─► Triggers Workflow #5 (status sync)
         └─► Pauses Workflow #3 (email sequences)
```

### 3. File Exchange Pattern (LinkedHelper Workaround)
```
┌──────────┐                    ┌──────────┐
│   n8n    │  CSV (formatted)  │Dropbox/  │
│Workflow#4├───────────────────>│ Drive    │
└──────────┘                    └────┬─────┘
                                     │
                              [Manual Download]
                                     │
                                     ▼
                              ┌──────────┐
                              │LinkedHlpr│
                              │   VM     │
                              └────┬─────┘
                                   │
                            [Auto Export Daily]
                                   │
                                   ▼
                              ┌──────────┐
                              │Activity  │
                              │  CSV     │
                              └────┬─────┘
                                   │
                            [n8n File Watch]
                                   │
                                   ▼
                              ┌──────────┐
                              │   n8n    │
                              │Workflow#5│
                              └──────────┘
```

---

## Time Savings Breakdown

### Per Workflow (Daily Savings)

| Workflow | Task | Time Before | Time After | Saved |
|----------|------|-------------|------------|-------|
| #1 | Enrichment | 2-3 hours | 5 minutes | **2.5h** |
| #2 | CRM Import | 1-2 hours | 5 minutes | **1.5h** |
| #3 | Email Enroll | 45 minutes | 2 minutes | **43m** |
| #4 | LH CSV Prep | 30 minutes | 5 minutes | **25m** |
| #5 | Status Sync | 60 minutes | 10 minutes | **50m** |
| #6 | Meeting Handoff | 20 min/mtg | 2 min/mtg | **18m** |
| #7 | Daily Reporting | 60 minutes | 0 minutes | **60m** |

**Total Daily Savings: ~6+ hours per SDR**

### Scaled Impact (Team of 5 SDRs)

- **Daily**: 30+ hours saved
- **Weekly**: 150+ hours saved  
- **Monthly**: 600+ hours saved
- **Yearly**: 7,200+ hours saved

**At $30/hour SDR cost: $216,000/year in productivity gains**

---

## Decision Tree: Which Workflow to Build First?

```
START HERE
    │
    ▼
Do you spend >2 hours/day on data enrichment?
    │
    ├─YES──> BUILD WORKFLOW #1 FIRST ✅
    │         (Highest ROI, easiest to test)
    │
    └─NO───> Do you struggle with CRM duplicates/data entry?
               │
               ├─YES──> BUILD WORKFLOW #2 FIRST
               │         (Quality improvement focus)
               │
               └─NO───> Are you doing multi-channel (Email + LinkedIn)?
                          │
                          ├─YES──> BUILD WORKFLOW #5 FIRST
                          │         (Prevents double-touches)
                          │
                          └─NO───> Start with full Phase 1
                                    (Workflows #1 + #2)
```

---

## Success Metrics Dashboard

Track these metrics to measure ROI:

```
┌─────────────────────────────────────────────────┐
│ SDR AUTOMATION DASHBOARD                        │
├─────────────────────────────────────────────────┤
│                                                 │
│ TIME SAVINGS                                    │
│  Daily Admin Time: [Before] 3h → [After] 45m   │
│  Campaign Launch: [Before] 2h → [After] 30m    │
│  Meeting Handoff: [Before] 20m → [After] 2m    │
│                                                 │
│ QUALITY METRICS                                 │
│  Duplicate Creation: <2% ✅                     │
│  Email Validation: 95%+ ✅                      │
│  Multi-channel Sync: 95%+ ✅                    │
│                                                 │
│ WORKFLOW HEALTH                                 │
│  Workflow #1 (Enrichment): ✅ Running           │
│  Workflow #2 (CRM Import): ✅ Running           │
│  Workflow #3 (Email Enroll): ✅ Running         │
│  Workflow #4 (LH CSV): ✅ Running               │
│  Workflow #5 (Status Sync): ⚠️  95% accuracy   │
│  Workflow #6 (Meeting Handoff): ✅ Running      │
│  Workflow #7 (Reporting): ✅ Running            │
│                                                 │
│ ERROR TRACKING                                  │
│  Today's Errors: 3                              │
│  Week's Errors: 12                              │
│  Most Common: Unmatched LinkedIn URL (5)        │
│                                                 │
└─────────────────────────────────────────────────┘
```

---

## Next Steps

1. ✅ Review this visual overview
2. ➡️ Read START-HERE.md for implementation guide
3. ➡️ Choose your tech stack scenario
4. ➡️ Start with Phase 1 (Workflows #1 + #2)
5. ➡️ Measure, iterate, scale

**Questions?** All details are in `/documentation/` folder.
