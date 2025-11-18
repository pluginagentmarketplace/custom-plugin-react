---
name: ai-machine-learning
description: Build machine learning models with PyTorch, TensorFlow, and scikit-learn. Work with LLMs, implement MLOps pipelines, and deploy AI systems. Use when building predictive models, working with data, or implementing AI features.
---

# AI & Machine Learning

## Quick Start

### scikit-learn for classification:

```python
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import accuracy_score

# Load data
X, y = load_iris(return_X_y=True)
X_train, X_test, y_train, y_test = train_test_split(X, y)

# Train model
model = RandomForestClassifier(n_estimators=100)
model.fit(X_train, y_train)

# Evaluate
predictions = model.predict(X_test)
accuracy = accuracy_score(y_test, predictions)
print(f"Accuracy: {accuracy:.2f}")
```

### PyTorch neural network:

```python
import torch
import torch.nn as nn
from torch.utils.data import DataLoader, TensorDataset

class NeuralNet(nn.Module):
    def __init__(self, input_size, hidden_size, num_classes):
        super().__init__()
        self.fc1 = nn.Linear(input_size, hidden_size)
        self.relu = nn.ReLU()
        self.fc2 = nn.Linear(hidden_size, num_classes)

    def forward(self, x):
        x = self.fc1(x)
        x = self.relu(x)
        x = self.fc2(x)
        return x

# Training loop
model = NeuralNet(784, 128, 10)
criterion = nn.CrossEntropyLoss()
optimizer = torch.optim.Adam(model.parameters(), lr=0.001)

for epoch in range(10):
    for X_batch, y_batch in train_loader:
        outputs = model(X_batch)
        loss = criterion(outputs, y_batch)
        optimizer.zero_grad()
        loss.backward()
        optimizer.step()
```

### LLM prompting with OpenAI API:

```python
import openai

openai.api_key = "your-api-key"

response = openai.ChatCompletion.create(
    model="gpt-4",
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "What is machine learning?"}
    ],
    temperature=0.7,
    max_tokens=1000
)

print(response['choices'][0]['message']['content'])
```

## Core Concepts

**Machine Learning Workflow**
- Problem definition and data collection
- Exploratory data analysis (EDA)
- Feature engineering and preprocessing
- Model selection and training
- Evaluation and hyperparameter tuning
- Deployment and monitoring

**Deep Learning**
- Neural network architecture design
- Activation functions and layers
- Backpropagation and gradient descent
- Convolutional networks (CNN)
- Recurrent networks (RNN, LSTM)
- Transformer architecture

**Large Language Models (LLMs)**
- Prompt engineering techniques
- Fine-tuning vs in-context learning
- Retrieval Augmented Generation (RAG)
- Model evaluation metrics
- Cost and latency optimization

**MLOps & Production**
- Data versioning and validation
- Model training pipelines
- Experiment tracking
- Model deployment and serving
- Monitoring and retraining

## Best Practices

1. **Data Handling**
   - Understand your data distribution
   - Handle missing values and outliers
   - Perform proper train-test split
   - Use stratified sampling for imbalanced data
   - Document data preprocessing steps

2. **Model Development**
   - Start simple, increase complexity
   - Use cross-validation
   - Monitor training and validation loss
   - Prevent overfitting with regularization
   - Use appropriate metrics for your task

3. **Experiment Tracking**
   - Log hyperparameters and results
   - Version control for code and data
   - Use MLflow, Weights & Biases
   - Document model decisions
   - Version your models

4. **Deployment**
   - API serve models with FastAPI, Flask
   - Use containers for reproducibility
   - Monitor model performance in production
   - Implement retraining pipelines
   - Test with real-world data

## Common Patterns

**Data pipeline with Pandas:**
```python
import pandas as pd
from sklearn.preprocessing import StandardScaler

# Load and explore
df = pd.read_csv('data.csv')
print(df.describe())

# Preprocessing
df = df.dropna()
df['age'] = df['age'].fillna(df['age'].median())

# Feature scaling
scaler = StandardScaler()
df[['age', 'income']] = scaler.fit_transform(df[['age', 'income']])
```

**Model evaluation:**
```python
from sklearn.metrics import (
    accuracy_score, precision_score, recall_score,
    f1_score, confusion_matrix, roc_auc_score
)

y_pred = model.predict(X_test)
print(f"Accuracy: {accuracy_score(y_test, y_pred):.3f}")
print(f"Precision: {precision_score(y_test, y_pred):.3f}")
print(f"Recall: {recall_score(y_test, y_pred):.3f}")
print(f"F1: {f1_score(y_test, y_pred):.3f}")
```

**FastAPI model serving:**
```python
from fastapi import FastAPI
import pickle

app = FastAPI()

with open('model.pkl', 'rb') as f:
    model = pickle.load(f)

@app.post("/predict")
async def predict(data: dict):
    features = [data['feature1'], data['feature2']]
    prediction = model.predict([features])[0]
    return {"prediction": float(prediction)}
```

## Resources

- **PyTorch**: pytorch.org
- **TensorFlow**: tensorflow.org
- **scikit-learn**: scikit-learn.org
- **Hugging Face**: huggingface.co
- **Papers**: arxiv.org, paperswithcode.com
- **MLOps**: mlflow.org, wandb.ai
