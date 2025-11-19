---
name: ai-machine-learning
description: PyTorch, TensorFlow, scikit-learn, LLMs, data pipelines, model deployment, MLOps workflows.
---

# AI & Machine Learning Skills

## PyTorch Fundamentals

```python
import torch
import torch.nn as nn
from torch.utils.data import DataLoader, Dataset

# Custom Dataset
class CustomDataset(Dataset):
    def __init__(self, X, y):
        self.X = torch.FloatTensor(X)
        self.y = torch.LongTensor(y)
    
    def __len__(self):
        return len(self.X)
    
    def __getitem__(self, idx):
        return self.X[idx], self.y[idx]

# Neural Network
class NeuralNet(nn.Module):
    def __init__(self, input_size, hidden_size, num_classes):
        super(NeuralNet, self).__init__()
        self.fc1 = nn.Linear(input_size, hidden_size)
        self.relu = nn.ReLU()
        self.dropout = nn.Dropout(0.2)
        self.fc2 = nn.Linear(hidden_size, num_classes)
    
    def forward(self, x):
        x = self.fc1(x)
        x = self.relu(x)
        x = self.dropout(x)
        x = self.fc2(x)
        return x

# Training loop
def train(model, train_loader, criterion, optimizer, device):
    model.train()
    total_loss = 0
    
    for images, labels in train_loader:
        images, labels = images.to(device), labels.to(device)
        
        # Forward pass
        outputs = model(images)
        loss = criterion(outputs, labels)
        
        # Backward pass
        optimizer.zero_grad()
        loss.backward()
        torch.nn.utils.clip_grad_norm_(model.parameters(), max_norm=1.0)
        optimizer.step()
        
        total_loss += loss.item()
    
    return total_loss / len(train_loader)
```

## LLM Fine-tuning with LoRA

```python
from peft import LoraConfig, get_peft_model
from transformers import AutoModelForCausalLM, AutoTokenizer

# Load base model
model = AutoModelForCausalLM.from_pretrained("meta-llama/Llama-2-7b")
tokenizer = AutoTokenizer.from_pretrained("meta-llama/Llama-2-7b")

# LoRA Configuration
lora_config = LoraConfig(
    r=16,                          # LoRA rank
    lora_alpha=32,
    target_modules=["q_proj", "v_proj"],
    lora_dropout=0.05,
    bias="none",
    task_type="CAUSAL_LM"
)

# Apply LoRA
model = get_peft_model(model, lora_config)

# Fine-tune with much fewer parameters
# Total params: 7B -> Trainable: ~1M
```

## Data Processing with Pandas

```python
import pandas as pd
import numpy as np

# Load and explore
df = pd.read_csv('data.csv')
print(df.describe())
print(df.info())

# Data cleaning
df.dropna(inplace=True)
df['age'] = df['age'].fillna(df['age'].median())
df['category'] = df['category'].astype('category')

# Feature engineering
df['age_group'] = pd.cut(df['age'], bins=[0, 18, 35, 50, 100])
df['is_premium'] = (df['plan'] == 'premium').astype(int)

# Normalization
from sklearn.preprocessing import StandardScaler
scaler = StandardScaler()
df[['age', 'income']] = scaler.fit_transform(df[['age', 'income']])
```

## Model Evaluation

```python
from sklearn.metrics import (
    confusion_matrix, classification_report,
    roc_auc_score, f1_score, accuracy_score
)

y_pred = model.predict(X_test)
y_proba = model.predict_proba(X_test)[:, 1]

# Classification metrics
print(f"Accuracy: {accuracy_score(y_test, y_pred):.3f}")
print(f"F1 Score: {f1_score(y_test, y_pred):.3f}")
print(f"ROC AUC: {roc_auc_score(y_test, y_proba):.3f}")
print("\nClassification Report:")
print(classification_report(y_test, y_pred))

# Confusion matrix
cm = confusion_matrix(y_test, y_pred)
```

## Model Serving with FastAPI

```python
from fastapi import FastAPI
from pydantic import BaseModel
import pickle

app = FastAPI()
model = pickle.load(open('model.pkl', 'rb'))

class PredictionInput(BaseModel):
    features: list

@app.post("/predict")
async def predict(data: PredictionInput):
    prediction = model.predict([data.features])[0]
    probability = model.predict_proba([data.features])[0]
    
    return {
        "prediction": int(prediction),
        "probabilities": probability.tolist()
    }

@app.get("/health")
async def health():
    return {"status": "ok"}
```

