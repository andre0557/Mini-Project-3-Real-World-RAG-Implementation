# Mini-Project #3 — PhishRAG

**Course:** AI Mini-Project Series  
**Student:** Andre McCloud  
**Date:** November 12, 2025

---

## 1. Project Context & Use Case

**Goal:** Identify a realistic RAG use case and clearly describe its purpose.

### 1.1 Problem / Context

**Domain:** Cybersecurity / Threat Intelligence / Phishing Analysis

**Dataset:** Public phishing intelligence sources including PhishTank (verified phishing URLs), CISA Advisories (government alerts), Nazario Corpus (real phishing emails), SpamAssassin (spam and ham emails), and the Hugging Face phishing email dataset.

**Problem:** Security consultants and SOC analysts spend 3 to 5 hours manually reviewing phishing emails, advisories, and threat reports to find patterns and extract tactics and procedures (TTPs). This is slow, inconsistent, and hard to scale during major attack periods.

**Information Gap:** There is no single tool that lets analysts search, summarize, and generate training insights from public phishing intelligence with citations and consistent structure.

### 1.2 Primary Use Case

☑ Research assistant / document summarization (focused on phishing campaign analysis and security training content generation).

### 1.3 Users / Audience

- Cybersecurity consultants (preparing client threat assessments)
- SOC and blue-team analysts (triaging phishing incidents)
- Security awareness managers (building training content)
- Threat intelligence researchers (studying phishing trends and TTP evolution)

### 1.4 Success Criteria

- At least 80% retrieval accuracy (Recall@5)
- At least 90% of generated claims grounded in cited sources
- Average latency under 10 seconds per query
- Average utility rating of 4 out of 5 from test users
- Every answer includes at least one source citation

```yaml
context:
  domain: "Cybersecurity / Threat Intelligence / Phishing Analysis"
  use_case: "Research assistant for phishing campaign analysis"
  users:
    - "Cybersecurity consultants"
    - "SOC and blue-team analysts"
    - "Security awareness managers"
    - "Threat intelligence researchers"
  success:
    - "≥80% retrieval accuracy (Recall@5)"
    - "≥90% groundedness"
    - "≤10s latency per query"
    - "≥4/5 analyst utility rating"
    - "all responses include at least one citation"
```

---

## 2. Data & Constraints

**Goal:** Define the dataset, data format, and project limitations.

### 2.1 Corpus Details

**Sources:** Public phishing datasets that are safe, anonymized, and freely available.

- **PhishTank:** ~30,000 verified phishing URLs (CSV).
- **CISA Advisories:** ~200 government alerts and reports (HTML/PDF).
- **Nazario Corpus:** ~9,000 real phishing emails (MBOX).
- **SpamAssassin Corpus:** ~6,000 spam and legitimate emails (TXT).
- **Hugging Face Phishing Dataset:** ~18,000 labeled emails (Parquet).

**Corpus Characteristics:** Covers 2015–2025 with most samples from 2023–2025. Approx. 95% English; 40% finance, 25% retail, 15% healthcare, 10% technology, 10% other. No PII.

### 2.2 Constraints

- Runs locally on a laptop (no cloud vector DB).
- Budget ≤ $50 for API calls and embeddings.
- Simple, analyst-friendly Streamlit UI.
- Sanitized HTML and no live URL access.
- Public and anonymized sources only.

```yaml
data_constraints:
  sources:
    - "PhishTank (verified phishing URLs)"
    - "CISA Advisories"
    - "Nazario Phishing Email Corpus"
    - "SpamAssassin Corpus"
    - "Hugging Face Phishing Email Dataset"
  formats:
    - "CSV"
    - "HTML"
    - "PDF"
    - "MBOX"
    - "TXT"
    - "Parquet"
  size: "approximately 50,000 documents and emails"
  local_only: true
  cost_limit: "$50 total for embeddings and generation"
  latency_target: 10
  security: "sanitized HTML, no live URL visits, no PII, public datasets only"
```

---

## 3. RAG Architecture (MVP)

**Goal:** Outline the RAG pipeline and main design decisions.

### 3.1 Pipeline

**Ingestion → Preprocessing → Chunking → Embedding → Indexing → Retrieval → Reranking → Generation**

1. Load data from CSV, HTML, PDF, MBOX, TXT, and Parquet.
2. Clean HTML and normalize URLs.
3. Chunk emails into ~500-token segments with header/body separation.
4. Embed text using OpenAI text-embedding-3-large.
5. Index vectors in FAISS and keywords with BM25.
6. Retrieve using hybrid search (FAISS + BM25).
7. Rerank top 10 results with Cohere ReRank.
8. Generate final answers using GPT-4 Turbo with citations.

