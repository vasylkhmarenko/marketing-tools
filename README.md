# Marketing Tools

A collection of Claude Code skills for marketing analysis and competitive intelligence.

## Skills

### analyze-quiz

Analyze competitor quiz funnels with comprehensive reporting.

**Usage:**

```
/analyze-quiz <quiz-url>
/analyze-quiz <quiz-url> --name "CompanyName"
/analyze-quiz <quiz-url> --format json
/analyze-quiz <quiz-url> --mobile-only
/analyze-quiz <url1> <url2> --compare
```

**Flags:**

| Flag              | Description                           |
| ----------------- | ------------------------------------- |
| `--name "Name"`   | Override company name                 |
| `--format <type>` | Output: `md`, `json`, `csv`, `html`   |
| `--mobile-only`   | Mobile viewport only (390x844)        |
| `--desktop-only`  | Desktop viewport only (1280x800)      |
| `--compare`       | Compare multiple quizzes side-by-side |

**Features:**

- Dual viewport capture (desktop + mobile) by default
- Smart obstacle handling (cookie banners, popups, slow pages)
- Multiple export formats (Markdown, JSON, CSV, HTML)
- Competitor comparison mode
- Resume capability for interrupted analysis

**What it analyzes:**

- Quiz structure and flow
- Personalization tactics
- Paywall and pricing strategy
- Psychological triggers
- Mobile experience differences
- Strengths and weaknesses

**Output:**

- Screenshots: `~/Downloads/quiz-analysis-[name]-[timestamp]/`
- Report: `~/Downloads/quiz-analysis-[name]-[timestamp].[format]`
- Comparison: `~/Downloads/quiz-comparison-[timestamp].md`

## Requirements

- [Claude Code](https://claude.ai/code) CLI
- Playwright MCP server (for browser automation)

## Installation

Add this repo to your Claude Code skills directory or symlink the skill folder.

## License

MIT
