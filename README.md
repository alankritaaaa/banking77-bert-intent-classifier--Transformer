# 🏦 Banking Support AI

An end-to-end banking support AI prototype combining **BERT intent classification** with a **QLoRA fine-tuned TinyLlama** response generator.

The system first identifies what a customer is asking about and then generates a concise support response.

## 🚀 Architecture

Customer Query
      ↓
BERT Intent Classifier
      ↓
Intent + Confidence
      ↓
Intent Router
      ↓
TinyLlama 1.1B + QLoRA
      ↓
Banking Support Response

### Model responsibilities

**BERT**
- Fine-tuned on the Banking77 dataset
- Classifies queries into all **77 Banking77 intents**
- Test Accuracy: **87.86%**
- Test Weighted F1: **86.82%**

**TinyLlama + QLoRA**
- TinyLlama 1.1B Chat used as the base model
- Fine-tuned using LoRA adapters with 4-bit NF4 quantization
- Currently supports response generation for **15 selected intents**

## 🧠 Why two models?

The models perform different tasks.

BERT answers:

> "What is the customer asking about?"

TinyLlama answers:

> "How should the system respond?"

This separation makes the pipeline modular and allows the classification and generation components to be developed independently.

## 📊 Dataset

The project uses the **Banking77** dataset.

BERT uses all 77 intent categories.

For the QLoRA response generator, 15 intents were selected for the initial prototype because Banking77 provides intent-labeled queries rather than official support responses. Human-authored demonstration responses were therefore created for the selected intents.

Selected response-generation intents include:

- Card arrival
- Card not working
- Declined card payment
- Lost or stolen card
- Change PIN
- Card activation
- Pending transfer
- Failed transfer
- Declined transfer
- Refund not showing up
- Unrecognized cash withdrawal
- Exchange rate
- Top-up by card charge
- Wrong cash amount received
- Identity verification

## ⚙️ QLoRA Configuration

- Base model: TinyLlama 1.1B Chat
- Quantization: 4-bit NF4
- LoRA rank: 8
- LoRA alpha: 16
- LoRA dropout: 0.05
- Target modules: `q_proj`, `v_proj`
- Training epochs: 1
- Learning rate: `2e-4`
- Maximum sequence length: 256
- Hardware: NVIDIA T4 GPU

The QLoRA adapter is trained separately from the base TinyLlama model.

## 📈 Results

### BERT

| Metric | Result |
|---|---:|
| Intent classes | 77 |
| Test Accuracy | 87.86% |
| Test Weighted F1 | 86.82% |

### QLoRA

The trained response generator successfully produced relevant responses for the selected banking-support intents during end-to-end testing.

Example:

**Query**

> My card payment was declined

**Predicted intent**

`declined_card_payment`

**Confidence**

80.3%

**Generated response**

> I'm sorry your card payment was declined. Please check that your card is active and that you have sufficient available funds. If the issue continues, please contact customer support.

## 🛠️ Tech Stack

- Python
- PyTorch
- Hugging Face Transformers
- Hugging Face PEFT
- BERT
- TinyLlama
- LoRA / QLoRA
- bitsandbytes
- scikit-learn
- Pandas
- Google Colab
- NVIDIA T4 GPU

## 📁 Repository Structure

```text
banking-support-ai/
│
├── README.md
├── requirements.txt
├── .gitignore
└── Banking_Support_AI_Final.ipynb
