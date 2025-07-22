# 🌾 FarmChatBot: Farming Q&A Assistant with RAG and Evaluation

FarmBot is an intelligent agricultural assistant that answers farming-related queries using a hybrid Retrieval-Augmented Generation (RAG) pipeline. It combines dynamic web-based context retrieval with a large static Q&A dataset, powered by a state-of-the-art LLM via Groq.

---

## 🧠 Key Features

- 🔍 **Retrieval-Augmented Generation (RAG)**: Combines static QA knowledge base with real-time web scraping using `n8n` workflows.
- 🌐 **Dynamic Context**: Scrapes top relevant farming articles using URLs from an `n8n` webhook.
- 📚 **Q&A Embedding Retrieval**: Uses `ChromaDB` to store and retrieve over 170,000+ QA pairs.
- 🤖 **Groq + LLaMA 4**: Uses Groq API to generate high-speed, context-aware answers.
- ✅ **Evaluation Metrics**:
  - BLEU Score
  - ROUGE Score
  - Cosine Similarity between answer and reference embeddings
  - Precision, Recall, and F1 Score (based on keyword overlap)
- 🌍 **Multilingual + Audio Support**: Seamless interaction for users in Hindi and other Indian languages via voice input/output.
- 🔧 **Modular Pipeline**: Built using Jupyter notebooks for extensibility, analysis, and debugging.

---

## 📁 Project Structure

```
📂 FarmBot/
│
├── farmBot_Final.ipynb             # Embedding creation and pipeline setup
├── RAG_test(web+qa).ipynb          # Real-time RAG + Evaluation code
├── evaluated_out_ref2.csv          # Evaluation output with various metrics
├── questionsv4.csv                 # Main QA dataset
└── README.md                       # This file
```

## 📦 Dependencies

Install all required libraries:

```bash
pip install pandas nltk rouge-score sentence-transformers chromadb groq scikit-learn tqdm trafilatura
```

## 🧠 How It Works

### 1. **Training Phase**
- Load ~1.78 lakh QA pairs from the `questionsv4.csv`.
- Convert questions into embeddings using `all-MiniLM-L6-v2`.
- Store them in a persistent `ChromaDB` collection `qa_context`.

### 2. **Testing Phase**
- For each new query:
  - 🔍 Fetch dynamic web context via `n8n` webhook.
  - 🧹 Clean HTML using `trafilatura`.
  - 🧠 Convert scraped content into embeddings and store in `web_context`.
  - 🎯 Retrieve top-2 contexts from both QA and Web.
  - 🤖 Send combined context + query to Groq LLM for answer generation.

### 3. **Evaluation**
- Evaluate answer vs reference using:
  - BLEU (n-gram precision)
  - ROUGE (recall-based)
  - Cosine Similarity (semantic closeness)
  - Precision/Recall/F1 based on manually selected or auto-extracted keywords.

## 📊 Sample Evaluation Output

| Query                               | BLEU | ROUGE-L | Precision | Recall | F1   |
|------------------------------------|------|---------|-----------|--------|------|
| How to treat aphids in brinjal?    | 0.62 | 0.81    | 0.75      | 0.67   | 0.71 |
| How to apply for Kisan Credit Card | 0.49 | 0.74    | 0.60      | 0.80   | 0.69 |

## 🔍 Planned Improvements

- ✅ Integrate automatic keyword extraction (e.g., using YAKE or spaCy).
- ⏳ Reduce Groq token usage by compressing scraped content.
- ⛅ Move to Dev Tier for better rate limits on Groq API.
- 📈 Create a Streamlit interface for real-time interaction.

## 🤝 Acknowledgements

- [Groq](https://groq.com) for fast LLM API access.
- [ChromaDB](https://www.trychroma.com/) for vector DB.
- [n8n](https://n8n.io) for serverless scraping automation.
- [DA-IICT](https://www.daiict.ac.in/) faculty for project guidance.
