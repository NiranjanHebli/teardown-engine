6-Layer Architectural Teardown: Swiggy (AI-Powered Food Delivery Platform)

Layer 1 — Data Foundation
▸ What's happening:
Swiggy ingests massive multi-source data streams: customer order history, restaurant menus, delivery partner GPS traces, weather feeds, and real-time traffic data. This data is stored in a hybrid architecture — transactional data in relational DBs, behavioral/event data in data lakes, and geospatial data in specialized stores. Every order generates ~50+ discrete events (search → browse → cart → pay → dispatch → deliver) all captured for downstream ML.
▸ Key technologies:
Apache Kafka (event streaming), Apache Spark (batch processing), Amazon S3 / Delta Lake (data lake), PostgreSQL (transactional), Redis (session/cache), Google BigQuery (analytics warehouse)
▸ Core engineering challenge:
Synchronizing real-time GPS streams from 300,000+ delivery partners with live order states without data loss or latency spikes during peak hours (e.g., dinner rush).
▸ Skill required:
"5+ years in data engineering; experience with real-time streaming pipelines (Kafka/Flink), distributed storage, and geospatial data modeling at scale."
▸ Honesty check:
✅ Fully utilized — this is the oxygen layer. Every other layer starves without clean, low-latency data pipelines.

Layer 2 — Statistical Analysis & Signal Engineering
▸ What's happening:
Swiggy engineers hundreds of features from raw data: restaurant popularity scores, user price sensitivity indices, delivery time variance by zone/time, and demand surge coefficients. Statistical models detect anomalies like fake reviews, unusual cancellation rates, or fraudulent payment patterns. Time-series decomposition is used to forecast demand by micro-zone for pre-positioning delivery partners.
▸ Key technologies:
Python (pandas, statsmodels, scipy), Apache Spark MLlib, Feast (feature store), Prometheus (metrics monitoring), custom anomaly detection pipelines
▸ Core engineering challenge:
Building a low-latency feature store that serves pre-computed features to ML models in <10ms during real-time inference, while keeping features consistent between training and serving (train-serve skew).
▸ Skill required:
"Strong statistical modeling background; experience with feature stores (Feast/Tecton), time-series forecasting, and fraud signal engineering."
▸ Honesty check:
✅ Heavily used — demand forecasting and surge pricing alone justify a dedicated statistical engineering team.

Layer 3 — Classical / Supervised ML Models
▸ What's happening:
Swiggy deploys gradient-boosted models for ETA prediction (combining distance, traffic, restaurant prep time, and partner availability), recommendation ranking (what restaurants/dishes to surface per user), and dynamic pricing/surge calculation. Bandit algorithms (contextual multi-armed bandits) optimize homepage layout per user in real time. Clustering models (k-means, DBSCAN) segment delivery zones for partner allocation.
▸ Key technologies:
XGBoost / LightGBM (ETA, pricing), Vowpal Wabbit (bandit algorithms), scikit-learn, MLflow (experiment tracking), Kubeflow (ML pipelines), FAISS (similarity search for recommendations)
▸ Core engineering challenge:
ETA prediction accuracy degrades sharply during rain, festivals, or traffic incidents — the model must detect distribution shift in real time and fall back gracefully or retrain on fresh data without downtime.
▸ Skill required:
"ML engineering experience with tree-based models, recommendation systems, and online learning; familiarity with MLflow/Kubeflow and production model monitoring."
▸ Honesty check:
✅ This is the workhorse layer — the vast majority of Swiggy's ML value (ETA, search ranking, recommendations) runs on well-tuned classical models, not LLMs.

Layer 4 — LLM & Generative AI
▸ What's happening:
LLMs power Swiggy's conversational support bot for resolving order complaints, refund requests, and delivery queries without human agents. NLP models (likely fine-tuned BERT variants) handle dish search — mapping colloquial queries like "something spicy under ₹200" to structured menu filters. Generative AI is likely used for automated restaurant description writing and personalized push notification copy.
▸ Key technologies:
Fine-tuned BERT / DistilBERT (search NLP), GPT-4 / Claude API or fine-tuned Llama 2/3 (support bot), LangChain (agent orchestration), Elasticsearch with dense vector search (semantic menu search)
▸ Core engineering challenge:
Keeping the support LLM grounded — it must access live order state (via RAG or tool calls) to answer "where is my order?" accurately, without hallucinating delivery times or refund policies.
▸ Skill required:
"LLM fine-tuning, RAG pipeline design, prompt engineering, and experience integrating generative models with real-time transactional systems."
▸ Honesty check:
⚠️ Partially used — LLMs are genuine but supplementary. Core operations (ETA, ranking, pricing) don't use LLMs. Support automation and search are the real LLM use cases here.

Layer 5 — Deployment & MLOps Infrastructure
▸ What's happening:
Swiggy runs ML models behind low-latency inference APIs, with A/B testing frameworks to shadow-deploy new model versions before full rollout. Models are containerized and served via Kubernetes clusters with auto-scaling tied to order volume metrics. A shadow scoring system runs challenger models in parallel with champion models, comparing predictions before promoting updates.
▸ Key technologies:
Kubernetes + Helm (orchestration), TorchServe / TensorFlow Serving (model serving), Istio (service mesh), Seldon Core or custom inference servers, Grafana + Prometheus (monitoring), ArgoCD (GitOps deployment)
▸ Core engineering challenge:
Zero-downtime model updates during peak hours — a bad ETA model rollout at 8 PM on a Friday could cascade into mass cancellations and partner misrouting within minutes.
▸ Skill required:
"MLOps engineering; Kubernetes, model serving frameworks, CI/CD for ML, and experience with canary/shadow deployment strategies."
▸ Honesty check:
✅ Fully utilized — with dozens of models in production, MLOps isn't optional. It's a dedicated engineering function at Swiggy's scale.

Layer 6 — System Design & Scalability
▸ What's happening:
Swiggy's architecture is microservices-based, with separate services for order management, partner allocation, payments, notifications, and ML inference — each independently deployable and scalable. The dispatch system (matching orders to partners) is an event-driven real-time optimizer, solving a variant of the vehicle routing problem (VRP) under hard latency constraints. During IPL finals or festive sales, the system must handle 10–50x normal traffic spikes with pre-warmed capacity and circuit breakers preventing cascade failures.
▸ Key technologies:
gRPC (inter-service communication), Apache Kafka (event bus), Redis Cluster (distributed caching), AWS Auto Scaling Groups, CDN (CloudFront) for static assets, Circuit Breaker pattern (Hystrix/Resilience4j)
▸ Core engineering challenge:
The dispatch optimizer must assign 300,000+ active delivery partners to incoming orders in <500ms globally, while re-optimizing continuously as partners move, orders arrive, and restaurants update prep times — all in a single consistent system state.
▸ Skill required:
"Distributed systems design, event-driven architecture, experience with high-throughput real-time optimization systems, and chaos engineering."
▸ Honesty check:
✅ This layer is where Swiggy's deepest engineering complexity lives — logistics optimization at this scale is a genuinely hard distributed systems problem.

Overall Analysis
▸ Most critical layer: Layer 3 — Classical ML Models
ETA prediction is Swiggy's trust contract with users. A 5-minute ETA miss degrades NPS, increases cancellations, and loses repeat orders. Every other feature (search, recommendations, pricing) amplifies revenue, but ETA accuracy is existential.