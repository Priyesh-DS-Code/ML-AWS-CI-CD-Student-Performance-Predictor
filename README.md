# Student Math Score Predictor

An end-to-end Machine Learning project that predicts a student's **math score** based on demographic and academic features. Built with modular code, containerized with Docker, and deployed on **AWS ECS** via an automated **GitHub Actions CI/CD pipeline**.

---

## Problem Statement

Predict a student's math exam score given inputs like gender, parental education level, lunch type, test preparation course completion, and reading/writing scores.

**Target variable:** `math_score` (continuous — regression problem)

---

## Architecture

```
Raw Data → Data Ingestion → Data Transformation → Model Training
                                                        ↓
User Input → Flask Web App → Prediction Pipeline → Predicted Score
                                                        ↑
                                              Model + Preprocessor (artifacts/)
```

---

## Project Structure

```
├── .aws/                        # ECS task definition
├── .github/workflows/aws.yml    # GitHub Actions CI/CD pipeline
├── src/
│   ├── components/
│   │   ├── data_ingestion.py
│   │   ├── data_transformation.py
│   │   └── model_trainer.py
│   ├── pipeline/
│   │   ├── train_pipeline.py
│   │   └── predict_pipeline.py
│   ├── exception.py             # Custom exception handler
│   ├── logger.py                # Centralized logging
│   └── utils.py                 # save/load object, model evaluation
├── artifacts/  
│   ├── data.csv                 # Saved model.pkl & 
│   ├── train.csv
│   ├── test.csv
│   ├── preprocessor.pkl
│   ├── model.pkl               
├── notebook/ 
│   ├── data/   
│   │   └── stud.csv             # student data
├── templates/  
│   │   └── home.html            # Flask HTML templates
│   │   └── index.html                
├── app.py                       # Flask application
├── Dockerfile
├── requirements.txt
└── setup.py
```

---

## Input Features

| Feature | Type | Example |
|---|---|---|
| Gender | Categorical | male / female |
| Race/Ethnicity | Categorical | group A–E |
| Parental Education | Categorical | bachelor's degree |
| Lunch | Categorical | standard / free-reduced |
| Test Prep Course | Categorical | completed / none |
| Reading Score | Numeric | 72 |
| Writing Score | Numeric | 68 |

---

## Models Evaluated

Trained and compared using `GridSearchCV` (3-fold CV), scored by **R²**:

- Linear Regression 
- Random Forest Regressor
- Decision Tree Regressor
- Gradient Boosting Regressor
- AdaBoost Regressor

Best model and preprocessor are saved to `artifacts/` for inference.

---

## Quickstart

### Local Setup

```bash
git clone https://github.com/Priyesh-DS-Code/ML-AWS-CI-CD-Project.git
cd ML-AWS-CI-CD-Project

python -m venv venv && source venv/bin/activate   # Windows: venv\Scripts\activate
pip install -r requirements.txt

python src/pipeline/train_pipeline.py   # trains model, saves artifacts
python app.py                           # starts Flask on http://localhost:5000
```

### Docker

```bash
docker build -t student-score-predictor .
docker run -p 5000:5000 student-score-predictor
```

---

## CI/CD — GitHub Actions + AWS

On every push to `main`:

1. Docker image is built
2. Pushed to **AWS ECR**
3. Deployed to **AWS ECS** (Fargate)

**Required GitHub Secrets:**

```
AWS_ACCESS_KEY_ID
AWS_SECRET_ACCESS_KEY
AWS_REGION
ECR_REPOSITORY
ECS_CLUSTER
ECS_SERVICE
```

---

## Tech Stack

`Python` · `Scikit-learn` · `Flask` · `Docker` · `AWS ECR/ECS` · `GitHub Actions`

---

## Connect

**Priyesh** · [GitHub](https://github.com/Priyesh-DS-Code)
