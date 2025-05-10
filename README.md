# 📆 AI-Based Emergency Assistance Research

This repository presents a research-driven prototype of a bilingual AI system for emergency preparedness. It includes two key AI models:

1. **Personalized Equipment Recommendation System**: GPT-4-powered generator that creates a custom emergency supply list based on user context.
2. **Retrieval-Augmented Generation (RAG) QA System**: LangChain-based pipeline for answering emergency-related questions based on official Home Front Command documents.

---

## 🚀 Project Goals

* Provide **clear and personalized guidance** in emergency scenarios
* Support **Hebrew and English** input/output
* Ensure **accuracy, relevance, and explainability**
* Evaluate and compare different prompting and retrieval strategies

---

## 📊 System Overview

### 1. Personalized Equipment Recommendation

> Accepts free-form user input (e.g., "I live with my partner, our baby, and a dog") and generates a tailored emergency equipment list.

**Key Features**:

* Language detection via `langdetect`
* Named Entity Recognition:

  * `spaCy` (English)
  * GPT-4-based fact extraction
* GPT-4 prompt construction using official recommendations
* Output: Markdown table with `| name | quantity | category |`
* Quantity constraint: numeric + unit only (e.g., `3 liters`, `5 copies`)
* Saved in Supabase (`equipment_lists` & `equipment_items`)

### 2. Emergency QA via RAG

> Answers user questions like "Where should I go if there’s no shelter?"

**Pipeline**:

* Local HTML scraping of Pikud HaOref site
* LangChain `Document` creation + `FAISS` vector indexing
* Prompting with:

  * Base template (contextual answers only)
  * StepBack prompting (reasoning before answer)
* Fallback logic:

  * If retrieval fails or is weak, use GPT-4 general knowledge with disclaimer

**Methods Compared**:

* `RAG`: Simple retrieval + answer
* `StepBack`: Think-then-answer
* `Hybrid`: TF-IDF re-ranking
* `Fallback`: Pure GPT-4 if needed

---

## 📊 Testing & Evaluation

### Equipment System:

* 5 diverse test scenarios
* Hybrid keyword matching (exact, fuzzy, GPT-based)
* Achieved 100% accuracy in all semantic match tests

### RAG QA System:

* Compared methods on 5 questions (Hebrew + English)
* Evaluation criteria:

  * Relevance
  * Completeness
  * Clarity
* Scored using GPT-based grader

**Summary Table:**

| Method   | Relevance | Completeness | Clarity | Average  |
| -------- | --------- | ------------ | ------- | -------- |
| StepBack | 1.00      | 1.00         | 1.00    | **1.00** |
| Fallback | 0.90      | 0.94         | 1.00    | 0.95     |
| RAG      | 0.60      | 0.60         | 1.00    | 0.73     |
| Hybrid   | 0.00      | 0.00         | 1.00    | 0.33     |

---

## 🚀 Operational Recommendation

> For production use:

* Default to **StepBack prompting**
* Use **Fallback** when documents are irrelevant or missing
* Avoid pure Hybrid method unless re-ranking is improved

This ensures:

* High accuracy and contextuality
* Safety-oriented answers
* Minimal hallucination risk

---

## 🛋️ Setup Instructions

### Requirements:

```bash
pip install openai supabase pandas python-dotenv langdetect transformers fuzzywuzzy spacy
python -m spacy download en_core_web_sm
```

### Environment:

* Requires `.env` file with `OPENAI_API_KEY`
* Supabase credentials for saving equipment lists

---

## 📂 File Structure

```
├── data_sources_en/         # Local HTMLs from Pikud HaOref (English)
├── data_sources_heb/        # Local HTMLs from Pikud HaOref (Hebrew)
├── pikud_haoref_faiss_index # FAISS index directory
├── equipment_model.py       # Personalized equipment logic
├── rag_model.py             # RAG logic and question answering
├── tests.ipynb              # Evaluation notebook
└── README.md                # This file
```

---

## 📊 Future Work

* Add location-aware shelter finder
* Improve TF-IDF hybrid performance
* Cache fallback completions for reduced cost
* Expand support for children’s and elderly-specific needs

---

## 😊 Credits

Developed as part of a research project at the Academic College of Tel Aviv-Yaffo.

---

## 💡 License

This is a research prototype. Not intended for real-time use during emergencies.
