# Stress Test: Swiggy Architectural Teardown
## Evaluator: Senior AI Systems Architect
## Method: Layer-by-layer challenge — assumptions, gaps, and weak claims

---

## Layer 1 — Data Foundation

| Claim Made | Stress Test Question | Verdict |
|------------|---------------------|---------|
| 50+ events per order captured | What happens to event ordering guarantees in Kafka during a broker failure at 8 PM peak? | Weak — not addressed |
| Hybrid architecture (PostgreSQL + Delta Lake + Redis) | How is write consistency maintained between Redis cache and PostgreSQL during a flash sale spike? | Gap — no consistency model mentioned |
| GPS traces from 300,000+ partners | At what sampling frequency? 1s? 5s? Too frequent = storage explosion. Too sparse = ETA error. | Unspecified |
| Offline warehouse for training | How stale can training data be before ETA model degrades? No SLA defined. | Gap |

**Verdict: Passes basic structure. Fails on consistency guarantees and data freshness SLAs.**

---

## Layer 2 — Statistical Analysis & Signal Engineering

| Claim Made | Stress Test Question | Verdict |
|------------|---------------------|---------|
| Demand forecasting by micro-zone | How granular is "micro-zone"? 500m radius? 2km? Granularity directly affects partner pre-positioning accuracy. | Unspecified |
| Train-serve skew mentioned | What is the actual detection mechanism? Feature drift alerts? Shadow scoring comparison? | Mentioned but not solved |
| Surge pricing coefficients | Is surge a rule-based multiplier or a learned model output? Big architectural difference. | Ambiguous |

**Verdict: Concepts are correct. Lacks operational specifics on zone granularity and skew detection implementation.**

---

## Layer 3 — Classical / Supervised ML Models

| Claim Made | Stress Test Question | Verdict |
|------------|---------------------|---------|
| XGBoost/LightGBM for ETA | ETA is a regression problem. What is the loss function? MAE? MAPE? How is uncertainty quantified? | Not addressed |
| Bandit algorithms for homepage | What is the reward signal? Click? Order placed? Time to order? Reward definition changes everything. | Unspecified |
| Distribution shift detection during rain | What is the actual mechanism — PSI threshold? KL divergence alert? Automated retraining trigger? | Vague |
| FAISS for recommendations | What embedding model generates the vectors? Collaborative filtering? Two-tower neural net? | Gap |

**Verdict: Strongest layer overall. Still has gaps in loss function definition and reward signal specification.**

---

## Layer 4 — LLM / Generative AI

| Claim Made | Stress Test Question | Verdict |
|------------|---------------------|---------|
| Support bot via RAG over live order state | What is the retrieval latency budget? If order state fetch takes 200ms, the bot becomes unusable. | Not addressed |
| NLP maps colloquial queries to filters | What happens with code-switching? e.g. "thoda spicy aur cheap wala" (Hindi-English mix). | Gap — critical for India |
| LLMs flagged as supplementary | Correct and honest. | Pass |
| Fine-tuned BERT for search | What training data? User query logs? Manually labeled? Data quality defines model quality here. | Unspecified |

**Verdict: Honest about LLM scope. Misses India-specific code-switching challenge which is a real production problem.**

---

## Layer 5 — Deployment & MLOps Infrastructure

| Claim Made | Stress Test Question | Verdict |
|------------|---------------------|---------|
| Shadow scoring before promotion | What metric triggers promotion? Absolute MAPE improvement? Statistical significance threshold? | Unspecified |
| Auto-scaling tied to order volume | What is the scale-up lag? If it takes 3 minutes to spin new pods, a sudden spike causes model timeout. | Gap |
| Zero-downtime model updates | How is this achieved — blue-green deployment? Rolling update? The mechanism matters. | Vague |
| Kubernetes + TorchServe | Is GPU inference used for any model? If yes, GPU cold start latency is a real problem. | Not addressed |

**Verdict: Infrastructure choices are correct. Deployment strategy lacks specifics on rollback criteria and scale-up lag.**

---

## Layer 6 — System Design & Scalability

| Claim Made | Stress Test Question | Verdict |
|------------|---------------------|---------|
| VRP solved in <500ms for 300k partners | VRP is NP-hard. What approximation algorithm is used? Hungarian? Greedy nearest-neighbor? Auction algorithm? | Critical gap |
| Circuit breakers for traffic spikes | If the ML inference service is circuit-broken, what is the fallback? Rule-based ETA? Static estimate? | Not defined |
| 10–50x traffic on IPL/festive days | Is capacity pre-warmed or reactive? Pre-warming requires demand forecasting accuracy at day-level. | Circular dependency not acknowledged |
| Event-driven microservices | How many services touch a single order flow? More services = more failure points. | Not quantified |

**Verdict: Biggest gaps here. VRP approximation method and circuit breaker fallback are production-critical and completely unaddressed.**

---

## Overall Stress Test Scorecard

| Layer | Accuracy | Completeness | Depth | Score |
|-------|----------|--------------|-------|-------|
| Layer 1 — Data Foundation | High | Medium | Medium | 3/5 |
| Layer 2 — Statistical Analysis | High | Medium | Low | 3/5 |
| Layer 3 — ML Models | High | Medium | High | 4/5 |
| Layer 4 — LLM / Generative AI | High | Medium | Medium | 3/5 |
| Layer 5 — MLOps Infrastructure | High | Low | Low | 3/5 |
| Layer 6 — System Design | Medium | Low | Low | 2/5 |

---

## Top 3 Critical Gaps

**1. VRP approximation algorithm (Layer 6)**
Saying "solves VRP in <500ms" without naming the approximation method is like saying
"sorts 1M records instantly" without naming the algorithm. This is the hardest
engineering problem in the entire teardown and gets the least specificity.

**2. Online-offline feature consistency mechanism (Layer 1)**
Train-serve skew is mentioned but the detection and correction mechanism is never defined.
This is exactly the gap that causes silent model degradation in production.

**3. Circuit breaker fallback behavior (Layer 6)**
What does Swiggy show the user when the ML system is down? A hardcoded 30-minute ETA?
A rule-based estimate? This fallback defines user experience during the worst moments
and is completely unaddressed.

---

## Final Verdict

> The teardown is **architecturally honest and technically directionally correct.**
> It would pass a junior design review but fail a senior production readiness review.
> The surface-level claims are solid. The operational specifics — fallbacks,
> approximation methods, consistency guarantees, reward signals — are missing.
> A production architect would ask: "What happens when this breaks?" and the
> teardown has no answer for most layers.

**Rating: Good First Draft. Not Production-Ready Documentation.**