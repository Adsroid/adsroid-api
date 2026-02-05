# Webhooks

Webhooks enable event-driven workflows with Adsroid. Instead of polling the API on a schedule, external systems can trigger Adsroid analysis based on specific events.

## When to Use Webhooks

**Use webhooks when:**
- An external system needs to trigger Adsroid analysis
- You want event-driven automation instead of scheduled polling
- You're integrating Adsroid into existing workflows (e.g., deployment pipelines, CRM events)

**Don't use webhooks when:**
- Simple scheduled analysis is sufficient (use Make/Zapier schedulers instead)
- You need sub-second response times (Adsroid analysis takes 3-15 seconds)

## Webhook Integration Patterns

Adsroid doesn't send webhooks. Instead, you trigger Adsroid via incoming webhooks from automation platforms.

### Pattern 1: Receive Event → Trigger Adsroid

**Example: CRM deal closed → Analyze campaign that generated the lead**

```
External System → Webhook Platform (Make/Zapier) → Adsroid API → Action
```

**Implementation (Make):**
1. Create webhook URL in Make
2. Configure CRM to send webhook when deal closes
3. Make receives webhook, extracts campaign ID
4. Make calls Adsroid API: "Analyze performance of campaign X and recommend budget adjustments"
5. Make posts recommendations to Slack

### Pattern 2: Threshold-Based Triggers

**Example: Analytics detects traffic spike → Ask Adsroid which campaigns drove it**

```
Analytics → Threshold Checker → Webhook → Adsroid → Report
```

**Implementation:**
1. Analytics tool monitors traffic
2. When traffic > threshold, trigger webhook
3. Webhook calls Adsroid: "Which campaigns had the highest click volume in the last 2 hours?"
4. Post results to dashboard

### Pattern 3: Multi-Step Workflows

**Example: Deploy new landing page → Trigger Adsroid to analyze impact**

```
Deployment → Webhook → Wait 24h → Adsroid Analysis → Compare performance
```

**Implementation (Zapier):**
1. Deployment tool sends webhook to Zapier
2. Zapier delays 24 hours
3. Zapier calls Adsroid: "Compare landing page performance before and after [timestamp]"
4. Zapier sends analysis to project management tool

## Webhook Payload Examples

### Example 1: CRM Event

**Incoming webhook from CRM:**
```json
{
  "event": "deal_closed",
  "deal_id": "123456",
  "campaign_source": "google-ads",
  "campaign_id": "987654321",
  "deal_value": 5000
}
```

**Triggered Adsroid request:**
```json
{
  "organisation_id": "org-id",
  "project_id": "project-id",
  "message": "Analyze campaign 987654321 performance over the last 30 days. Should we increase budget based on conversion quality?"
}
```

### Example 2: Budget Alert

**Incoming webhook from monitoring system:**
```json
{
  "event": "budget_threshold_exceeded",
  "campaign_id": "brand-search",
  "current_spend": 8500,
  "budget_limit": 10000,
  "threshold_percent": 85
}
```

**Triggered Adsroid request:**
```json
{
  "organisation_id": "org-id",
  "project_id": "project-id",
  "message": "Campaign 'brand-search' has spent 85% of monthly budget. Analyze pacing and recommend whether to reduce spend, increase budget, or maintain current trajectory."
}
```

### Example 3: Performance Anomaly

**Incoming webhook from anomaly detector:**
```json
{
  "event": "performance_anomaly",
  "platform": "meta_ads",
  "metric": "ctr",
  "change_percent": -45,
  "detected_at": "2024-02-05T14:23:00Z"
}
```

**Triggered Adsroid request:**
```json
{
  "organisation_id": "org-id",
  "project_id": "project-id",
  "message": "Meta Ads CTR dropped 45% suddenly. Analyze what changed and recommend immediate actions."
}
```

## Typical Automation Scenarios

### Scenario 1: New Product Launch

**Event:** Product team deploys new product page  
**Webhook chain:**
1. Deployment system sends webhook to Make
2. Make waits 48 hours
3. Make calls Adsroid: "Analyze traffic and conversion performance for [product page URL] since [launch date]"
4. Make creates report in Notion with Adsroid insights
5. Make notifies product and marketing teams

