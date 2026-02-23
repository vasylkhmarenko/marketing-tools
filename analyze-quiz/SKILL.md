---
name: analyze-quiz
description: Analyze quiz funnels for competitive intelligence. Navigates through entire quiz, captures every screen, and generates comprehensive analysis report with structure, personalization, paywall analysis, and strengths/weaknesses.
---

## Usage

```
/analyze-quiz <quiz-url>
/analyze-quiz <quiz-url> --name "CompanyName"
/analyze-quiz <quiz-url> --format json
/analyze-quiz <quiz-url> --mobile-only
/analyze-quiz <quiz-url> --desktop-only
/analyze-quiz <url1> <url2> --compare
```

### Flags

| Flag              | Description                                            |
| ----------------- | ------------------------------------------------------ |
| `--name "Name"`   | Override company name (default: extracted from domain) |
| `--format <type>` | Output format: `md` (default), `json`, `csv`, `html`   |
| `--mobile-only`   | Capture mobile viewport only (390x844)                 |
| `--desktop-only`  | Capture desktop viewport only (1280x800)               |
| `--compare`       | Compare multiple quiz URLs side-by-side                |

**Default behavior:** Captures both desktop and mobile viewports for comprehensive analysis.

## Instructions

You are analyzing a quiz funnel for competitive intelligence. Follow this process exactly.

### Phase 1: Setup

1. Parse arguments:
   - Extract company name from URL or `--name` parameter (default to domain name)
   - Check for `--format` flag (default: `md`)
   - Check for `--mobile-only` or `--desktop-only` flags (default: both viewports)
   - Check for `--compare` flag with multiple URLs

2. Create timestamp: `YYYY-MM-DD-HHmm`

3. Create output directory using Bash:

   ```
   mkdir -p ~/Downloads/quiz-analysis-[name]-[timestamp]
   ```

4. Initialize working notes to track:
   - Screen data for each viewport
   - Dismissed popups/banners
   - Any errors or stuck points

5. Set initial viewport:
   ```
   browser_resize with width: 1280, height: 800
   ```

### Phase 2: Robustness Setup

Before starting quiz navigation, prepare for common obstacles:

**Cookie Banner Dismissal Patterns:**
When you encounter cookie/consent banners, look for and click these buttons (in order):

- "Accept", "Accept All", "Accept Cookies"
- "Got it", "OK", "I understand"
- "Allow", "Allow All"
- "Agree", "I Agree"
- Close/X buttons on the banner

**Popup/Modal Handling:**

- If a modal/overlay appears before the quiz starts, look for close buttons (X, "Close", "No thanks", "Maybe later")
- Capture exit-intent popups as bonus screenshots: `[##]-popup-exit-intent.png`
- If a popup offers a discount/special, note it in the analysis

**Slow Page Handling:**

- After navigation, wait 2-3 seconds for page to stabilize
- If elements aren't loading, wait additional 2 seconds
- If still stuck after 3 attempts, note the issue and try to proceed

### Phase 3: Quiz Navigation

Navigate to the quiz URL using `browser_navigate`.

**For each screen, repeat until you reach the paywall/final offer:**

#### Step 1: Handle Obstacles First

Check for and dismiss:

- Cookie banners (see patterns above)
- Newsletter popups
- Chat widgets blocking content
- Any overlay that obscures the quiz

#### Step 2: Capture Screenshots

**Desktop capture (if not --mobile-only):**

```
browser_resize with width: 1280, height: 800
browser_wait_for with time: 1
browser_take_screenshot with filename: ~/Downloads/quiz-analysis-[name]-[timestamp]/[##]-[screen-type]-desktop.png
```

**Mobile capture (if not --desktop-only):**

```
browser_resize with width: 390, height: 844
browser_wait_for with time: 1
browser_take_screenshot with filename: ~/Downloads/quiz-analysis-[name]-[timestamp]/[##]-[screen-type]-mobile.png
```

Use sequential numbering: 01, 02, 03...
Screen types: landing, question, email-capture, loading, results, paywall, upsell, popup

#### Step 3: Capture Accessibility Snapshot

```
browser_snapshot
```

Extract: question text, answer options, button labels, progress indicators

#### Step 4: Record Screen Data

In your working notes, track:

- Screen number
- Question/content
- Answer options (if any)
- Question type (multiple choice, slider, text input, etc.)
- UI elements noted
- Mobile vs desktop differences observed

#### Step 5: Handle Special Inputs

- Email field → type `test@example.com`
- Name field → type `Test User`
- Phone field → type `555-0100`
- Age/number fields → use reasonable values (30, etc.)

#### Step 6: Navigate to Next Screen

Look for CTA buttons in this priority order:

1. Explicit: "Next", "Continue", "Get Started", "Start Quiz", "Get Results", "See Results"
2. Arrows: "→", "❯", right-arrow icons
3. Submit buttons on forms
4. Any prominent button that advances the flow

**Fallback selectors if primary fails:**

