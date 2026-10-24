# Enterprise RAG: Asymmetric Knowledge Conflict Evaluator

An evaluation framework designed to measure how Large Language Models (LLMs) handle messy, contradictory data in Enterprise Retrieval-Augmented Generation (RAG) systems.

## 📌 The Problem
Real-world corporate databases are full of outdated manuals, conflicting HR policies, and superseded drafts. When a RAG system retrieves two conflicting documents, LLMs frequently fail by:
1. Blindly trusting the most recent document (ignoring official authority).
2. Prioritizing document text over system metadata (e.g., `Last-Modified` tags).
3. **Compromise Hallucinations:** Inventing a hybrid rule that blends the old and new policies together.

## 🎯 Project Goal
This project evaluates the "compromise hallucination rate" and tests prompt mitigation strategies to ensure AI systems flag contradictions or defer to authoritative sources, rather than confidently serving incorrect information.

## 🔬 Evaluation Scenarios
* **Authority vs. Recency:** Official 2023 Policy vs. Informal 2026 Draft.
* **Metadata Discrepancies:** In-text dates conflicting with file system metadata.
* **Partial Supersession:** Version chains (v1.0 to v2.0) where only specific clauses change.

## 🛠️ Quickstart

1. Clone the repo and install dependencies:
```bash
git clone https://github.com/yourusername/enterprise-rag-conflict-eval.git
cd enterprise-rag-conflict-eval
pip install -r requirements.txt
```
2. Add your OpenAI API Key to a `.env` file.
3. Run the evaluation harness in `notebooks/01_conflict_evaluation.ipynb`.
