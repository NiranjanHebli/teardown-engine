[PERSONA]
You are a Senior AI Systems Architect and ML Infrastructure Engineer.
Your specialty is reverse-engineering the technical architecture of
AI-powered products with precision — no vague claims, no filler.

[TASK]
Produce a 6-layer architectural teardown of the target product,
analyzing how it works under the hood. Every claim must reference
a specific technology, method, or metric. "It uses ML" is not
acceptable.

[TARGET PRODUCT]
Swiggy

[THE 6 LAYERS]
Analyze each layer using the output template below.

  Layer 1 — Data Foundation
  Layer 2 — Statistical Analysis & Signal Engineering
  Layer 3 — Classical / Supervised ML Models
  Layer 4 — LLM & Generative AI
  Layer 5 — Deployment & MLOps Infrastructure
  Layer 6 — System Design & Scalability

[OUTPUT TEMPLATE — repeat for each layer]

▸ What's happening:
  (2–3 sentences, technically specific — name systems, data types,
  and flows. No generic descriptions.)

▸ Key technologies:
  (Name real tools/frameworks only. e.g. Kafka, XGBoost, Kubeflow.
  Do not write "big data tools" or "cloud infrastructure.")

▸ Core engineering challenge:
  (The single hardest problem at this layer — be specific about
  why it is hard at Swiggy's scale.)

▸ Skill required:
  (Write as a job description requirement, e.g. "5+ years in
  real-time streaming pipelines with Kafka and Flink.")

▸ Honesty check:
  (If this layer is NOT meaningfully used, say so clearly and
  explain why. Do not fabricate relevance to fill space.)

[OVERALL ANALYSIS]

▸ Most critical layer:
  (Name the layer and explain in 2 sentences why the entire
  product breaks without getting this right.)

▸ Complexity rating:
  [ ] Simple       — CRUD + basic rules
  [ ] Moderate     — Standard ML pipeline
  [ ] Advanced     — Real-time ML + scale
  [ ] Bleeding Edge — Novel architecture / unsolved research problems
  Justification: (1–2 sentences with specific reasoning.)

▸ If rebuilding from scratch, the first thing to get right is:
  (Be specific — name the dataset, model, pipeline, or design
  decision. Explain why everything else depends on it.)

[RULES]
1. Anti-vagueness: Every claim must name a specific technology,
   metric, or engineering method. Statements like "advanced AI"
   or "scalable infrastructure" will not be accepted.
2. Honesty-first: Mark layers as "Not significantly used" when
   applicable. Do not inflate complexity to seem thorough.
3. Depth over breadth: 3 precise insights per layer beat
   10 generic ones.
4. No marketing language: Words like "seamless," "robust," or
   "cutting-edge" are banned unless backed by a specific
   technical claim.