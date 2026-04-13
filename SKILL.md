---
name: fact-checker
description: Fact-check documents by verifying claims against user-provided sources AND automatic web search. Checks numerical data, quotes, citations, and factual claims. Generates a detailed Markdown report with findings and an annotated version of the original document highlighting problems. Works with PDF, DOCX, Markdown, plain text, and PPTX files. Output is in the same language as the input document. Use this skill whenever the user wants to: fact-check a document, verify claims or data, check citations, validate numbers, confirm quotes, review accuracy, or audit a report for errors. Trigger on phrases like "verify this", "check if this is accurate", "fact check", "is this correct", "validate the data", or when the user shares a document and asks about its accuracy.
---

# Fact Checker

A systematic workflow for verifying document accuracy against source materials.

## What this skill checks

Focus on these claim types:
- **Numerical data** — Statistics, percentages, dates, quantities, financial figures
- **Quotes** — Attributed statements, paraphrased quotes
- **Citations** — References to studies, reports, articles, books
- **Factual claims** — Verifiable statements about the world (not opinions or interpretations)

Skip: Opinions, subjective assessments, predictions, hypotheticals, and obvious common knowledge.

## Workflow

### Step 1: Receive and parse the document

1. Ask the user for the document to check
2. Identify the document format (PDF, DOCX, Markdown, plain text, PPTX)
3. Extract the full text content, preserving structure (headings, paragraphs, tables)

### Step 2: Gather source materials

Ask the user for sources to verify against:
- **Internal documents** — PDF, DOCX, Markdown, PPTX, plain text files
- **Personal know-how** — Text the user provides directly in conversation
- **Web search** — Automatically used for claims not covered by provided sources

Clarify with the user:
- Any domains or sources to prefer or avoid
-  Whether certain claims should be prioritized

### Step 3: Extract and verify claims

Read through the document and identify each verifiable claim. For each claim:
- **Claim text** — The exact statement from the document
- **Claim type** — Numerical data, quote, citation, or factual claim
- **Location** — Where in the document (section, paragraph, or line number)
- **Context** — Surrounding text if needed for interpretation

Verify each claim against sources:
1. **Search provided sources first** — Look for supporting or contradicting evidence
2. **If not found** — Search the web for authoritative sources
3. **Assess correctness**:
   - **Correct** — Accurately reflects the source
   - **Incorrect** — Contradicts the source or is factually wrong
   - **Partially correct** — Contains some inaccuracy (wrong number, misattribution, out-of-context)
   - **Unverifiable** — No reliable source found

4. **Assign confidence level**:
   - **High** — Direct match from authoritative source
   - **Medium** — Indirect evidence or less authoritative source
   - **Low** — Weak evidence, conflicting sources, or source has unclear authority

5. **Document findings**:
   - The source used for verification
   - Any problems identified
   - Suggested correction (if applicable)

### Step 4: Generate the fact-checker report

Create a Markdown report with this structure:

```markdown
# Fact-Check Report

**Document:** [filename]
**Date:** [date]
**Total claims checked:** [number]
**Summary:** [X correct, Y incorrect, Z partially correct, W unverifiable]

---

## Summary Table

| # | Claim | Type | Status | Confidence | Source |
|---|-------|------|--------|------------|--------|
| 1 | [claim text] | Numerical | Correct | High | [source] |
| 2 | [claim text] | Quote | Incorrect | High | [source] |

---

## Detailed Findings

### Claim 1: [brief description]

**Location:** [section/paragraph]
**Claim text:** [exact text]
**Type:** [Numerical data / Quote / Citation / Factual claim]
**Status:** [Correct / Incorrect / Partially correct / Unverifiable]
**Confidence:** [High / Medium / Low]
**Source:** [where verified]

**Problem:** [description of issue, if any]

**Suggestion:** [recommended correction, if any]

---

[Repeat for each claim]
```

### Step 5: Generate the annotated document

Create an annotated version of the original document:

**For Markdown/plain text:**
- Use `==highlighted text==` for problems
- Add inline comments: `<!-- SUGGESTION: [correction] -->`
- Or use footnotes with `[problematic text]^1` and list corrections at the end

**For DOCX:**
- Use highlighting or colored text for problematic sections
- Add comments in the margin with suggested corrections

**For PDF:**
- Create an annotated version with highlights and comment bubbles
- If annotation is not feasible, create a separate annotation summary document

**Annotation key:**
- 🟡 Yellow highlight — Partially correct
- 🔴 Red highlight — Incorrect
- 🔵 Blue highlight — Unverifiable (needs review)

## Handling conflicting sources

When multiple sources disagree:
1. **Prefer primary sources** — Original research papers, official statements, legal documents over news articles or summaries
2. **Prefer more recent sources** — Newer data supersedes older data
3. **Prefer authoritative sources** — Government agencies, academic institutions, established publications over blogs or social media
4. **Note the conflict** — If sources genuinely disagree, mark as "Unverifiable" and explain the conflicting evidence in your report

## Verifying tables and structured data

For tables, charts, and structured 
- Verify each cell or data point individually
- Check that totals and subtotals are calculated correctly
- Verify that percentages add up correctly
- Check that data in charts matches the underlying numbers
- Note any inconsistencies between text descriptions and table data

## Output language

The report and annotations should be in the **same language as the original document**.

## Tips for accuracy

1. **Quote verification** — Check exact wording, speaker attribution, and context
2. **Numerical data** — Verify the number itself, units, time period, and any percentage calculations
3. **Citations** — Confirm the source exists, check author names, publication dates, and that the cited claim matches the source
4. **Factual claims** — Cross-reference multiple sources when possible, prefer primary sources

## Examples

### Example: Numerical data verification

**Document claim:** "Revenue grew 23% from Q2 to Q3."
**Source data:** Q2 revenue: $3.52M, Q3 revenue: $4.15M
**Calculation:** ($4.15M - $3.52M) / $3.52M = 17.9%
**Status:** Incorrect
**Confidence:** High
**Suggestion:** Change "23%" to "17.9%" or "approximately 18%"

### Example: Quote verification

**Document claim:** The CEO said, "Our remote employees are 40% more productive."
**Source transcript:** "Our data shows about a 25% improvement in output metrics for remote employees. Not 40%—that would be extraordinary—but 25% is significant."
**Status:** Incorrect
**Confidence:** High
**Suggestion:** Correct the quote to "Our data shows about a 25% improvement in output metrics for remote employees."

### Example: Partially correct

**Document claim:** "The company has over 500 employees across 30 countries."
**Source ** 520 employees across 28 countries
**Status:** Partially correct
**Confidence:** High
**Suggestion:** "Over 500 employees" is accurate (520), but "30 countries" should be "28 countries"

## Handling ambiguity

When a claim is ambiguous or could be interpreted multiple ways:
- Note the ambiguity in the report
- Check all reasonable interpretations
- Suggest clarifying language for the original document

## When to ask for clarification

Ask the user if:
- The document contains specialized terminology without clear definitions
- A claim seems to reference internal knowledge not in provided sources
- Multiple sources conflict and it's unclear which is authoritative