# LLM Comparison — Product Teardown: Swiggy

## Models Tested
| # | Model | Mode | Response Time |
|---|-------|------|---------------|
| 1 | Claude | Standard | 6s |
| 2 | Gemini | Fast | 8s |
| 3 | ChatGPT | Standard | 7s |

---

## Layer-by-Layer Scorecard

### Layer 1 — Data Foundation
| Criteria | Claude | Gemini | ChatGPT | Winner |
|----------|--------|--------|---------|--------|
| Specificity (1–5) | 4 | 3 | 3 | Claude |
| Named real technologies? | Y | Y | Y | — |
| Identified real engineering challenge? | Y | Y | Y | — |
| Notes | Most detailed pipeline description | Generic | Generic | |

### Layer 2 — Statistical Analysis & Signal Engineering
| Criteria | Claude | Gemini | ChatGPT | Winner |
|----------|--------|--------|---------|--------|
| Specificity (1–5) | 4 | 4 | 4 | ChatGPT |
| Named real technologies? | Y | Y | Y | — |
| Identified real engineering challenge? | Y | N | Y | Claude |
| Notes | | Missed challenge | Best framing | |

### Layer 3 — Classical / Supervised ML Models
| Criteria | Claude | Gemini | ChatGPT | Winner |
|----------|--------|--------|---------|--------|
| Specificity (1–5) | 5 | 4 | 4 | Claude |
| Named real technologies? | Y | Y | Y | Claude |
| Named model family? | Y | N | N | Claude |
| Identified real engineering challenge? | Y | Y | Y | ChatGPT |
| Notes | Named XGBoost, LightGBM, bandits | Vague on models | Good challenge framing | |

### Layer 4 — LLM / Generative AI
| Criteria | Claude | Gemini | ChatGPT | Winner |
|----------|--------|--------|---------|--------|
| Specificity (1–5) | 3 | 4 | 4 | Gemini |
| Honest if layer not applicable? | N | Y | Y | ChatGPT |
| Notes | Overstated LLM role | Balanced | Honest + specific | |

### Layer 5 — Deployment & MLOps Infrastructure
| Criteria | Claude | Gemini | ChatGPT | Winner |
|----------|--------|--------|---------|--------|
| Specificity (1–5) | 3 | 3 | 4 | ChatGPT |
| Named real technologies? | Y | Y | Y | — |
| Notes | | | Best toolchain detail | |

### Layer 6 — System Design & Scalability
| Criteria | Claude | Gemini | ChatGPT | Winner |
|----------|--------|--------|---------|--------|
| Specificity (1–5) | 3 | 3 | 4 | ChatGPT |
| Named real technologies? | Y | Y | Y | ChatGPT |
| Notes | | | Best systems framing | |

---

## Layer Winners Summary
| Layer | Winner |
|-------|--------|
| Layer 1 — Data Foundation | Claude |
| Layer 2 — Statistical Analysis | ChatGPT |
| Layer 3 — ML Models | Claude |
| Layer 4 — LLM / Generative AI | ChatGPT |
| Layer 5 — MLOps Infrastructure | ChatGPT |
| Layer 6 — System Design | ChatGPT |

---

## Overall Verdict
| Dimension | Winner | Reason |
|-----------|--------|--------|
| Most technically specific | ChatGPT | Consistently named precise tools across all layers |
| Best at naming real technologies | ChatGPT | Accurate, layer-appropriate tech choices throughout |
| Least hallucination | ChatGPT | Straight to the point; did not overstate applicability |
| Best "hardest problem" insight | Claude | Covered all aspects with depth and nuance |
| Best structured output | ChatGPT | Clean, scannable, well-organized across all layers |
| Fastest useful response | Claude | Quickest response at 6s |

---

## Key Observations

> Claude excels at depth — best when you need exhaustive, well-reasoned breakdowns.
> Weak spot: occasionally overstates a layer's applicability rather than admitting limited use.

> ChatGPT wins on balance — specific, honest, and well-structured without being verbose.
> Best default choice for this type of technical teardown prompt.

> Gemini is reliable but plays it safe — rarely wrong, rarely exceptional.
> Good for quick sanity checks; less useful when you need sharp engineering insight.