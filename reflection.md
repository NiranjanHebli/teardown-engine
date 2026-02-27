# Reflection

## 1. Which of the 6 layers surprised you the most in terms of complexity for Swiggy? Why?

Layer 6 — System Design & Scalability was the most surprising. On the surface, Swiggy looks like a simple order-and-deliver app, but the dispatch engine is essentially solving a real-time Vehicle Routing Problem (VRP) for 300,000+ active partners in under 500ms. That is not a standard engineering problem — it sits at the intersection of operations research, distributed systems, and live geospatial optimization. The scale and latency constraint together make it far harder than it appears from the outside.

---

## 2. What was the single biggest difference you noticed between the LLMs you tested?

Claude went deep but wide — it explained every layer thoroughly, sometimes at the cost of admitting when a layer was not heavily used. ChatGPT stayed focused and honest — it answered precisely what was asked, flagged low-relevance layers clearly, and avoided over-explaining. Gemini sat in the middle — balanced and readable, but rarely offering the sharpest technical insight. The core difference is that Claude optimizes for completeness, ChatGPT optimizes for precision, and Gemini optimizes for clarity.