# Use Cases

Practical examples of how to use the Adsroid API to solve real marketing automation challenges.

## Google Ads Automation

### Use Case 1: Daily Campaign Health Check

**Problem:** Manually reviewing 20+ campaigns every morning takes 45 minutes

**Solution:**
```python
import requests
import os

def daily_health_check():
    response = requests.post(
        'https://rckoycauuwzdryvkjpac.supabase.co/functions/v1/adsroid',
        headers={
            'Authorization': f'Bearer {os.environ["ADSROID_API_KEY"]}',
            'Content-Type': 'application/json'
        },
        json={
            'organisation_id': os.environ['ORG_ID'],
            'project_id': os.environ['PROJECT_ID'],
            'message': 'Analyze all Google Ads campaigns from yesterday. Flag any campaigns with: CTR below 2%, CPA above $50, or Quality Score below 5.'
        }
    )
    
    return response.json()['message']
```

**Outcome:**
- Automated daily analysis
- Issues flagged immediately
- Manual review reduced to 5 minutes

### Use Case 2: Budget Pacing Monitor

**Problem:** Campaigns frequently exhaust monthly budgets by day 20

**Solution:**
Set up hourly check via Make/Zapier:

**Adsroid query:**
```
"Check budget pacing for all active campaigns. 
Flag any campaign spending faster than 5% per day 
or slower than 2% per day."
```

**Automation flow:**
1. Schedule: Every 6 hours
2. Call Adsroid with query above
3. Parse response for flagged campaigns
4. If any flags found, send Slack alert
5. Log to Google Sheets for trend analysis

**Outcome:**
- Budget issues detected early
- Prevents overspend and underspend
- Historical pacing data for planning

### Use Case 3: Keyword Opportunity Scanner

**Problem:** Missing high-value keywords because manual review is time-consuming

**Solution:**
Weekly Search Console + Google Ads analysis

**Adsroid query:**
```
"Analyze Google Search Console data for the last 30 days. 
Identify search queries with >500 impressions and <2% CTR 
that we're not bidding on in Google Ads. 
Recommend which queries to add as keywords."
```

**Outcome:**
- Discover untapped keywords automatically
- Prioritized recommendations based on volume and intent
- Faster expansion of keyword coverage

## Meta Ads Automation

### Use Case 4: Creative Fatigue Detection

**Problem:** Ad creative performance degrades over time but manual monitoring is inconsistent

**Solution:**
Bi-weekly creative analysis

**Adsroid query:**
```
"Analyze all active Meta Ads ad sets. 
Identify ads with declining CTR over the last 14 days. 
For any ad with CTR drop >20%, recommend whether to 
pause, refresh creative, or test new audiences."
```

**Automation:**
- Schedule: Every 2 weeks
- Call Adsroid API
- Create tasks in Asana for creative team
- Track which recommendations were implemented

**Outcome:**
- Proactive creative refresh
- Maintain campaign performance
- Data-driven creative planning

### Use Case 5: Audience Performance Comparison

**Problem:** Running 15 different audience segments but unclear which perform best

**Solution:**
Monthly cross-audience analysis

**Adsroid query:**
```
"Compare performance of all Meta Ads audience segments 
for the last 30 days. Rank by ROAS. 
For the bottom 3 performers, recommend whether to 
optimize, consolidate, or pause."
```

**Implementation:**
```javascript
async function monthlyAudienceReview() {
  const response = await fetch(ADSROID_API, {
    method: 'POST',
    headers: {
      'Authorization': `Bearer ${ADSROID_KEY}`,
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      organisation_id: ORG_ID,
      project_id: PROJECT_ID,
      message: 'Compare performance of all Meta Ads audience segments...'
    })
  });
  
  const insights = await response.json();
  await saveToNotion(insights.message);
  await notifyTeam(insights.message);
}
```

**Outcome:**
- Clear audience performance ranking
- Actionable consolidation recommendations
- Better budget allocation

## Cross-Channel Optimization

### Use Case 6: Platform ROI Comparison

**Problem:** Running ads on Google, Meta, and LinkedIn but unclear which drives best ROI

**Solution:**
Weekly cross-platform analysis

