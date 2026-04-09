# Prompt Engineering Best Practices

Reference guide for the R-CTCEO framework and effective prompt construction.

---

## The R-CTCEO Framework

### R — Role

Assign a **specific** expert persona. Generic roles produce generic results.

| ❌ Weak | ✅ Strong |
|--------|----------|
| `Act as an expert` | `Act as a senior AppSec engineer specializing in PHP and OWASP Top 10` |
| `You are a developer` | `You are a senior TypeScript developer with expertise in REST API design and error handling` |
| `Act as a writer` | `You are a senior technical writer specializing in API documentation for developer audiences` |

**Why it works:** Different roles bring different assumptions, vocabulary, and quality standards. A "senior" role implies production-quality output. A specialized role implies domain-specific frameworks and conventions.

---

### C — Context

Provide **background that removes ambiguity**. Tell the model what this is for, who uses it, and what's at stake.

**Elements to include:**
- What is this for? (project type, system name)
- Who is the audience? (senior devs, non-technical stakeholders, external partners)
- What technology is involved? (language, framework, version, cloud provider)
- What are the stakes? (production vs. prototype, compliance-required, public-facing)

**Example:**
```
CONTEXT: We are auditing a PHP authentication module prior to production
deployment. The application handles financial transactions and must comply
with PCI DSS. The codebase uses a custom MVC framework with a MySQL backend.
The review will be presented to the security team lead.
```

---

### T — Task

Define **specific, enumerable deliverables**. Use bullet points. Avoid vague objectives.

| ❌ Vague | ✅ Specific |
|---------|------------|
| `Review this code` | `Identify SQL injection, XSS, and authentication flaws with line numbers` |
| `Write documentation` | `Create endpoint docs with request schema, response examples, and error codes` |
| `Analyze this data` | `Identify p95 latency outliers, correlate with deployment events, rank by business impact` |

**Structure:**
```
TASK: [Main objective]. Specifically:
- [Deliverable 1]
- [Deliverable 2]
- [Deliverable 3]
```

---

### C — Constraints

Set **measurable, actionable limits**. Avoid subjective words like "secure," "fast," or "good."

| ❌ Vague Constraint | ✅ Measurable Constraint |
|--------------------|------------------------|
| `Make it secure` | `OWASP Top 10 2021 compliant with CVSS 3.1 scoring` |
| `Keep it short` | `Maximum 50 lines of code excluding comments` |
| `Use best practices` | `Follow Airbnb JavaScript style guide, strict TypeScript mode` |
| `Make it fast` | `Query must complete in under 100ms on 1M+ row tables` |
| `Handle errors` | `Handle all HTTP error codes; never expose stack traces to clients` |

**Types of constraints:**
- **Technical:** Language version, allowed/forbidden libraries, framework version
- **Quality:** Test coverage %, documentation requirements, lint rules
- **Scope:** Line count, word count, number of files, time limit
- **Compliance:** OWASP, NIST, GDPR, PCI DSS, SOC 2
- **Format:** Markdown, JSON, OpenAPI 3.0, numbered list

---

### E — Examples (Optional)

Include few-shot examples when the task is **complex, ambiguous, or style-sensitive**. Skip for clear, straightforward requests.

**When to include:**
- Specific output format required
- Complex reasoning tasks
- Style or tone matching
- Ambiguous requirements with multiple valid interpretations

**Example structure:**
```
EXAMPLES:

Good output:
[CRITICAL] SQL Injection — Line 47
CVSS 3.1: 9.1 | Severity: Critical
Issue: User input concatenated directly into query string.
Fix: Use PDO prepared statements (see remediation below).

Bad output:
"There is a SQL injection somewhere in the file."
```

---

### O — Output Format

Define the **explicit structure** for the AI's response. Never leave the format to chance — an unspecified format produces inconsistent results.

