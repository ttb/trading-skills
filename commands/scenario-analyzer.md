---
description: "Analyzes 18-month scenarios from news headlines. Generates a comprehensive report in English including primary/secondary/tertiary impacts, recommended stocks, and a second opinion review."
argument-hint: "<headline>"
---

# Scenario Analyzer

Analyzes news headlines to generate 18-month scenarios and evaluates their impact on sectors and individual stocks.

## Arguments

```
$ARGUMENTS
```

**Argument Interpretation**:
- If a headline is provided: use it as the subject of analysis
- If no argument is given: prompt the user to enter a headline

**Usage Examples**:
- `/scenario-analyzer Fed raises rates by 50bp` → Analyze a Fed rate hike scenario
- `/scenario-analyzer China announces new tariffs on US semiconductors` → Analyze a tariff scenario
- `/scenario-analyzer OPEC+ agrees to cut oil production` → Analyze an oil production cut scenario
- `/scenario-analyzer` → Prompt the user for a headline, then analyze

## Analysis Contents

| Item | Description |
|------|-------------|
| **Related News** | Collect relevant articles from the past 2 weeks via web search |
| **Scenarios** | Base / Bull / Bear (3 scenarios with probabilities) |
| **Impact Analysis** | Primary, secondary, and tertiary sector impacts |
| **Stock Selection** | 3–5 positive and 3–5 negative stocks (US-listed only) |
| **Review** | Second opinion — flags oversights and biases |

## Execution Steps

1. **Headline Parsing**:
   - Extract the headline from the provided argument
   - If no argument is given, prompt the user for input
   - Classify the event type (Monetary Policy / Geopolitical / Regulatory / Technology / Commodity / Corporate)

2. **Load Reference Materials**:
   ```
   Read skills/scenario-analyzer/references/headline_event_patterns.md
   Read skills/scenario-analyzer/references/sector_sensitivity_matrix.md
   Read skills/scenario-analyzer/references/scenario_playbooks.md
   ```

3. **Main Analysis (scenario-analyst agent)**:
   ```
   Agent tool:
   - subagent_type: "scenario-analyst"
   - prompt: headline + event type + reference materials
   ```

   Output:
   - List of related news articles
   - 3 scenarios (Base / Bull / Bear)
   - Sector impact analysis (primary / secondary / tertiary)
   - Stock recommendation list

4. **Second Opinion (strategy-reviewer agent)**:
   ```
   Agent tool:
   - subagent_type: "strategy-reviewer"
   - prompt: full output from Step 3
   ```

   Output:
   - Identified oversights
   - Commentary on scenario probabilities
   - Bias detection
   - Alternative scenario proposals

5. **Report Generation**:
   - Integrate results from both agents
   - Append final investment conclusions
   - Save to `reports/scenario_analysis_<topic>_YYYYMMDD.md`

## Reference Resources

- `skills/scenario-analyzer/references/headline_event_patterns.md` — Event patterns
- `skills/scenario-analyzer/references/sector_sensitivity_matrix.md` — Sector sensitivity matrix
- `skills/scenario-analyzer/references/scenario_playbooks.md` — Scenario playbook templates

## Key Requirements

- **Language**: All analysis and output must be in **English**
- **Target Market**: Stock selections must be **US-listed equities only**
- **Time Horizon**: Scenarios cover an **18-month** window
- **Probabilities**: Base + Bull + Bear must sum to **100%**
- **Second Opinion**: **Required** — always invoke the strategy-reviewer agent

## Output

Generate a final **Headline Scenario Analysis Report** that includes:
- Related news articles
- Scenario summaries (through 18 months)
- Sector and industry impacts (primary / secondary / tertiary)
- Positively impacted stocks (3–5 tickers)
- Negatively impacted stocks (3–5 tickers)
- Second opinion review
- Final investment conclusions and implications