- Look for `button` elements with action-oriented text
- Look for elements with `role="button"`
- Look for links styled as buttons
- Try clicking answer options directly (some quizzes auto-advance)

Use `browser_click` on the identified element.

#### Step 7: Wait for Page Transition

```
browser_wait_for with time: 2
```

If page appears to still be loading (spinners, skeleton screens), wait additional 2 seconds.

#### Step 8: Detect Endpoint

Stop navigation when you see:

- Pricing/payment information
- Checkout form
- "Subscribe", "Buy", "Purchase" CTAs
- Plan comparison tables
- Credit card input fields

### Phase 4: Analysis

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

**F. Mobile Experience Analysis**

- Layout differences between desktop and mobile
- Touch-friendly elements (button sizes, tap targets)
- Content prioritization changes
- Any features hidden/shown on mobile
- Mobile-specific friction points
- Scroll depth required on mobile

**G. Key Strengths**

List 5-10 things they do well:

- Strong UX patterns
- Clever copy techniques
- Effective design choices
- Smart psychological triggers
- Worth adopting for your own quizzes

**H. Key Weaknesses**

List 5-10 improvement opportunities:

- Friction points in the flow
- Missing trust elements
- Confusing or unclear screens
- Weak paywall elements
- Poor mobile considerations
- Missed personalization opportunities

### Phase 5: Report Generation

Generate report based on `--format` flag:

#### Markdown Format (default)

Write to `~/Downloads/quiz-analysis-[name]-[timestamp].md`:

```markdown
# Quiz Funnel Analysis: [Company Name]

**URL:** [quiz-url]
**Analyzed:** [date]
**Total Screens:** [count]
**Viewports:** Desktop (1280x800) & Mobile (390x844)

## Executive Summary

[2-3 paragraph overview of the quiz, its approach, and key findings]

## Quiz Structure

[Section A findings]

## Screen-by-Screen Breakdown

[Section B table with screenshot references]

Desktop: ![Screen 1](./quiz-analysis-[name]-[timestamp]/01-landing-desktop.png)
Mobile: ![Screen 1](./quiz-analysis-[name]-[timestamp]/01-landing-mobile.png)

## Personalization Strategy

[Section C findings]

## Paywall & Offer Analysis

[Section D findings]

## Psychological Tactics

[Section E findings]

## Mobile Experience

[Section F findings]

## Strengths

[Section G as bullet points]

## Weaknesses & Opportunities

[Section H as bullet points]

## Key Takeaways

[3-5 actionable insights for competitive advantage]
```

#### JSON Format

Write to `~/Downloads/quiz-analysis-[name]-[timestamp].json`:

```json
{
  "meta": {
    "company": "[Company Name]",
    "url": "[quiz-url]",
    "analyzed": "[ISO timestamp]",
    "totalScreens": 12,
    "viewports": ["desktop", "mobile"]
  },
  "screens": [
    {
      "number": 1,
      "type": "landing",
      "screenshots": {
        "desktop": "01-landing-desktop.png",
        "mobile": "01-landing-mobile.png"
      },
      "content": "Welcome headline text",
      "options": [],
      "questionType": null,
      "notes": "Strong hero image"
    }
  ],
  "analysis": {
    "structure": {
      "totalScreens": 12,
      "questions": 8,
      "informational": 4,
      "questionTypes": ["multiple-choice", "slider", "text-input"],
      "progressStyle": "percentage",
      "estimatedTime": "3-4 minutes"
    },
    "personalization": {
      "dataCollected": ["name", "email", "goal", "experience-level"],
      "personalizationPromises": ["Custom plan", "Personalized results"],
      "loadingScreenTactics": "Animated progress with persona matching"
    },
    "paywall": {
      "plans": [
        { "name": "Monthly", "price": 29.99, "duration": "month" },
        {
          "name": "Annual",
          "price": 99.99,
          "duration": "year",
          "savings": "72%"
        }
      ],
      "anchoring": true,
      "urgency": ["countdown-timer", "limited-spots"],
      "trial": "7-day free trial",
      "socialProof": ["testimonials", "user-count"],
      "trustSignals": ["money-back-guarantee", "secure-checkout"]
    },
    "psychology": {
      "commitmentConsistency": true,
      "socialProof": true,
      "authority": ["expert-badges", "press-logos"],
      "scarcity": "artificial",
      "lossAversion": true
    },
    "mobile": {
      "responsive": true,
      "touchFriendly": true,
      "contentPrioritization": "good",
      "frictionPoints": ["small-close-button"]
    }
  },
  "strengths": [
    "Clean, distraction-free design",
    "Strong personalization hooks"
  ],
  "weaknesses": ["Paywall lacks social proof", "Mobile scroll depth too high"],
  "takeaways": [
    "Adopt their progress indicator style",
    "Add more trust signals to paywall"
  ]
}
```

#### CSV Format

Write to `~/Downloads/quiz-analysis-[name]-[timestamp].csv`:

```csv
screen_number,type,content_summary,question_type,options,desktop_screenshot,mobile_screenshot,notes
1,landing,"Welcome to the quiz",null,"",01-landing-desktop.png,01-landing-mobile.png,"Strong hero"
2,question,"What is your goal?",multiple-choice,"Lose weight|Build muscle|Stay healthy",02-question-desktop.png,02-question-mobile.png,""
```

#### HTML Format

Write to `~/Downloads/quiz-analysis-[name]-[timestamp].html`:

Generate a self-contained HTML file with:

- Embedded CSS for styling
- Base64-encoded screenshots inline
- Collapsible sections for each analysis area
- Side-by-side desktop/mobile screenshot comparison
- Table of contents with jump links

Use this structure:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Quiz Analysis: [Company Name]</title>
    <style>
      /* Clean, professional styling */
      body {
        font-family: -apple-system, system-ui, sans-serif;
        max-width: 1200px;
        margin: 0 auto;
        padding: 20px;
      }
      .screenshot-comparison {
        display: grid;
        grid-template-columns: 1fr 1fr;
        gap: 20px;
      }
      .screenshot-comparison img {
        width: 100%;
        border: 1px solid #ddd;
        border-radius: 8px;
      }
      details {
        margin: 20px 0;
        padding: 15px;
        background: #f9f9f9;
        border-radius: 8px;
      }
      summary {
        cursor: pointer;
        font-weight: bold;
        font-size: 1.2em;
      }
      table {
        width: 100%;
        border-collapse: collapse;
      }
      th,
      td {
        padding: 10px;
        border: 1px solid #ddd;
        text-align: left;
      }
      .strength {
        color: #22863a;
      }
      .weakness {
        color: #cb2431;
      }
    </style>
  </head>
  <body>
    <!-- Full report content with embedded screenshots -->
  </body>
</html>
```

To embed screenshots as base64, read each PNG file and convert to data URI format.

### Phase 6: Comparison Mode (--compare)

When multiple URLs are provided with `--compare`:

1. Run Phase 1-5 for each quiz URL sequentially
2. Store analysis data for each quiz

3. Generate comparison report `~/Downloads/quiz-comparison-[timestamp].md`:

```markdown
# Quiz Funnel Comparison

**Analyzed:** [date]
**Quizzes Compared:** [count]

## Overview Comparison

| Metric         | [Company 1] | [Company 2] | [Company 3] |
| -------------- | ----------- | ----------- | ----------- |
| Total Screens  | 12          | 8           | 15          |
| Questions      | 8           | 5           | 10          |
| Est. Time      | 3-4 min     | 2 min       | 5 min       |
| Progress Style | Percentage  | Steps       | Bar         |
| Email Capture  | Screen 3    | Screen 2    | Screen 5    |
| Paywall Price  | $29/mo      | $19/mo      | $39/mo      |

## Pricing Comparison

[Detailed pricing table with all plans, trials, anchoring tactics]

## Common Patterns

[What tactics all quizzes share]

## Unique Tactics

### [Company 1]

- [Unique approach they use]

### [Company 2]

- [Unique approach they use]

## Best Practices Observed

- **Best Paywall:** [Company] - [Why]
- **Best Personalization:** [Company] - [Why]
- **Best Mobile Experience:** [Company] - [Why]
- **Best Progress Indicator:** [Company] - [Why]
- **Best Social Proof:** [Company] - [Why]

## Recommendations

[What to adopt from each competitor]

## Individual Reports

- [Company 1]: [link to individual report]
- [Company 2]: [link to individual report]
```

### Output Confirmation

After generating the report, confirm to the user:

- Report location: `~/Downloads/quiz-analysis-[name]-[timestamp].[format]`
- Screenshots folder: `~/Downloads/quiz-analysis-[name]-[timestamp]/`
- Number of screens captured
- Viewports captured (desktop, mobile, or both)
- Format generated
- Brief summary of key findings

If comparison mode:

- Comparison report location
- Individual report locations

## Error Handling

- **Cookie banners won't dismiss:** Note in report, proceed with quiz
- **Quiz requires account/login:** Stop and report, note what screens were captured
- **Quiz has CAPTCHA:** Stop and report
- **Navigation gets stuck:** Try fallback selectors (3 attempts), then save progress and report partial results
- **Paywall requires payment:** Stop at paywall (expected behavior)
- **Popup won't close:** Capture it as a screen, try Escape key, proceed if possible
- **Slow loading:** Wait up to 10 seconds per screen, note slow screens in report
- **Viewport resize fails:** Continue with current viewport, note in report

## Resume Capability

If the skill stops unexpectedly:

1. Check existing screenshots in output folder
2. Resume from last captured screen number
3. Note interruption in final report

## Tips

- Some quizzes have loading/animation screens - wait for them to complete
- Watch for A/B test indicators in URLs (variant=, ab=, etc.) and note them
- If options seem to branch the quiz, note which path was taken
- Capture any exit-intent popups or special offers that appear
- For mobile captures, note if quiz has a dedicated mobile app prompt
- Check if quiz behavior differs between desktop and mobile (different questions, shorter flow)