**Business value:** Automatic post-launch analysis without manual intervention

### Scenario 2: Competitor Campaign Detection

**Event:** Monitoring tool detects competitor ad activity  
**Webhook chain:**
1. Competitor monitoring tool sends webhook to Zapier
2. Zapier calls Adsroid: "Analyze our campaign performance in the last 24 hours. Are we losing impression share?"
3. Zapier checks if Adsroid identifies issues
4. If yes, send alert to marketing team with recommendations
5. If no, log for weekly review

**Business value:** Rapid response to competitive threats

### Scenario 3: Seasonal Event Tracking

**Event:** Calendar event (Black Friday start)  
**Webhook chain:**
1. Scheduled event in Make triggers at midnight
2. Make calls Adsroid every 4 hours: "Analyze Black Friday campaign performance vs. yesterday. Any issues?"
3. Make posts updates to dedicated Slack channel
4. At end of day, Make calls Adsroid: "Summarize Black Friday performance and compare to last year"
5. Make generates executive report

**Business value:** Real-time monitoring during critical sales periods

### Scenario 4: Lead Quality Feedback Loop

**Event:** Sales team marks lead as "qualified" or "unqualified" in CRM  
**Webhook chain:**
1. CRM sends webhook with lead status and source campaign
2. Make aggregates lead quality data
3. Once per week, Make calls Adsroid: "Here are the lead quality scores for each campaign. Which campaigns should we scale or pause?"
4. Make creates task list in project management tool
5. Make notifies account manager

**Business value:** Close feedback loop between sales and marketing

### Scenario 5: Agency Client Reporting

**Event:** End of month  
**Webhook chain:**
1. Scheduler triggers at month-end
2. For each client project, call Adsroid: "Summarize monthly performance across all platforms and provide 3 key recommendations"
3. Auto-populate client report template with Adsroid insights
4. Send draft report for review
5. Schedule review meeting

**Business value:** Standardized, scalable client reporting

## Implementation with Make

Make provides native Adsroid integration and webhook support.

### Step 1: Create Webhook Trigger
1. Add "Webhooks" module to scenario
2. Create webhook URL
3. Configure external system to send events to this URL

### Step 2: Process Webhook Data
1. Extract relevant fields from webhook payload
2. Transform data if needed
3. Construct Adsroid query

### Step 3: Call Adsroid
1. Add "Adsroid" module
2. Connect Adsroid account
3. Map webhook data to query: `"Analyze campaign {{campaign_id}} performance"`

### Step 4: Handle Response
1. Parse Adsroid response
2. Route to appropriate action (Slack, email, database)
3. Add conditional logic if needed

**Example scenario structure:**
```
[Webhook] → [Text Parser] → [Adsroid] → [Router]
                                           ├→ [Slack] if critical
                                           └→ [Google Sheets] always
```

## Implementation with Zapier

Zapier supports webhook triggers and has native Adsroid integration.

### Step 1: Create Webhook Trigger
1. Choose "Webhooks by Zapier" as trigger
2. Select "Catch Hook"
3. Copy webhook URL

### Step 2: Test Webhook
1. Send test payload from external system
2. Verify Zapier receives data correctly
3. Review available fields

### Step 3: Add Adsroid Action
1. Search for "Adsroid"
2. Connect Adsroid account
3. Enter organization ID, project ID
4. Compose message using webhook data

### Step 4: Add Follow-Up Actions
1. Add filters if needed ("only if X is true")
2. Add actions (Slack, email, database updates)
3. Test complete flow

**Example Zap structure:**
```
Webhook Trigger → Filter → Adsroid Action → Slack Notification
```

## Advanced Patterns

### Pattern: Conditional Analysis

Only trigger Adsroid if webhook meets certain conditions.

**Example:**
```
Webhook → Filter: spend > $500 → Adsroid → Action
```

This prevents unnecessary API calls for low-spend campaigns.

### Pattern: Batch Processing

Accumulate webhooks and trigger batch Adsroid analysis.

**Example:**
```
Webhook → Airtable (accumulate) → Daily Schedule → Adsroid (batch query) → Report
```

This reduces API calls while maintaining comprehensive analysis.

### Pattern: Feedback Loop

