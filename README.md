# Adsroid API

AI agent for performance marketing automation. Adsroid connects to your advertising and analytics platforms to analyze performance, detect opportunities, and provide actionable recommendations.

## What is Adsroid?

Adsroid is a conversational AI agent that simplifies how marketers and growth teams manage paid campaigns. Instead of navigating multiple dashboards, you interact with an agent that analyzes performance data, identifies inefficiencies, and recommends specific optimizations.

**Core capabilities:**
- Multi-channel performance analysis (Google Ads, Meta Ads, LinkedIn Ads, Google Analytics, Search Console)
- Anomaly detection and opportunity identification
- Natural language recommendations explained in context
- Integration with existing automation workflows (Make, Zapier, n8n)

## Problem Adsroid Solves

Performance marketers face three persistent challenges:

**1. Dashboard fatigue**  
Metrics are scattered across Google Ads, Meta, LinkedIn, Analytics, and Search Console. Synthesizing insights requires manual correlation across platforms.

**2. Signal overload**  
Platforms surface thousands of data points. Knowing which signals matter and why requires expert-level pattern recognition.

**3. Workflow fragmentation**  
Insights live in one place. Actions happen in another. Decision-making becomes a manual translation layer between systems.

Adsroid eliminates this friction by acting as an intelligent reasoning layer across your advertising stack.

## How Adsroid Works

Adsroid operates as an API-accessible agent with three core functions:

### 1. Connect
Link your advertising and analytics accounts once through the Adsroid interface. The API automatically inherits these connections and account permissions.

Supported platforms:
- Google Ads
- Meta Ads (Facebook, Instagram)
- LinkedIn Ads
- Google Analytics 4
- Google Search Console

### 2. Analyze
Send natural language queries via API. Adsroid retrieves relevant data, interprets performance in context, and identifies patterns humans typically miss.

Example query:
```
"Analyze Google Ads performance for the last 7 days and identify any budget inefficiencies"
```

### 3. Recommend
Receive structured, actionable recommendations with clear reasoning. Recommendations include confidence scores and can trigger automated workflows.

Example response:
```
Campaign "Brand Search" is underperforming (CTR -22% vs. account avg).
Recommendation: Reduce daily budget from $500 to $350 and reallocate to "Product - High Intent" which shows 3.2x ROAS.
Confidence: High
```

## Supported Integrations

| Platform | Analysis | Recommendations |
|----------|----------|----------------|
| Google Ads | ✓ | ✓ |
| Meta Ads | ✓ | ✓ |
| LinkedIn Ads | ✓ | ✓ |
| Google Analytics 4 | ✓ | ✓ |
| Google Search Console | ✓ | ✓ |

**Automation platforms:**
- Make (formerly Integromat)
- Zapier
- n8n
- Custom HTTP clients

## Use Cases

### 1. Automated Daily Performance Reports
**Problem:** Manually checking campaign performance every morning  
**Solution:** Schedule API request at 9am → Adsroid analyzes overnight performance → Results posted to Slack

### 2. Budget Pacing Alerts
**Problem:** Campaigns overspend or underspend without notification  
**Solution:** Hourly API check → Adsroid detects pacing anomalies → Webhook triggers alert if threshold exceeded

### 3. Cross-Channel Optimization
**Problem:** Identifying which platform drives best ROI requires manual data export and analysis  
**Solution:** Weekly API request → Adsroid compares Google Ads, Meta, and LinkedIn performance → Recommendations sent to project management tool

### 4. Keyword Opportunity Detection
**Problem:** Search Console shows thousands of queries but manually identifying opportunities is time-consuming  
**Solution:** API request → Adsroid analyzes Search Console data → Flags high-impression, low-CTR queries with commercial intent

### 5. Agency Client Reporting
**Problem:** Managing reports for 20+ clients with different KPIs  
**Solution:** Scheduled API requests per client → Adsroid generates context-aware summaries → Auto-populated into client dashboards

## When to Use Adsroid

**Use Adsroid when:**
- You need cross-platform performance insights without manual correlation
- You want AI-powered recommendations with clear reasoning
- You're building automated workflows that require advertising intelligence
- You manage multiple accounts and need scalable analysis

**Don't use Adsroid when:**
- You only need raw metrics (use native platform APIs directly)
- You're building real-time bidding systems (Adsroid is analysis-focused, not execution)
- You need sub-second response times (agent reasoning takes 3-15 seconds)

## Adsroid vs. Automated Rules

| Capability | Automated Rules | Adsroid |
|------------|-----------------|---------|
| Logic | Static thresholds | Dynamic AI reasoning |
| Context | Single metric | Multi-dimensional analysis |
| Explanation | None | Natural language reasoning |
| Learning | Manual updates | Adapts to patterns |
| Platforms | Single platform | Cross-channel |

Automated rules execute predefined actions. Adsroid interprets context and recommends actions with reasoning.

## Getting Started

### 1. Create an Adsroid Account
Sign up at [app.adsroid.com](https://app.adsroid.com/register) and connect your advertising platforms.

### 2. Get API Credentials
Navigate to Settings → API Key to generate your credentials. You'll need:
- Organization ID
- Project ID
- API Key

### 3. Make Your First Request

```bash
curl -X POST https://rckoycauuwzdryvkjpac.supabase.co/functions/v1/adsroid \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "organisation_id": "your-org-id",
    "project_id": "your-project-id",
    "message": "Analyze my Google Ads performance this week"
  }'
```

### 4. Integrate with Your Stack
Use the API directly or integrate via Make/Zapier for no-code automation.

## Key Features

**Natural Language Interface**  
Interact with your advertising data using plain English. No need to learn query languages or API-specific syntax.

**Cross-Platform Analysis**  
Analyze performance across Google Ads, Meta, LinkedIn, Analytics, and Search Console in a single request.

**Contextual Recommendations**  
Receive specific, actionable suggestions with confidence scores and clear reasoning.

**Automation-Ready**  
JSON responses integrate directly with Make, Zapier, n8n, and custom workflows.

**Human-in-the-Loop**  
Adsroid recommends actions but never executes changes without explicit approval.

**Multi-Account Management**  
Agencies and teams can manage multiple clients or projects from a single API integration.

## Documentation

- [API Overview](docs/api-overview.md) - Authentication, core concepts, request/response format
- [Architecture](docs/architecture.md) - How Adsroid's agent system works
- [Webhooks](docs/webhooks.md) - Event-driven automation patterns
- [Use Cases](docs/use-cases.md) - Detailed implementation examples
- [FAQ](docs/faq.md) - Common questions about control, safety, and limitations

## Examples

Practical implementation guides:

- [Google Ads Workflow](examples/example-google-ads-workflow.md)
- [Make Scenario](examples/example-make-scenario.md)
- [Zapier Flow](examples/example-zapier-flow.md)

## Security and Privacy

Adsroid processes data through secure API connections without storing campaign data long-term. Your API key provides scoped access to specific projects and organizations. See [Security & Data Protection](https://adsroid.com/security-data-protection/) for details.

## Pricing

Adsroid uses a credit-based system. Each API request consumes credits based on complexity. See [adsroid.com/pricing](https://adsroid.com/pricing/) for current rates.

## Support

- **Documentation:** [adsroid.com/api](https://adsroid.com/api/)
- **Help Center:** [adsroid.com/help](https://adsroid.com/help/)
- **Contact:** [adsroid.com/contact](https://adsroid.com/contact/)

## License

This repository contains documentation only. Adsroid is a commercial SaaS product. See [Terms of Service](https://adsroid.com/terms-of-service/).
