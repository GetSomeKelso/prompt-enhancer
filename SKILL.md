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

## Activation Flow

```
User types:  /skill prompt-enhancer
                      │
                      ▼
         ┌────────────────────────┐
         │  Display skill info    │
         │  End with:             │
         │    Enter Prompt:       │
         └────────────┬───────────┘
                      │ STOP — wait for user
                      ▼
         User pastes any text
                      │
         ┌────────────▼───────────┐
         │  PRE-FLIGHT CHECK      │
         │  ☐ Not reading files   │
         │  ☐ Not in plan mode    │
         │  ☐ Not asking questions│
         │  ☐ Treating as raw txt │
         └────────────┬───────────┘
                      │
                      ▼
         Apply R-CTCEO immediately
                      │
                      ▼
         Output:
           ── CONCISE VERSION
           ── DETAILED VERSION
           ── IMPROVEMENTS MADE
```

---

## The R-CTCEO Framework

**R**ole → **C**ontext → **T**ask → **C**onstraints → **E**xamples → **O**utput Format

### R — Role

Assign a specific expert persona. Be precise about domain and seniority.

```
❌  "Act as an expert..."
✅  "Act as a senior AppSec engineer specializing in PHP and OWASP standards..."
✅  "You are a senior backend developer with expertise in TypeScript and REST API design..."
```

### C — Context

Provide relevant background: what is this for, who is the audience, what technology is involved, what are the stakes (production vs. prototype).

```
CONTEXT: We need to audit a PHP authentication module before production
deployment. The application processes financial data and must comply with
PCI DSS. The review will be conducted by a senior security team.
```

### T — Task

Define specific deliverables and success criteria as bullet points. Avoid vague objectives.

```
TASK: Perform a security audit covering:
- SQL injection and parameterized query validation
- Authentication and session management flaws
- Hardcoded credentials and secrets
- Input validation and sanitization gaps
- Business logic vulnerabilities
```

### C — Constraints

Set measurable, actionable boundaries. Avoid vague words like "secure" or "fast."

```
CONSTRAINTS:
- Use OWASP Top 10 2021 as primary framework
- Provide CVSS 3.1 scores for each finding
- Rate severity: Critical, High, Medium, Low
- Include specific line numbers for each finding
- Provide remediation code for Critical and High findings
- Maximum 100 lines of remediation code per finding
```

### E — Examples (Optional)

Include few-shot examples for complex, ambiguous, or style-sensitive tasks. Skip for straightforward requests.

```
EXAMPLES:

Good finding format:
[HIGH] SQL Injection — Line 47
CVSS 3.1: 8.1 (AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:N)
Issue: User input concatenated directly into query string.
Fix: Use PDO prepared statements.

Bad finding format:
"There is a SQL injection issue somewhere."
```

### O — Output Format

Define the explicit structure for the AI's response. Never leave format to chance.

```
OUTPUT FORMAT:
1. Executive Summary (overall risk rating, finding count by severity)
2. Critical Findings (immediate action required)
3. High Priority Issues (fix before production)
4. Medium/Low Issues (address in next sprint)
5. Remediation Roadmap (prioritized by severity and effort)
```

---

## Domain-Specific Enhancement Rules

### Security / AppSec

- Role: AppSec engineer / penetration tester / security architect
- Reference: OWASP Top 10 2021, ASVS 4.0, NIST, CWE, CVE
- Include: CVSS 3.1 scores, severity ratings, line numbers, remediation code
- Output: Executive summary → Critical → High → Medium/Low → Remediation roadmap

### Development

- Role: Senior [language] developer with expertise in [domain]
- Include: Language/framework version, code style guide, error handling, testing requirements
- Constraints: Line count limits, JSDoc/docstring requirements, no external libraries if needed
- Output: Complete implementation with imports, types, usage example

### DevOps / Infrastructure

- Role: Senior DevOps/SRE engineer
- Include: Tool versions (Terraform 1.5+, AWS Provider 5.0+), compliance tags, cost considerations
- Constraints: Uptime SLA, rollback time, environment count, team size
- Output: File-by-file breakdown (main.tf, variables.tf, outputs.tf, README.md)

### Data Analysis

- Role: Senior data analyst / data scientist
- Include: Data sources, time range, methodology, statistical significance thresholds
- Constraints: Confidence intervals, p-value threshold, visualization format
- Output: Executive summary → Methodology → Findings → Recommendations

### Technical Writing

- Role: Senior technical writer specializing in [domain]
- Include: Audience expertise level, style guide (OpenAPI, Airbnb, etc.), word count limits
- Constraints: Jargon level, section length, format standard
- Output: Explicitly structured sections with headers