### 3.2 Design Choices

- Header/body chunking improves precision.
- Hybrid retrieval balances exact and semantic matches.
- FAISS and BM25 are lightweight and local.
- Cohere ReRank enhances precision with low cost.
- GPT-4 Turbo provides reliable citation structure.

```yaml
architecture_mvp:
  steps:
    - "ingest"
    - "preprocess"
    - "chunk"
    - "embed"
    - "index (FAISS + BM25)"
    - "retrieve"
    - "rerank"
    - "generate"
  chunking: "header/body split, 500-token chunks"
  embeddings: "OpenAI text-embedding-3-large"
  vector_db: "FAISS (local)"
  retrieval: "hybrid (FAISS + BM25)"
  reranker: "Cohere ReRank"
  llm: "GPT-4 Turbo"
  citations: true
  rationale: >
    The MVP uses hybrid retrieval and reranking to balance recall and precision
    for phishing text. GPT-4 Turbo is used for accurate, cited summaries that
    analysts can trust.
```

---

## 4. Component Alternatives (Mini-Bakeoff)

**Goal:** Compare tools and justify design choices.

| Component | Option A | Option B | Criteria | Selected | Why |
|-----------|----------|----------|----------|----------|-----|
| Framework | LangChain | LlamaIndex | Ease of use and retrieval chains | LangChain | Simpler setup and FAISS support |
| Vector DB | FAISS | Chroma | Local speed and scale | FAISS | Faster and handles large datasets |
| Embeddings | OpenAI text-embedding-3-large | Instructor-XL (local) | Accuracy vs cost | OpenAI | High semantic coverage for security language |
| Keyword Index | BM25 | Elasticsearch | Setup complexity | BM25 | Lightweight Python-only option |
| Reranker | Cohere ReRank | BGE-reranker-large | Quality vs latency | Cohere ReRank | Strong precision with low cost |
| Generator | GPT-4 Turbo | Claude 3.5 Sonnet | Citation reliability | GPT-4 Turbo | Better structured outputs |
| UI | Streamlit | Gradio | Ease of demo | Streamlit | Fast prototype interface |

```yaml
component_selection:
  framework:
    options: ["LangChain", "LlamaIndex"]
    selected: "LangChain"
    reason: "Simplest hybrid retrieval setup and FAISS integration"
  vector_db:
    options: ["FAISS", "Chroma"]
    selected: "FAISS"
    reason: "Faster local indexing and better scalability"
  embeddings:
    options: ["OpenAI text-embedding-3-large", "Instructor-XL (local)"]
    selected: "OpenAI text-embedding-3-large"
    reason: "Strong performance for phishing-related language"
  keyword_index:
    options: ["BM25 (rank-bm25)", "Elasticsearch"]
    selected: "BM25 (rank-bm25)"
    reason: "No server needed and works well for hybrid search"
  reranker:
    options: ["Cohere ReRank", "BGE-reranker-large"]
    selected: "Cohere ReRank"
    reason: "Improves result precision with minimal setup"
  llm:
    options: ["GPT-4 Turbo", "Claude 3.5 Sonnet"]
    selected: "GPT-4 Turbo"
    reason: "Produces consistent, cited answers"
  ui:
    options: ["Streamlit", "Gradio"]
    selected: "Streamlit"
    reason: "Quick to build and easy for analyst demos"
```

---

## 5. Evaluation Plan & Results

**Goal:** Design and report simple test outcomes.

### 5.1 Test Set

20 manual queries covering:

- 8 factual questions ("What domains were used in PayPal phishing 2024?")
- 7 analytical questions ("How did healthcare phishing change from Q1 to Q3 2024?")
- 5 actionable questions ("Write five training bullets for fake invoice scams.")

### 5.2 Metrics and Simulated Results

| Approach | Recall@5 | Faithfulness (1–5) | Utility (1–5) | Avg Latency (s) | Cost / Query |
|----------|----------|-------------------|---------------|-----------------|--------------|
| BM25 + GPT-4 | 0.65 | 3.8 | 3.2 | 4.2 | $0.08 |
| Hybrid + ReRank + GPT-4 | 0.82 | 4.5 | 4.1 | 8.7 | $0.12 |

**Interpretation:** Hybrid retrieval and reranking improve recall and grounding but slightly increase latency.

