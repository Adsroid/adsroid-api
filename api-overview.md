# API Overview

The Adsroid API provides programmatic access to Adsroid's AI agent for performance marketing automation. This document covers authentication, core concepts, and request/response patterns.

## API Philosophy

Adsroid's API is designed around three principles:

1. **Natural language first:** Send questions in plain English instead of constructing complex queries
2. **Context-aware:** The agent understands your connected platforms and provides relevant analysis
3. **Action-oriented:** Responses include specific recommendations, not just raw data

## Base URL

```
https://rckoycauuwzdryvkjpac.supabase.co/functions/v1/adsroid
```

All requests use `POST` method with JSON payloads.

## Authentication

### API Key

Every request requires an API key passed in the Authorization header:

```
Authorization: Bearer YOUR_API_KEY
```

**Getting your API key:**
1. Log in to [app.adsroid.com](https://app.adsroid.com/)
2. Navigate to **Settings → API Key**
3. Generate or copy your key

**Security notes:**
- Each organization has one active API key
- Keys can be regenerated at any time (invalidating the previous key)
- Never commit API keys to version control
- Use environment variables for key storage

### Organization ID

Identifies which organization is making the request. Found in Settings → Organization ID.

### Project ID

Specifies which project to analyze. Found in Project → Settings → Project ID.

Organizations can have multiple projects, each with different connected accounts and configurations.

## Request Format

### Required Fields

```json
{
  "organisation_id": "3f938ddc-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "project_id": "0392b0c1-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "message": "Your question or instruction in natural language"
}
```

### Complete Example

```bash
curl -X POST https://rckoycauuwzdryvkjpac.supabase.co/functions/v1/adsroid \
  -H "Authorization: Bearer adr_b80101ab-xxxx-xxxx-xxxx-xxxxxxxxxxxx" \
  -H "Content-Type: application/json" \
  -d '{
    "organisation_id": "3f938ddc-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "project_id": "0392b0c1-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "message": "Analyze my Google Ads campaigns for the last 7 days and identify any underperforming campaigns"
  }'
```

## Response Format

### Successful Response

```json
{
  "status": 200,
  "message": "Here is your Google Ads performance analysis for the last 7 days..."
}
```

**Fields:**
- `status`: HTTP-like status code (200 for success)
- `message`: The agent's response in natural language

### Response Content

The `message` field contains:
- Performance summaries
- Identified issues or opportunities
- Specific recommendations
- Reasoning and context

Example response structure:
```
Campaign Performance Summary:
- Brand Search: 127 conversions, CPA $4.21 (-12% vs. last period)
- Product Keywords: 89 conversions, CPA $6.83 (+23% vs. last period)

Issues Identified:
1. "Product Keywords" campaign showing CPA increase
   - CTR declined from 3.2% to 2.1%
   - Quality Score dropped on 4 keywords

Recommendations:
1. Review ad copy for "Product Keywords" campaign
   - CTR decline suggests messaging issue
   - Test 2-3 new ad variations
   
2. Pause or reduce bids on low Quality Score keywords
   - Identify the 4 keywords with QS drop
   - Reduce bid by 30% or pause if QS < 4
```

### Error Response

```json
{
  "error": "invalid_api_key",
  "message": "Your API key is invalid.",
  "status": 401
}
```

**Fields:**
- `error`: Machine-readable error code
- `message`: Human-readable explanation
- `status`: HTTP-like error code

See [Error Handling](error-handling.md) for complete error reference.

## Core Concepts

### Projects

A project represents a collection of connected advertising accounts and configurations.

**Use cases:**
- **Single business:** One project with all connected platforms
- **Agency:** One project per client
- **Multi-brand company:** One project per brand or business unit

When you make an API request, specify which project to analyze. The agent automatically accesses that project's connected accounts.

### Connected Accounts

Connected accounts are advertising and analytics platforms linked to a project.

**Connection process:**
1. Log in to Adsroid
2. Navigate to project settings
3. Click "Connect Account" for each platform
4. Complete OAuth flow

Once connected, the API can access these platforms without additional authentication.

**Supported connections:**
- Google Ads (via Google OAuth)
- Meta Ads (via Facebook OAuth)
- LinkedIn Ads (via LinkedIn OAuth)
- Google Analytics 4 (via Google OAuth)
- Google Search Console (via Google OAuth)

### Agent Behavior

The Adsroid agent is context-aware. When you send a query like "analyze performance," the agent:
1. Checks which platforms are connected to the project
2. Retrieves relevant data from those platforms
3. Analyzes performance across all connected platforms
4. Provides unified insights

You don't need to specify which platforms to query. The agent determines the appropriate scope based on your question and available connections.

### Recommendations

Adsroid responses include actionable recommendations when relevant.

**Recommendation structure:**
- **Finding:** What the agent observed
- **Impact:** How it affects performance
- **Action:** Specific step to take
- **Reasoning:** Why this action is suggested

Example:
```
Finding: Campaign "Brand Search" is spending 45% faster than planned
Impact: Current pace will exhaust monthly budget by day 19
Action: Reduce daily budget from $500 to $350
Reasoning: Budget pacing indicates targeting or bidding issues. Reduction prevents premature budget exhaustion while you investigate root cause.
```

### Natural Language Queries

The API accepts queries in plain English. No need to learn specialized syntax.

**Query examples:**

**Performance analysis:**
- "Analyze my Google Ads performance this week"
- "Show me which Meta campaigns are underperforming"
- "Compare LinkedIn Ads vs Google Ads ROI"

**Issue detection:**
- "Are any campaigns overspending?"
- "Identify campaigns with declining CTR"
- "Find keywords with low Quality Score"

**Optimization:**
- "What should I optimize first?"
- "Suggest budget reallocations"
- "Which campaigns should I pause?"

**Specific metrics:**
- "What's my average CPA across all platforms?"
- "Show conversion trends for the last 30 days"
- "Calculate ROAS by campaign"

### Time Ranges

The agent understands relative time expressions:
- "today"
- "yesterday"
- "this week"
- "last 7 days"
- "last 30 days"
- "this month"
- "last month"

You can also request specific date ranges:
- "between January 1 and January 31"
- "Q4 2024"

## How Adsroid Fits Into Existing Stacks

### Use Case 1: Scheduled Analysis

**Tools:** Cron + Adsroid API + Slack  
**Flow:** Daily cron job → API request → Post results to Slack channel

```python
import requests
import os

response = requests.post(
    'https://rckoycauuwzdryvkjpac.supabase.co/functions/v1/adsroid',
    headers={
        'Authorization': f'Bearer {os.environ["ADSROID_API_KEY"]}',
        'Content-Type': 'application/json'
    },
    json={
        'organisation_id': os.environ['ADSROID_ORG_ID'],
        'project_id': os.environ['ADSROID_PROJECT_ID'],
        'message': 'Analyze yesterday\'s performance and flag any issues'
    }
)

if response.status_code == 200:
    post_to_slack(response.json()['message'])
```

### Use Case 2: Dashboard Enrichment

**Tools:** Custom dashboard + Adsroid API  
**Flow:** User opens dashboard → Dashboard calls Adsroid → Display AI insights alongside metrics

```javascript
async function getAdsroidInsights() {
  const response = await fetch('https://rckoycauuwzdryvkjpac.supabase.co/functions/v1/adsroid', {
    method: 'POST',
    headers: {
      'Authorization': `Bearer ${process.env.ADSROID_API_KEY}`,
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      organisation_id: process.env.ADSROID_ORG_ID,
      project_id: process.env.ADSROID_PROJECT_ID,
      message: 'Summarize overall account health and top 3 priorities'
    })
  });
  
  return response.json();
}
```

### Use Case 3: Alert System

**Tools:** Make/Zapier + Adsroid API + Email/Slack  
**Flow:** Scheduled trigger → API request → Filter for critical issues → Send alert if found

No-code configuration:
1. Trigger: Schedule (every 6 hours)
2. Action: Call Adsroid API
3. Filter: Check if response contains "critical" or "urgent"
4. Action: Send alert to Slack/Email

### Use Case 4: Recommendation Pipeline

**Tools:** n8n + Adsroid API + Notion/Airtable  
**Flow:** Daily trigger → API request for recommendations → Parse recommendations → Save to database → Notify team

This creates an audit trail of all recommendations and allows tracking which suggestions were implemented.

## Rate Limits

Adsroid API requests are rate-limited based on your subscription plan. Rate limits prevent:
- Accidental over-consumption of credits
- Overwhelming connected platform APIs
- Resource exhaustion

If you exceed rate limits, the API returns a 429 status code with a retry-after header.

## Credits and Billing

Each API request consumes credits based on:
- Complexity of the query
- Amount of data analyzed
- Number of platforms queried

**Credit consumption examples:**
- Simple metric query: 1 credit
- Campaign performance analysis: 2-3 credits
- Cross-platform comparison: 4-5 credits
- Complex optimization recommendations: 5-10 credits

Credits are purchased through the Adsroid interface. See [adsroid.com/pricing](https://adsroid.com/pricing/) for current rates.

**Monitoring usage:**
Check credit balance in Adsroid Settings → Billing

## Best Practices

### 1. Be Specific
Instead of "analyze my ads," try "analyze Google Ads campaigns for the last 7 days and identify budget pacing issues."

### 2. Use Time Ranges
Specify timeframes to get relevant insights: "last 7 days," "this month," "yesterday."

### 3. Ask Follow-Up Questions
If the first response isn't detailed enough, refine your query: "provide more detail on campaign X's CTR decline."

### 4. Combine with Existing Data
Use Adsroid for insights, not as a replacement for platform APIs. Adsroid adds reasoning layer on top of raw metrics.

### 5. Test Queries Manually First
Use the Adsroid web interface to test queries before automating them via API.

## Common Patterns

### Pattern 1: Daily Digest
```json
{
  "message": "Summarize yesterday's performance across all platforms and highlight the top 3 priorities"
}
```

### Pattern 2: Budget Alert
```json
{
  "message": "Check if any campaigns are spending more than 15% faster than planned this month"
}
```

### Pattern 3: Optimization Queue
```json
{
  "message": "List the top 5 campaigns that need attention this week, ranked by potential impact"
}
```

### Pattern 4: Platform Comparison
```json
{
  "message": "Compare Google Ads and Meta Ads performance for the last 30 days. Which platform is more efficient?"
}
```

### Pattern 5: Specific Metric Check
```json
{
  "message": "What is the average CPA across all active campaigns? Is it higher than last month?"
}
```

## Next Steps

- [Authentication Details](https://adsroid.com/api/authentication/)
- [Error Handling](https://adsroid.com/api/error-handling/)
- [Use Cases](use-cases.md)
- [Webhooks](webhooks.md)
