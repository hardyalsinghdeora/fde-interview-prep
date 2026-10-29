# Vector Databases

Almost every RAG or semantic-search design ends with "where do the embeddings live, and how do you search them fast and correctly?". Senior and principal interviewers use it to test whether you understand approximate search, filtering and cost rather than just naming a product. Agentic-AI Architect, Forward Deployed Engineer and Staff/Principal platform roles ask about it most.

> Related: [AI / LLM](../topics/06-ai-llm/README.md) · [System design](../topics/03-system-design/README.md) · [Data engineering & SQL](../topics/07-data-and-sql/README.md)

---

## A. Fundamentals — must answer crisply
- [ ] What is an embedding, and what does a vector database add on top of storing arrays in a table?
- [ ] Cosine similarity vs. dot product vs. Euclidean (L2) distance — when do they give the same ranking?
- [ ] Why do we normalise embeddings, and what breaks if you mix normalised and unnormalised vectors?
- [ ] Exact (brute-force) k-NN vs. approximate nearest neighbour (ANN) — why accept approximate results?
- [ ] What is recall@k for an ANN index, and how is it different from retrieval relevance?
- [ ] How does HNSW work at a high level (layers, greedy search, `M`, `ef_construction`, `ef_search`)?
- [ ] How does IVF work (centroids, lists, `nprobe`), and how is IVF-PQ different?
- [ ] What is product quantisation / scalar quantisation, and what does it trade away?

## B. Working depth
- [ ] pgvector: column types, index types (HNSW vs. IVFFlat), and the operator for each distance metric *(verify)*.
- [ ] Why must an IVFFlat index in pgvector be built after the table has representative data?
- [ ] Pre-filtering vs. post-filtering vs. filtered (in-graph) search — what goes wrong with each?
- [ ] A metadata filter matches 0.1% of rows and your ANN query returns 2 results instead of 10. Why, and how do you fix it?
- [ ] What is hybrid search, and how do you combine BM25/full-text scores with vector scores (weighted sum vs. reciprocal rank fusion)?
- [ ] Where does a cross-encoder re-ranker fit, and what does it cost in latency?
- [ ] How do chunk size and overlap affect what the vector index can and cannot find?
- [ ] What happens to an existing index when you change embedding model or dimension?
- [ ] How do you handle updates and deletes in an HNSW index (tombstones, rebuilds, vacuum)?
- [ ] Namespaces/collections/partitions — how do you model multi-tenancy in a vector store?

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] pgvector vs. a dedicated store (Pinecone, Weaviate, Qdrant, Milvus, Chroma) — what would make you move off Postgres? *(verify)*
- [ ] Estimate the memory for 50M vectors of 1,536 dimensions in float32 with HNSW. How do you bring it down?
- [ ] How do you tune the recall/latency/memory triangle, and how do you measure recall in production without labels?
- [ ] Per-tenant indexes vs. one shared index with a tenant filter — isolation, noisy neighbours, cost.
- [ ] How do you enforce document-level permissions in retrieval so that nothing leaks to the wrong user?
- [ ] How do you re-embed 100M chunks for a new model without downtime (dual-write, shadow index, cut-over)?
- [ ] Managed serverless vector DB pricing vs. self-hosted — which cost drivers dominate (storage, reads, writes, replicas)? *(verify)*
- [ ] When does keyword search alone beat vector search, and how would you show that with data?
- [ ] What consistency guarantees do you need between the source of truth and the vector index, and how do you keep them in sync (CDC, outbox)?

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] Design retrieval for "ask your docs" over 10M documents with per-user ACLs and a 300 ms p95 budget.
- [ ] A customer says search "got worse" after a data load. Walk through how you would diagnose it.
- [ ] Design a semantic cache for LLM responses: key, similarity threshold, invalidation, false-hit risk.
- [ ] Your Postgres primary is CPU-bound since adding vector search. What are your options, in order?
- [ ] Design product search that mixes exact SKU matches, filters (price, stock) and semantic intent.

## E. Hands-on checks (things to be able to DO)
- [ ] Create a pgvector table and HNSW index, insert embeddings, and write a filtered top-k query.
- [ ] Write a hybrid query in Postgres that fuses `tsvector` full-text rank and vector distance with RRF.
- [ ] Build a small recall benchmark: exact k-NN vs. HNSW at several `ef_search` values, and plot recall vs. latency.
- [ ] Run `EXPLAIN ANALYZE` on a vector query and tell whether the index was used.

---

**Good answers show:**
- Clear separation of ANN recall (index quality) from retrieval relevance (did the right chunk come back).
- Back-of-envelope sizing for memory, storage and cost before picking a product.
- Filtering and permissions treated as first-class design concerns, not an afterthought.
- A default of "start in Postgres, move when a measured limit is hit", with the limit named.
- A re-indexing and model-migration plan, because embeddings are never written once.
- Evaluation numbers (recall@k, MRR, latency percentiles) instead of adjectives.

**Prove it:** Load a public dataset (e.g. 1M Wikipedia passages) into pgvector and Qdrant, build HNSW in both, and publish a short write-up comparing recall@10, p95 latency, memory and build time, with and without a selective metadata filter and with hybrid BM25 + vector fusion.