**Implementation Note:** Results are simulated estimates based on tool documentation and expected behavior.

```yaml
evaluation:
  questions: 20
  mode: "simulated (manual Q&A and estimated metrics)"
  baseline:
    approach: "BM25 + GPT-4"
    recall5: 0.65
    faithful: 3.8
    utility: 3.2
    avg_latency: 4.2
    cost_per_query: 0.08
  improved:
    approach: "Hybrid (BM25 + FAISS) + Cohere ReRank + GPT-4"
    recall5: 0.82
    faithful: 4.5
    utility: 4.1
    avg_latency: 8.7
    cost_per_query: 0.12
  notes: >
    Evaluation simulates typical analyst queries to show expected performance
    gains from hybrid retrieval and reranking in a phishing intelligence workflow.
```

---

## 6. Risks, Edge Cases, and Future Work

**Goal:** Reflect on real-world concerns and next steps.

**Edge Cases:** Obfuscated URLs, multilingual emails, dead links, imbalanced sectors, and long documents.

**Risks:** Hallucinations, outdated data, prompt injection, and HTML security.

**Mitigations:** Regex normalization, language translation, date-weighted retrieval, "no confident answer" fallback, and sanitization.

**Future Work:** Add time filters, experiment with GraphRAG, connect to SIEM tools, fine-tune embeddings, and automate data refresh.

```yaml
improvements:
  edge_cases:
    - "Obfuscated URLs reduce exact-match retrieval accuracy"
    - "Multilingual data lowers embedding quality"
    - "Dead or outdated links add noise"
    - "Imbalanced sectors bias retrieval"
    - "Long advisories exceed context windows"
  risks:
    - "Hallucinations from weak retrieval"
    - "Outdated summaries from older data"
    - "Prompt injection attempts"
    - "Unsafe HTML content"
  mitigations:
    - "Normalize URLs before embedding"
    - "Translate non-English text before indexing"
    - "Weight retrieval by recency"
    - "Use fallback 'no confident answer' mode"
    - "Fully sanitize all HTML"
  future_work:
    - "Add temporal filters for query results"
    - "Prototype GraphRAG for campaign relationships"
    - "Integrate with SIEM systems"
    - "Fine-tune embeddings and rerankers"
    - "Automate weekly ingestion and indexing"
```

---

## 7. References

**Goal:** List 3–5 key resources actually used.

1. **PhishTank** – Verified Phishing URL Dataset  
   https://phishtank.org/developer_info.php

2. **CISA** – Cybersecurity Advisories and Alerts Portal (2015–2025)  
   https://www.cisa.gov/news-events/cybersecurity-advisories

3. **Nazario, Jose** – Phishing Email Corpus  
   https://academictorrents.com/details/a77cda9a9d89a60dbdfbe581adf6e2df9197995a  
   (Original source archived at http://monkey.org/~jose/phishing/)

4. **Hugging Face** – Phishing Email Dataset (multiple mirrors)
   - https://huggingface.co/datasets/ealvaradob/phishing-dataset
   - https://huggingface.co/datasets/cybersectony/PhishingEmailDetectionv2.0
   - https://huggingface.co/datasets/zefang-liu/phishing-email-dataset

5. **LangChain Documentation** – Retrieval and VectorStore Modules
   - https://python.langchain.com/docs/how_to/vectorstore_retriever/
   - https://python.langchain.com/api_reference/core/vectorstores/langchain_core.vectorstores.base.VectorStore.html

```yaml
references:
  - "PhishTank. Verified Phishing URL Dataset. https://phishtank.org/developer_info.php"
  - "CISA. Cybersecurity Advisories and Alerts Portal (2015–2025). https://www.cisa.gov/news-events/cybersecurity-advisories"
  - "Nazario, Jose. Phishing Email Corpus. https://academictorrents.com/details/a77cda9a9d89a60dbdfbe581adf6e2df9197995a"
  - "Hugging Face. Phishing Email Datasets. 
     https://huggingface.co/datasets/ealvaradob/phishing-dataset; 
     https://huggingface.co/datasets/cybersectony/PhishingEmailDetectionv2.0; 
     https://huggingface.co/datasets/zefang-liu/phishing-email-dataset"
  - "LangChain Documentation. Retrieval and VectorStore Modules. 
     https://python.langchain.com/docs/how_to/vectorstore_retriever/; 
     https://python.langchain.com/api_reference/core/vectorstores/langchain_core.vectorstores.base.VectorStore.html"
```
