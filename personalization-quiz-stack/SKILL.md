---
name: personalization-quiz-stack
description: Manage a personalization quiz via Typeform and store results in Airtable. Use when building the quiz, processing responses, generating recommendations, or syncing quiz data. Triggers on quiz, typeform, airtable, personalization, recommendation, or any mention of the wellness quiz or customer intake form.
originally_built_for: [Your Brand]
---

> Originally developed for a supplement subscription business, now genericized for universal use.

# Typeform/Airtable Quiz Integration — [Your Brand]

## Overview

Build and manage the [Your Brand] personalization quiz using Typeform for the front-end and Airtable for data storage and recommendation generation. This is the core of Phase 0 — manual concierge with automated data capture.

## Authentication

Environment variables:
- `TYPEFORM_API_KEY` — Typeform personal access token
- `AIRTABLE_API_KEY` — Airtable personal access token
- `AIRTABLE_BASE_ID` — [Your Brand] base ID
- `AIRTABLE_TABLE_NAME` — Quiz responses table name

## Quiz Structure

### Section 1: Basic Profile

```
Q1. What's your name?
Q2. What's your email?
Q3. What's your age range?
   - Under 30
   - 30-39
   - 40-49
   - 50-59
   - 60+

Q4. What's your primary goal?
   - More energy
   - Better sleep
   - Stress management
   - Gut health
   - Hormone balance
   - Overall wellness
```

### Section 2: Lifestyle

```
Q5. How many hours of sleep do you get?
   - Less than 5
   - 5-6
   - 7-8
   - 9+

Q6. How would you describe your stress level?
   - Low
   - Moderate
   - High
   - Very high

Q7. Do you exercise regularly?
   - Daily
   - 3-4 times/week
   - 1-2 times/week
   - Rarely/Never

Q8. How's your diet?
   - Very healthy (whole foods, home cooked)
   - Mostly healthy
   - Average (mix of healthy and processed)
   - Could be better
```

### Section 3: Health Signals

```
Q9. Do you experience any of these? (Select all)
   - [ ] Low energy / fatigue
   - [ ] Digestive issues
   - [ ] Poor sleep
   - [ ] Stress / anxiety
   - [ ] Brain fog
   - [ ] Mood swings
   - [ ] None of the above

Q10. Are you interested in at-home lab testing?
    - Yes, very interested
    - Maybe later
    - No, not now
```

## Typeform Setup

### Create Quiz via API

```bash
curl -s -H "Authorization: Bearer $TYPEFORM_API_KEY" \
  -H "Content-Type: application/json" \
  -X POST "https://api.typeform.com/forms" \
  -d '{
    "title": "[Your Brand] Personalised Wellness Quiz",
    "settings": {
      "progress_bar": "percentage",
      "show_progress_bar": true
    },
    "fields": [
      {
        "type": "short_text",
        "title": "What\'s your name?",
        "ref": "name"
      },
      {
        "type": "email",
        "title": "What\'s your email?",
        "ref": "email"
      },
      {
        "type": "multiple_choice",
        "title": "What\'s your age range?",
        "ref": "age_range",
        "properties": {
          "choices": [
            {"label": "Under 30"},
            {"label": "30-39"},
            {"label": "40-49"},
            {"label": "50-59"},
            {"label": "60+"}
          ]
        }
      }
    ]
  }'
```

### Get Quiz Responses

```bash
curl -s -H "Authorization: Bearer $TYPEFORM_API_KEY" \
  "https://api.typeform.com/forms/FORM_ID/responses?page_size=50" \
  | jq '.items[] | {submitted_at, answers: [.answers[] | {field: .field.ref, answer: .text | .choice.label | .number}]}'
```

## Airtable Setup

### Store Quiz Response

```bash
curl -s -H "Authorization: Bearer $AIRTABLE_API_KEY" \
  -H "Content-Type: application/json" \
  -X POST "https://api.airtable.com/v0/$AIRTABLE_BASE_ID/$AIRTABLE_TABLE_NAME" \
  -d '{
    "fields": {
      "Name": "Customer Name",
      "Email": "customer@email.com",
      "Age Range": "40-49",
      "Primary Goal": "More energy",
      "Sleep Hours": "5-6",
      "Stress Level": "High",
      "Exercise": "1-2 times/week",
      "Diet": "Average",
      "Health Signals": ["Low energy", "Poor sleep"],
      "Lab Test Interest": "Maybe later",
      "Recommended Base": "Core Daily + Gut Microbe",
      "Recommended Add-Ons": ["Sleep Support", "Stress Relief"],
      "Status": "Pending Review",
      "Quiz Score": "85",
      "Notes": ""
    }
  }'
```

