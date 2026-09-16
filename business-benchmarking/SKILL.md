---
name: business-benchmarking
description: "Conduct professional business benchmarking analysis for any domain. Designs expert research questionnaires, searches authoritative sources for benchmark data, builds dimension frameworks from findings, performs gap scoring, and generates interactive HTML reports + Feishu documents. Use when user mentions benchmarking, competitive analysis, gap analysis, industry comparison, capability assessment, best practice research, peer comparison, or maturity evaluation. Keywords: 对标, benchmarking, competitive analysis, gap analysis, industry benchmark, 差距分析, 行业对标, 最佳实践."
version: 1.0.0
---

# Business Benchmarking

## Overview

Conducts professional business benchmarking through 8 structured steps. Acts as a senior research consultant: designs research questionnaires from expert perspective, systematically searches authoritative external sources, extracts dimension frameworks from findings, scores gaps with cited evidence, and delivers interactive reports.

All user-facing output (questions, report text, document content) MUST match the user's conversation language. Skill instructions are in English for internal execution only.

## Prerequisites

- **WebSearch**: Required for external research
- **lark-doc / lark-wiki**: Optional for Feishu document output; degrade gracefully to Markdown file if unavailable
- **lark-base**: Optional if user's internal data lives in a Feishu bitable
- **Subagent support (Agent tool)**: Required for parallel research across multiple questions and targets
- **HTML generation**: Required for interactive report output

---

## Workflow

### Step 1: Scope Definition

Gather context via AskUserQuestion (all options are illustrative — generate domain-relevant choices):

```
Q1: What business domain are you benchmarking?
    → Free text (e.g., "Supply chain WMS capabilities", "AI coding adoption in engineering")

Q2: What is the primary purpose?
    → Gap identification / Project proposal justification / Executive reporting / Strategic planning / Team capability assessment

Q3: Benchmarking depth?
    → Landscape overview (broad, shallow) / Capability deep-dive (narrow, deep) / Full assessment (comprehensive)

Q4: Do you have specific benchmark targets in mind?
    → Yes, I'll specify / Recommend for me / Both (I'll add to your recommendations)
```

Summarize confirmed scope before proceeding. Detect language from user's input and lock output language for all subsequent steps.

---

### Step 2: Research Questionnaire Design

**Core principle**: Act as a senior research consultant. Generate questions a professional analyst would ask to produce a thorough, credible benchmarking study. Do NOT ask the user to define dimensions or topics — derive them from the domain.

Generate 15–30 questions across three layers:

#### Layer A — Landscape Questions (framework-level)
Understand the macro picture: industry structure, key players, standards, evolution path.

Search strategy hint: Gartner / IDC / McKinsey / industry association reports.

Example: "What are the top 5 WMS platforms globally by market share, and what differentiates them?"

#### Layer B — Capability Questions (dimension-level)
Probe specific capabilities: process maturity, technology depth, organizational design, talent composition, KPI baselines.

Search strategy hint: Case studies, vendor white papers, technical blogs, academic papers, conference proceedings.

Example: "What inventory accuracy rates do best-in-class warehouses achieve, and what practices drive that?"

#### Layer C — Frontier Questions (trend-level)
Surface emerging practices, technology adoption curves, and forward-looking moves.

Search strategy hint: Tech media, startup ecosystems, patent filings, recent conference keynotes.

Example: "Which enterprises have deployed AI agents for autonomous supply chain decisions, and at what scale?"

#### Coverage checklist (adapt to domain — not all apply to every scope)

| Topic area        | Typical question angles                                    |
|-------------------|-----------------------------------------------------------|
| Organization      | Team size, reporting structure, cross-functional setup    |
| Process maturity  | Automation rate, exception handling, SLA adherence        |
| Technology        | Architecture, platform stack, integration maturity        |
| Talent            | Skill composition, training investment, retention          |
| AI / Data         | AI use cases in production, data quality, model ROI       |
| KPIs              | Industry benchmarks for cost, speed, quality, throughput  |
| Ecosystem         | Partner network, vendor lock-in, standards adoption        |

#### Per-question metadata

Each question carries:
- `type`: `landscape` / `capability` / `frontier`
- `priority`: `must-answer` / `good-to-have`
- `search_hint`: preferred source type or named source (e.g., "Gartner Magic Quadrant 2025")
- `target_focus`: which benchmark targets this question is most relevant to (filled after Step 3)

#### Questionnaire output format

Present as a grouped list with type badges and priority markers. Example:

