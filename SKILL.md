---
name: antislop
description: Detect and fix AI-generated writing patterns. Includes general anti-slop editing plus Wikipedia-style forensic checks for markup artefacts, hallucinated references, broken citations, policy cosplay, talk-page tells, and weak indicators that should not be over-weighted. Use when scanning content for AI tells, auditing drafts before publishing, checking whether text sounds like AI, humanising AI-assisted writing, or reviewing Wikipedia/mediawiki-style text for undisclosed LLM artefacts.
use_when: User wants to detect AI patterns, audit content authenticity, humanize AI text, check for slop, verify writing doesn't sound AI-generated, or inspect Wikipedia-style text for AI-generated markup/citation/comment artefacts before publishing.
user-invocable: true
tools: [Read, Edit, Write]
last-refreshed: 2026-06-07
pattern-count: 75+
---

# The AntiSlop

A comprehensive AI writing pattern detector and fixer. It combines broad anti-slop editing with Wikipedia-style forensic checks from **Wikipedia:Signs of AI writing**.

This skill has two jobs:

1. **General writing cleanup** — remove generic AI style, weak phrasing, template rhythm, false polish, filler, and fake enthusiasm.
2. **Forensic audit** — flag hard artefacts that may show copy-pasted LLM output: Markdown/wikitext mixups, `turn0search0`, `contentReference`, invalid citations, fake DOI/ISBNs, hallucinated templates, non-existent policy shortcuts, and talk-page boilerplate.

## Core Caveat

Do not treat any single sign as proof. AI detection is probabilistic and error-prone. Human writers can use AI-like wording, and LLM-assisted writing can be heavily edited. The right output is a risk assessment with concrete evidence, not an accusation.

When evidence is weak, say so. When evidence is strong, explain the exact artefact.

---

## The 30-Second Test

**The Horoscope Test:**

> "Could anyone have written this, for anyone?"

If yes, it is probably slop. Like a horoscope: technically applicable to everyone, resonant with no one.

**What fails:**

- Vague claims without specific examples
- Advice that applies universally without context
- Content missing the author's distinct perspective
- Writing that could have any byline

**What passes:**

- Specific tools, dates, outcomes, places, or constraints
- Personal observations grounded in experience
- Opinions that not everyone would agree with
- Details only this author or editor would know

---

## Modes

### 1. General Anti-Slop Mode

Use for articles, posts, emails, README files, reports, bios, cover letters, and AI-assisted drafts.

Default behaviour: **edit and fix** unless the user asks for audit only.

### 2. Wikipedia Forensics Mode

Use for Wikipedia drafts, wikitext, talk-page comments, deletion discussions, article-for-creation submissions, policy arguments, and citation-heavy text.

Default behaviour: **audit and flag**. Do not silently remove forensic artefacts if that would hide provenance or make an unreliable draft look clean. Fix obvious formatting only when the user owns the draft and asks for editing.

Trigger phrases include:

- "check this for Wikipedia AI signs"
- "AI forensics"
- "audit this wikitext"
- "check citations for hallucination"
- "does this look like ChatGPT pasted into Wikipedia"
- "scan for turn0search0 / oaicite / fake DOI"

---

## How It Works

1. Run the Horoscope Test.
2. Scan content, language, style, structure, communication, markup, citations, miscellaneous provenance, and talk-page/comment patterns.
3. Separate **strong artefacts** from weak style tells.
4. Calculate a slop/forensics score.
5. Fix style issues when safe.
6. Flag citation/provenance issues for manual verification instead of hiding them.
7. Report exactly what changed and what still needs human judgement.

---

## Severity Tiers

### Tier 1: Strong AI Artefacts or High-Risk Signals

These deserve immediate attention. In Wikipedia-style text, several of these can justify a deeper source/provenance check.

