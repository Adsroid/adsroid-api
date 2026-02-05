# Example: Zapier Flow

Complete Zapier workflow for budget monitoring and automatic alerts.

## Scenario Overview

**Goal:** Monitor campaign budget pacing every 6 hours. If any campaign is spending faster than expected, send alert to Slack and email the account manager.

**Frequency:** Every 6 hours  
**Tools:** Zapier, Adsroid, Slack, Gmail  
**Complexity:** Medium  

## Zap Structure

```
[Schedule by Zapier] 
    → [Adsroid: Analyze Budget Pacing]
    → [Filter by Zapier: Only if issues detected]
    → [Slack: Send Alert]
    → [Gmail: Send Email]
```

## Step-by-Step Implementation

### Step 1: Install Adsroid on Zapier

1. Visit [zapier.com/developer/public-invite](https://zapier.com/developer/public-invite/234304/ac2d85f2e9a7e6c9f5365aef31c70b11/)
2. Click "Accept Invite"
3. Adsroid is now available in your Zapier account

### Step 2: Create New Zap

1. Log in to Zapier
2. Click "Create Zap"
3. Name it: "Budget Pacing Monitor"

### Step 3: Configure Trigger (Schedule)

1. Search for "Schedule by Zapier"
2. Choose "Every Day"
3. Configure:
   - Time of Day: 12:00 AM, 6:00 AM, 12:00 PM, 6:00 PM
   - This creates 4 checks per day
4. Click "Continue"
5. Click "Test trigger" (optional)

### Step 4: Add Adsroid Action

1. Click "+" to add action
2. Search for "Adsroid"
3. Select "Analyze Campaigns"
4. Click "Sign in" to connect Adsroid

**Connection:**
- API Key: [Paste from Adsroid Settings → API Key]
- Click "Yes, Continue"

**Action configuration:**
- Organization ID: `[Your org ID]`
- Project ID: `[Your project ID]`
- Message: 
```
Check budget pacing for all active campaigns.

Today is day {{formatDate(now, "D")}} of the month.
Expected budget usage at this point: {{formatDate(now, "D")}}% of monthly budget.

Flag any campaign where:
- Current spend is >5% ahead of expected pace
- Campaign will exhaust monthly budget before month-end based on current pace

For each flagged campaign, provide:
- Campaign name
- Current spend
- Projected month-end spend
- Recommended action
```

**Note:** The dynamic date fields (`{{formatDate(...)}}`) automatically insert current date values.

5. Click "Continue"
6. Click "Test action" to verify Adsroid responds correctly

### Step 5: Add Filter

Critical step to avoid unnecessary alerts:

1. Click "+" after Adsroid
2. Search for "Filter by Zapier"
3. Select "Only continue if..."
4. Configure condition:
   - Field: `Message` (from Adsroid step)
   - Condition: `Text Contains`
   - Value: `flagged`

Alternative filter if you want to check for multiple keywords:
```
(Text Contains "flagged") OR (Text Contains "WARNING") OR (Text Contains "ALERT")
```

5. Click "Continue"
6. Click "Test & Review" (this will fail if no issues exist, which is expected)

### Step 6: Add Slack Alert

1. Click "+" after Filter
2. Search for "Slack"
3. Select "Send Channel Message"
4. Connect Slack workspace (if not connected)
5. Configure:
   - Channel: `#budget-alerts`
   - Message Text:
```
🚨 *Budget Pacing Alert*

{{Message from Adsroid}}

*Time:* {{formatDate(now, "MMMM D, YYYY - h:mm A")}}

Please review and take action as recommended.
```

   - Bot Name: "Budget Monitor"
   - Bot Icon: [Optional emoji or image URL]

6. Click "Continue"
7. Click "Test action" to verify Slack message posts correctly

### Step 7: Add Email Notification

1. Click "+" after Slack
2. Search for "Gmail"
3. Select "Send Email"
4. Connect Gmail account
5. Configure:
   - To: `manager@company.com`
   - From Name: "Adsroid Budget Monitor"
   - Subject: `⚠️ Budget Alert: Campaign Pacing Issues Detected`
   - Body:
```
A budget pacing issue has been detected in your campaigns.

Analysis from Adsroid:

{{Message from Adsroid}}

---
Time: {{formatDate(now, "MMMM D, YYYY - h:mm A")}}

Please review the flagged campaigns and take appropriate action.

View campaigns: https://ads.google.com
```

   - Body Type: Plain Text or HTML (your preference)

6. Click "Continue"
7. Click "Test action" to verify email sends

### Step 8: Publish Zap

1. Review all steps
2. Click "Publish Zap"
3. Zap is now active and will run every 6 hours

## Example Adsroid Response (No Issues)

```
Budget Pacing Analysis - February 5, 2024 (Day 5 of month)
Expected budget usage: 5% of monthly budget

All campaigns are pacing as expected. No action required.

Campaign Summary:
- Brand Search: 4.8% spent (on track)
- Product Keywords: 5.1% spent (on track)
- Display Remarketing: 4.2% spent (on track)
- Competitor Targeting: 5.3% spent (slightly ahead but within threshold)
```

In this case, the Filter step blocks the Zap from continuing. No Slack or email is sent.

## Example Adsroid Response (Issues Detected)

```
Budget Pacing Analysis - February 5, 2024 (Day 5 of month)
Expected budget usage: 5% of monthly budget

⚠️ 2 campaigns flagged for pacing issues:

1. CAMPAIGN FLAGGED: "Summer Sale - Broad Match"
   - Current spend: $412 (8.2% of monthly budget)
   - Pace: 3.2% ahead of expected
   - Projected month-end: $2,472 (exceeds $2,000 monthly budget by 24%)
   - Recommended action: Reduce daily budget from $30 to $20 immediately to prevent overspend

2. CAMPAIGN FLAGGED: "New Product Launch"
   - Current spend: $187 (9.4% of monthly budget)
   - Pace: 4.4% ahead of expected
   - Projected month-end: $1,122 (exceeds $800 monthly budget by 40%)
   - Recommended action: Pause campaign for review. Current pace is unsustainable. 
     Investigate cause: recent bid increases or targeting expansion.

Other campaigns:
- Brand Search: 4.8% spent (on track)
- Product Keywords: 5.1% spent (on track)
```

This response contains "flagged" so the Filter allows the Zap to continue. Slack and email notifications are sent.

## Slack Message Output

```
Budget Monitor  2:00 PM

🚨 Budget Pacing Alert

Budget Pacing Analysis - February 5, 2024 (Day 5 of month)
Expected budget usage: 5% of monthly budget

⚠️ 2 campaigns flagged for pacing issues:

[...full analysis...]

Time: February 5, 2024 - 2:00 PM

Please review and take action as recommended.
```

## Email Output

```
From: Adsroid Budget Monitor <noreply@zapier.com>
To: manager@company.com
Subject: ⚠️ Budget Alert: Campaign Pacing Issues Detected

A budget pacing issue has been detected in your campaigns.

Analysis from Adsroid:

[...full analysis...]

---
Time: February 5, 2024 - 2:00 PM

Please review the flagged campaigns and take appropriate action.

View campaigns: https://ads.google.com
```

## Advanced Enhancements

### Enhancement 1: Severity-Based Routing

Add multiple filters to route different severity levels:

**Path 1: Critical Issues**
- Filter: `Text Contains "CRITICAL"` OR `spend ahead >10%`
- Action: Send to `#urgent` Slack channel + call PagerDuty

**Path 2: Warning Issues**
- Filter: `Text Contains "flagged"` (current setup)
- Action: Send to `#budget-alerts` + email manager

**Path 3: Minor Issues**
- Filter: `Text Contains "monitor"`
- Action: Log to Google Sheets only

### Enhancement 2: Add Google Sheets Logging

After Adsroid step, before Filter:

1. Add "Google Sheets" action
2. Select "Create Spreadsheet Row"
3. Configure:
   - Spreadsheet: "Budget Monitoring Log"
   - Worksheet: "Daily Checks"
   - Fields:
     - Date: `{{formatDate(now, "YYYY-MM-DD")}}`
     - Time: `{{formatDate(now, "HH:mm")}}`
     - Status: [Extract from message, or use `{{formatDate(...)}}`]
     - Full Analysis: `{{Message from Adsroid}}`

This creates a historical record of all checks, not just issues.

### Enhancement 3: Create Asana Tasks

For flagged campaigns, automatically create tasks:

1. Add "Asana" action after Slack
2. Select "Create Task"
3. Configure:
   - Project: "Campaign Management"
   - Task Name: `Fix Budget Pacing: [Campaign Name]`
   - Description: `{{Message from Adsroid}}`
   - Assignee: Account Manager
   - Due Date: Today + 1 day

### Enhancement 4: SMS Alerts for Critical Issues

For campaigns exceeding budget by >20%:

1. Add Filter after Adsroid: `Text Contains "exceeds" AND Number > 20`
2. Add "SMS by Zapier" action
3. Configure:
   - To Number: Manager's phone
   - Message: 
```
URGENT: Campaign budget alert
{{substring(Message from Adsroid, 0, 100)}}
Check email for details.
```

### Enhancement 5: Weekly Summary

Create second Zap that runs weekly:

1. Trigger: Schedule (Monday 9am)
2. Adsroid: "Summarize budget pacing trends for last 7 days"
3. Google Docs: Create document with summary
4. Email: Send to team with document attached

## Troubleshooting

### Issue: Zap Not Triggering

**Check:**
1. Verify Zap is turned on (toggle at top of editor)
2. Confirm schedule is set correctly
3. Check Task History for errors

### Issue: Adsroid Connection Failed

**Solution:**
1. Go to Adsroid Settings → API Key
2. Copy key
3. In Zapier, disconnect and reconnect Adsroid
4. Paste new key

### Issue: Filter Always Blocks Zap

**Cause:** Response never contains trigger word ("flagged")

**Solution:**
1. Test Adsroid action manually
2. Check exact text in response
3. Update filter condition to match actual response format
4. Consider using `Text Contains "alert"` or `Text Contains "warning"` as alternatives

### Issue: Too Many False Alerts

**Solution:** Make filter more restrictive:
```
(Text Contains "flagged") AND (Text Contains "ahead" OR Text Contains "exceeds")
```

Or adjust Adsroid query to only flag if >10% ahead:
```
Flag campaigns >10% ahead of expected pace
```

### Issue: Emails Go to Spam

**Solution:**
1. Use company email domain if possible
2. Add Budget Monitor to company whitelist
3. Or use Slack only for alerts

## Cost Analysis

**Zapier tasks consumed:**
- Schedule trigger: 1 task
- Adsroid action: 1 task
- Filter: 0 tasks
- Slack: 1 task
- Gmail: 1 task
- Total per run: 4 tasks

**Monthly usage:**
- 4 runs/day × 30 days = 120 runs/month
- 120 runs × 4 tasks = 480 tasks/month

**Zapier plan needed:**
- Free tier: 100 tasks/month (insufficient)
- Starter: 750 tasks/month (sufficient)
- Cost: ~$20/month

**Adsroid credits:**
- 4 API calls/day × 30 days = 120 calls/month
- Each call consumes ~2-3 credits (budget check is simple query)
- Total: ~300 credits/month

## Benefits

**Proactive monitoring:**
- Catch budget issues before they become problems
- 6-hour check interval means maximum overspend is limited

**Automatic escalation:**
- No manual checking required
- Relevant people notified immediately

**Historical tracking:**
- If Google Sheets logging added, full history of budget trends

**Peace of mind:**
- Set it and forget it
- Know you'll be alerted if anything goes wrong

## Variations

### Variation 1: Platform-Specific Monitoring

Create separate Zaps for each platform:
- "Google Ads Budget Monitor"
- "Meta Ads Budget Monitor"
- "LinkedIn Ads Budget Monitor"

Each queries its respective platform. This provides:
- Clearer alert messages (no need to check which platform)
- Different check frequencies per platform
- Platform-specific alert channels

### Variation 2: Client-Specific Monitoring (for agencies)

Create one Zap template, then duplicate for each client:
- Use different Project IDs
- Route alerts to client-specific Slack channels
- Tag different account managers in emails

### Variation 3: Threshold-Based Checking

Instead of fixed schedule, check based on spend threshold:

1. Trigger: Google Ads (via webhook when spend reaches $X)
2. Adsroid: "Analyze budget pacing now that spend hit $X"
3. Continue with alerts

This is more efficient (only runs when needed) but requires additional setup.

## Next Steps

1. Publish Zap and monitor Task History for first few days
2. Refine filter conditions based on actual alerts
3. Adjust check frequency if 6 hours is too frequent or infrequent
4. Consider adding enhancements (Google Sheets logging, Asana tasks)
5. Create similar Zaps for other monitoring needs (CTR drops, Quality Score issues, etc.)
