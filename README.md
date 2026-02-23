# Marketing Tools

A collection of Claude Code skills for marketing analysis and competitive intelligence.

## Skills

### analyze-quiz

Analyze competitor quiz funnels with comprehensive reporting.

**Usage:**

```
/analyze-quiz <quiz-url>
/analyze-quiz <quiz-url> --name "CompanyName"
```

**What it does:**

- Navigates through the entire quiz funnel automatically
- Captures screenshots of every screen
- Analyzes quiz structure, personalization tactics, and paywall design
- Identifies psychological tactics and conversion techniques
- Generates a detailed markdown report with strengths/weaknesses

**Output:**

- Screenshots saved to `~/Downloads/quiz-analysis-[name]-[timestamp]/`
- Full analysis report at `~/Downloads/quiz-analysis-[name]-[timestamp].md`

**Report includes:**

- Executive summary
- Screen-by-screen breakdown with screenshots
- Personalization strategy analysis
- Paywall & offer analysis
- Psychological tactics identified
- Strengths and weaknesses
- Actionable takeaways

## Requirements

- [Claude Code](https://claude.ai/code) CLI
- Playwright MCP server (for browser automation)

## Installation

1. Clone this repo or add it to your Claude Code skills directory
2. Ensure the skill is symlinked or accessible to Claude Code

## License

MIT