| Pattern | Example | Action |
|---|---|---|
| Chatbot citation residue | `turn0search0`, `turn1view0`, `contentReference`, `oaicite`, `oai_citation`, `grok_card`, `attributableIndex`, `attached_file`, `+1` markers | Flag as strong paste artefact; remove only after checking source trail |
| Markdown inside wikitext | `# Heading`, `**bold**`, fenced code blocks, `---`, Markdown links inside Wikipedia draft | Convert to valid wikitext if editing; flag as likely copy-paste if auditing |
| Placeholder URLs or source slots | `PASTE_URL_HERE`, `SOURCE NEEDED`, `Add if available with citation` in generated-looking sections | Flag and replace with real verified source or remove |
| Invalid DOI/ISBN | DOI fails to resolve, ISBN checksum invalid | Flag possible hallucinated reference; verify externally |
| DOI mismatch | DOI resolves but title/authors/journal do not match the cited claim | Flag as high-risk hallucination |
| Broken citation cluster | Multiple new citations are 404/dead/non-existent and not archived | Flag; do not trust facts until verified |
| Unused list-defined references | `<references><ref name="x">...</ref></references>` but no inline use | Fix markup and verify that sources support text |
| Hallucinated template/category | Non-existent infobox, fake template parameter, red-linked category that looks SEO-plausible | Check template/category exists before keeping |
| Fake Wikipedia shortcut | `WP:BIOSIG`, `WP:NOTENGLISH`, `WP:AFDPURPOSE`, plausible but non-existent shortcut | Flag policy hallucination; replace with real policy only if valid |
| Fabricated source assessment | Claims about sources that the source does not make | Flag as synthesis or hallucination |
| Prompt refusal / chatbot self-reference | "I can't browse", "as an AI", "my training data" | Remove; investigate surrounding text |

### Tier 2: Suspicious When Repeated or Clustered

| Pattern | Example | Fix |
|---|---|---|
| Delve | "Let's delve into..." | Remove or state the topic directly |
| Game-changer / revolutionary | "game-changing approach" | Describe the concrete effect |
| Unlock potential | "unlock your potential" | Remove |
| Leverage as verb | "leverage these insights" | "use" or concrete action |
| It's worth noting | "It's worth noting that..." | Just state the point |
| Moreover / furthermore | Repeated transitions | Remove or use natural flow |
| Today's digital landscape | Generic opening | Remove |
| Cutting-edge | Promotional filler | Replace with specific capability |
| Pivotal moment | Inflated significance | State what happened |
| Tapestry, intricate, vibrant, interplay, garner, underscore | AI vocabulary cluster | Replace with concrete words or cut |
| Here's the thing / bottom line / at the end of the day | Repeated rhetorical framing | Keep at most one if voice-appropriate |
| Paired adjectives | "comprehensive and thorough" | Pick one or be specific |
| Template openings | "In this post, we'll cover..." | Start with the point |

### Tier 3: Weak Alone, Useful in Clusters

Do not over-weight these. They can be normal human style.

| Pattern | Example | Safer interpretation |
|---|---|---|
| Perfect grammar | Clean formal writing | Weak indicator only |
| Formal tone | Academic, careful, professional prose | Could be code-switching or edited prose |
| Transition words | however, additionally, firstly | Weak unless repetitive and template-like |
| Unsourced content | Missing citations | Bad editing, not necessarily AI |
| Bland prose | Generic style | Weak unless paired with concrete artefacts |
| Markdown alone | Technical users often write Markdown | Stronger only when mixed with wikitext incorrectly |
| English variety mismatch | Indian topic in American English, etc. | Consider non-native speakers and editing context |

---

## Content Patterns

| # | Pattern | Before | After / Action |
|---|---|---|---|
| 1 | **Significance inflation** | "marking a pivotal moment in the evolution of..." | "was established in 1989 to collect statistics" |
| 2 | **Legacy/broader trend stuffing** | "reflects a broader shift toward..." | Keep only sourced, relevant claims |
| 3 | **Canned notability name-dropping** | "cited in NYT, BBC, FT, and The Hindu" | Explain what each source actually says, or just cite it |
| 4 | **Media coverage theatre** | "profiled in multiple high-quality outlets" | State the coverage directly with citations |
| 5 | **Active social media presence** | "maintains an active social media presence" | Remove unless specifically notable and sourced |
| 6 | **Superficial -ing analysis** | "reflecting..., symbolizing..., showcasing..." | Remove or support with a source |
| 7 | **Promotional language** | "nestled within the breathtaking region" | Neutral description |
| 8 | **Vague attribution** | "Experts believe..." | Name the expert/source or remove |
| 9 | **Outline-like conclusion** | "Challenges and future prospects..." | Replace with sourced facts or delete |
| 10 | **Treating broad article titles as proper nouns** | "The History of X has evolved..." | Rewrite as normal prose |
| 11 | **Conservation/ecosystem generic padding** | Generic claims about species and ecosystem importance | Keep only specific sourced ecological facts |
| 12 | **Discussion-about-discussion padding** | "has sparked debate about authenticity and identity" | Cite actual debate or remove |