Use Adsroid recommendations to trigger platform API actions.

**Example:**
```
Schedule → Adsroid Analysis → Parse recommendations → Google Ads API (adjust bids) → Log action
```

**Warning:** Only implement this pattern with clear thresholds and human oversight. Adsroid recommendations are advisory, not commands.

### Pattern: Multi-Platform Correlation

Trigger Adsroid when events occur across multiple platforms.

**Example:**
```
Google Analytics Webhook → Detect traffic spike
Meta Ads Webhook → Detect spend increase
→ Trigger Adsroid: "Why did traffic and Meta spend both spike simultaneously?"
```

This helps identify cross-platform patterns that aren't visible in individual dashboards.

## Error Handling in Webhook Flows

### Handle Adsroid API Errors

If Adsroid API returns an error:
1. Log error details
2. Send alert to monitoring system
3. Retry with exponential backoff
4. Escalate if retry fails

**Make error handler:**
```
[Adsroid] → [Error Handler] → [Slack Alert] + [Retry Logic]
```

### Handle Invalid Webhook Data

If incoming webhook has missing or invalid data:
1. Log the issue
2. Send alert to integration owner
3. Don't call Adsroid API (prevents wasted credits)

**Filter example:**
```
IF webhook.campaign_id EXISTS THEN call Adsroid ELSE log error
```

### Handle Timeout

Adsroid can take up to 25 seconds for complex queries:
1. Set webhook timeout to 30+ seconds
2. If timeout occurs, log and retry once
3. If second timeout, send alert for investigation

## Monitoring Webhook Integrations

### Key Metrics to Track

1. **Webhook delivery success rate:** What % of webhooks are successfully received?
2. **Adsroid API success rate:** What % of API calls succeed?
3. **End-to-end latency:** Time from webhook receipt to final action
4. **Credit consumption:** How many credits are webhooks consuming?

### Recommended Monitoring

1. Set up alerts for webhook failures
2. Track Adsroid API error rates
3. Monitor credit consumption trends
4. Review webhook payloads weekly to ensure data quality

## Security Considerations

### Validate Webhook Sources

Only accept webhooks from trusted sources:
1. Use webhook secret/signature verification if available
2. Whitelist IP addresses if possible
3. Validate payload structure before processing

### Protect Adsroid Credentials

Store API keys securely:
1. Use environment variables or secret management systems
2. Never include API keys in webhook URLs
3. Rotate keys periodically

### Rate Limiting

Implement rate limiting on webhook endpoints to prevent abuse:
1. Limit webhooks per minute
2. Implement exponential backoff
3. Alert if rate limits are hit frequently

## Example: Complete Webhook Flow

**Business scenario:** E-commerce company wants to analyze campaign performance whenever a high-value order is placed.

**Implementation:**

1. **E-commerce platform sends webhook when order > $500:**
```json
{
  "event": "high_value_order",
  "order_id": "ORD-12345",
  "value": 750,
  "utm_source": "google",
  "utm_campaign": "summer-sale",
  "timestamp": "2024-02-05T15:30:00Z"
}
```

2. **Make receives webhook and extracts campaign info:**
- Source: google
- Campaign: summer-sale

3. **Make calls Adsroid API:**
```json
{
  "organisation_id": "org-id",
  "project_id": "project-id",
  "message": "Analyze the 'summer-sale' Google Ads campaign over the last 7 days. Is it driving high-value orders consistently? Should we increase budget?"
}
```

4. **Adsroid responds:**
```
Campaign 'summer-sale' analysis (last 7 days):
- 45 conversions, average order value $685
- Consistently outperforms account average ($425)
- CPA: $52 (excellent given high AOV)
- ROAS: 13.2x

Recommendation: Increase daily budget from $500 to $750
Reasoning: Campaign consistently drives high-value orders at efficient CPA. Budget increase likely profitable.
```

5. **Make posts to Slack:**
- Channel: #marketing-ops
- Message: Summary + recommendation + link to campaign

6. **Make creates task in Asana:**
- Task: "Review budget increase for summer-sale campaign"
- Assignee: Marketing manager
- Context: Adsroid analysis attached

**Result:** High-value order triggers automated analysis and actionable recommendation delivered to the right people at the right time.
