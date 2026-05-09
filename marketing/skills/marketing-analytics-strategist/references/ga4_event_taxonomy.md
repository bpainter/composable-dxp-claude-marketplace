---
type: reference
project: skills-library
scope: skill
plugin: marketing
parent: "[[analytics-strategist]]"
tags: [type/reference, plugin/marketing, scope/skill, topic/marketing]
status: active
---
# GA4 Event Taxonomy and Implementation Guide

## Event Naming Conventions

### Rules
1. **Start with lowercase letter** (ga4 is case-sensitive)
2. **Use underscores for separation** (not spaces, hyphens, or camelCase)
3. **Be specific and descriptive** (not generic like "click" or "event")
4. **Align with business logic** (mirrors how product team thinks about actions)
5. **Avoid event explosion** (too many events = data quality issues)

### Naming Pattern
`[domain]_[entity]_[action]` or `[action]_[entity]`

Examples:
- `purchase_completed` (not "buy" or "transaction")
- `cart_item_added` (not "add" or "e-commerce_action")
- `video_started` (not "play")
- `user_invited` (not "invite")
- `feature_enabled` (not "toggle")
- `support_ticket_created` (not "create_ticket")

---

## Recommended Events (Use When Available)

GA4 has predefined events that integrate with Google Ads and other features. Use these when applicable:

### E-commerce Events
```javascript
// View item
gtag('event', 'view_item', {
  items: [{
    item_id: 'SKU123',
    item_name: 'Blue Shirt',
    price: 29.99,
    quantity: 1
  }]
});

// Add to cart
gtag('event', 'add_to_cart', {
  items: [{
    item_id: 'SKU123',
    item_name: 'Blue Shirt',
    price: 29.99,
    quantity: 1
  }]
});

// Purchase
gtag('event', 'purchase', {
  transaction_id: 'ORDER123',
  value: 59.98,
  currency: 'USD',
  items: [{
    item_id: 'SKU123',
    item_name: 'Blue Shirt',
    price: 29.99,
    quantity: 2
  }]
});
```

### Lead Gen Events
```javascript
// Sign up
gtag('event', 'sign_up', {
  method: 'email' // or 'google', 'facebook', etc.
});

// Generate lead (form submission)
gtag('event', 'generate_lead', {
  value: 0, // optional
  currency: 'USD' // optional
});

// Contact (email, phone, form)
gtag('event', 'contact', {
  method: 'email' // or 'phone', 'form'
});
```

### Engagement Events
```javascript
// Page view (auto-tracked, but can be custom)
gtag('event', 'page_view', {
  page_path: '/checkout',
  page_title: 'Checkout'
});

// Scroll
gtag('event', 'scroll', {
  percent_scrolled: 90
});

// Video engage
gtag('event', 'video_start', {
  video_title: 'Product Demo',
  video_url: '/videos/demo.mp4'
});
```

---

## Custom Events by Business Model

### SaaS / B2B Software

**Awareness & Acquisition**
- `webinar_registered`
- `demo_requested`
- `whitepaper_downloaded`
- `free_trial_started`

**Activation & Engagement**
- `account_created`
- `onboarding_completed`
- `first_feature_used` (feature_name: "reporting", "integrations", etc.)
- `dashboard_viewed`
- `api_key_generated`

**Retention**
- `feature_adoption` (feature_name: "advanced_analytics")
- `user_invited` (to workspace/team)
- `document_shared`

**Monetization**
- `upgrade_initiated`
- `payment_method_added`
- `subscription_purchased` (plan: "starter", "pro", "enterprise")
- `seat_added`

**Churn Risk**
- `feature_abandoned`
- `support_ticket_created` (reason: "bug", "feature_request")
- `unsubscribe_initiated`

### E-commerce / DTC

**Discovery**
- `search_performed` (search_term: "blue shirts")
- `filter_applied` (filter_type: "color", filter_value: "blue")
- `product_category_viewed`

**Consideration**
- `product_viewed` (item_id, item_name, price)
- `product_review_viewed`
- `size_guide_opened`
- `comparison_viewed` (comparing products)

**Purchase**
- `add_to_cart` (product, price, quantity)
- `remove_from_cart`
- `checkout_started`
- `promo_applied` (coupon_code)
- `purchase_completed` (value, currency, items)

**Post-Purchase**
- `order_status_checked`
- `return_initiated`
- `product_review_submitted`
- `referral_shared` (method: "email", "sms", "social")

### Content / Publishing

**Engagement**
- `article_opened` (article_id, article_title)
- `article_read_fully` (time_spent, article_id)
- `comment_posted` (on_article, on_video)
- `subscribe_to_newsletter`

**Viral**
- `content_shared` (content_type: "article", "video", platform: "twitter")
- `referral_signup` (referral_source: "friend_name")

### SaaS / Marketplace

**Transaction**
- `booking_completed` (event_type: "consultation")
- `transaction_completed` (item_type: "service")
- `payment_processed`

---

## Parameter Structure

### Standard Parameters (Always Include When Relevant)
```javascript
{
  // User / Session
  user_id: 'user_123',           // Authenticated user ID
  session_id: 'session_abc',     // Session identifier

  // Context
  page_location: window.location.href,
  page_title: document.title,

  // Custom
  item_id: 'SKU123',
  item_name: 'Blue Shirt',
  value: 29.99,
  currency: 'USD'
}
```

### Avoid Parameter Anti-Patterns
**Bad**: Too many unique parameters (one per action)
```javascript
gtag('event', 'form_submitted', {
  form_id_1_submitted: true,    // Bad: event explosion
  form_id_2_submitted: true,    // Bad: one event per form
  form_id_3_submitted: true     // Bad: unmaintainable
});
```

**Good**: Single event, parameterized by form type
```javascript
gtag('event', 'form_submitted', {
  form_type: 'contact',    // Better: reusable
  form_name: 'Contact Us'  // Better: semantic
});
```

---

## Registering Custom Parameters

To use custom parameters in GA4 reports, you must register them as custom dimensions (for text) or custom metrics (for numbers).

### Steps
1. Go to GA4 Admin → Custom Definitions
2. Click "Create Custom Dimension"
   - Name: "Form Type"
   - Parameter: "form_type"
   - Scope: Event
3. Click "Create Custom Metric" for numeric parameters
   - Name: "Form Submission Value"
   - Parameter: "form_value"
   - Unit: Currency

### Common Dimensions to Register
| Parameter | Type | Example |
|-----------|------|---------|
| `user_segment` | Dimension | "free", "paying", "enterprise" |
| `feature_name` | Dimension | "reporting", "integrations", "api" |
| `campaign_name` | Dimension | "summer_sale", "product_launch" |
| `form_type` | Dimension | "contact", "signup", "newsletter" |
| `error_type` | Dimension | "payment_failed", "invalid_email" |
| `plan_type` | Dimension | "starter", "pro", "enterprise" |

### Common Metrics to Register
| Parameter | Type | Example |
|-----------|------|---------|
| `revenue` | Metric | 99.99 (currency value) |
| `cart_value` | Metric | 199.50 |
| `form_completion_time` | Metric | 45 (seconds) |
| `feature_adoption_score` | Metric | 1-10 rating |

---

## Event Firing Patterns

### Fire on Button Click
```javascript
document.getElementById('signup-btn').addEventListener('click', () => {
  gtag('event', 'signup_button_clicked', {
    button_location: 'homepage_hero',
    user_status: 'anonymous'
  });
});
```

### Fire on Form Submission
```javascript
document.getElementById('contact-form').addEventListener('submit', (e) => {
  gtag('event', 'contact_form_submitted', {
    form_type: 'contact',
    form_name: 'Contact Us'
  });
  // form.submit() continues
});
```

### Fire on Page View (Next.js Router)
```javascript
import { useRouter } from 'next/router';
import { useEffect } from 'react';

export default function PageViewTracker() {
  const router = useRouter();

  useEffect(() => {
    const handleRouteChange = (url) => {
      gtag('event', 'page_view', {
        page_path: url,
        page_title: document.title
      });
    };

    router.events.on('routeChangeComplete', handleRouteChange);
    return () => router.events.off('routeChangeComplete', handleRouteChange);
  }, [router.events]);

  return null;
}
```

### Fire on Video Play
```javascript
document.getElementById('video').addEventListener('play', () => {
  gtag('event', 'video_started', {
    video_title: 'Product Demo',
    video_duration: 120 // seconds
  });
});
```

### Fire on Scroll (Milestone)
```javascript
window.addEventListener('scroll', () => {
  const scrollPercent = (window.scrollY / (document.body.scrollHeight - window.innerHeight)) * 100;

  if (scrollPercent === 50) {
    gtag('event', 'page_scroll_50_percent', {
      page_title: document.title
    });
  }
  if (scrollPercent === 100) {
    gtag('event', 'page_scroll_100_percent', {
      page_title: document.title
    });
  }
});
```

---

## Event Taxonomy by Company Stage

### Early Stage (Pre-PMF)
Focus on: activation, engagement, retention signals

**Minimal viable tracking:**
- `user_signup` (via email, google)
- `feature_used` (feature_name)
- `user_returned` (day 1, 7, 30)
- `support_contacted` (reason)

**Why**: Understand if users activate and come back. Validate product-market fit.

---

### Growth Stage (Post-PMF)
Focus: acquisition channels, monetization, churn prevention

**Core tracking:**
- `user_acquired` (source, campaign, medium)
- `feature_adopted` (feature_name, adoption_time)
- `upgrade_initiated` (from_plan, to_plan)
- `trial_ended` (converted: true/false)

**Why**: Understand acquisition ROI. Measure conversion funnel. Identify expansion revenue.

---

### Scale Stage (Large User Base)
Focus: attribution, LTV cohorts, feature engagement

**Advanced tracking:**
- `user_acquired` (detailed attribution)
- `feature_engagement` (time_spent, adoption_rate)
- `revenue_impacting_event` (feature, customer_segment)
- `churn_signal` (feature_abandonment, support_tickets)
- `upsell_opportunity` (usage_tier, seat_count)

**Why**: Optimize marketing spend by channel. Identify high-LTV user cohorts. Predict churn.

---

## Event Quality Checklist

For each event, ensure:
- [ ] Event name is clear and specific (not generic)
- [ ] Event fires reliably 100% of the time (no random failures)
- [ ] Parameters are always included (no missing data)
- [ ] Parameter values are consistent (same format, naming)
- [ ] Event doesn't fire more than once per action (no duplicates)
- [ ] Event respects user consent (only fires if user consented)
- [ ] Event is documented (what triggers it, what parameters, why it matters)
- [ ] Custom parameters are registered in GA4 (can report on them)
- [ ] Events are tested in Preview mode before production release

---

## Common Event Taxonomy Mistakes

1. **Event explosion**: Too many events (100+) = data quality issues, unmaintainable
2. **Generic names**: "click", "event", "action" (not descriptive)
3. **Inconsistent parameters**: Same event with different parameter names
4. **Missing parameters**: Events fire without key context
5. **No registration**: Custom parameters used but not registered as dimensions/metrics
6. **Duplicate firing**: Event fires multiple times per action (GTM + direct tag)
7. **No documentation**: Only you know why events exist; team can't maintain
8. **Consent ignored**: Events fire even when user hasn't consented
