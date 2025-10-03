# Day 5: Tech Stack Setup

> **Time Required:** 4-6 hours  
> **Goal:** Configure CRM, sequencer, and tracking systems for seamless execution

## 🎯 Overview

You have lists and messaging. Now you need to set up your systems so you can:
1. Send emails efficiently
2. Track all activity
3. Measure what's working
4. Stay organized as you scale

Think of this as building your "SDR command center."

## ✅ Day 5 Checklist

### Morning (2-3 hours): CRM Configuration

#### 1. Create List Views
- [ ] Create one view per segment (6 views total)
- [ ] Name convention: "[Segment Name] - Active"
- [ ] Set filters:
  - Segment tag = [Segment name]
  - Cadence Status = "Not Started" OR "In Progress"
  - Owner = You

**Example Filter Logic:**
```
Segment = "VP Sales - SaaS"
AND (Cadence Status = "Not Started" OR "In Progress")
AND Owner = [Your Name]
```

#### 2. Add Custom Fields

If not already created, add these fields to Contact/Lead object:

- [ ] **Segment** (Dropdown)
  - Options: All 6 segment names
- [ ] **List Date** (Date)
  - When contact was added to this campaign
- [ ] **Cadence Status** (Dropdown)
  - Options: Not Started, In Progress, Completed, Replied, Meeting Booked, DQ'd
- [ ] **Last Touch Date** (Date)
  - Last time you contacted them
- [ ] **Last Touch Type** (Dropdown)
  - Options: Email, Call, LinkedIn, Meeting
- [ ] **Touch Count** (Number)
  - How many times you've reached out
- [ ] **Response Status** (Dropdown)
  - Options: No Response, Objection, Interested, Not Interested

#### 3. Create Task/Activity Templates

- [ ] **Call task template** ("Call [Segment Name]")
- [ ] **Email task template** ("Send Email [#] to [Segment Name]")
- [ ] **Follow-up task template** ("Follow up on reply from [Name]")

#### 4. Build Reporting Dashboard

Create dashboard with these metrics:

**Daily Activity:**
- Emails sent today
- Calls made today
- Conversations (>2 min) today
- Meetings booked today

**Weekly Performance:**
- Total touches
- Reply rate %
- Conversation rate %
- Meeting conversion %

**By Segment:**
- Activity by segment
- Performance by segment
- Best performing segment (highest reply rate)

**CRM-Specific Instructions:**

<details>
<summary>Salesforce Setup</summary>

1. Create List Views:
   - Go to Leads/Contacts → List Views → New
   - Set filters as described above
   - Save view

2. Custom Fields:
   - Setup → Object Manager → Lead/Contact
   - Fields & Relationships → New
   - Create each field

3. Dashboard:
   - Dashboards → New Dashboard
   - Add components (reports)
   - Schedule email refresh
</details>

<details>
<summary>HubSpot Setup</summary>

1. Create List Views:
   - Contacts → Lists → Create List
   - Set filters as Active List
   - Save

2. Custom Properties:
   - Settings → Properties
   - Create property for each field
   - Add to form/view

3. Dashboard:
   - Reports → Dashboards → Create
   - Add reports
   - Set refresh frequency
</details>

### Afternoon (2-3 hours): Sequencer/Automation Setup

#### 5. Email Sequence Configuration

**If using Outreach/Salesloft/Apollo:**

- [ ] Create 6 sequences (one per segment)
- [ ] Name: "[Segment Name] - [Month Year]"
- [ ] Load emails:
  - Step 1: Email 1 (Day 0)
  - Step 2: Auto email 2 (Day 3)
  - Step 3: Auto email 3 (Day 7)
  - Step 4: Auto email 4 (Day 10)
- [ ] Add manual call steps:
  - Between Email 1 and 2: Manual call
  - Between Email 2 and 3: Manual call

**Timing Configuration:**
- [ ] Email 1: Send at 8:00 AM local time
- [ ] Email 2: Send at 10:00 AM local time (Day 3)
- [ ] Email 3: Send at 11:00 AM local time (Day 7)
- [ ] Email 4: Send at 9:00 AM local time (Day 10)

**Auto-Pause Rules:**
- [ ] On reply: Pause sequence
- [ ] On meeting booked: Pause and mark complete
- [ ] On unsubscribe: Remove from all sequences
- [ ] On hard bounce: Mark invalid

#### 6. Manual Tracking Sheet Setup

**If NOT using a sequencer, create a Google Sheet:**

Columns:
- Date
- Segment
- Contact Name
- Company
- Touch Type (Email/Call)
- Touch #
- Outcome
- Next Step
- Next Step Date

**Template:** `../tracking/daily-activity-tracker.csv`

#### 7. Calendar Blocking

- [ ] Block daily prospecting time:
  - 8:00-9:30 AM: Email sends
  - 10:00 AM-12:00 PM: Calling block
  - 1:00-2:00 PM: Follow-ups & CRM hygiene
  - 3:00-5:00 PM: Calling block

- [ ] Set up calendar link for meeting booking
  - Use: Calendly, Chili Piper, HubSpot Meetings
  - Set 15-min and 30-min meeting options
  - Include in email signatures