**Adsroid query:**
```
"Compare Google Ads, Meta Ads, and LinkedIn Ads performance 
for the last 7 days. Calculate ROAS for each platform. 
Recommend budget reallocation if one platform significantly 
outperforms others."
```

**Automation (Make scenario):**
1. **Trigger:** Schedule (Monday 9am)
2. **Action:** Adsroid API call
3. **Action:** Parse response
4. **Action:** Update executive dashboard (Google Sheets)
5. **Action:** Post summary to #marketing Slack channel
6. **Action:** If reallocation recommended, create Asana task

**Outcome:**
- Data-driven budget allocation
- Weekly performance visibility
- Faster response to platform trends

### Use Case 7: Landing Page Performance Analysis

**Problem:** Multiple landing pages across campaigns but no unified view of which converts best

**Solution:**
Bi-weekly landing page analysis combining GA4 + ad platform data

**Adsroid query:**
```
"Analyze landing page performance across all platforms 
for the last 14 days. Which landing pages have the highest 
and lowest conversion rates? For low-performing pages, 
recommend whether the issue is traffic quality or page design."
```

**Outcome:**
- Identify underperforming landing pages
- Distinguish traffic quality issues from page issues
- Prioritize optimization efforts

## Budget Monitoring and Alerts

### Use Case 8: Real-Time Overspend Detection

**Problem:** Campaign overspends sometimes go unnoticed for hours

**Solution:**
Hourly budget check with threshold-based alerting

**Adsroid query:**
```
"Check if any campaigns spent more than their daily budget 
in the last hour. For any overspend, explain likely cause 
and recommend immediate action."
```

**Automation (Zapier):**
1. **Trigger:** Schedule every hour
2. **Action:** Adsroid API call
3. **Filter:** Only continue if overspend detected
4. **Action:** Send email to account manager
5. **Action:** Post to #urgent-alerts Slack channel
6. **Action:** Log to Airtable

**Outcome:**
- Overspends caught within 1 hour
- Root cause analysis included
- Immediate escalation

### Use Case 9: Monthly Budget Forecast

**Problem:** Difficult to predict if monthly budget will be exhausted early or underspent

**Solution:**
Mid-month budget projection

**Adsroid query:**
```
"Based on current spending pace, project whether we'll 
underspend or overspend monthly budget for each campaign. 
For any projected variance >10%, recommend adjustment."
```

**Implementation timing:**
- Run on day 15 of each month
- Provides 2-week window for adjustments

**Outcome:**
- Predictive budget management
- Reduces month-end surprises
- Optimization time for corrective action

## AI-Assisted Decision Workflows

### Use Case 10: Campaign Launch Analysis

**Problem:** After launching new campaigns, unclear how long to wait before making changes

**Solution:**
3-day post-launch analysis

**Adsroid query:**
```
"Campaign [NAME] launched 3 days ago. Analyze early performance 
and determine if there are any immediate red flags requiring 
action or if we should continue collecting data."
```

**Workflow:**
1. Campaign launches
2. Wait 3 days (Make delay module)
3. Trigger Adsroid analysis
4. If red flags, create urgent task
5. If no issues, schedule 7-day follow-up

**Outcome:**
- Early detection of setup errors
- Reduces wasted spend on broken campaigns
- Confidence in when to intervene vs. wait

### Use Case 11: Competitive Response Analysis

**Problem:** Competitors launch campaigns but response is often reactive rather than data-driven

**Solution:**
Weekly competitive context analysis

**Adsroid query:**
```
"Analyze our campaign performance for the last 7 days. 
Are we seeing unusual changes in impression share, CTR, 
or CPC that might indicate competitive activity? 
Recommend response strategy."
```

**Outcome:**
- Contextual understanding of performance changes
- Distinguishes competitive impacts from internal issues
- Strategic response recommendations

### Use Case 12: Seasonal Performance Planning

**Problem:** Knowing how to adjust campaigns for seasonal events

**Solution:**
Pre-season and in-season analysis

**Pre-season query (2 weeks before):**
```
"Analyze performance from last year's [EVENT] period. 
What worked well? What didn't? Recommend adjustments 
for this year's campaigns."
```

