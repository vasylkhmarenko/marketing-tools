---
name: analyze-quiz
description: Analyze quiz funnels for competitive intelligence. Navigates through entire quiz, captures every screen, and generates comprehensive analysis report with structure, personalization, paywall analysis, and strengths/weaknesses.
---

## Usage

```
/analyze-quiz <quiz-url>
/analyze-quiz <quiz-url> --name "CompanyName"
```

## Instructions

You are analyzing a quiz funnel for competitive intelligence. Follow this process exactly.

### Phase 1: Setup

1. Extract company name from URL or `--name` parameter (default to domain name)
2. Create timestamp: `YYYY-MM-DD-HHmm`
3. Create output directory using Bash:
   ```
   mkdir -p ~/Downloads/quiz-analysis-[name]-[timestamp]
   ```
4. Initialize a working notes variable to track screen data

### Phase 2: Quiz Navigation

Navigate to the quiz URL using `browser_navigate`.

For each screen, repeat until you reach the paywall/final offer:

1. **Capture screenshot**:

   ```
   browser_take_screenshot with filename: ~/Downloads/quiz-analysis-[name]-[timestamp]/[##]-[screen-type].png
   ```

   Use sequential numbering: 01, 02, 03...
   Screen types: landing, question, email-capture, loading, results, paywall, upsell

2. **Capture accessibility snapshot**:

   ```
   browser_snapshot
   ```

   Extract: question text, answer options, button labels, progress indicators

3. **Record screen data** in your working notes:
   - Screen number
   - Question/content
   - Answer options (if any)
   - Question type (multiple choice, slider, text input, etc.)
   - UI elements noted

4. **Handle special inputs**:
   - Email field → type `test@example.com`
   - Name field → type `Test User`
   - Phone field → type `555-0100`
   - Age/number fields → use reasonable values (30, etc.)

5. **Navigate to next screen**:
   Look for CTA buttons in this priority order:
   - Explicit: "Next", "Continue", "Get Started", "Start Quiz", "Get Results", "See Results"
   - Arrows: "→", "❯", right-arrow icons
   - Submit buttons on forms
   - Any prominent button that advances the flow

   Use `browser_click` on the identified element.

6. **Wait for page transition**:
   Use `browser_wait_for` with time: 1-2 seconds for animations

7. **Detect endpoint**:
   Stop navigation when you see:
   - Pricing/payment information
   - Checkout form
   - "Subscribe", "Buy", "Purchase" CTAs
   - Plan comparison tables
   - Credit card input fields

### Phase 3: Analysis

After capturing all screens, analyze your collected data:

**A. Quiz Structure**

- Total number of screens
- Number of actual questions vs informational screens
- Question types used (list them)
- Progress indicator style (percentage, steps, bar, none)
- Estimated completion time
- Any branching/personalization indicators

**B. Screen-by-Screen Summary**
Create a table:
| # | Screenshot | Type | Content Summary | Options/Input | Notes |
|---|------------|------|-----------------|---------------|-------|

**C. Personalization Analysis**

- What personal data is collected (list all fields)
- How is personalization promised to the user?
- "Calculating/analyzing" screen tactics (what do they show?)
- How does the results/offer page use collected data?
- Personalized copy patterns observed

**D. Paywall/Offer Analysis**

- Pricing structure (plans, prices, durations)
- Price anchoring tactics (crossed out prices, "was/now")
- Urgency/scarcity elements (timers, limited spots, etc.)
- Trial offers (free trial, money-back guarantee)
- Social proof on paywall (testimonials, user count, ratings)
- Trust signals (guarantees, security badges, payment icons)
- CTA button copy and design
- Multiple payment options shown?
- Upsells or downsells present?

**E. Psychological Tactics Identified**

- Commitment/consistency (small yeses leading to big yes)
- Social proof usage (where and how)
- Authority signals (experts, certifications, press logos)
- Scarcity/urgency (real or artificial)
- Loss aversion triggers
- Personalization/relevance hooks
- Sunk cost indicators (progress bars, time invested)

**F. Key Strengths**
List 5-10 things they do well:

- Strong UX patterns
- Clever copy techniques
- Effective design choices
- Smart psychological triggers
- Worth adopting for your own quizzes

**G. Key Weaknesses**
List 5-10 improvement opportunities:

- Friction points in the flow
- Missing trust elements
- Confusing or unclear screens
- Weak paywall elements
- Poor mobile considerations
- Missed personalization opportunities

### Phase 4: Report Generation

Write the final markdown report to `~/Downloads/quiz-analysis-[name]-[timestamp].md`:

```markdown
# Quiz Funnel Analysis: [Company Name]

**URL:** [quiz-url]
**Analyzed:** [date]
**Total Screens:** [count]

## Executive Summary

[2-3 paragraph overview of the quiz, its approach, and key findings]

## Quiz Structure

[Section A findings]

## Screen-by-Screen Breakdown

[Section B table with screenshot references like ![Screen 1](./quiz-analysis-[name]-[timestamp]/01-landing.png)]

## Personalization Strategy

[Section C findings]

## Paywall & Offer Analysis

[Section D findings]

## Psychological Tactics

[Section E findings]

## Strengths

[Section F as bullet points]

## Weaknesses & Opportunities

[Section G as bullet points]

## Key Takeaways

[3-5 actionable insights for competitive advantage]
```

### Output Confirmation

After generating the report, confirm to the user:

- Report location: `~/Downloads/quiz-analysis-[name]-[timestamp].md`
- Screenshots folder: `~/Downloads/quiz-analysis-[name]-[timestamp]/`
- Number of screens captured
- Brief summary of key findings

## Error Handling

- If quiz requires account/login: Stop and report, note what screens were captured
- If quiz has CAPTCHA: Stop and report
- If navigation gets stuck: Try alternative button selectors, report if still stuck
- If paywall requires payment to continue: Stop at paywall (expected behavior)

## Tips

- Some quizzes have loading/animation screens - wait for them to complete
- Watch for A/B test indicators in URLs (variant=, ab=, etc.) and note them
- If options seem to branch the quiz, note which path was taken
- Capture any exit-intent popups or special offers that appear
