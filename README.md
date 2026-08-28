# Team Balancing Algorithm — SC1003 Group Project

Assigns 6,000 students into balanced teams of five within their tutorial groups, optimising for school diversity, gender diversity, and CGPA fairness.

## Problem

Organise 6,000 students into teams of five, with each team drawn from a single tutorial group. Teams are balanced across three factors, weighted by priority:

1. Gender diversity (40%)
2. School affiliation diversity (30%)
3. CGPA fairness (30%)

## Approach

1. **Parsing** — read and group all 6,000 student records (tutorial group, student ID, school, name, gender, CGPA) from `records.csv` into 120 tutorial-group lists of 50 students each.
2. **CGPA clustering** — within each tutorial group, students are sorted by CGPA and split into 5 performance tiers. Initial teams are formed by drawing one student from each tier, guaranteeing balanced academic capability from the start.
3. **Diversity scoring** — each team is scored using a normalized Simpson Diversity Index for school and gender balance, and a Gaussian similarity score for CGPA fairness (weighted 70% team mean, 30% team standard deviation, against the tutorial group's overall distribution). Component scores are combined into a single team score via the 40/30/30 weighting above.
4. **Swap optimization** — teams are iteratively improved by randomly selecting two teams within the same tutorial group, swapping one member between them, and keeping the swap only if it improves total team score without violating gender or school constraints (no team majority-dominated by one gender or school).
5. **Output generation** — final teams are written to a new CSV with columns for tutorial group, student details, and assigned team number.

## Results

- Median team score improved from ~0.88 (random baseline) to ~0.92 after CGPA clustering, to ~0.997 after swap optimization.
- Full pipeline runs in approximately 20 seconds across all 6,000 students / 120 tutorial groups.
- Per-tutorial-group breakdowns and score distributions are in the notebook's diversity metrics and evaluation sections.

## Challenges

- **Local optima** — the swap optimization uses a hill-climbing approach (accepts a swap only if it improves the score), which can get stuck in a locally optimal team arrangement rather than a globally optimal one. Multiple retry attempts per tutorial group help mitigate this.
- **Small-team bias** — the Simpson Diversity Index penalizes any category repetition disproportionately in small teams of five, meaning a single repeated school or gender pairing has an outsized effect on the score.
- **Normalization edge cases** — when the number of available categories (e.g. schools present in a tutorial group) is small relative to team size, the normalizing factor requires careful handling to avoid divide-by-zero or scores exceeding 1.0.

## Setup & Running

1. Install dependencies: `pip install -r requirements.txt`
2. Place `records.csv` in the same folder as the notebook
3. Run all cells top to bottom — diversity metrics, CGPA clustering, and swap optimization are each in their own section, feeding into a combined main pipeline cell
4. Output is written to a new CSV containing the finalised team assignments