**In-season query (daily during event):**
```
"Compare current [EVENT] performance to last year. 
Are we on track? Should we adjust budgets or targeting?"
```

**Outcome:**
- Learn from historical performance
- Real-time seasonal adjustment
- Maximize seasonal opportunity

### Use Case 13: Quality Score Improvement

**Problem:** Low Quality Scores increase costs but fixing them is manual and unclear

**Solution:**
Monthly Quality Score audit

**Adsroid query:**
```
"Identify all keywords with Quality Score below 5. 
For each, analyze likely causes (ad relevance, landing page, 
or expected CTR) and recommend specific fixes."
```

**Automation:**
1. Monthly schedule
2. Call Adsroid
3. Parse recommendations by keyword
4. Create tasks in project management tool
5. Track which keywords improved after fixes

**Outcome:**
- Systematic Quality Score improvement
- Reduced CPCs over time
- Clear action plan for each keyword

### Use Case 14: New Market Entry Analysis

**Problem:** Expanding to new markets but unclear how to adjust campaigns

**Solution:**
Weekly performance comparison during initial rollout

**Adsroid query:**
```
"Compare campaign performance in [NEW MARKET] vs. existing 
markets for the last 7 days. Are CPA, CTR, and conversion rates 
within expected range? Recommend adjustments specific to new market."
```

**Outcome:**
- Faster market entry optimization
- Learn regional differences quickly
- Avoid costly extended learning periods

### Use Case 15: Agency Client Onboarding

**Problem:** Onboarding new clients requires comprehensive account audit

**Solution:**
Automated account audit at onboarding

**Adsroid query:**
```
"Perform comprehensive audit of this account: 
- Campaign structure and naming conventions
- Budget distribution
- Performance by campaign
- Audience targeting quality
- Top 5 quick wins
- Top 3 risks requiring immediate attention"
```

**Automation:**
1. New client added to CRM
2. CRM triggers webhook
3. Webhook calls Adsroid
4. Save audit to Notion
5. Create onboarding tasks from recommendations
6. Schedule 30-day follow-up analysis

**Outcome:**
- Standardized client onboarding
- Immediate value demonstration
- Clear roadmap for first 90 days

## Advanced Patterns

### Pattern 1: Recommendation Tracking

Track which Adsroid recommendations were implemented and their impact:

1. Save all Adsroid recommendations to database with timestamp
2. Add "implemented" field (boolean)
3. Add "outcome" field (text)
4. After 7 days, query Adsroid: "Compare performance before and after [change]"
5. Update outcome field with results

**Value:** Build historical record of what works

### Pattern 2: Multi-Stage Analysis

Break complex questions into sequential API calls:

```python
# Stage 1: Identify issues
issues_response = call_adsroid("What are the top 3 issues in the account?")

# Stage 2: Deep dive on top issue
issue_1 = parse_top_issue(issues_response)
deep_dive = call_adsroid(f"Analyze {issue_1} in detail and provide step-by-step fix")

# Stage 3: Validate solution
after_fix = call_adsroid(f"Has {issue_1} improved since implementing the fix?")
```

**Value:** Complex problems require layered analysis

### Pattern 3: Human-AI Collaboration

Route Adsroid recommendations through human review before execution:

```
Adsroid → Parse confidence score → 
  IF confidence == 'high' AND budget_change < $500:
    Execute automatically
  ELSE:
    Send to human for review → Human approves → Execute
```

**Value:** Balances automation with safety

### Pattern 4: Experimentation Framework

Use Adsroid to design and evaluate tests:

**Test design query:**
```
"I want to test [HYPOTHESIS]. Design an A/B test structure 
including budget allocation, duration, and success metrics."
```

**Test evaluation query (after test runs):**
```
"Evaluate results of [TEST NAME]. Is there a statistical 
winner? Should we scale the winning variant?"
```

**Value:** Structured experimentation approach

### Pattern 5: Executive Reporting

Automated monthly executive summary:

**Adsroid query:**
```
"Create executive summary of last month's performance. Include:
- Overall performance vs. target
- Top 3 wins
- Top 2 challenges
- Budget efficiency
- Recommended priorities for next month
Format for non-technical audience."
```

**Value:** Consistent, accessible executive communication