```
## 1. Industry Landscape [Landscape · Must-answer]

Q1. What is the global WMS market size and growth trajectory (2023–2027)?
    ↳ Search hint: Gartner, IDC MarketScape, Grand View Research

Q2. Who are the top 5 WMS vendors by market share, and what are their core differentiators?
    ↳ Search hint: Gartner Magic Quadrant, ARC Advisory Group

## 2. AI & Automation [Capability · Must-answer]

Q3. What AI use cases in warehouse operations have documented ROI data?
    ↳ Search hint: McKinsey, vendor case studies (Blue Yonder, Körber)
...
```

#### Gate: User confirms questionnaire

Use AskUserQuestion:
- **Accept as-is** → proceed
- **Modify** → user specifies additions/removals/reprioritization → regenerate affected section → reconfirm
- **Regenerate** → restate scope assumptions and redesign

---

### Step 3: Benchmark Target Recommendation

Search for and recommend 3–5 targets across three categories:

| Category              | What it means                                  | Example                           |
|-----------------------|-----------------------------------------------|-----------------------------------|
| Industry leader       | Top performer setting the standard             | SAP EWM, Manhattan Associates    |
| Specific competitor   | Direct rival the user wants to compare against | JD Logistics, Cainiao            |
| Best practice framework | Structured standard or maturity model        | SCOR model, APQC PCF, Gartner MQ |

For each recommendation provide: name, why it's relevant, what information is publicly available (feasibility note).

After user confirms, back-fill `target_focus` on each question in the questionnaire.

#### Gate: User confirms target list

---

### Step 4: Systematic Research Execution

Execute research for each `must-answer` question. `good-to-have` questions are researched if time/token budget allows.

#### Parallel execution strategy

Launch one subagent per question (or per question group of 2–3 closely related questions). Each subagent prompt must include:
1. The exact question text
2. Benchmark target names to focus on
3. Search hint from question metadata
4. Required output format (see below)
5. Instruction to run WebSearch 2–3 times with varied query phrasings per question

#### Subagent output contract

Each subagent returns:

```yaml
question_id: Q3
answer: |
  [2–5 paragraph synthesized answer]
sources:
  - title: "McKinsey: AI in Logistics 2025"
    url: https://...
    credibility: high   # high=authoritative report/standard org, medium=vendor case study/tech blog, low=news/opinion
  - title: "Blue Yonder case study — DHL"
    url: https://...
    credibility: medium
confidence: medium      # high=multiple corroborating sources, medium=1-2 sources, low=inference/extrapolation
coverage_gaps:
  - "ROI data specific to automotive OEM warehouses not found"
target_relevance:
  "SAP EWM": "Strong — detailed capability data found"
  "JD Logistics": "Weak — only press releases, no independent analysis"
```

#### Post-research synthesis

After all subagents return, compile a research summary matrix:

```
| Question | Targets Covered | Confidence | Key Coverage Gaps         |
|----------|----------------|------------|---------------------------|
| Q1       | All 4 targets  | High       | None                      |
| Q2       | 3 of 4         | Medium     | Target D: limited data    |
| Q3       | 2 of 4         | Low        | ROI data missing for 2/4  |
```

If any `must-answer` question has `confidence: low`, flag to user and offer to run additional targeted searches.

---

### Step 5: Dimension Framework Extraction

From aggregated research findings, **inductively** derive the dimension framework (data-driven, not pre-defined).

#### Extraction method

1. Cluster all research findings into thematic groups
2. Each cluster becomes a Level-1 dimension
3. Within each cluster, identify specific measurable aspects as Level-2 sub-dimensions
4. Cross-reference against the questionnaire coverage to detect blind spots

#### Output: Comparison matrix draft

```
| Dimension (L1)         | Sub-dimension (L2)        | Target A | Target B | Target C | Industry Avg |
|------------------------|---------------------------|----------|----------|----------|--------------|
| 1. Technology Platform | Architecture modernity    | Cloud-native | Hybrid | Monolith | Hybrid     |
|                        | API maturity              | Open, RESTful | Partial | Proprietary | Mixed   |
| 2. Process Automation  | Inbound automation rate   | 85%      | 60%      | 30%      | ~55%         |
|                        | Exception auto-resolution | AI-based | Rule-based | Manual | Rule-based |
...
```

Flag sub-dimensions where data is insufficient for reliable comparison with `[data gap]`.

#### Gate: User confirms dimension framework

Use AskUserQuestion:
- **Accept framework** → proceed
- **Add dimension** → user specifies → add and re-research if needed
- **Remove dimension** → user specifies → remove
- **Refine** → user specifies which dimension needs sub-dimension changes

