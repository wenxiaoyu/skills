# Scoring Rubric

5-level maturity scale used for benchmark gap analysis. Each level has a generic definition applicable to any dimension, plus domain-adaptation guidance.

---

## Scale Definitions

### Level 1 — Initial

**Generic**: Ad-hoc, reactive, no systematic approach. Practices are undocumented and person-dependent. No dedicated ownership or budget.

**Indicators**:
- Process relies on manual work and tribal knowledge
- No formal documentation or SOPs
- Success depends on individual heroics rather than systems
- Failures are common and handled reactively

**Example (Technology)**: Spreadsheets for core operations, no integrated system, data silos.

**Example (Organization)**: No dedicated team; the function is a side responsibility of another role.

**Example (AI/Data)**: No AI or analytics usage; decisions are entirely experience-based.

---

### Level 2 — Emerging

**Generic**: Some repeatable patterns exist but are not standardized across the organization. Early tool adoption is fragmented.

**Indicators**:
- Some teams or sites have adopted better practices; others haven't
- Tools are deployed but not fully integrated
- Documentation is incomplete or inconsistent
- Metrics are collected but not systematically reviewed

**Example (Technology)**: Basic system in place (e.g., ERP module) but heavily customized per site, poor integration.

**Example (Organization)**: Part-time ownership, limited budget, no formal talent pipeline.

**Example (AI/Data)**: Pilot projects exist but none have reached production; data quality is acknowledged as a problem.

---

### Level 3 — Defined

**Generic**: Processes are documented, standardized, and consistently followed. Basic metrics are tracked and reviewed regularly.

**Indicators**:
- SOPs exist and are maintained
- System is integrated across primary workflows
- KPIs are defined, measured, and reported on a regular cadence
- Dedicated team with clear ownership

**Example (Technology)**: Core platform deployed enterprise-wide, API integrations for key partners, cloud migration in progress.

**Example (Organization)**: Dedicated team of appropriate size, defined roles, regular training program.

**Example (AI/Data)**: 1–2 AI use cases in production with measurable ROI; data governance framework established.

---

### Level 4 — Managed

**Generic**: Quantitative management of processes. Data-driven optimization is systematic and continuous. Predictive capabilities are emerging.

**Indicators**:
- Process performance is monitored against statistical baselines
- Optimization initiatives are continuous, not one-off
- Predictive analytics inform operational decisions
- Cross-functional integration is mature

**Example (Technology)**: Real-time dashboards, automated exception detection, predictive alerts, full API ecosystem.

**Example (Organization)**: Center of excellence, cross-functional squads, innovation budget, succession planning.

**Example (AI/Data)**: 3–5 AI use cases in production covering different process stages; ML models retrained on production data; data quality score >90%.

---

### Level 5 — Leading

**Generic**: Continuous innovation is embedded in the culture. The organization sets the industry standard and is recognized as a reference by peers and analysts.

**Indicators**:
- Autonomous operation with human-in-the-loop exception handling
- Novel practices that others benchmark against
- Published thought leadership (papers, conference talks, patents)
- Ecosystem influence (shapes vendor roadmaps, standards bodies)

**Example (Technology)**: Fully autonomous core processes, AI-native architecture, digital twin for simulation, open API platform others build on.

**Example (Organization)**: Industry-recognized talent bench, publishes research, hosts industry events, active in standards bodies.

**Example (AI/Data)**: AI agents make autonomous operational decisions with human oversight; proprietary models trained on unique datasets; AI ROI tracked and reported to board.

---

## Scoring Guidelines

### How to score a benchmark target (external)

1. Gather research findings relevant to the sub-dimension
2. Map observed capabilities to the level that best matches the indicators
3. If evidence spans multiple levels, score at the **lowest fully-met level** (conservative principle)
4. Cite the specific source(s) that informed the score
5. Mark `[low confidence]` if evidence is from Tier 4–5 sources only

### How to score the internal state

1. Use user-provided data from Step 6 (Internal State Collection)
2. Map stated capabilities to the level indicators
3. If user provides quantitative data, prefer that over qualitative descriptions
4. Mark `[estimate]` if user provided rough/approximate figures
5. When in doubt between two levels, score at the **lower level** and note the uncertainty

### Scoring sub-dimensions vs. dimensions

- Score each **sub-dimension** independently
- Compute the **dimension-level score** as the weighted average of sub-dimension scores (equal weights unless user specifies otherwise)
- Round dimension scores to one decimal place for display; keep raw scores for gap calculation

### Gap calculation

```
gap = benchmark_score - internal_score
```

Where `benchmark_score` is the highest-scoring confirmed benchmark target for that sub-dimension (not the average). This surfaces the maximum achievable standard.

### Classification

| Gap value   | Symbol | Label          | Recommended action                      |
|-------------|--------|----------------|-----------------------------------------|
| ≤ −1.0      | ★      | Advantage      | Leverage as competitive differentiator  |
| −0.9 to 0   | ●      | Parity         | Maintain; monitor for shifts            |
| +0.1 to +1.0| △      | Mild gap       | Plan incremental improvement (6–12 mo)  |
| > +1.0      | ▲      | Critical gap   | Prioritize for immediate action (< 6 mo)|

---

## Domain Adaptation Notes

The generic indicators above apply to any business domain. When benchmarking a specific domain, the AI should adapt indicator language to match domain terminology. For example:

- **Supply chain domain**: "Process" → "logistics workflow", "System" → "WMS/TMS/ERP", "Exception handling" → "disruption management"
- **Software engineering domain**: "Process" → "SDLC/CI-CD", "System" → "dev toolchain", "Exception handling" → "incident response"
- **Manufacturing domain**: "Process" → "production process", "System" → "MES/SCADA", "Exception handling" → "quality deviation management"

Adaptation should be done at the start of Step 7 (Gap Scoring), before presenting scores to the user, so that all labels and examples in the report resonate with the user's domain vocabulary.
