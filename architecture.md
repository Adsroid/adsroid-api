# Architecture

Adsroid operates as an agent-based system that connects to advertising platforms, analyzes performance data, and generates recommendations through AI reasoning.

## System Overview

```
┌─────────────────┐
│  User/Workflow  │
└────────┬────────┘
         │ API Request (natural language)
         ↓
┌─────────────────────────────────────┐
│      Adsroid Agent Layer            │
│  ┌──────────────────────────────┐   │
│  │  Query Interpreter           │   │
│  └──────────────────────────────┘   │
│  ┌──────────────────────────────┐   │
│  │  Multi-Platform Connector    │   │
│  └──────────────────────────────┘   │
│  ┌──────────────────────────────┐   │
│  │  Performance Analyzer        │   │
│  └──────────────────────────────┘   │
│  ┌──────────────────────────────┐   │
│  │  Recommendation Engine       │   │
│  └──────────────────────────────┘   │
└─────────┬───────────────────────────┘
          │
          │ Secure API Connections
          ↓
┌─────────────────────────────────────┐
│   Connected Advertising Platforms   │
│  - Google Ads                       │
│  - Meta Ads                         │
│  - LinkedIn Ads                     │
│  - Google Analytics 4               │
│  - Google Search Console            │
└─────────────────────────────────────┘
```

## Core Components

### 1. Query Interpreter

Converts natural language requests into structured analysis tasks.

**Input:** "Analyze my Google Ads performance this week and identify budget issues"

**Interpreted as:**
- Platform: Google Ads
- Timeframe: Last 7 days
- Analysis type: Budget efficiency
- Output format: Issues + recommendations

The interpreter handles ambiguous queries and clarifies intent when needed.

### 2. Multi-Platform Connector

Manages authenticated connections to advertising platforms. Each API request inherits permissions from the project's connected accounts.

**Key behaviors:**
- Uses OAuth tokens stored during account connection
- Enforces least-privilege access (read-only by default)
- Refreshes tokens automatically
- Handles rate limits and retry logic

### 3. Performance Analyzer

Retrieves relevant data and performs multi-dimensional analysis.

**Analysis patterns:**
- Trend detection (performance shifts over time)
- Comparative analysis (campaign vs. campaign, channel vs. channel)
- Anomaly detection (unusual patterns relative to baseline)
- Efficiency scoring (ROI, CPA, budget pacing)

The analyzer operates across timeframes (daily, weekly, monthly) and accounts for seasonality and external factors.

### 4. Recommendation Engine

Generates actionable suggestions with confidence scores and reasoning.

**Recommendation structure:**
```json
{
  "finding": "Campaign X is underperforming",
  "data": {
    "campaign_name": "Brand Search",
    "current_ctr": 1.2,
    "account_avg_ctr": 3.1,
    "performance_delta": -61.3
  },
  "recommendation": "Reduce daily budget from $500 to $350",
  "reasoning": "CTR significantly below account average indicates poor ad relevance or targeting mismatch",
  "confidence": "high",
  "expected_impact": "Free up $150/day for reallocation to higher-performing campaigns"
}
```

Confidence levels:
- **High:** Clear pattern with historical validation
- **Medium:** Pattern present but limited historical data
- **Low:** Exploratory suggestion requiring testing

## Agent Logic Flow

### Step 1: Connect
When a user sends an API request, Adsroid checks which platforms are connected to the specified project.

Example project connections:
- Google Ads (Account ID: 123-456-7890)
- Meta Ads (Ad Account: act_987654321)
- Google Analytics 4 (Property: GA4-XXXXX)

### Step 2: Retrieve
Adsroid queries relevant platforms for the data needed to answer the user's question.

Example for "Analyze last week's Google Ads performance":
- Retrieves campaign metrics for the past 7 days
- Pulls historical baseline for comparison (previous 30 days)
- Fetches account-level benchmarks

### Step 3: Analyze
The agent processes retrieved data to identify patterns, anomalies, and opportunities.

Analysis dimensions:
- Performance trends (improving, declining, stable)
- Efficiency metrics (CPA, ROAS, CTR)
- Budget pacing (overspend, underspend, on-target)
- Competitive positioning (if applicable)