| ❌ No Format | ✅ Explicit Format |
|-------------|-------------------|
| [No format specified] | `Output: 1. Root cause 2. Fixed code 3. Verification steps` |
| `Give me a report` | `Output: Executive Summary → Findings by Severity → Remediation Roadmap` |
| `Just explain it` | `Output: One-paragraph summary, then a bulleted breakdown of each component` |

**Elements to specify:**
- Section order and headers
- File names (for code output: `main.tf`, `variables.tf`, etc.)
- Whether to include examples, diagrams, or code snippets
- Length expectations per section

---

## Anti-Patterns to Avoid

### 1. Vague Objectives

```
❌  "Help me with this"
❌  "Make this better"
❌  "Do something useful"
✅  "Refactor this function to reduce cyclomatic complexity below 10 and add JSDoc comments"
```

### 2. Missing Context

```
❌  "Write documentation"
❌  "Fix the bug"
❌  "Review this"
✅  "Write OpenAPI 3.0 documentation for a payment processing endpoint used by external partners"
```

### 3. Subjective Constraints

```
❌  "Make it secure"
❌  "Write clean code"
❌  "Keep it simple"
✅  "No external libraries; strict TypeScript; handle null/undefined; maximum 50 lines"
```

### 4. No Output Format

```
❌  [No format specified — AI decides structure]
✅  "Output: 1. Root cause 2. Fixed code 3. How to verify 4. Regression test"
```

### 5. Ambiguous Scope

```
❌  "Review the whole codebase"
✅  "Review /app/auth/ directory only — focus on authentication and session management"
```

---

## Common Patterns by Domain

### Code / Development

```
ROLE: Senior [language] developer with expertise in [domain/framework]
CONTEXT: [Project type, tech stack, production vs. prototype, team context]
TASK:
- [Feature/function to implement]
- [Error handling requirements]
- [Testing expectations]
CONSTRAINTS:
- Language: [e.g., TypeScript 5.x, strict mode]
- Max lines: [e.g., 100 excluding comments]
- Style: [e.g., Airbnb JS style guide]
- No external libraries (or: use only [specific libraries])
- Include JSDoc/docstrings for all public functions
OUTPUT FORMAT: Complete implementation with imports, types, and usage example
```

### Security / AppSec

```
ROLE: AppSec engineer specializing in [technology] and OWASP standards
CONTEXT: [Application type, language, database, compliance requirements]
TASK:
- [Specific vulnerability classes to check]
- [Authentication/authorization review]
- [Input validation and sanitization]
CONSTRAINTS:
- Framework: OWASP Top 10 2021, ASVS 4.0
- Score all findings with CVSS 3.1
- Severity: Critical / High / Medium / Low
- Include line numbers and code snippets
- Provide remediation code for Critical and High findings
OUTPUT FORMAT:
1. Executive Summary
2. Critical Findings
3. High Priority Issues
4. Medium/Low Issues
5. Remediation Roadmap
```

### DevOps / Infrastructure

```
ROLE: Senior DevOps/SRE engineer with expertise in [cloud/tool]
CONTEXT: [Environment count, team size, uptime requirements, compliance]
TASK:
- [Infrastructure components to provision]
- [Monitoring and alerting setup]
- [Security group and IAM configuration]
CONSTRAINTS:
- Tool version: [e.g., Terraform 1.5+, AWS Provider 5.0+]
- Max lines per file: [e.g., 300]
- Must pass: tflint, checkov
- Include cost optimization tags
- Support [N] environments via variables
OUTPUT FORMAT: main.tf, variables.tf, outputs.tf, README.md with usage
```

### Data Analysis

```
ROLE: Senior data analyst / data scientist specializing in [domain]
CONTEXT: [Data source, time range, business question, current baseline]
TASK:
- [What to measure and why]
- [Specific metrics to calculate]
- [Comparisons or correlations to surface]
CONSTRAINTS:
- Statistical significance: p < 0.05
- Include confidence intervals
- Focus on actionable insights only
- Account for [seasonality/outliers/known events]
OUTPUT FORMAT:
1. Executive Summary (key findings)
2. Methodology
3. Findings with visualizations
4. Root Cause Analysis
5. Recommendations (ranked by impact/effort)
```