#### 8. Email Signature & Assets

- [ ] Create professional email signature:
  - Name, title
  - Company, phone
  - Calendar link
  - LinkedIn profile

**Example:**
```
[Your Name]
Sales Development Representative
[Company Name]
📞 [Phone] | 📧 [Email]
📅 [Calendar Link]
🔗 [LinkedIn Profile]
```

- [ ] Prepare email assets:
  - Company logo (if needed)
  - Case study links (ready to share)
  - Product demo link
  - One-pagers/decks

## 📊 Deliverables

By end of Day 5, you should have:
- [ ] **6 CRM list views** (one per segment)
- [ ] **Custom fields configured** (segment, status, touch count)
- [ ] **Reporting dashboard built** (daily metrics visible)
- [ ] **6 email sequences loaded** (if using sequencer)
- [ ] **Manual tracking sheet** (if not using sequencer)
- [ ] **Calendar blocked** (prospecting time reserved)
- [ ] **Assets ready** (signatures, links, documents)

## 🧪 Self-Test: Are You Ready?

You're ready for Day 6 when you can:
- [ ] Pull up any segment list in CRM in < 30 seconds
- [ ] Show where you'll track daily activity
- [ ] Demonstrate how sequences will send
- [ ] Explain how you'll measure success
- [ ] Find any email/call script instantly

## ⚠️ Common Mistakes

1. **Skipping custom fields** - You'll lose track of activity
2. **Not testing sequences** - Broken sequences = wasted days
3. **No tracking system** - Can't improve what you don't measure
4. **Over-automating** - Don't auto-call, always manual
5. **Not calendar blocking** - Prospecting gets pushed aside
6. **Forgetting about data hygiene** - Update CRM as you go

## 🔧 Tools Checklist

**Must Have:**
- [ ] CRM (Salesforce, HubSpot, Pipedrive)
- [ ] Prospecting DB (Apollo, ZoomInfo, Sales Nav)
- [ ] Email (Gmail, Outlook)
- [ ] Phone/dialer

**Nice to Have:**
- [ ] Sequencer (Outreach, Salesloft, Apollo Sequences)
- [ ] Email tracking (built-in or Yesware, Mixmax)
- [ ] Calendar scheduler (Calendly, Chili Piper)
- [ ] LinkedIn Sales Navigator

**Workarounds if you lack tools:**
- **No sequencer?** Use manual tracking sheet + Gmail templates
- **No dialer?** Use your mobile phone + Google Voice for tracking
- **No email tracking?** Use read receipts or Gmail's track feature

## 🤖 AI-Assisted Setup Documentation

Create a "Setup Doc" for future reference:

**Prompt:**
```
Document my SDR tech stack setup:

**Tools:**
- CRM: [Name]
- Prospecting DB: [Name]
- Sequencer: [Name]
- Dialer: [Name]

**Configuration:**
- List views: [How created]
- Custom fields: [List]
- Sequences: [How configured]
- Reporting: [What tracked]

Create a "Quick Start Guide" that explains:
1. Where to find each segment
2. How to add contacts to sequences
3. How to log activity
4. Where to check daily metrics
5. How to pull reports

Format as a simple SOP doc.
```

## 📊 Tracking Sheet Templates

### Daily Activity Tracker

| Date | Segment | Emails Sent | Calls Made | Conversations | Meetings Booked | Notes |
|------|---------|-------------|------------|---------------|-----------------|-------|
| 10/7 | VP-SaaS | 100 | 0 | 0 | 0 | Email only day |
| 10/8 | VP-SaaS | 0 | 80 | 12 | 2 | Good response |

### Weekly Metrics Dashboard

| Week | Total Touches | Reply Rate | Conv Rate | Meetings | Opps |
|------|---------------|------------|-----------|----------|------|
| 1 | 780 | 8% | 15% | 3 | 1 |
| 2 | 850 | 10% | 18% | 5 | 2 |

### Segment Performance Tracker

| Segment | Contacts | Replies | Reply % | Meetings | Conv % |
|---------|----------|---------|---------|----------|--------|
| VP-SaaS | 125 | 11 | 8.8% | 2 | 18% |
| VP-FinTech | 115 | 8 | 7.0% | 1 | 12% |

**Download templates:** `../tracking/` folder

## 💡 Pro Tips

### CRM Hygiene:
- Update status after every touch
- Log call outcomes immediately
- Set follow-up tasks in the moment
- Review and clean data weekly

### Sequence Best Practices:
- Test sequences with yourself first
- Check spam score before going live
- Monitor bounce rates daily
- Pause underperforming sequences

### Time Management:
- Batch similar activities (all emails, then all calls)
- Don't context switch mid-block
- Use timers for focus (50 min on, 10 min off)
- Reserve 30 min at EOD for admin

## 📊 Time Breakdown

- CRM list views: 45 min
- Custom fields: 30 min
- Reporting dashboard: 60 min
- Sequence setup: 90 min
- Tracking sheets: 30 min
- Calendar/assets: 45 min
- Testing everything: 30 min

**Total: 5-6 hours**

## 🎯 Next Step

With systems configured, you're ready to test everything before launch.

→ Go to `day-6-dry-run.md`
