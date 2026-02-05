# FAQ

Common questions about Adsroid's API, functionality, and limitations.

## General Questions

### What is Adsroid?

Adsroid is an AI agent for performance marketing automation. It connects to your advertising platforms, analyzes performance data, and provides actionable recommendations in natural language.

### How is Adsroid different from platform APIs?

Platform APIs (Google Ads, Meta Ads) provide raw data access. Adsroid adds an AI reasoning layer that interprets data, identifies patterns, and explains what actions to take and why.

### Who should use Adsroid?

Adsroid is designed for:
- Performance marketers managing multiple campaigns
- Growth teams automating acquisition workflows
- Agencies managing client accounts at scale
- Developers building marketing automation tools
- Founders who want AI-powered insights without hiring analysts

### Can Adsroid execute changes automatically?

No. Adsroid provides recommendations but never executes changes to campaigns automatically. You retain full control over all decisions. If you want automated execution, you must explicitly configure it through external automation tools (Make, Zapier) with appropriate safety thresholds.

## Technical Questions

### What platforms does Adsroid support?

**Currently supported:**
- Google Ads
- Meta Ads (Facebook, Instagram)
- LinkedIn Ads
- Google Analytics 4
- Google Search Console

**Integration method:**
OAuth-based connections configured through the Adsroid interface.

### What programming languages can I use?

Any language that supports HTTPS requests. The API accepts JSON and returns JSON.

**Common choices:**
- Python (with requests library)
- JavaScript/Node.js (with fetch or axios)
- PHP (with cURL)
- Ruby (with net/http)
- Go (with net/http package)

### How long do API requests take?

**Typical response times:**
- Simple queries: 3-5 seconds
- Standard analysis: 8-12 seconds
- Complex cross-platform queries: 15-25 seconds

Response time depends on query complexity, data volume, and platform API speed.

### What are the rate limits?

Rate limits depend on your subscription plan. Typical limits:
- 60 requests per hour (Free tier)
- 300 requests per hour (Pro tier)
- 1000+ requests per hour (Enterprise tier)

Check current limits in Settings → API.

### How much does each API call cost?

Costs are credit-based:
- Simple queries: 1-2 credits
- Standard analysis: 3-5 credits
- Complex multi-platform analysis: 5-10 credits