### Get Pending Recommendations

```bash
curl -s -H "Authorization: Bearer $AIRTABLE_API_KEY" \
  "https://api.airtable.com/v0/$AIRTABLE_BASE_ID/$AIRTABLE_TABLE_NAME?filterByFormula={Status}=\'Pending Review\'&maxRecords=50" \
  | jq '.records[] | {id, fields: {Name, Email, Recommended Base, Recommended Add-Ons, Status}}'
```

### Update Recommendation Status

```bash
curl -s -H "Authorization: Bearer $AIRTABLE_API_KEY" \
  -H "Content-Type: application/json" \
  -X PATCH "https://api.airtable.com/v0/$AIRTABLE_BASE_ID/$AIRTABLE_TABLE_NAME/RECORD_ID" \
  -d '{
    "fields": {
      "Status": "Recommendation Sent",
      "Notes": "Sent personalised email with supplement plan"
    }
  }'
```

## Recommendation Logic

### Base Product Selection

```python
def recommend_base(quiz_data):
    """Determine base product from quiz answers."""
    
    # Everyone starts with Core Daily + Gut Microbe
    base = "Core Daily + Gut Microbe"
    
    return base

def recommend_addons(quiz_data):
    """Determine add-ons from quiz answers."""
    
    addons = []
    
    # Sleep Support
    if quiz_data.get("sleep_hours") in ["Less than 5", "5-6"]:
        addons.append("Sleep Support")
    
    # Stress Relief
    if quiz_data.get("stress_level") in ["High", "Very high"]:
        addons.append("Stress Relief")
    
    # Recovery
    if quiz_data.get("exercise") in ["Daily", "3-4 times/week"]:
        addons.append("Recovery")
    
    # Vitality
    if quiz_data.get("primary_goal") == "More energy":
        addons.append("Vitality")
    
    # Hormone Balance
    if quiz_data.get("age_range") in ["40-49", "50-59"] and quiz_data.get("health_signals") and "Mood swings" in quiz_data.get("health_signals", []):
        addons.append("Hormone Balance")
    
    # Men\'s Testosterone
    # Only recommended based on additional signals, not default
    
    return addons[:3]  # Max 3 add-ons
```

## Email Template (Recommendation)

```html
Subject: Your [Your Brand] Personalised Plan is Ready

Hi {{name}},

Thanks for completing the [Your Brand] wellness quiz. Based on your responses, here's your personalised supplement plan:

**Your Base:**
Core Daily + Gut Microbe — [£XX]/month
30 sachets, one drink per day

{% if addons %}
**Your Add-Ons:**
{% for addon in addons %}
{{addon}} — [£XX]/month
{% endfor %}
{% endif %}

**Total:** £[XX]/month

**Why this plan?**
Based on your quiz, you mentioned {{primary_goal}} as your top priority. {{recommendation_reason}}

**Next Steps:**
1. Review your plan above
2. Start with a 7-day sample pack ([£XX]) or subscribe now
3. Receive your first delivery within 3-5 working days

[Start My Subscription] [Try Sample Pack First]

Questions? Just reply to this email.

[Founder Name]
Founder, [Your Brand]
```

## Webhook Integration

### Typeform → Airtable Webhook

Set up a webhook in Typeform to push responses to Airtable automatically:

```bash
curl -s -H "Authorization: Bearer $TYPEFORM_API_KEY" \
  -H "Content-Type: application/json" \
  -X POST "https://api.typeform.com/forms/FORM_ID/webhooks" \
  -d '{
    "url": "https://hooks.airtable.com/...",
    "enabled": true
  }'
```

### Alternative: Manual Sync

If webhook isn't available, poll Typeform responses every hour and sync to Airtable:

```bash
# Cron job or heartbeat
# Check for new responses, process, store in Airtable
```

## Rules

- Never make medical recommendations based on quiz
- All recommendations are "support" not "treatment"
- Include disclaimer: "This quiz provides general wellness suggestions, not medical advice"
- Flag responses that need human review (severe health signals, medication mentions)
- Send recommendation within 24 hours of quiz completion
- Track conversion: quiz → sample pack → subscription
- Keep quiz under 3 minutes to complete
