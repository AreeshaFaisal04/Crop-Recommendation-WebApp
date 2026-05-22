# Crop Recommendation System

A full-stack machine learning web application that recommends the most suitable crop for cultivation based on soil nutrient levels and climatic conditions. The system accepts seven agronomic parameters as input and predicts the optimal crop from a set of 57 classes using a Decision Tree classifier served through a Flask REST API and a React-based frontend.

---

## Table of Contents

- [Overview](#overview)
- [Tech Stack](#tech-stack)
- [Dataset](#dataset)
- [Machine Learning Model](#machine-learning-model)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Backend Setup](#backend-setup)
  - [Frontend Setup](#frontend-setup)
- [API Reference](#api-reference)
- [Input Validation](#input-validation)
- [Screenshots](#screenshots)
- [Authors](#authors)

---

## Overview

Selecting the right crop for a given soil and climate profile is one of the most consequential decisions in agriculture. Poor crop selection leads to yield loss, resource waste, and financial strain for farmers. This system addresses that problem by providing an accessible, data-driven recommendation tool that any user can operate through a web browser.

The user enters readings for nitrogen, phosphorous, potassium, temperature, humidity, soil pH, and rainfall. The system processes these inputs through a trained machine learning model and returns the crop best suited to those conditions.

Deployed Link: https://crop-recommendation-app34.vercel.app/

---

## Tech Stack

**Backend**
- Python 3.10+
- Flask
- Flask-CORS
- scikit-learn
- pandas
- NumPy

**Frontend**
- React 19
- Vite
- React Router v6
- Axios
- CSS3

---

## Dataset

The model is trained on a custom dataset of 57,000 records spanning 57 distinct crop classes. Each record encodes min/max ranges for seven agronomic parameters across different soil types, seasons, and water sources. The dataset is perfectly balanced at 1,000 records per class.

| Property | Value |
|---|---|
| Total records | 57,000 |
| Crop classes | 57 |
| Raw features | 23 |
| Derived model features | 7 |
| Records per class | 1,000 (balanced) |

Crops covered include cereals (rice, wheat, maize), pulses (blackgram, greengram), oilseeds (groundnut, sunflower), vegetables (tomato, brinjal, capsicum), cash crops (sugarcane, cotton, jute), and minor millets.

---

## Machine Learning Model

Raw min/max feature pairs are reduced to single representative values by computing their arithmetic mean, producing seven derived features: N\_AVG, P\_AVG, K\_AVG, TEMP\_AVG, HUMIDITY\_AVG, PH\_AVG, and WATER\_AVG. A Decision Tree classifier is trained on these features using the Gini impurity criterion. The trained model and label encoder are serialized as pickle files and loaded by the Flask API at runtime.

The model can be retrained at any time by running `retrain.py` after updating the dataset.

---

## Project Structure

```
crop-recommendation-app/
├── backend/
│   ├── app.py                  # Flask REST API
│   ├── retrain.py              # Model training script
│   ├── Crop_recommendation.csv # Training dataset
│   ├── best_model.pkl          # Trained Decision Tree
│   ├── label_encoder.pkl       # Fitted label encoder
│   └── requirements.txt
├── frontend/
│   ├── public/
│   ├── src/
│   │   ├── components/
│   │   │   ├── Navbar.jsx
│   │   │   ├── Footer.jsx
│   │   │   ├── Hero.jsx
│   │   │   ├── CropForm.jsx
│   │   │   └── ResultCard.jsx
│   │   ├── pages/
│   │   │   ├── Home.jsx
│   │   │   ├── Predict.jsx
│   │   │   └── About.jsx
│   │   ├── App.js
│   │   └── index.js
│   ├── vite.config.js
│   └── package.json
└── README.md
```

---

## Getting Started

### Prerequisites

- Python 3.10 or higher
- Node.js and npm

### Backend Setup

Navigate to the backend directory and install dependencies:

```bash
cd backend
pip install flask flask-cors numpy pandas scikit-learn
```

Start the Flask server:

```bash
python app.py
```

The API will be available at `http://127.0.0.1:5000`. Keep this terminal open while using the application.

### Frontend Setup

Open a second terminal, navigate to the frontend directory, and install dependencies:

```bash
cd frontend
npm install
npm install vite @vitejs/plugin-react --save-dev
```

Start the development server:

```bash
npm run dev
```

Open `http://localhost:5173` in your browser.

Deployed Link: https://crop-recommendation-app34.vercel.app/

---

## API Reference

### POST /predict

Accepts soil and climate parameters and returns a crop recommendation.

**Request body:**

```json
{
  "Nitrogen": 90,
  "Phosphorous": 42,
  "Potassium": 43,
  "Temperature": 25.5,
  "Humidity": 80.0,
  "pH": 6.5,
  "Rainfall": 200.0
}
```

**Success response:**

```json
{
  "crop": "rice"
}
```

**Error response:**

```json
{
  "error": "Rainfall must be between 330 and 2500 mm"
}
```

---

## Input Validation

All inputs are validated server-side against physiologically valid agronomic ranges before the model is invoked. Requests outside these bounds are rejected with HTTP 422.

| Parameter | Min | Max | Unit |
|---|---|---|---|
| Nitrogen | 20 | 200 | kg/ha |
| Phosphorous | 20 | 100 | kg/ha |
| Potassium | 20 | 150 | kg/ha |
| Temperature | 5 | 47 | C |
| Humidity | 15 | 100 | % |
| Soil pH | 5 | 9 | pH |
| Rainfall | 330 | 2500 | mm |

---

## Authors

Developed by Areesha Faisal, Maryam Sana, and Mishal as part of a coursework project at Kinnaird College for Women University, Lahore.
