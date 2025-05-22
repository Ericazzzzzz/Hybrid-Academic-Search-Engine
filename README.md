# Hybrid Academic Search Engine (BM25 + MiniLM)

This project implements a hybrid academic search engine that combines traditional lexical retrieval (BM25) with semantic re-ranking using transformer-based MiniLM embeddings. It features an end-to-end pipeline for document preprocessing, search API development, frontend UI, and performance evaluation.

---

## 📁 Project Structure
The project is organised into three major components, each provided in its own directory or zip file:
- **File1_Preprocessing_and_Retrieval**
  - Preprocessing pipeline and implementation of BM25 and hybrid (BM25 + MiniLM) retrieval.

- **File2_API_and_GUI**
  - FastAPI-based backend and a simple frontend interface for querying the search engine.

- **File3_Evaluation**
  - Evaluation scripts and metrics to assess model performance.


---

## 🔍 File 1: Preprocessing and Retrieval

This component performs the following steps:

- Preprocessing metadata from the ArXiv dataset
- Filtering for `cs.CV` (Computer Vision) category papers
- Text normalization: lowercasing, punctuation & stopword removal, lemmatization
- Creation of an inverted index
- Implementation of document retrieval using BM25 and a hybrid BM25 + MiniLM method

### ✅ Colab Compatibility

- Fully compatible with **Google Colab**
- No need to mount Google Drive
- All files are saved to `/content/` during execution

### 📄 Output Files

| File                        | Description                                   |
|----------------------------|-----------------------------------------------|
| `1_cs_cv_subcategories.csv`| Filtered metadata for CV papers               |
| `2_lemmatized_data.csv`    | Cleaned and lemmatized abstracts              |
| `3_inverted_index.json`    | Inverted index of tokenized terms             |

These files are automatically generated during notebook execution.

---

## 🌐 File 2: API and GUI

This component exposes the hybrid search engine via a FastAPI backend and includes a simple web UI.

### ⚠ Compatibility

- ✅ Supported: **Windows**
- ❌ Not supported: **macOS** (due to MiniLM GPU incompatibility with Apple's MPS backend)

### 📉 Dataset Notice

To meet submission constraints, a **10% subset** of the full dataset (`2_lemmatized_data_subset.csv`) is included.

- **Download full dataset (~100MB)**:  
  [Click here (Google Drive)](https://drive.google.com/uc?export=download&id=1QHjuTZprI-tkLLSQtyE-Fg0L2mpDjsPJ)

To switch to the full dataset, edit the `search_api.py` file:

```python
# Load the full dataset (after download)
# df = pd.read_csv("2_lemmatized_data.csv")
```

### ⚙ Setup Instructions (Windows Only)
Run all commands from the root of the API directory:

```python
# Step 1: Create virtual environment
python -m venv venv

# Step 2: Activate the environment
call venv\Scripts\activate

# Step 3: Install dependencies
pip install -r requirements.txt

# Step 4: Run the FastAPI server
uvicorn search_api:app --reload

# Step 5: Visit API docs in browser
http://127.0.0.1:8000/docs
```
💡 If search takes longer than 30s, try refreshing. Typical response time is under 20s.

---
## 📊 Evaluation

This module assesses the performance of both retrieval models:

- **BM25**
- **Hybrid (BM25 + MiniLM)**

Evaluation is based on manually labeled relevance judgments for a set of queries and their LLM-generated variants.

### 🧪 Evaluation Setup

- 5 base test queries
- Each query has 5 LLM-generated variants (total 25 variants)
- Relevance labels (`1` for relevant, `0` for irrelevant) were manually assigned by reading the retrieved abstracts


### ▶️ How to Run `evaluation.py`

You can run the script either locally or on Google Colab.

#### *Option 1: Running Locally*

1. Ensure `evaluation.py` and the `Data/` folder are in the same directory.
2. Open a terminal and run:

```bash
python evaluation.py
```
#### *Option 2: Running on Google Colab*
1. Upload evaluation.py and the Data/ folder to the Colab environment. In a code cell, run:
   
```python
!python evaluation.py
```

⚠️ Important: The script uses relative paths (e.g., Data/file.csv). Make sure:

- You do not use absolute paths (e.g., /content/filename.csv)

- The `Data/` folder is in the same directory as the script. The `Data/` folder includes:

| File                        | Description                                   |
|----------------------------|-----------------------------------------------|
| `queries.csv`| 5 base queries and 25 LLM-generated variants              |
| `bm25_retrieval_results.csv`    | Ranked retrieval results using BM25    |
| `hybrid_retrieval_results.csv`    | Ranked retrieval results using Hybrid model |
| `bm25_relevance_labels.csv`  | Binary relevance labels for BM25 results |
| `hybrid_relevance_labels.csv` | Binary relevance labels for Hybrid results |

All retrieval results were saved manually from the retrieval notebook. Relevance labels were manually annotated based on abstract content and used as ground truth for computing evaluation metrics.

---

## 📈 Performance Summary

| Model   | Precision  |
|---------|------------|
| BM25    | 47%        |
| BM25 + MiniLM | 73%  |

The hybrid model achieved a 26% absolute improvement in retrieval precision over BM25 alone.

---

## 🛠 Technologies Used
- Python (NLTK, Pandas, Scikit-learn)

- HuggingFace Transformers (MiniLM)

- FastAPI + Uvicorn (RESTful API)

- HTML / CSS / JavaScript (UI)


