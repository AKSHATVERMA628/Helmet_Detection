# 🪖 YOLOv8 Helmet Detection (Render Deployed API)

Real-time helmet detection using **YOLOv8** for industrial safety applications.
This project provides a **complete pipeline** from dataset preparation and training to a **cloud-deployed REST API on Render** with live image inference and downloadable results.

---

## 🚀 Live API (Render)

Your FastAPI service is deployed on **Render**.

* **Base URL:**

  ```
  https://<your-render-app>.onrender.com
  ```

* **Health Check:**

  ```
  GET /
  ```

* **Swagger Docs (UI):**

  ```
  https://<your-render-app>.onrender.com/docs
  ```

The `/docs` endpoint provides an interactive interface where you can upload images and see detections instantly.

---

## 📌 Project Overview

* **Problem:** Detect whether workers are wearing helmets in industrial environments.
* **Goal:** Train and deploy a YOLOv8 model for real-time helmet detection.
* **End-to-End Pipeline:**

  1. Dataset collection & annotation
  2. Model training (YOLOv8)
  3. Inference on images
  4. REST API using FastAPI
  5. **Cloud deployment using Render**
  6. Live testing via Swagger UI

---

## 📂 Project Structure

```
Helmet_Detection/
├── app/
│   ├── app.py          # FastAPI app
│   ├── uploads/        # Uploaded images
│   └── results/        # Annotated outputs
├── data/
│   └── images/
│       ├── train/
│       └── valid/
├── model/
│   └── best.pt         # YOLOv8 trained model
├── notebooks/
│   └── helmet_detection_yolov8.ipynb
├── src/
│   ├── train.py
│   ├── detect.py
│   └── utils.py
├── requirements.txt
├── README.md
└── .gitignore
```

---

## ⚙️ Installation (Local)

```bash
git clone https://github.com/AKSHATVERMA628/Helmet_Detection.git
cd Helmet_Detection
pip install -r requirements.txt
```

---

## 📊 Dataset Preparation

1. Collect helmet images (Roboflow, Kaggle, CCTV, etc.)
2. Annotate using **LabelImg** or **Roboflow** (YOLO format).
3. Organize as:

```
data/
├── images/
│   ├── train/
│   └── valid/
└── labels/
    ├── train/
    └── valid/
```

Create `data.yaml` with dataset paths and class names.

---

## 🧠 Model Training

### Notebook

Open:

```
notebooks/helmet_detection_yolov8.ipynb
```

### Script

```bash
python src/train.py --data data/data.yaml --epochs 50 --img-size 640 --batch-size 16
```

---

## 🔍 Inference (Local)

```python
from ultralytics import YOLO

model = YOLO("model/best.pt")
results = model.predict("data/images/valid/sample.jpg", conf=0.5, save=True)
```

---

## 🌐 REST API (FastAPI + Render)

### Run Locally

```bash
uvicorn app.app:app --reload
```

### Endpoints

| Method | Path                     | Description                     |
| ------ | ------------------------ | ------------------------------- |
| GET    | `/`                      | Health check                    |
| POST   | `/predict/`              | Upload image and get detections |
| GET    | `/download/{image_name}` | Download annotated image        |
| GET    | `/docs`                  | Swagger API UI                  |

---

## 🧪 Test via Swagger UI

1. Open:

```
https://<your-render-app>.onrender.com/docs
```

2. Click **POST /predict/**
3. Upload an image
4. Click **Execute**
5. You’ll receive:

```json
{
  "filename": "image.jpg",
  "detections": [...],
  "result_url": "https://<your-app>.onrender.com/download/xxxx_result.jpg"
}
```

Open `result_url` to view the detection image.

---

## ☁️ Render Deployment

**Build Command**

```
pip install -r requirements.txt
```

**Start Command**

```
uvicorn app.app:app --host 0.0.0.0 --port $PORT
```

---

## 🔮 Future Enhancements

* Helmet type classification
* Video & webcam stream detection
* Real-time dashboard
* Multi-camera monitoring
