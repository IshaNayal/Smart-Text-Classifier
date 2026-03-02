# Smart-Text-Classifier

## 1. The Problem: 
In high-volume customer support environments, speed and accuracy are everything. Routing a "Billing" question to a "Technical" support agent wastes time and frustrates customers. 

This project solves this by automating the triage process. It's a machine learning system designed to instantly classify customer queries into three core buckets: **Billing**, **Technical**, and **General**. The goal isn't just "accuracy"—it's building a system that is **fast enough for real-time use** while being **smart enough to understand nuance**.

---

## 2. Approach: A Hybrid Model Strategy
Instead of relying on a single algorithm, I have  implemented a **Dual-Model Architecture** to balance performance and cost. This demonstrates a practical understanding of production constraints where resources aren't infinite.

### ⚡ Model A: LinearSVC (The "Fast Lane")
- **Role**: The workhorse.
- **Why**: Support questions often contain specific keywords (e.g., "invoice", "reset password") that don't require deep neural networks to understand. LinearSVC is lightweight, CPU-efficient, and provides explainable results.
- **Performance**: Achieved **~99.8% F1-Score** on the test set.
- **Use Case**: High-throughput, low-latency categorization.

### 🧠 Model B: DistilBERT (The "Smart Lane")
- **Role**: The expert.
- **Why**: Some queries are ambiguous or sarcastic. DistilBERT (a transformer model) understands *context* rather than just keywords.
- **Optimization**: I have  used *DistilBERT* instead of full BERT to reduce inference latency by 40% while retaining 97% of the performance.
- **Use Case**: Complex queries where the Linear model has low confidence.

---

## 3. Project Structure & Code Clarity


```text
Optima/
├── data/                   # Raw and processed datasets
│   ├── dataset.csv         # Clean, balanced training data
│   └── bitext_raw.csv      # Real-world noisy data for robustness testing
├── models/                 # serialized models & metrics
│   ├── text_classifier.joblib  # LinearSVC model 
│   └── bert/                   # DistilBERT weights 
├── src/                    # Source code
│   ├── app.py              # FastAPI backend
│   ├── streamlit_app.py    # Interactive Frontend 
│   ├── train.py            # LinearSVC Training Pipeline
│   ├── train_bert.py       # BERT Fine-tuning Pipeline
│   └── diagnostic_check.py # Self-healing system check script
└── requirements.txt        # Dependency management
```

---

## 4. Practical Applicability: The "Production" Mindset

1.  **Rest API (FastAPI)**: The core logic is exposed via a standard API, making it easy to integrate into existing CRM systems.
2.  **Interactive UI (Streamlit)**: A dashboard allows stakeholders to "touch and feel" the model, test edge cases, and visualize confidence scores without writing code.
3.  **Resilience**: The system includes a `diagnostic_check.py` to verify environment health (dependencies, model files) before startup—crucial for deployment stability.

---

## 5. Getting Started

### Prerequisites
- Python 3.8+
- Git

### Installation
```bash
# 1. Clone the repo
git clone https://github.com/IshaNayal/Smart-Text-Classifier
cd Optima

# 2. Install dependencies
pip install -r requirements.txt
```

### Running the System
**Option 1: The API (Backend)**
Starts the server at `http://localhost:8000`.
```bash
uvicorn src.app:app --reload
```

**Option 2: The Dashboard (Frontend)**
Opens a web interface in your browser.
```bash
streamlit run src/streamlit_app.py
```

---

## 6. Future Improvements
The next steps can be:
- **Feedback Loop**: Implementing a mechanism for support agents to correct wrong predictions, retraining the model periodically (Active Learning).

![b553ad32-fc93-4074-bda9-b26e8373b913](https://github.com/user-attachments/assets/17caf017-b602-44a4-b49e-a85ca96daa1b)


# 7. Images of the interface running locally
![856bb12c-8632-45ca-9197-5375ab189da0](https://github.com/user-attachments/assets/28ee13ed-a5c3-47cc-8576-8c10a5e6b956)
![0f89d923-315f-4411-851b-2c9af9aabfd5](https://github.com/user-attachments/assets/bdf576d3-f67f-413c-9ce5-ac28c57829d9)
![5ad8f7bb-22e4-4082-9396-ac74d8f52514](https://github.com/user-attachments/assets/7f604ebe-fab8-42be-af02-92604888ba79)
