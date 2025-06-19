# 🔐 Machine Learning-Based Intrusion Detection System (IDS)

This project implements a Machine Learning-based Intrusion Detection System that analyzes network traffic to detect malicious activities. The system uses a **Random Forest** model trained on real-world network datasets and is integrated with:

- 📊 **Prometheus & Grafana** for real-time monitoring
- 📧 **Email alerts** for critical threats
- 🌐 **Flask API** for receiving data and returning predictions
- 🔎 **tshark** (Wireshark CLI) for real-time packet capture and analysis (optional)

---

## 📁 Project Structure

```
.
├── model.ipynb                   # Jupyter Notebook for model training and evaluation
├── app.py                        # Flask API with Prometheus integration and email alerts
├── attack_detection_model.pkl    # Final trained Random Forest model
├── label_encoder.pkl             # Label encoder for decoding predictions
├── requirements.txt              # Python dependencies
├── prometheus.yml                # Prometheus scrape configuration
├── dashboards/                   # Grafana JSON templates (optional)
├── sample_input.csv              # Example input file format
└── README.md                     # This file
```

---

## 🎯 Objective

To reduce false positives and negatives in intrusion detection by leveraging machine learning, replacing static rule-based methods. The system provides:

- High-accuracy intrusion prediction
- Real-time alerts and observability
- Integration with monitoring & DevOps tools

---

## 🧠 ML Model: Random Forest

### ✅ Why Random Forest?

- Robust against overfitting  
- Handles imbalanced data well  
- Easy to interpret using feature importance  
- Performed best across evaluation metrics

### 📊 Evaluation Metrics

| Metric    | Score   |
|-----------|---------|
| Accuracy  | 98.23%  |
| Precision | 97.65%  |
| Recall    | 98.41%  |
| F1-Score  | 98.03%  |

> 📷 **Insert Screenshot 1**: Confusion matrix and classification report from Jupyter Notebook.

---

## 📦 Installation

### 1. Clone Repository

```bash
git clone https://github.com/your-username/ids-ml-system.git
cd ids-ml-system
```

### 2. Install Dependencies

```bash
pip install -r requirements.txt
```

### 3. Run Flask API

```bash
python app.py
```

- API: `http://localhost:5000`  
- Metrics: `http://localhost:5000/metrics`  
- Prometheus: `http://localhost:9090`

---

## 🧪 API Usage

### POST `/predict`

Uploads a `.csv` file and returns predicted labels.

```bash
curl -X POST -F 'file=@sample_input.csv' http://localhost:5000/predict
```

**Sample Output:**

```json
{
  "results": [
    {"prediction": 0, "label": "Normal"},
    {"prediction": 1, "label": "DoS"}
  ],
  "status": "success"
}
```

---

## 📧 Email Alerts

Alerts are sent when high-risk attack labels are detected:

- `DoS`, `Brute Force`, `Web Attack`, `U2R`

> ⚠️ You must configure a Gmail App Password:

```python
sender_email = "your_email@gmail.com"
app_password = "your_16_char_app_password"
```

> 📷 **Insert Screenshot 2**: Example of received email alert.

---

## 📊 Monitoring with Prometheus & Grafana

### Prometheus Metrics

- `flask_predictions_total`
- `flask_success_total`
- `flask_errors_total`
- `flask_prediction_label_total{label="X"}`

### Grafana Setup

Add this to `prometheus.yml`:

```yaml
scrape_configs:
  - job_name: 'flask_ids_api'
    static_configs:
      - targets: ['localhost:5000']
```

- Import dashboards from `/dashboards`
- Run Grafana at: `http://localhost:3000`

> 📷 **Insert Screenshot 3**: Grafana dashboard showing live prediction metrics.

---

## 🌐 Real-Time Packet Detection (tshark)

```bash
tshark -i eth0 -T fields -e frame.time -e ip.src -e ip.dst ... > live_traffic.csv
curl -X POST -F 'file=@live_traffic.csv' http://localhost:5000/predict
```

> 📷 **Insert Screenshot 4**: Tshark CLI and API prediction result.

---

## 📌 Feature Columns Used

```python
REQUIRED_FEATURES = [
  'Src Port', 'Dst Port', 'Protocol', 'Flow Duration',
  'Tot Fwd Pkts', 'Tot Bwd Pkts', 'Pkt Len Mean',
  'Flow Byts/s', 'Flow Pkts/s', 'SYN Flag Cnt',
  'Init Fwd Win Byts', 'ACK Flag Cnt', 'RST Flag Cnt'
]
```

> 📷 **Insert Screenshot 5**: Head of input DataFrame (pandas).

---

## 🚀 Future Enhancements

- Deploy as Docker container
- Use deep learning (e.g., LSTM)
- Enable continuous learning from live traffic

---

## 👨‍💻 Authors

- **Yanushka Kumar**
- **Sahan Amarasena**
- **Kalana Thathsara**

> Focus: Machine Learning, Flask API, Prometheus Integration