---

### Step 6: Internal State Collection

For each confirmed dimension and sub-dimension, ask the user targeted questions to fill the "internal" (self-assessment) column.

#### Collection strategy

- Go dimension by dimension (group related sub-dimensions)
- Ask specific, measurable questions: "What is your current inbound automation rate (% of receipts processed without manual intervention)?" rather than "How automated is your inbound process?"
- Accept qualitative descriptions when quantification is genuinely impossible; mark as `[qualitative]`
- Record uncertainty level: `[estimate]` when user provides rough figures

#### Gap-driven questioning

Prioritize sub-dimensions where the external benchmark data is strongest (high confidence from Step 4). Skip sub-dimensions marked `[data gap]` unless user insists.

#### Gate: Confirm all dimensions have internal data before proceeding

---

### Step 7: Gap Scoring

Score each sub-dimension using the 1–5 maturity scale. See `references/scoring-rubric.md` for level definitions.

#### Scoring rules

1. **Internal score**: Based on user-provided data from Step 6, mapped to rubric descriptions
2. **External score**: Based on research findings from Step 4, mapped to rubric descriptions
3. **Citation required**: Every external score must cite at least one source from the research
4. **Low-confidence scoring**: If research confidence is `low` for a sub-dimension, score it but mark with `[low confidence]` and note the uncertainty
5. **Gap = External score − Internal score** (positive = you are behind, negative = you are ahead)

#### Classification thresholds

| Gap value | Label          | Action implication                    |
|-----------|----------------|---------------------------------------|
| ≤ −1      | ★ Advantage    | Document as competitive strength      |
| 0         | ● Parity       | Maintain; monitor for shifts          |
| +1        | △ Mild gap     | Plan incremental improvement          |
| ≥ +2      | ▲ Critical gap | Prioritize for immediate action plan  |

#### Scoring output format

Present as a table for user review:

```
| # | Dimension             | Internal | Benchmark | Gap | Label        | Evidence              |
|---|-----------------------|----------|-----------|-----|--------------|-----------------------|
| 1 | Architecture          | 3        | 5         | +2  | ▲ Critical   | SAP: cloud-native[1]  |
| 2 | Inbound automation    | 2        | 4         | +2  | ▲ Critical   | JD: 85% auto[2]       |
| 3 | Exception handling    | 3        | 3         | 0   | ● Parity     | Industry avg ~rule[3] |
```

#### Gate: User reviews and optionally adjusts scores

User may override any score with justification. Record overrides and propagate to report.

---

### Step 8: Report Generation

Produce two outputs. Always generate the HTML report first (it is the primary deliverable). Generate the Feishu document only if the user requests it and lark-doc is available.

#### Output A: Interactive HTML Report

Single self-contained `.html` file. All CSS and JS inline. Load Chart.js from CDN (`https://cdn.jsdelivr.net/npm/chart.js`).

**Tab structure (5 tabs):**

1. **Executive Summary** — Key findings in prose (3–5 paragraphs), radar chart of all Level-1 dimensions, top 3 strengths and top 3 gaps with action recommendations

2. **Gap Matrix** — Heatmap-style table: dimensions as rows, targets as columns, cells color-coded (red=critical gap, orange=mild gap, gray=parity, green=advantage). Click a cell to expand to sub-dimensions.

3. **Dimension Deep Dive** — Tabbed sub-sections per Level-1 dimension. Each shows: sub-dimension comparison table, sourced evidence snippets, confidence notes, and score breakdown.

4. **Evidence & Sources** — Full source list with title, URL, credibility rating, and which questions/dimensions each source informed. Grouped by credibility tier.

5. **Recommendations** — Prioritized action items in a 2×2 matrix (Impact: High/Low × Effort: High/Low). Each item references the specific gap it addresses and cites relevant benchmark evidence.

**HTML technical requirements:**
- Responsive design; readable on desktop and tablet
- Use a professional, neutral color palette (slate gray + accent colors for gap severity: `#dc2626` critical, `#f97316` mild, `#6b7280` parity, `#16a34a` advantage)
- All text dynamically inserted from JavaScript data object — no hardcoded report text in HTML template
- Include print-friendly CSS (`@media print`)
- Charts: radar chart (Chart.js) for dimension overview; horizontal bar chart for gap magnitudes