---

## Language and Grammar Patterns

| # | Pattern | Before | After |
|---|---|---|---|
| 13 | **High-density AI vocabulary** | pivotal, crucial, intricate, underscore, foster, realm, tapestry | Use plain specific words |
| 14 | **Copula avoidance** | "serves as", "features", "boasts" | "is", "has" |
| 15 | **Negative parallelism** | "not just X, but Y" | State Y directly |
| 16 | **Rule of three** | "innovation, inspiration, and insight" | Use the natural number of items |
| 17 | **Synonym cycling** | protagonist / central figure / main character | Repeat the clearest term |
| 18 | **False ranges** | "from X to Y" when not a true range | List the actual items |
| 19 | **Clinical formality** | individuals / utilize / implement / facilitate | people / use / do / help |
| 20 | **Over-hedging** | could potentially possibly | Use one qualifier if needed |
| 21 | **Filler phrases** | in order to / due to the fact that | to / because |

---

## Style Patterns

| # | Pattern | Before | After / Action |
|---|---|---|---|
| 22 | **Em dash overuse** | Several dashes in one paragraph | Use commas, semicolons, periods, or rewrite |
| 23 | **Boldface overuse** | `**OKRs**, **KPIs**, **BMC**` | Remove most bolding |
| 24 | **Emoji headers** | `✅ Action`, `🚀 Next Steps` | Remove emojis in formal text |
| 25 | **Title case headings** | "Strategic Negotiations And Partnerships" | Sentence case unless style guide says otherwise |
| 26 | **Inline-header vertical lists** | Bold mini-heading + sentence for every bullet | Convert to prose or fewer bullets |
| 27 | **List addiction** | Everything becomes bullets | Convert where prose is clearer |
| 28 | **Unnecessary tables** | 3-row table for simple comparison | Use a sentence or short list |
| 29 | **Curly quotes/apostrophes inconsistency** | Mixed `"`, `“”`, `'`, `’` | Standardise for the target format |
| 30 | **Skipping heading levels** | H2 then H4 | Fix hierarchy |
| 31 | **Thematic breaks before headings** | `---` before every section | Remove unless actually needed |

---

## Structural Patterns

### Staccato Fragment Spam

Three or more consecutive short declarative sentences that could be bullets.

**Before:**

> The model is impressive. Complex code ships fast. Documentation writes itself. Problems get solved quickly.

**After:**

> The model is impressive: complex code ships in a single session, documentation becomes easier to draft, and problems that used to take a weekend can sometimes be solved in an afternoon.

**Detection rule:** 3+ consecutive sentences that are all short, declarative, parallel, and bullet-like.

### Sentence Uniformity

Every sentence lands in the same length range. Real writing has rhythm. Mix short and long sentences only when the meaning needs it, not as a trick.

### Comparator Sentences

**Before:**

> This isn't theoretical. It's practical.

**After:**

> Here is how it works in practice.

### Over-Balanced Sections

Every section has the same length and structure. Real writing reflects priority; some points need a line, others need a paragraph.

### Manufactured Personality

AI trying to sound human can be worse than plain AI prose.

**Before:**

> Five services. Five tabs. Five headaches. That got old fast. So I built an MCP server.

**After:**

> I use five services in this workflow. Each one is useful, but each adds another dashboard and another context switch.

---

## Communication Patterns

| # | Pattern | Before | Action |
|---|---|---|---|
| 32 | **Chatbot artefacts** | "I hope this helps! Let me know if..." | Remove |
| 33 | **Sycophantic opener** | "Great question! You're absolutely right!" | Answer directly |
| 34 | **Cutoff disclaimer** | "While details are limited in available sources..." | Find sources or state uncertainty plainly |
| 35 | **Placeholder text** | "[insert example here]" | Replace or remove |
| 36 | **Canned collaboration** | "I welcome constructive feedback..." | Make a concrete request or cut |
| 37 | **Canned focus-on-content appeal** | "Let's focus on improving the article rather than accusations" | Check whether it avoids the actual issue |
| 38 | **Subject-line leakage** | "Subject: Request for Review..." pasted into comment | Remove and flag as copy-paste artefact |
| 39 | **Plain-text section titles in comments** | "Importance of Thorough Research" followed by essay | Convert to normal comment if needed |