Credits are purchased in packs. See [adsroid.com/pricing](https://adsroid.com/pricing/) for current rates.

### Can I query specific date ranges?

Yes. Adsroid understands both relative ("last 7 days") and absolute ("January 1 to January 31") date references.

### Can I query specific campaigns or keywords?

Yes. Reference campaigns, ad groups, keywords, or other entities by name or ID in your query:

```
"Analyze performance of campaign 'Brand Search' for the last 30 days"
```

### Does Adsroid store my campaign data?

No. Adsroid retrieves data during analysis but doesn't store campaign-level data long-term. Only aggregated, anonymized patterns may be retained to improve the agent.

### Can multiple team members use the same API key?

Each organization has one API key that can be used by multiple team members or systems. For security, consider:
- Using environment variables to store the key
- Rotating keys if compromised
- Monitoring usage to detect unauthorized access

### Can I use Adsroid without coding?

Yes. Use Make or Zapier's native Adsroid integrations for no-code automation. These platforms provide visual workflow builders that don't require programming.

## Trust and Safety Questions

### How does Adsroid access my advertising accounts?

Through OAuth connections configured in the Adsroid interface. You grant read-only access to specific accounts. Adsroid cannot modify campaigns, budgets, or settings through its platform connections.

### Can Adsroid see data from accounts I haven't connected?

No. Adsroid can only access accounts explicitly connected to your project. Each project has isolated permissions.

### What if I want to revoke access?

Disconnect platforms in Project Settings → Connected Accounts. This immediately revokes Adsroid's access to that platform.

### Can Adsroid make changes to my campaigns?

Not through its native integrations. Adsroid connections are read-only. Changes can only happen if you:
1. Manually implement recommendations yourself
2. Build custom automation that uses Adsroid recommendations to trigger platform API actions

Even with custom automation, you control when and how recommendations are executed.

### How do I ensure recommendations are safe to implement?

Review confidence scores:
- **High confidence:** Pattern validated by historical data
- **Medium confidence:** Pattern present but limited validation
- **Low confidence:** Exploratory suggestion

For high-impact changes (large budget increases, campaign pauses), always validate recommendations against your own analysis before implementing.

### What happens if Adsroid makes a wrong recommendation?

Adsroid provides recommendations based on data analysis, but AI can misinterpret context. Always evaluate recommendations against your business knowledge. Key safeguards:

1. Adsroid never executes changes automatically
2. Confidence scores indicate uncertainty
3. Recommendations include reasoning you can evaluate
4. You retain final decision authority

### Can I give feedback on recommendations?

Not currently through the API, but feedback can be provided through the Adsroid web interface. This helps improve future recommendations.

## Integration Questions

### How do I integrate Adsroid with Make?

1. Install the Adsroid Make app: [make.com/app-invitation](https://www.make.com/en/hq/app-invitation/ae5b35dfaa22a58eb2cb950f57e80c8a)
2. Add Adsroid module to your scenario
3. Connect your Adsroid account (requires API key)
4. Configure organization ID, project ID, and query
5. Map responses to other modules

See [Using Adsroid with Make](https://adsroid.com/api/using-adsroid-with-make-no-code-automation/) for details.

### How do I integrate Adsroid with Zapier?

1. Install the Adsroid Zapier integration: [zapier.com/developer/public-invite](https://zapier.com/developer/public-invite/234304/ac2d85f2e9a7e6c9f5365aef31c70b11/)
2. Add Adsroid as an Action in your Zap
3. Connect your Adsroid account
4. Configure query and map output to other apps

See [Using Adsroid with Zapier](https://adsroid.com/api/using-adsroid-with-zapier/) for details.

### Can I use Adsroid with n8n?

Yes. Use n8n's HTTP Request node to call the Adsroid API directly. Configure:
- Method: POST
- URL: `https://rckoycauuwzdryvkjpac.supabase.co/functions/v1/adsroid`
- Headers: Authorization + Content-Type
- Body: JSON with org ID, project ID, message

### Can I build a custom dashboard that uses Adsroid?

Yes. Call the Adsroid API from your dashboard backend and display responses alongside other metrics. This creates an "AI insights" section in your existing dashboard.

### Can agencies use one API key for multiple clients?

Yes, but best practice is:
- Create separate projects for each client
- Use the same API key
- Specify different project IDs per client in API requests

This provides isolated analytics per client while centralizing billing.

## Performance and Limitations Questions

### What's the maximum query length?

Queries can be up to 500 words. Most effective queries are 10-50 words.

### Can Adsroid predict future performance?

Adsroid analyzes historical and current data but doesn't provide forecasting models. It can identify trends ("CTR declining") and recommend actions, but doesn't predict specific future metrics.

### Can Adsroid optimize bid strategies?

Adsroid can recommend bid strategy changes but doesn't manage bidding directly. Recommendations might include:
- "Switch from manual to automated bidding"
- "Target CPA too aggressive, increase by 20%"
- "Bid strategy performing well, no changes needed"

### Can Adsroid create new campaigns or ads?

No. Adsroid analyzes existing campaigns and can recommend campaign structures or targeting strategies, but you must create campaigns manually or through platform APIs.

### Does Adsroid understand my business context?

Adsroid analyzes performance metrics but doesn't have access to business-specific context (product margins, inventory, customer lifetime value) unless you include it in queries.

**Example contextual query:**
```
"Analyze campaign performance. Our target CPA is $25 and 
customer LTV is $400. Which campaigns should we scale?"
```

### Can Adsroid handle multiple currencies?

Yes. Adsroid recognizes currencies from your connected accounts. Specify currency preferences in queries if needed:

```
"Compare campaign performance. Convert all metrics to USD."
```

### What languages does Adsroid support?

Currently English only. Queries and responses are in English.

### Can Adsroid analyze competitor data?

No. Adsroid only accesses data from your connected advertising accounts. It cannot access competitor campaign data.

However, you can ask Adsroid to interpret performance changes that might indicate competitive activity:

```
"Impression share dropped 15% last week. Does this suggest 
competitive activity or internal issues?"
```

### Does Adsroid work with Google Ads scripts or automated rules?

Adsroid operates independently of Google Ads scripts and automated rules. You can use both simultaneously, but they don't communicate with each other. Consider coordinating automation:
- Use scripts for simple threshold-based actions
- Use Adsroid for complex analysis and strategic recommendations

## Comparison Questions

### Adsroid vs. Google Ads automated bidding?

**Google Ads automated bidding:**
- Executes bid changes automatically
- Uses machine learning to optimize towards target CPA/ROAS
- Limited to Google Ads platform

**Adsroid:**
- Analyzes performance across platforms
- Provides strategic recommendations with reasoning
- Never executes changes automatically
- Covers more than just bidding (budget, targeting, creative, structure)

**Use both:** Automated bidding handles real-time bid optimization. Adsroid provides strategic oversight and cross-platform analysis.

### Adsroid vs. Agency services?

**Agency:**
- Full-service: strategy, implementation, creative, reporting
- Expensive (typically 10-20% of ad spend + retainer)
- Human expertise and account management

**Adsroid:**
- Analysis and recommendations only
- Fixed credit-based pricing
- AI-powered insights at scale

**Best fit:** Use Adsroid to augment internal teams or reduce agency dependency for routine analysis. Agencies provide strategic value beyond what Adsroid covers.

### Adsroid vs. In-house data analysts?

**Data analyst:**
- Custom analysis tailored to business
- Strategic thinking and context awareness
- Can execute complex multi-system analysis
- Expensive (salary + tools)

**Adsroid:**
- Automated analysis at scale
- Fast turnaround (seconds vs. hours)
- Limited to advertising platform data
- Cost-effective for routine analysis

**Best fit:** Use Adsroid for daily/weekly routine analysis. Use analysts for strategic projects and complex business questions.

### Adsroid vs. Business intelligence tools?

**BI tools (Looker, Tableau, Power BI):**
- Visualize data
- Build custom dashboards
- Require manual metric selection and interpretation

**Adsroid:**
- Interprets data automatically
- Provides recommendations, not just visualizations
- Natural language interface (ask questions instead of building dashboards)

**Use both:** BI tools for ongoing metric tracking. Adsroid for ad-hoc analysis and recommendation generation.

## Troubleshooting Questions

### Why am I getting authentication errors?

**Common causes:**
1. Invalid API key (regenerate in Settings)
2. Incorrect Authorization header format (must be `Bearer YOUR_KEY`)
3. API key from wrong organization
4. API key regenerated but cached key still in use

### Why are responses slow?

**Common causes:**
1. Complex query requiring data from multiple platforms
2. Large date ranges (30+ days)
3. Many campaigns to analyze
4. Platform APIs responding slowly

**Solutions:**
- Break complex queries into smaller parts
- Use shorter date ranges initially
- Query specific campaigns instead of entire account

### Why is my query not returning relevant information?

**Common causes:**
1. Query too vague ("analyze performance" without context)
2. Requesting data for platforms not connected
3. Date range outside available data
4. Campaign/entity names misspelled

**Solutions:**
- Be specific: mention platform, timeframe, metric
- Verify connected accounts in project settings
- Use exact campaign names (or IDs)

### Why do recommendations seem generic?

If recommendations lack specificity, provide more context in your query:

**Generic query:**
```
"Optimize my campaigns"
```

**Specific query:**
```
"Analyze Google Ads campaigns from last 7 days. 
Target CPA is $30. Identify campaigns above target 
and recommend specific fixes."
```

## Future Roadmap Questions

### Will Adsroid add more platforms?

Platform expansion is planned. Priority is based on user demand. Provide feedback through [adsroid.com/contact](https://adsroid.com/contact/).

### Will Adsroid add predictive analytics?

Predictive capabilities are under consideration. Current focus is improving analysis of historical and current data.

### Will Adsroid support direct campaign execution?

No current plans. Adsroid's design philosophy is recommendation-first with explicit human approval for execution.

### Will Adsroid add custom reporting?

Structured reporting formats may be added in future. Currently, responses are natural language optimized for AI-to-human communication.

### Will there be a webhook-out feature?

Webhook-out (Adsroid pushes notifications) is under consideration. Current focus is webhook-in (external systems trigger Adsroid).

## Getting Help

### Where can I find examples?

See the [examples/](../examples/) directory in this repository for practical implementation guides.

### Where is the complete API reference?

Full API documentation: [adsroid.com/api/introduction-adsroid-api/](https://adsroid.com/api/introduction-adsroid-api/)

### How do I report bugs or issues?

Contact support through [adsroid.com/contact](https://adsroid.com/contact/) or email support@adsroid.com.

### Can I request features?

Yes. Submit requests through [adsroid.com/contact](https://adsroid.com/contact/) with "Feature Request" as the subject.

### Is there a community or forum?

Not currently. For help, contact support or consult the documentation.

### Do you offer implementation support?

Implementation support is available for Enterprise customers. Contact sales@adsroid.com for details.
