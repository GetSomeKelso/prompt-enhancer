# Prompt Enhancement Examples Library

Before/after examples showing R-CTCEO enhancement across diverse domains.

---

## Table of Contents

1. [Security & AppSec](#security--appsec)
2. [Data Analysis](#data-analysis)
3. [Creative Writing](#creative-writing)

---

## Security & AppSec

### Example 1: Code Security Review

**Before:**
```
Check this code for security issues
```

**After (Concise):**
```
Act as an AppSec engineer. Perform a security review of the provided code against
OWASP Top 10 2021. Identify vulnerabilities with specific line references, CVSS 3.1
scores, severity ratings (Critical/High/Medium/Low), and concrete remediation code
examples for all Critical and High findings.
```

**After (Detailed):**
```
ROLE: You are an Application Security Engineer specializing in web application
security, OWASP standards, and secure code review.

CONTEXT: A codebase requires a thorough security audit prior to production
deployment. The review must identify exploitable vulnerabilities, rate risk using
CVSS 3.1, and provide actionable remediation guidance with corrected code.

TASK:
- SQL injection and parameterized query validation
- Cross-site scripting (XSS) — reflected, stored, and DOM-based
- Authentication and session management flaws
- Insecure direct object references (IDOR)
- Hardcoded credentials, API keys, and secrets
- Input validation and sanitization gaps
- Privilege escalation paths
- Business logic vulnerabilities

CONSTRAINTS:
- Use OWASP Top 10 2021 and ASVS 4.0 as primary frameworks
- Provide CVSS 3.1 scores for every finding
- Rate all findings: Critical, High, Medium, Low
- Include specific line numbers and code snippets for each finding
- Provide corrected remediation code for all Critical and High findings
- Do not report informational items — findings must be exploitable

OUTPUT FORMAT:
1. Executive Summary (overall risk rating, finding count by severity)
2. Critical Findings (immediate action required)
3. High Priority Issues (fix before production)
4. Medium/Low Issues (address in next sprint)
5. Remediation Roadmap (prioritized by severity and effort)
```

---

## Data Analysis

### Example 2: Performance Log Analysis

**Before:**
```
Analyze this data
```

**After (Concise):**
```
Act as a senior data analyst. Analyze 7 days of production application logs across
12 microservices to identify latency outliers (p95, p99), error patterns, and
performance bottlenecks. Correlate degradation events with deployment timestamps
and provide ranked recommendations by business impact.
```

**After (Detailed):**
```
ROLE: You are a senior data analyst specializing in application performance
monitoring and distributed systems observability.

CONTEXT: A microservices platform has seen average response time increase from
200ms to 800ms over 7 days across 12 services. The engineering team needs root
cause identification and prioritized fixes. Business impact: 12% increase in
user-reported errors and 8% drop in conversion rate.

TASK:
- Calculate p50, p95, p99 latency by service and endpoint
- Identify error patterns (rate, type, recurrence) by service
- Correlate latency spikes with deployment events (timestamps provided)
- Identify resource utilization hotspots (CPU, memory, DB connections)
- Detect anomalous request patterns (volume spikes, unusual clients)
- Surface the top 5 contributors to overall latency increase

CONSTRAINTS:
- Statistical significance: p < 0.05 for all claims
- Include confidence intervals for all estimates
- Focus on actionable findings only — exclude noise
- Prioritize by business impact (revenue/error rate correlation)
- Account for time-of-day and day-of-week patterns

OUTPUT FORMAT:
1. Executive Summary (3 key findings, overall risk rating)
2. Methodology
3. Latency Analysis (with charts described as tables)
4. Root Cause Analysis (ranked by impact)
5. Recommendations (ranked by impact vs. effort matrix)
6. Implementation Plan (immediate / this sprint / next quarter)
```

---

## Creative Writing

### Example 3: Blog Post

**Before:**
```
Write a blog post about AI
```

**After (Concise):**
```
Act as a senior content strategist writing for a tech-savvy but non-engineer
audience. Write a 1,200-word blog post explaining how AI code assistants are
changing developer workflows. Use a conversational, authoritative tone with
concrete examples, avoid hype and jargon, and end with a clear takeaway the
reader can act on today.
```

**After (Detailed):**
```
ROLE: You are a senior content strategist and technology writer with expertise
in making complex technical topics accessible to informed non-engineers.

CONTEXT: A B2B SaaS company's blog targets engineering managers and technical
decision-makers. The blog averages 5,000 monthly readers with an 8-minute
average read time. The goal is thought leadership, not product promotion.

TASK:
- Write a 1,200-word blog post on how AI code assistants impact developer workflows
- Open with a compelling hook grounded in a real-world scenario
- Cover 3 specific workflow changes with concrete before/after examples
- Address skepticism honestly — include limitations and trade-offs
- End with a clear, actionable takeaway

CONSTRAINTS:
- Tone: Conversational and authoritative — no hype, no "revolutionary"
- Reading level: 10th grade (Flesch-Kincaid)
- No product mentions or promotional language
- Maximum 1 statistic per paragraph (cite source)
- Subheadings every 200–300 words for scannability
- Include a meta description (under 160 characters) and 3 SEO keywords

OUTPUT FORMAT:
1. Meta description + SEO keywords
2. Title (under 70 characters)
3. Hook / Introduction
4. Body sections (3 with subheadings)
5. Conclusion with actionable takeaway
```

---

## Key Patterns

From all examples above, the consistent R-CTCEO pattern is:

1. **Role specificity** — Not "expert" but "senior [domain] specialist with expertise in [specific area]"
2. **Stakes in context** — Production vs. prototype, compliance requirements, audience size
3. **Bullet-point tasks** — Enumerable, specific deliverables — never vague objectives
4. **Measurable constraints** — Numbers, frameworks, standards — never subjective adjectives
5. **Examples when needed** — Few-shot demos for ambiguous or style-sensitive tasks
6. **Explicit output format** — Numbered sections, named files, specific headers

The more specific and structured the prompt, the more consistent and high-quality the AI output.

---

**Part of:** [Prompt Enhancer Skill](../SKILL.md)  
**Last Updated:** April 2026