**Data embedding pattern:**
```html
<script>
const REPORT_DATA = {
  title: "...",
  scope: "...",
  targets: [...],
  dimensions: [
    { name: "...", subDimensions: [
      { name: "...", internalScore: 3, benchmarkScore: 5, gap: 2, label: "Critical gap", evidence: [...] }
    ]}
  ],
  sources: [...],
  recommendations: [...]
};
</script>
```

Save HTML to workspace output directory. Use `present_files` to deliver to user.

#### Output B: Feishu Document (optional, on request)

Only if user explicitly requests it AND lark-doc skill is accessible.

Structure:
1. 对标概述（Scope, targets, methodology）
2. 维度框架（Dimension list with definitions）
3. 逐项对标分析（Per-dimension: comparison + evidence + gap label）
4. 差距汇总（Gap summary table）
5. 建议与行动项（Recommendations matrix）

Use lark-doc create workflow. Populate content section by section.

#### Template save

After report delivery, offer to save the dimension framework + questionnaire as a reusable template:

Save to: `~/.qoderwork/skills/business-benchmarking/templates/{sanitized-scope-name}-{YYYY-MM-DD}.yaml`

Template YAML schema:

```yaml
name: "Scope name"
domain: "Business domain"
created: "YYYY-MM-DD"
language: "zh"  # or "en"
questionnaire:
  - group: "Group name"
    questions:
      - q: "Question text"
        type: "landscape"  # landscape | capability | frontier
        priority: "must-answer"  # must-answer | good-to-have
        search_hint: "Source hint"
dimensions:
  - name: "Level-1 dimension"
    sub_dimensions:
      - "Sub-dimension text"
scoring:
  scale: 5
  labels: ["Initial", "Emerging", "Defined", "Managed", "Leading"]
```

On future runs, check `templates/` directory for existing frameworks. If found, list them in Step 2 as optional starting points (user can adopt, adapt, or ignore).

---

## Research Quality Standards

### Source authority hierarchy (high to low)

1. **Tier 1** — Analyst firm reports: Gartner, IDC, McKinsey, BCG, Bain, Deloitte, PwC, Forrester
2. **Tier 2** — Industry standards and associations: APQC, SCOR (ASCM), ISO, IEEE, industry-specific bodies
3. **Tier 3** — Vendor documentation: official white papers, case studies with named clients, product documentation
4. **Tier 4** — Technical and trade media: reputable tech blogs, conference proceedings, peer-reviewed articles
5. **Tier 5** — News and opinion: mainstream tech/business news, analyst personal blogs, social media

For any scoring, Tier 1–2 sources carry full weight; Tier 3 carries medium weight; Tier 4–5 are supplementary and cannot independently support a score.

### Search query design principles

- Use English queries for international/technology topics; use Chinese queries for China-domestic-market topics; use both when uncertain
- Include year in query for time-sensitive topics (e.g., "WMS market share 2025")
- Vary query phrasing across 2–3 searches per question (synonym rotation, specific vs. general)
- If a named benchmark target has limited public information, search for analyst evaluations or customer testimonials mentioning it

### Confidence calibration

- **High**: ≥ 2 independent Tier 1–2 sources corroborate the finding
- **Medium**: 1 Tier 1–2 source, or ≥ 2 Tier 3 sources with consistent data
- **Low**: Only Tier 4–5 sources, or sources are > 2 years old, or data is extrapolated

---

## Edge Cases

### Sparse research results
If a `must-answer` question returns only `low` confidence results after initial search:
1. Run 2 additional targeted searches with different query strategies
2. If still low confidence: mark the dimension as `[insufficient data]` in the framework, exclude from scoring, note in report limitations section
3. Ask user if they can provide internal or proprietary sources to fill the gap

### User without Feishu access
Skip Output B entirely. Generate HTML report as primary deliverable. Optionally offer a Markdown summary file as a lightweight alternative.

### Highly specialized domain outside common training data
Rely more heavily on WebSearch results. Be explicit about confidence levels. Let user validate and correct scores more aggressively in Step 7.

### Token or time budget pressure
Prioritize `must-answer` questions. Batch related questions into single subagent calls. Skip `good-to-have` questions. Generate report with available data and clearly mark coverage gaps.

### Contradictory sources
When two Tier 1–2 sources disagree on a data point, present both in the report with a note. Do not average them. Default to the more recent source for scoring unless the older source is more specific to the target.

---

## Files

| Path | Purpose |
|------|---------|
| `references/scoring-rubric.md` | Detailed 1–5 scale definitions with generic and domain-adapted examples |
| `templates/*.yaml` | Saved dimension framework + questionnaire templates from past benchmarking runs |
