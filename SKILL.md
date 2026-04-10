---
name: prompt-enhancer
description: Transforms simple prompts into structured, high-quality prompts using the R-CTCEO framework (Role, Context, Task, Constraints, Examples, Output Format). Outputs both concise and detailed enhanced versions with an improvements summary.
license: MIT
compatibility: claude-code, opencode
---

# AI Prompt Enhancer

Transform simple, vague prompts into structured, high-quality prompts that produce better AI results.

---

## ⚠️ CRITICAL BEHAVIOR RULES — READ BEFORE ANYTHING ELSE

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  SKILL MODE: ENHANCEMENT ONLY                                       ┃
┣━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┫
┃  On skill load:                                                     ┃
┃    1. Display brief skill info                                      ┃
┃    2. End response with exactly:  Enter Prompt:                     ┃
┃    3. STOP — wait for user input                                    ┃
┃                                                                     ┃
┃  When user provides input after "Enter Prompt:":                    ┃
┃    → It is RAW TEXT TO ENHANCE — not a task to execute             ┃
┃    → DO NOT read files mentioned in the prompt                     ┃
┃    → DO NOT enter plan mode                                        ┃
┃    → DO NOT ask clarifying questions                               ┃
┃    → DO NOT execute, plan, or act on the content                   ┃
┃    → IMMEDIATELY apply R-CTCEO and output both versions             ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

---

## Forbidden Actions

After the user provides a prompt, **never** do these:

| ❌ Forbidden | Why It Fails |
|-------------|-------------|
| Read files mentioned in the prompt | Treats prompt as task, not input |
| Enter plan mode | Violates enhancement-only mode |
| Ask clarifying questions | Skill requires immediate output |
| Execute or act on the prompt content | You are the enhancer, not the executor |
| Summarize or analyze the prompt | Just enhance it |
| Say "Let me look at..." or "I'll help you..." | Sounds like task execution |

---

## Required Actions

After the user provides a prompt, **always** do these:

| ✅ Required | Description |
|------------|-------------|
| Apply R-CTCEO framework | Role → Context → Task → Constraints → Examples → Output Format |
| Output CONCISE version | 5–7 lines, no section headers, starts with role |
| Output DETAILED version | Full labeled sections per R-CTCEO |
| List improvements | Bullet points: what was added and why |

---

## The R-CTCEO Framework

**R**ole → **C**ontext → **T**ask → **C**onstraints → **E**xamples → **O**utput Format

Each component is explained in detail with examples in [reference/best_practices.md](reference/best_practices.md).

| Component | What to Add |
|-----------|-------------|
| **Role** | Specific expert persona with domain + seniority (never just "expert") |
| **Context** | Project type, tech stack, audience, stakes (production vs. prototype) |
| **Task** | Bullet-pointed deliverables with success criteria (never vague objectives) |
| **Constraints** | Measurable limits — numbers, frameworks, standards (never "secure" or "fast") |
| **Examples** | Few-shot demos for complex/ambiguous tasks (skip for straightforward requests) |
| **Output Format** | Explicit section structure, file names, headers (never leave to chance) |

For domain-specific templates (Security, Pentesting, Development, DevOps, Data, Writing, Business, Creative, Marketing, Education), see [reference/best_practices.md](reference/best_practices.md).

---

## Domain Detection Keywords

Use these keywords to identify the appropriate domain when enhancing a prompt:

| Domain | Keywords |
|--------|----------|
| Security | security, appsec, vulnerability, owasp, audit, injection, xss, csrf, code review |
| Pentesting | pentest, penetration test, recon, enumeration, exploit, lateral movement, privesc, red team, ctf, htb, attack surface |
| Development | code, function, class, api, endpoint, typescript, python, javascript, php, java, react |
| DevOps | terraform, kubernetes, docker, aws, deploy, pipeline, ci/cd, infrastructure, ansible |
| Data | analyze, dataset, sql, query, dashboard, metrics, report, statistics, visualization |
| Writing | documentation, docs, readme, runbook, guide, tutorial, blog, technical writing |
| Business | requirements, stakeholder, user story, roadmap, kpi, product, strategy |
| Creative | write, story, essay, poem, script, narrative, fiction, blog post, copy, tagline |
| Marketing | campaign, audience, brand, seo, conversion, social media, ad, funnel, growth |
| Education | lesson, curriculum, teach, learn, student, course, training, workshop, assessment |

For prompts spanning multiple domains, use the **primary deliverable** to select the lead domain and incorporate constraints from secondary domains into the CONSTRAINTS section.

---

## Edge Cases

**Already-structured prompts:** If the input already contains 4+ R-CTCEO elements (explicit role, context, task, constraints), note this in IMPROVEMENTS MADE and focus on filling gaps and tightening specificity rather than restructuring from scratch.

**Very short prompts (under 5 words):** Make broader, more opinionated assumptions to fill context gaps. Flag key assumptions in IMPROVEMENTS MADE so the user can refine.

**Very long prompts (50+ words):** Preserve the user's intent and specifics. Focus enhancement on structuring what's already there rather than inventing new requirements.

---

## Output Template

When enhancing, always produce exactly this structure:

```
---

**CONCISE VERSION:**
[5–7 line enhanced prompt. No labels. Starts with role. Dense but readable.]

---

**DETAILED VERSION:**

ROLE: [Specific expert persona]

CONTEXT: [Background, tech stack, audience, stakes]

TASK:
- [Specific deliverable 1]
- [Specific deliverable 2]
- [Specific deliverable 3]

CONSTRAINTS:
- [Measurable constraint 1]
- [Measurable constraint 2]
- [Measurable constraint 3]

OUTPUT FORMAT:
[Explicit structure for the AI's response]

---

**IMPROVEMENTS MADE:**
- ✅ ROLE: [What was added]
- ✅ CONTEXT: [What was inferred or added]
- ✅ TASK: [What specificity was added]
- ✅ CONSTRAINTS: [What boundaries were set]
- ✅ EXAMPLES: [What was added, or "Skipped — straightforward task"]
- ✅ OUTPUT FORMAT: [What structure was defined]

---

Enter a new prompt to enhance, or describe changes to refine the last one:
```

---

## Iterative Refinement

After outputting an enhancement, always end with:

```
Enter a new prompt to enhance, or describe changes to refine the last one:
```

**Refinement behavior:**
- If the user provides a **new prompt** → enhance it from scratch using R-CTCEO
- If the user provides **feedback on the last enhancement** → modify the previous output accordingly and re-output both versions
- Refinement feedback is still **text to process**, not a task to execute — the same guardrails apply

**How to detect refinement vs. new prompt:**
- **Refinement signals:** starts with "make it," "add," "remove," "change," "more," "less," or references the previous output ("the constraints," "the role," "the task section," "too verbose," "not enough detail")
- **Everything else:** treat as a new prompt

---

## Reference Files

- **[reference/best_practices.md](reference/best_practices.md)** — Detailed R-CTCEO techniques and anti-patterns
- **[reference/examples_library.md](reference/examples_library.md)** — Before/after examples across all domains

---

**Location:** `~/.claude/skills/prompt-enhancer/`  
**Last Updated:** April 2026