### Business Analysis

- Role: Senior business analyst / product manager
- Include: Stakeholders, budget, timeline, compliance requirements, integration constraints
- Constraints: Team size, concurrent users, SLA, regulatory requirements
- Output: Stakeholder map → user stories → risk assessment → roadmap

### Creative Writing

- Role: Senior creative writer / copywriter / content strategist
- Include: Audience demographics, brand voice, publication venue, emotional tone
- Constraints: Word count, reading level, tone (formal/casual/witty), format (blog/essay/story)
- Output: Structured piece with hook → body → call-to-action or conclusion

### Marketing

- Role: Senior marketing strategist / growth marketer / brand strategist
- Include: Target audience, campaign goals, channels, budget, competitive landscape
- Constraints: Platform requirements (character limits, image specs), brand guidelines, KPIs
- Output: Campaign brief → messaging framework → channel plan → success metrics

### Education

- Role: Senior instructional designer / curriculum developer / educator
- Include: Learner level, prerequisites, learning objectives, assessment criteria
- Constraints: Session duration, class size, accessibility requirements, Bloom's taxonomy level
- Output: Learning objectives → lesson plan → activities → assessment rubric

---

## Domain Detection Keywords

Use these keywords to identify the appropriate domain when enhancing a prompt:

| Domain | Keywords |
|--------|----------|
| Security | security, appsec, vulnerability, owasp, audit, pentest, injection, xss, csrf, auth |
| Development | code, function, class, api, endpoint, typescript, python, javascript, php, java, react |
| DevOps | terraform, kubernetes, docker, aws, deploy, pipeline, ci/cd, infrastructure, ansible |
| Data | analyze, dataset, sql, query, dashboard, metrics, report, statistics, visualization |
| Writing | documentation, docs, readme, runbook, guide, tutorial, blog, technical writing |
| Business | requirements, stakeholder, user story, roadmap, kpi, product, strategy |
| Creative | write, story, essay, poem, script, narrative, fiction, blog post, copy, tagline |
| Marketing | campaign, audience, brand, seo, conversion, social media, ad, funnel, growth |
| Education | lesson, curriculum, teach, learn, student, course, training, workshop, assessment |

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
- If the user provides **feedback on the last enhancement** (e.g., "make it more focused on security," "add constraints about performance," "remove the examples section") → modify the previous output accordingly and re-output both versions
- Refinement feedback is still **text to process**, not a task to execute — the same guardrails apply

---

## Negative Examples (What NOT to Do)

### ❌ Failure: Treating Input as a Task

```
User: "Review the security of the auth module in /app/auth/login.php"

WRONG:
  "Let me read /app/auth/login.php first and analyze the code..."
  [Reads file, enters plan mode, analyzes content]

CORRECT:
  CONCISE VERSION:
  Act as an AppSec engineer. Review /app/auth/login.php for security
  vulnerabilities against OWASP Top 10 2021...
  [Continues with enhancement — does NOT read the file]
```

### ❌ Failure: Asking Clarifying Questions

```
User: "Create a system for user authentication"

WRONG:
  "To provide the best solution, I need some clarification:
   1. What language?
   2. Session or token-based?
   3. What database?"

CORRECT:
  [Make intelligent assumptions, enhance immediately]
  CONCISE VERSION:
  Act as a senior security engineer. Design a production-ready user
  authentication system with JWT tokens, bcrypt password hashing...
```

### ❌ Failure: Entering Plan Mode

```
User: "Build a microservices architecture"

WRONG:
  ▣ Plan Mode Active
  Phase 1: Requirements Analysis...
  [Continues planning]

CORRECT:
  [Exit plan mode, enhance immediately]
  CONCISE VERSION:
  Act as a solutions architect. Design a scalable microservices
  architecture with service discovery, API gateway...
```

---

## Pre-Processing Checklist

Before generating any output after "Enter Prompt:", verify:

```
☐ I have NOT read any files mentioned in the input
☐ I am NOT in plan mode
☐ I have NOT asked any questions
☐ I am treating the input as raw text to enhance
☐ I will apply R-CTCEO framework immediately
```

**If any item would be unchecked — stop and re-read this file.**

---

## Reference Files

- **[reference/best_practices.md](reference/best_practices.md)** — Detailed R-CTCEO techniques and anti-patterns
- **[reference/examples_library.md](reference/examples_library.md)** — Before/after examples across all domains

---

**Location:** `~/.claude/skills/prompt-enhancer/`  
**Last Updated:** April 2026
