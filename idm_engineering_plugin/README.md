# IDM-Engineering-Plugin

A Claude Code plugin that scores and improves scientific research code against the [IDM Software Engineering Quality Guidelines](https://github.com/InstituteforDiseaseModeling).

## Features

- **Engineering Quality Checker** (`/idm-engineering-plugin:eng-quality-checker`): Evaluates a project across 10 metrics in three categories (quality, usability, safety) and produces a scored report with recommendations.
- **Engineering Quality Fixer** (`/idm-engineering-plugin:eng-quality-fixer`): Reads the score report and implements prioritized, feasible improvements automatically.

## Quality Tiers

| Tier | Description | Example |
|------|-------------|---------|
| 1 | Software library | FPsim, LASER |
| 2 | Small-scale project | A model calibrated to one country |
| 3 | One-off / exploratory | A script to plot simulation outputs |

## Scoring Metrics

| Category (weight) | Metric | Within-category weight |
|---|---|---|
| **Quality (40%)** | Correct | 70% |
| | Clear | 20% |
| | Concise | 10% |
| **Usability (40%)** | Simple | 30% |
| | Powerful | 20% |
| | Performant | 20% |
| | Documented | 20% |
| | Accessible | 10% |
| **Safety (20%)** | Compliant | 60% |
| | Reproducible | 40% |

**Failure conditions**: `quality.correct` and `safety.compliant` can trigger a FAIL (score=0) if serious violations are found (e.g., scientific bugs, exposed secrets).

## Usage

### Score a project

```
/idm-engineering-plugin:eng-quality-checker                    # score current directory at tier 2
/idm-engineering-plugin:eng-quality-checker . 3                # score current directory at tier 3
/idm-engineering-plugin:eng-quality-checker /path/to/repo 1    # score a local path at tier 1
/idm-engineering-plugin:eng-quality-checker https://github.com/org/repo 2   # score a GitHub repo
```

Output: `engineering_score.md` written to the project directory.

### Fix a project

```
/idm-engineering-plugin:eng-quality-fixer                     # fix current directory
/idm-engineering-plugin:eng-quality-fixer /path/to/repo       # fix a specific path
```

Requires `engineering_score.md` to exist (run the checker first).

## Output Format

The checker writes `engineering_score.md` with:
- **Summary**: plain-language overview of results
- **Recommendations**: concrete, prioritized, actionable steps
- **Full Results**: complete JSON with per-metric scores, weights, and reasons

## Installation

```bash
# From the IDM marketplace: https://github.com/InstituteforDiseaseModeling/idm_standards
# Add to .claude-plugin/marketplace.json or install via Claude Code settings
```

## Updating (for internal use only)

1. Download the engineering quality guidelines document as `admin/engineering_quality_guidelines.md`.
2. Copy `admin/update_prompt.md` into Claude and run.
