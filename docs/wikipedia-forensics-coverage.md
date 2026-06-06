# Wikipedia forensics coverage

This file tracks the extra checks added on top of the original AntiSlop writing-style patterns.

The goal is not to accuse a writer based on one phrase. The goal is to separate weak style tells from hard artefacts that need source/provenance checks.

## Added coverage

### Markup and platform artefacts

- Markdown mixed into wikitext: `# Heading`, `**bold**`, fenced code blocks, Markdown links.
- Broken wikitext: unclosed templates, unclosed `<ref>` tags, placeholder URLs, generated comments.
- Chatbot citation residue: `turn0search0`, `turn1view0`, `contentReference`, `oaicite`, `oai_citation`, `grok_card`, `attached_file`, `sandbox:/mnt/data/`.
- Orphaned citation glyphs: `【`, `】`, `cite`.
- AI-tool tracking artefacts such as `utm_source=chatgpt.com`.

### Citation forensics

- Invalid DOI.
- DOI resolving to the wrong paper.
- Invalid ISBN checksum.
- Broken or non-existent source links.
- Citations that exist but do not support the claim.
- Undefined and unused named references.
- Vague book citations without page support.
- Fake publication details.

### Wikipedia-specific structures

- Hallucinated templates.
- Fake template parameters.
- Plausible but non-existent categories.
- Obsolete categories returned from model memory.
- Hallucinated policy shortcuts.
- SEO-like category stuffing.

### Talk-page and process comments

- Subject-line leakage in comments.
- Canned good-faith / quality statements.
- Canned constructive criticism offers.
- Deflection to "focus on content" without addressing source/provenance issues.
- Fake policy quotes or non-existent shortcuts.
- Plain-text essay headings in a comment.
- Canned article-for-creation or deletion-discussion replies.

### False-positive control

The skill now explicitly down-weights weak signs:

- Perfect grammar alone.
- Formal tone alone.
- Transition words alone.
- Unsourced content alone.
- Bland prose alone.
- Markdown in contexts where Markdown is expected.
- English variety mismatch without other evidence.

## Scoring split

The updated skill uses two scores:

1. **Writing Slop Score** for style and rhythm.
2. **Forensic Risk Score** for hard artefacts and source/provenance issues.

This avoids treating a polished non-native English writer the same as a draft containing `turn0search0` plus fake DOI references.

## Refresh protocol

The skill frontmatter now has:

```yaml
last-refreshed: 2026-06-07
pattern-count: 75+
```

Refresh it by comparing against:

- `Wikipedia:Signs of AI writing`
- `Wikipedia:WikiProject AI Cleanup`

Only add genuinely new signals. Keep the caveat that signs are not proof.
