# 🛡️ PhishGuard — Phishing Website Detection System

> Instantly detect whether a URL is legitimate or a phishing threat using Machine Learning.

PhishGuard is a machine learning-powered web application that analyzes URLs and classifies them as **safe** or **phishing** in real time. Built with a Logistic Regression model trained on thousands of real-world URLs, it achieves ~96% accuracy.

---

## ✨ Features

- 🔍 **URL Analysis** — strips protocol/www prefix and analyzes the raw domain structure
- 🤖 **ML-Powered Detection** — Logistic Regression model trained on a labeled phishing dataset
- ⚡ **Real-time Prediction** — results displayed instantly on form submission
- 🌐 **Simple Web UI** — clean, responsive interface built with Tailwind CSS + Bootstrap
- 📦 **Pre-trained Model** — no training needed; just load and run

---

## 🧠 How It Works

```
User enters URL
      ↓
Strip https:// / www. prefix
      ↓
Tokenize → Stem → Vectorize (CountVectorizer)
      ↓
Logistic Regression Model predicts: good / bad
      ↓
Result displayed on screen ✅ or 🚨
```

### ML Pipeline Summary

| Step | Detail |
|---|---|
| Dataset | `phishing_site_urls.csv` — labeled URLs (`good` / `bad`) |
| Tokenization | NLTK `RegexpTokenizer` — extracts alphabetic tokens from URL |
| Stemming | NLTK `SnowballStemmer` — reduces tokens to root forms |
| Vectorization | `CountVectorizer` — converts text to bag-of-words features |
| Model | `LogisticRegression` (scikit-learn) |
| Train/Test Split | 80% train / 20% test |
| Accuracy | **~96%** on test set (not overfitted) |

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Backend | Python, Flask |
| ML | scikit-learn, NLTK |
| Frontend | HTML, Tailwind CSS, Bootstrap 5 |
| Model Storage | Pickle (`.pkl`) |
| Data | CSV dataset of labeled URLs |

---

## 📁 Project Structure

```
PhishGuard/
│
├── app.py                           # Flask web server — URL input & prediction logic
├── phishing.pkl                     # Trained Logistic Regression model
├── vectorizer.pkl                   # Fitted CountVectorizer
│
├── templates/
│   └── index.html                   # Web UI (Tailwind CSS + Bootstrap 5)
│
├── Phishing_Website_Detection.ipynb # Model training & EDA notebook
├── phishing_site_urls.csv           # Dataset used for training
│
└── README.md
```

---

## ⚙️ Setup & Installation

### Prerequisites

- Python 3.8+
- pip

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/PhishGuard.git
cd PhishGuard
```

### 2. Install Dependencies

```bash
pip install flask scikit-learn nltk pandas numpy matplotlib wordcloud
```

### 3. Run the App

```bash
python app.py
```

### 4. Open in Browser

```
http://localhost:5000
```

---

## 🧪 Usage

1. Open the app in your browser
2. Enter any website URL in the input field (e.g., `https://www.youtube.com` or a suspicious link)
3. Click **Submit**
4. The result will display:
   - ✅ `This is not a Phishing Website` — the URL appears safe
   - 🚨 `This is a Phishing Website!!` — the URL is flagged as a phishing threat

### Example URLs

| URL | Prediction |
|---|---|
| `www.youtube.com` | ✅ Safe |
| `yeniik.com.tr/wp-admin/js/login.alibaba.com` | 🚨 Phishing |
| `restorevisioncenters.com/html/technology.html` | ✅ Safe |
| `svision-online.de/mgfi/administrator/components/...` | 🚨 Phishing |

---

## 📊 Model Training (Notebook)

The `Phishing_Website_Detection.ipynb` notebook walks through the full ML pipeline:

1. **EDA** — dataset shape, null checks, label distribution
2. **Text Preprocessing** — tokenization + stemming of URL strings
3. **Word Cloud Visualizations** — patterns in good vs bad URL text
4. **Feature Engineering** — CountVectorizer bag-of-words representation
5. **Model Training** — Logistic Regression
6. **Evaluation** — ~96% test accuracy, no overfitting confirmed
7. **Model Export** — saved as `phishing.pkl` and `vectorizer.pkl` via Pickle

To retrain the model:

```bash
jupyter notebook Phishing_Website_Detection.ipynb
```

---

## 📌 Notes

- The model strips `https://`, `http://`, and `www.` from input URLs before prediction — you can enter URLs with or without these prefixes
- Detection is based purely on **URL string patterns** (text features) — no actual HTTP requests are made to the website
- For best results, provide the full URL path rather than just the domain name

---

## 🤝 Contributing

Pull requests are welcome! If you'd like to improve model accuracy, add new features, or enhance the UI — feel free to fork and submit a PR.

---

---

<p align="center">Made with 🛡️ using Flask, scikit-learn & NLTK</p>
