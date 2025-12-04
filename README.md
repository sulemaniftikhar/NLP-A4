# DiReCT: Diagnostic Reasoning for Clinical Notes

[![Python 3.11+](https://img.shields.io/badge/python-3.11+-blue.svg)](https://www.python.org/downloads/)

A Retrieval-Augmented Generation (RAG) system for clinical diagnostic assistance using MIMIC-IV clinical notes.

## 🎯 Overview

DiReCT combines semantic search with LLMs to provide evidence-based clinical responses with full source attribution.

**Features:**
- 🔍 Semantic search across 511 clinical documents (3,444 chunks)
- 🤖 Zephyr-7B-Beta LLM with 4-bit quantization
- 📊 Automated cohesion scoring (1-5 scale)
- 🌐 Interactive Streamlit web interface
- ⚡ Sub-second retrieval, ~5-8s total query time

## 🏗️ Architecture

```
User Query → Embedding (MiniLM) → Vector Search (ChromaDB) 
→ Context + Prompt → LLM (Zephyr-7B) → Response + Sources + Score
```

## 📚 Dataset

**MIMIC-IV-Ext Direct**: De-identified clinical notes covering 27 conditions
- Cardiovascular, Respiratory, Neurological, GI, Endocrine disorders
- 511 documents → 3,444 chunks (1000 chars, 200 overlap)

## 🚀 Installation

```bash
# Clone repo
git clone https://github.com/sulemaniftikhar/NLP-A4.git
cd NLP-A4

# Install dependencies
pip install -r requirements.txt

# Place dataset in data/mimic-iv-ext-direct-1.0.0/Finished/
```

**Requirements:**
- Python 3.11+
- CUDA GPU (8GB+ VRAM recommended)
- 16GB RAM, 10GB disk space

## 💻 Usage

### Streamlit App
```bash
streamlit run app.py
```

### Jupyter Notebook
```bash
jupyter notebook notebookc7a10b025b.ipynb
```

### Python Script
```python
from direct_rag import DiReCTSystem

system = DiReCTSystem(dataset_path="./data/...", db_path="./chroma_db")
system.build_database()  # First time only

response = system.get_answer("What are symptoms of heart failure?")
print(response['answer'])
```

## 🔧 Components

| Component | Technology | Purpose |
|-----------|-----------|---------|
| Embeddings | all-MiniLM-L6-v2 | 384-dim semantic vectors |
| Vector DB | ChromaDB | Persistent similarity search |
| LLM | Zephyr-7B-Beta (4-bit) | Response generation |
| Frontend | Streamlit | Web interface |
| Evaluation | LLM-as-judge | Cohesion scoring |

## 💡 Example

**Query**: "What are diagnostic criteria for heart failure?"

**Response**: 
```
Heart failure diagnosis involves:
- LVEF < 40% (reduced ejection fraction)
- Symptoms: dyspnea, fatigue, edema
- Physical findings: elevated JVP, pulmonary crackles
- Tests: BNP levels, echocardiography, chest X-ray
```

## 🎯 Performance

| Metric | Value |
|--------|-------|
| Query latency | ~5-8s |
| Retrieval time | <50ms |
| GPU memory (4-bit) | ~7GB |
| Database size | ~20MB |

## ⚠️ Limitations

- Limited to 27 medical conditions
- General embeddings (not medical-specific)
- Top-3 retrieval only
- Research prototype—NOT for clinical use

## 🚧 Future Work

- Domain-specific embeddings (BioBERT)
- Hybrid search (keyword + semantic)
- Multi-modal support (images, labs)
- Temporal reasoning for disease progression
- Fine-tune on medical literature

## 📁 Project Structure

```
DiReCT-RAG/
├── app.py                    # Streamlit app
├── notebookc7a10b025b.ipynb # Jupyter notebook
├── requirements.txt          # Dependencies
├── data/                     # Dataset (place here)
├── chroma_db/               # Vector database (generated)
└── README.md
```
## Medium Blog
- Blog: [Medium Article](https://ifsuleman.medium.com/building-direct-a-rag-system-for-clinical-diagnosis-9d23a244ec51)