---

## Wikipedia / MediaWiki Markup Forensics

Use this section when text is meant for Wikipedia, another MediaWiki site, or a citation-heavy encyclopaedic draft.

### Markdown-in-Wikitext Checks

Flag these in Wikipedia-style text:

- `# Heading` instead of `== Heading ==`
- `**bold**` or `__bold__` instead of `'''bold'''`
- `*italic*` instead of `''italic''`
- `[text](https://example.com)` instead of `[https://example.com text]` or a citation template
- fenced code blocks: triple backticks, especially ```wikitext
- `---`, `***`, or `___` as thematic breaks instead of proper wikitext use
- Markdown bullets nested in ways that break MediaWiki rendering

### Broken Wikitext Checks

Flag and repair when safe:

- Unclosed `{{template` or `<ref>` tags
- Mixed Markdown and wikitext in the same list or section
- Comments such as `<!-- Add if available with citation -->` in generated-looking infobox edits
- Placeholder URLs, fake filenames, or generated image captions
- Sections that look like copied chatbot output rather than article text

### Chatbot Reference Artefacts

Search for exact strings and regex-like variants:

- `turn0search0`, `turn1search`, `turn0view`, `turn1view`
- `contentReference`, `oaicite`, `oai_citation`, `oai_cite`
- `grok_card`, `attributableIndex`, `attached_file`, `sandbox:/mnt/data/`
- malformed citation markers such as `【`, `】`, `cite`, or orphaned citation glyphs
- `utm_source=chatgpt.com`, `utm_source=openai`, `utm_source=copilot.com`, `referrer=grok.com`

### Categories and Templates

Flag:

- red-linked categories that look plausible but wrong
- SEO-like categories added to drafts
- obsolete categories returned by model memory
- non-existent infoboxes
- fake template parameters that existing templates ignore
- hallucinated language templates or maintenance templates

Do not assume every red link is AI. New editors and old copied material can also create bad categories/templates.

---

## Citation Forensics

When citation issues appear, do **not** merely rewrite around them. Verify the source.

| Check | Strong signal when... | Action |
|---|---|---|
| Broken external links | Several new links are dead, unarchived, or never existed | Mark facts unsupported until checked |
| DOI invalid | DOI does not resolve | Flag possible hallucination |
| DOI mismatch | DOI resolves to unrelated paper | Flag high-risk fabricated citation |
| ISBN invalid | Checksum fails | Flag likely wrong book reference |
| Book citation too vague | No page, chapter, URL, edition, or quote for a specific claim | Ask for page-level support |
| Reference says less than prose claims | Citation exists but does not support the sentence | Mark failed verification |
| Unused named references | list-defined refs not used inline | Fix markup and verify sources |
| Undefined named references | `<ref name="x" />` without definition | Find or remove |
| URL tracking artefacts | `utm_source=chatgpt.com` or similar | Remove tracking; flag provenance concern |
| Fake publication details | plausible title, journal, date, author pattern | Verify externally before trusting |

### Source-Support Rule

For any factual claim in audited text, ask:

1. Does the cited source exist?
2. Does it say this exact thing?
3. Is the source reliable for this claim?
4. Is the prose adding synthesis or significance that the source does not support?

If any answer fails, flag the claim even if the prose sounds human.

---

## Miscellaneous Provenance Patterns

| Pattern | Why it matters | Caution |
|---|---|---|
| Sudden style shift | A user's new edit/comment is suddenly polished, formal, or US-English compared with older writing | Non-native speakers and code-switching can explain this; do not overstate |
| Exhaustive edit summaries | Long polished summaries for simple edits | Weak alone, stronger with other artefacts |
| Article-for-creation submission statements | "I believe this meets all criteria..." | Common AI boilerplate; check sources |
| Pre-placed maintenance templates | Draft includes cleanup tags before review | Could be human, but can indicate generated draft packaging |
| Permissions gaming | Reassuring statements about permission/compliance without substance | Treat as conduct/provenance flag, not proof |
| Historical model artefacts | abrupt cutoffs, prompt refusal, old access dates, section summaries | Useful for older text; less useful for current models |

---

## Indicators of AI-Written Wikipedia Comments

These are for talk pages, deletion discussions, unblock requests, and review desks.

| Pattern | Example | Action |
|---|---|---|
| Canned quality/good-faith language | "I am committed to improving the article in line with Wikipedia standards" | Ask for concrete diffs/sources |
| Canned constructive-criticism offer | "I welcome any guidance from experienced editors" | Check if they respond specifically after feedback |
| Focus-on-content deflection | "Let's focus on improving the article rather than accusations" | Do not let it dodge source/provenance issues |
| Subject-line leakage | `Subject: Request for Permission to Edit...` | Strong copy-paste artefact |
| Plain-text section headings | Essay-like headings in a talk comment | Flag template generation style |
| Fake policy/guideline citations | `WP:NOTELOCAL`, `WP:BIOSIG`, `WP:NOTENGLISH` if non-existent | Verify shortcut before accepting argument |
| Wikilawyering with invented quotes | policy quote not found on cited page | Flag and correct |
| Emoji as formatting | repeated ✅/⚠️/📌 in formal discussion | Remove; weak alone |
| Canned source-assessment requests | "Please assess whether these meet reliable source standards" without specifics | Ask for exact source-claim mapping |
| Confusion over declined draft reason | generic reply to a specific decline reason | Ask for targeted correction |

---

## Signs of Human Writing / Lower-Risk Evidence

Use this to avoid false positives.

- Text predates public ChatGPT release on 30 November 2022.
- The author's older and newer writing is consistent in quirks, grammar, formatting, and English variety.
- The author can explain their own editorial choices, source selection, and revisions in concrete terms.
- The writing contains specific local knowledge, constraints, mistakes, or rough edges that fit the author.
- The source trail is real, relevant, and supports the claims.
- Markdown use is expected because the context is GitHub, Reddit, Slack, Obsidian, technical notes, or a README.

Never clear text as human solely because it has typos. Never accuse text solely because it is clean.

---

## Scoring System

Use two scores where relevant: **Writing Slop Score** and **Forensic Risk Score**.

### Writing Slop Score

| Pattern Type | Points |
|---|---:|
| Each Tier 2 phrase | +2 |
| Tier 3 cluster | +2 |
| Failed Horoscope Test | +5 |
| Staccato fragment spam | +4 |
| Sentence uniformity | +3 |
| Comparator sentences | +2 each |
| Manufactured personality | +4 |
| Self-promotional framing | +5 |
| Template headers | +2 each |
| Excessive bullets/tables/bold | +2 |

**Interpretation:**

- **0-5:** Low risk; minor edits
- **6-12:** Medium risk; significant editing required
- **13+:** High risk; likely unedited or lightly edited AI style

### Forensic Risk Score

| Pattern Type | Points |
|---|---:|
| Chatbot citation residue (`turn0search0`, `oaicite`, etc.) | +8 |
| Invalid DOI/ISBN | +6 |
| DOI/source mismatch | +8 |
| Multiple broken, unarchived citations | +6 |
| Hallucinated template/category/policy shortcut | +6 |
| Markdown-wikitext hybrid in Wikipedia draft | +5 |
| Unused/undefined named refs | +4 |
| URL tracking artefact from AI tool | +5 |
| Subject-line leakage in talk comment | +5 |
| Canned talk-page policy boilerplate | +3 |
| Sudden style shift | +2 only with context |
| Weak indicator alone | +0 to +1 |

**Interpretation:**

- **0-4:** Low forensic risk
- **5-11:** Medium risk; verify sources and markup
- **12+:** High risk; do not publish until source/provenance issues are checked

---

## Editor Mode

This skill is an editor for style issues, but a forensic auditor for source/provenance issues.

### Safe to fix directly

- generic phrases
- repeated transitions
- sentence uniformity
- staccato fragments
- weak openings/conclusions
- excessive bold, emoji, bullets, and tables
- Markdown-to-wikitext formatting when the meaning is clear and the user owns the draft

### Do not silently fix without reporting

- fake or broken citations
- invalid DOI/ISBN
- source mismatches
- hallucinated categories/templates/policies
- chatbot citation artefacts
- provenance indicators such as `turn0search0` or `oaicite`

For these, flag the issue and explain what needs verification.

---

## Output Format

```markdown
## AntiSlop Report

**Horoscope Test:** [PASS/FAIL] - [reason]
**Writing Slop Score:** [X] - [Low/Medium/High]
**Forensic Risk Score:** [Y] - [Low/Medium/High/Not applicable]

### Strong Artefacts
- [exact artefact] at [location] - [why it matters]

### Content / Language / Style Issues
- [pattern] at [location] - [fix]

### Markup Issues
- [Markdown/wikitext/template/category issue] - [fix or verification needed]

### Citation Issues
- [source/DOI/ISBN/ref issue] - [verify/remove/fix]

### Comment / Policy Issues
- [talk-page or policy-shortcut issue] - [verify or rewrite]

### Weak or Ineffective Indicators Ignored
- [e.g., formal tone alone, perfect grammar alone]

### Fixes Applied
| Location | Before | After |
|---|---|---|
| ... | ... | ... |

### Needs Human Verification
- [source or provenance issue]

### Verdict
[Clear but cautious conclusion: low/medium/high risk, not proof]
```

---

## Full Example

**Before:**

> Subject: Request for Review and Clarification Regarding Draft Article
>
> The subject has been profiled in multiple high-quality, independent, and widely-read outlets, including The Australian and SBS News. This demonstrates significant, sustained, and verifiable coverage, meeting WP:BIOSIG and WP:SIGCOV.
>
> ```wikitext
> # Legacy and Impact
> **The organisation** plays a pivotal role in the evolving landscape of community advocacy.
> <ref>{{cite web|title=Example|url=PASTE_URL_HERE|access-date=2026-02-09}}</ref>
> ```

**Report summary:**

- Subject-line leakage: strong copy-paste artefact.
- `WP:BIOSIG` must be verified; plausible fake shortcut.
- Markdown heading/bold inside fenced wikitext block.
- Placeholder citation URL.
- Significance inflation: "pivotal role", "evolving landscape".
- Do not publish until sources and shortcuts are verified.

**After, if the user owns the draft and sources are real:**

> The draft should state what each independent source says and cite it directly. Remove the subject-line text, replace invalid shortcuts with real policy references, convert Markdown to wikitext, and delete unsupported claims about significance.

---

## Pattern Refresh Protocol

Patterns go stale as models and copy-paste artefacts change. Before scanning, check `last-refreshed` in frontmatter. If it is more than 30 days old, refresh.

**Refresh workflow:**

```bash
# Signs of AI writing - full wikitext
curl -s "https://en.wikipedia.org/w/api.php?action=parse&page=Wikipedia:Signs_of_AI_writing&prop=wikitext&format=json" | python3 -c "
import json, sys
data = json.load(sys.stdin)
print(data['parse']['wikitext']['*'][:50000])
" > /tmp/antislop-signs.txt

# WikiProject AI Cleanup
curl -s "https://en.wikipedia.org/w/api.php?action=parse&page=Wikipedia:WikiProject_AI_Cleanup&prop=wikitext&format=json" | python3 -c "
import json, sys
data = json.load(sys.stdin)
print(data['parse']['wikitext']['*'][:50000])
" > /tmp/antislop-cleanup.txt
```

Then:

1. Diff the latest page against this skill.
2. Add genuinely new signals only; do not duplicate renamed patterns.
3. Separate hard artefacts from weak style tells.
4. Update `last-refreshed` and `pattern-count`.
5. Preserve the caveat that signs are not proof.

---

## References

- [Wikipedia:Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing)
- [Wikipedia:WikiProject AI Cleanup](https://en.wikipedia.org/wiki/Wikipedia:WikiProject_AI_Cleanup)
- [Wikipedia:Large language models](https://en.wikipedia.org/wiki/Wikipedia:Large_language_models)

---

## Core Principle

**AI slop is not just about words. It is about patterns, missing source support, and machine-shaped defaults.**

One "moreover" means little. "Moreover" plus generic significance claims plus Markdown-wikitext mixups plus fake DOI plus `turn0search0` means stop and verify before publishing.

The goal is not to hide AI use. The goal is writing that is specific, sourced, honest, and fit for its context.