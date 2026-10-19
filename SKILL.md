---
name: seo-polish
description: Audit SEO health with Seo-Audit, then generate optimized alternatives with Wordsmith. Use when the user wants a full technical + on-page SEO diagnosis followed by concrete rewrite alternatives for titles, meta descriptions, headings, content gaps, and internal linking — and wants both the audit findings and polished fixes. Chains seo-audit (diagnosis) → wordsmith (alternatives) in one workflow. Not for keyword cannibalization resolution (use keyword-resolver) or mass page generation (use programmatic-seo).
version: 1.0.0
argument-hint: "[site URL | page URL | sitemap]"
user-invocable: true
metadata:
  version: "1.0.0"
---

# SEO-Polish

Chain two existing skills into a single workflow: **Seo-Audit** performs a comprehensive technical + on-page SEO diagnosis, then **Wordsmith** generates polished alternatives that fix the identified issues while preserving the author's voice and search intent.

## Invocation

```text
/seo-polish https://example.com
/seo-polish [paste page content]
Audit my site's SEO and give me fixes.
```

If no target is provided, ask the user for the site URL, specific page, or pasted content.

## Workflow

### Phase 1: Run Seo-Audit

Invoke the **Seo-Audit skill** with the provided target:

1. Assess crawlability & indexation (robots.txt, sitemap, canonicals, redirects).
2. Check technical foundations (Core Web Vitals, mobile-friendliness, HTTPS, URL structure).
3. Audit on-page optimization (title tags, meta descriptions, heading structure, image alt text, internal linking).
4. Evaluate content quality (E-E-A-T signals, depth, thin content, keyword targeting).
5. Output: a prioritized action plan with High/Medium/Low severity findings.

**Handoff rule:** Capture seo-audit's full report verbatim, especially the "Prioritized Action Plan" section. Do not summarize yet.

### Phase 2: Generate Alternatives with Wordsmith

Pass seo-audit's findings to the **Wordsmith skill** with this instruction:

```
For each High and Medium severity finding below, generate 2–3 alternative versions
that fix the issue while preserving the author's voice and search intent. Focus on:

- Title tags: rewrite to include primary keyword near beginning, 50–60 chars
- Meta descriptions: compelling value proposition, 150–160 chars, includes keyword
- H1 headings: ensure one per page, contains primary keyword
- Content improvements: add depth, answer search intent, remove thin sections
- Internal linking: suggest descriptive anchor text for orphan pages

Original audit:
[Paste seo-audit's full report here]

Target pages:
[List the specific URLs flagged for each finding]
```

Wordsmith should:
- Treat each High/Medium finding as a separate rewrite task
- Provide 2–3 options per finding (labeled Option A/B/C)
- Keep facts intact and improve clarity
- Highlight which option best resolves each SEO issue

### Phase 3: Present Results

Return a three-part response:

1. **Seo-Audit Executive Summary** (top 3–5 priority issues, quick wins)
2. **Wordsmith Alternatives** (grouped by severity, with options labeled per page)
3. **Recommended Actions** (which combination of alternatives to apply first, in 2–3 sentences)

Format the alternatives so the user can copy-paste directly into their CMS or HTML files.

## Boundaries

- **Does not** resolve keyword cannibalization between competing pages (use keyword-resolver).
- **Does not** generate mass template pages at scale (use programmatic-seo).
- **Does not** implement structured data markup directly (use schema skill for that).
- **Does not** access Search Console automatically — works from what the user provides or public crawl.

## Dependencies

This skill requires both `seo-audit` and `wordsmith` to be installed. If either is missing, tell the user and stop.
