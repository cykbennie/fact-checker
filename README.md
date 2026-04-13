# Fact Checker — Claude Code Skill

A [Claude Code](https://docs.anthropic.com/en/docs/claude-code) skill that systematically verifies document accuracy by cross-referencing claims against user-provided sources and web search.

## What it does

Given a document, the skill:

1. **Extracts** every verifiable claim — statistics, quotes, citations, dates, names, financial figures
2. **Verifies** each claim against user-supplied reference documents and/or web search
3. **Produces** a detailed Markdown report with verdicts and confidence levels
4. **Annotates** the original document, highlighting problems inline

### Supported formats

PDF, DOCX, Markdown, plain text, PPTX — input and annotated output.

### Claim types checked

| Type | Examples |
|------|----------|
| Numerical data | Statistics, percentages, dates, quantities, financial figures |
| Quotes | Attributed statements, paraphrased quotes |
| Citations | References to studies, reports, articles, books |
| Factual claims | Verifiable statements about the world |

Opinions, predictions, and subjective assessments are skipped.

## Install

```bash
claude skill install --path /path/to/fact-checker
```

Or manually copy the `fact-checker/` directory into `~/.claude/skills/`:

```bash
cp -r fact-checker ~/.claude/skills/
```

## Usage

Trigger the skill with natural language:

```
> fact-check this document: report.pdf
> verify the claims in slides.pptx against these sources: source1.pdf source2.docx
> check if the numbers in quarterly-results.md are accurate
```

The skill will:

1. Ask for the document to check (if not provided)
2. Ask for reference sources — PDFs, DOCX, text files, or your own knowledge
3. Search the web for anything the provided sources don't cover
4. Generate `verification-report.md` with a summary table and detailed findings
5. Generate an annotated copy of the original document with highlights

### Output

**verification-report.md:**

```
# Fact-Check Report

**Document:** report.pdf
**Total claims checked:** 12
**Summary:** 8 correct, 2 incorrect, 1 partially correct, 1 unverifiable

## Summary Table
| # | Claim | Type | Status | Confidence | Source |
|---|-------|------|--------|------------|--------|
| 1 | Revenue grew 23% | Numerical | Incorrect | High | Annual report |
...

## Detailed Findings
### Claim 1: Revenue growth
...
```

**Annotated document** — same format as input, with color-coded highlights:

- 🟡 Yellow — Partially correct
- 🔴 Red — Incorrect
- 🔵 Blue — Unverifiable

### Language matching

Reports and annotations are generated in the **same language as the input document**. Feed it a Chinese document, get a Chinese report.

## Verdict system

| Verdict | Meaning |
|---------|---------|
| Correct | Accurately reflects the source |
| Incorrect | Contradicts the source or is factually wrong |
| Partially correct | Mostly right but has a specific inaccuracy |
| Unverifiable | No reliable source found |

Each verdict includes a confidence level (High / Medium / Low) based on source authority.

## License

MIT