### Step 4: Recommend
The agent generates specific, actionable recommendations based on analysis findings.

Recommendation categories:
- **Optimization:** Improve existing campaign performance
- **Reallocation:** Shift budget between campaigns or platforms
- **Investigation:** Identify areas requiring deeper analysis
- **Alert:** Flag critical issues requiring immediate attention

### Step 5: Explain
Every recommendation includes natural language reasoning explaining why the action is suggested and what impact to expect.

The explanation connects data points to business outcomes, making recommendations easy to evaluate and trust.

## Human-in-the-Loop Concepts

Adsroid follows a recommendation-first approach: it suggests actions but never executes changes automatically.

### Default Behavior
1. User sends query via API
2. Adsroid analyzes data
3. Adsroid returns recommendations
4. User evaluates recommendations
5. User decides whether to implement changes

This design ensures humans retain full control over advertising decisions.

### Optional Automation
For users who want automated execution:
1. Integrate Adsroid with automation platforms (Make, Zapier)
2. Define execution rules in the automation workflow
3. Adsroid recommendation triggers automated action based on user-defined conditions

Example automation rule:
```
IF recommendation.confidence == "high" 
   AND recommendation.type == "budget_reduction"
   AND recommendation.amount < 500
THEN execute_automatically()
ELSE notify_team_for_review()
```

## Safety and Control Mechanisms

### 1. Read-Only API Access
Adsroid's platform connections are read-only by default. The agent cannot modify campaigns, budgets, or settings through its native integrations.

### 2. Scoped Permissions
API keys are tied to specific organizations and projects. Requests cannot access data outside the specified scope.

### 3. Audit Trail
All API requests and responses are logged, providing a complete history of what was analyzed and when.

### 4. Rate Limiting
The API enforces rate limits to prevent accidental over-consumption of credits or platform API quotas.

### 5. Explicit Approval Required
Any automated execution requires explicit user configuration through external automation tools. Adsroid never assumes permission to take action.

## Data Flow and Storage

**During a Request:**
1. API receives query
2. Agent retrieves necessary data from connected platforms
3. Agent processes data in memory
4. Agent generates recommendations
5. Response returned to user

**After a Request:**
- Campaign data is not stored long-term
- Only aggregated, anonymized performance patterns may be used to improve the agent
- User-specific configurations (connected accounts, API keys) are stored securely

## Performance Characteristics

**Typical Response Times:**
- Simple queries (single metric): 3-5 seconds
- Standard analysis (campaign performance): 8-12 seconds
- Complex cross-platform analysis: 15-25 seconds

**Factors Affecting Speed:**
- Number of campaigns being analyzed
- Time range of data retrieval
- Platform API response times
- Complexity of reasoning required

## Scalability

Adsroid's agent architecture scales horizontally:
- Multiple projects can be analyzed concurrently
- Each API request is independent (no shared state)
- Rate limits prevent resource exhaustion
- Platform API quotas are managed per connection

## Integration Points

Adsroid integrates at three levels:

### 1. Direct API Integration
Custom scripts, backend services, and internal tools call the API directly.

### 2. No-Code Platforms
Make, Zapier, and n8n provide native Adsroid modules for visual workflow building.

### 3. Webhook-Driven Automation
External systems can trigger Adsroid analysis via webhooks, enabling event-driven workflows.

## Failure Handling

**Platform API Errors:**
If a connected platform returns an error (authentication failure, rate limit), Adsroid returns a clear error message indicating which platform failed and why.

**Incomplete Data:**
If data is missing for part of the requested analysis, Adsroid continues with available data and notes gaps in the response.

**Ambiguous Queries:**
If a query is unclear, Adsroid returns clarifying questions or suggests a more specific query structure.

## Future Architecture Considerations

The agent system is designed to accommodate future enhancements:
- Additional platform integrations (e.g., TikTok Ads, Pinterest Ads)
- Multi-step reasoning workflows (breaking complex queries into sequential analyses)
- Predictive modeling (forecasting performance based on historical patterns)
- Custom recommendation rules (user-defined logic for specific use cases)
