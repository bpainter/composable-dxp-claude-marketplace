---
type: reference
project: skills-library
scope: skill
plugin: consulting
parent: "[[personalization-strategist]]"
tags: [type/reference, plugin/consulting, scope/skill, topic/consulting]
status: active
---
# Audience Segmentation Framework for Demand Generation

## Segmentation Dimensions

### 1. Firmographic Attributes
- **Industry/Vertical**: Enterprise SaaS, Healthcare, Financial Services, etc.
- **Company Size**: SMB (1-100), Mid-Market (101-1000), Enterprise (1000+)
- **Revenue/Growth Stage**: Series A, Growth, Maturity
- **Geography**: Regional preferences, localization needs
- **Technology Stack**: Existing tools, modernization readiness

### 2. Behavioral Signals
- **Website Engagement**: Pages visited, time on site, return visit frequency
- **Feature Interest**: Pricing page views, demo requests, whitepaper downloads
- **Campaign Interactions**: Email opens, link clicks, webinar attendance
- **Account Activity**: Inbound requests, referral source, content consumption patterns
- **Purchase Readiness**: High-intent signals (demo requests, competitor research, RFP downloads)

### 3. Intent Signals
- **Search Behavior**: Keywords researched, trend keywords (if searchable)
- **Reverse-IP Intelligence**: Company identification via IP address (Demandbase, Clearbit)
- **Intent Data Platforms**: Third-party intent signals (buyer research activity)
- **Social & Content Signals**: LinkedIn engagement, blog comments, forum activity
- **CRM Indicators**: Lead score, contact properties, engagement history

### 4. Lifecycle Stage
- **Awareness**: Early-stage researchers, content consumers, no immediate buying timeline
- **Consideration**: Evaluating solutions, comparing vendors, engaging with demos
- **Decision**: RFP stage, final vendor evaluations, negotiation phase
- **Customer**: Post-purchase, expansion/upsell, retention and advocacy

## Segmentation Strategy Template

| Segment Name | Firmographics | Behaviors | Intent Signals | Lifecycle | Messaging Hook | Variant CTA |
|--------------|--------------|-----------|---|----------|---|---|
| **Enterprise Growth Stage** | $100M+ revenue, 500+ employees, Tech-forward | VIP pages, Pricing research 5+ visits, Feature exploration | Reverse-IP match, intent data shows competitive research | Decision | Scalability, security, enterprise support | Schedule demo |
| **Mid-Market Evaluators** | $10-100M, 100-500 employees, Selective tech adoption | Solution page, 3-5 visits, pricing hesitancy | IP match + email domain enrichment | Consideration | ROI calculators, peer case studies, cost comparison | View pricing |
| **SMB Price-Sensitive** | <$10M, <100 employees, Budget-conscious | Pricing page, resources, self-serve content, low engagement | First-party data only (ethical targeting) | Awareness/Consideration | Affordability, ease of use, quick ROI | Start free trial |
| **High-Intent Competitors' Customers** | Known competitor users (via enrichment API) | Demo pages, competitor comparison content, RFP signals | Third-party intent + behavioral | Decision | Migration support, feature parity messaging | Talk to expert |

## Implementation Approach

1. **Start with High-Value Segments**: Identify 2-3 segments with highest business impact
2. **Data Audit**: Confirm available data in CRM, analytics, intent platforms
3. **Hypothesis per Segment**: Define what messaging/experience each segment needs to convert
4. **Content Variant Mapping**: Map existing content to segments; identify gaps
5. **Technical Enablement**: Choose platform(s) to activate segmentation (HubSpot smart content, Contentful variants, etc.)
6. **Test & Learn**: Run initial A/B test, measure uplift, expand successful variants
7. **Scale with Privacy**: Ensure first-party data focus, consent compliance, transparent targeting