### Technical Writing

```
ROLE: Senior technical writer specializing in [domain] documentation
CONTEXT: [Audience expertise level, publication venue, style guide, purpose]
TASK:
- [Content type and key sections]
- [Code examples in which languages]
- [Compliance with which spec/standard]
CONSTRAINTS:
- Audience: [Junior devs / senior devs / non-technical stakeholders]
- Max words per section: [e.g., 200]
- Format: [OpenAPI 3.0 / Markdown / Google Developer Style Guide]
- Include working code examples
OUTPUT FORMAT: [Explicit section structure with headers]
```

### Creative Writing

```
ROLE: Senior creative writer / copywriter specializing in [medium/genre]
CONTEXT: [Audience demographics, publication venue, brand voice, purpose]
TASK:
- [Content piece and key themes]
- [Emotional arc or narrative structure]
- [Call-to-action or conclusion goal]
CONSTRAINTS:
- Tone: [formal / conversational / witty / authoritative]
- Word count: [e.g., 1,200 words]
- Reading level: [e.g., Flesch-Kincaid 10th grade]
- No [jargon / clichés / promotional language]
OUTPUT FORMAT: Title → Hook → Body sections with subheadings → Conclusion
```

### Marketing

```
ROLE: Senior marketing strategist specializing in [channel/domain]
CONTEXT: [Target audience, campaign goals, competitive landscape, budget]
TASK:
- [Campaign deliverables and assets]
- [Messaging framework and positioning]
- [Channel strategy and distribution plan]
CONSTRAINTS:
- Platform: [character limits, image specs, format requirements]
- Brand guidelines: [voice, colors, forbidden terms]
- KPIs: [specific metrics like CTR > 2%, CPA < $15]
- Budget: [total and per-channel allocation]
OUTPUT FORMAT: Campaign Brief → Messaging → Channel Plan → Success Metrics
```

### Education

```
ROLE: Senior instructional designer specializing in [subject/level]
CONTEXT: [Learner level, prerequisites, class size, delivery format]
TASK:
- [Learning objectives aligned to Bloom's taxonomy]
- [Lesson structure and activities]
- [Assessment design and rubrics]
CONSTRAINTS:
- Session duration: [e.g., 50 minutes]
- Bloom's level: [e.g., Apply and above]
- Accessibility: [WCAG 2.1 / UDL principles]
- Prior knowledge: [what learners already know]
OUTPUT FORMAT: Learning Objectives → Lesson Plan → Activities → Assessment Rubric
```

---

## Advanced Techniques

### Chain of Thought

For complex reasoning or analysis tasks:

```
Think through this step by step:
1. Identify all relevant [components/vulnerabilities/requirements]
2. Evaluate each against [framework/criteria]
3. Rank by [severity/impact/effort]
4. Recommend [action/fix/approach] for each
5. Summarize findings with a prioritized roadmap
```

### Self-Review

Ask the model to check its own work before finalizing:

```
After completing the task:
1. Verify all TASK bullet points are addressed
2. Check for edge cases not handled
3. Confirm all CONSTRAINTS are met
4. Flag anything that requires human review
```

### Multi-Perspective Analysis

```
Analyze this from three perspectives:
1. As a security engineer — what are the risks?
2. As a developer — what are the implementation challenges?
3. As a product manager — what is the business impact?
Provide all three perspectives before making a recommendation.
```

---

## Prompt Quality Checklist

Before using a prompt, verify:

```
☐ Role is specific and domain-appropriate
☐ Context includes project type, tech stack, and stakes
☐ Task is expressed as specific bullet points
☐ Constraints are measurable, not subjective
☐ Output format is explicitly defined
☐ Examples included (if task is complex or ambiguous)
☐ File paths are absolute (if referencing files)
☐ No vague words: "secure," "fast," "good," "clean," "best"
```

---

**Part of:** [Prompt Enhancer Skill](../SKILL.md)  
**Last Updated:** April 2026
