# SC1003-ECDS-Project — Team Allocation Simulator

## Overview
A Python-based algorithm that allocates 6,000 students across 120 tutorial
groups into balanced teams of 5, optimizing for diversity across school,
gender, and CGPA fairness. Uses the Simpson Diversity Index (normalized)
for school and gender balance, and Gaussian scoring for CGPA fairness,
combined with a swap-based optimization algorithm.

## Team
Team 2

- [Name] ([email])
- [Name] ([email])
- [Name] ([email])
- [Name] ([email])

## How It Works
1. **CGPA Clustering** — students in each tutorial group are sorted by
   CGPA and divided into performance tiers to prevent academically
   imbalanced teams
2. **Initial Team Formation** — one student is drawn from each tier to
   form balanced starting teams
3. **Swap Optimization** — teams are iteratively improved by swapping
   members between teams, subject to school and gender constraints
4. **Scoring** — team quality is measured via a weighted combination of
   normalized Simpson Diversity (school, gender) and Gaussian CGPA
   fairness scores

## How to Run
1. Clone this repo
2. Install dependencies:
