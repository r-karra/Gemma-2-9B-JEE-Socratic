# JEE Advanced Socratic Tutor Project

> An AI-powered Socratic tutor for IIT-JEE Advanced aspirants, designed to encourage conceptual understanding through guided questioning rather than direct solutions.

---

## 🎯 Objective

This project focuses on building an AI-driven educational assistant specifically for students preparing for the **IIT-JEE Advanced** examination.

Unlike conventional AI tutors that directly provide answers, this model follows the **Socratic Method**, encouraging students to think critically by asking carefully designed questions that guide them toward the solution.

The goal is to promote:

- 🧠 Deep conceptual understanding
- 📈 Self-paced learning
- 🎓 Strong problem-solving skills
- 🔍 Reasoning instead of memorization

---

# 📚 Pedagogical Approach

The tutor acts as a **learning facilitator**, not merely an answer generator.

For every student question, the model:

1. Analyzes the student's query.
2. Identifies the underlying JEE Advanced concept.
3. Breaks the concept into smaller reasoning steps.
4. Asks guiding questions instead of revealing the answer.
5. Encourages students to derive the final result independently.

### Example Topics

- Thermodynamics
- Rotational Dynamics
- Electrostatics
- Organic Chemistry
- Coordinate Geometry
- Calculus
- Mechanics
- Chemical Equilibrium
- Physical Chemistry
- Modern Physics

---

# 🛠️ Project Structure

```
.
├── Dataset
│   ├── JEE Advanced Papers
│   ├── Chemistry Syllabus
│   ├── Physics Notes
│   └── Mathematics Problems
│
├── Data Processing
│   ├── Cleaning
│   ├── Formatting
│   └── Socratic Prompt Creation
│
├── Fine-Tuning
│   ├── Unsloth
│   ├── Hugging Face Transformers
│   └── Gemma-2-9B
│
├── Model
│   └── Gemma-2-9B-JEE-Socratic-Final
│
└── Inference
    └── Hugging Face Transformers
```

---

# 📂 Dataset

The dataset consists of carefully curated **IIT-JEE Advanced** educational resources.

## Question Papers

- JEE Advanced 2013
- JEE Advanced 2015
- JEE Advanced 2017
- JEE Advanced 2018
- JEE Advanced 2019
- JEE Advanced 2022

Subjects include:

- Physics
- Chemistry
- Mathematics

---

## Support Material

Additional curated educational resources include:

- Rotational Motion
- Gaseous State
- Liquid State
- Official Chemistry Syllabus (2026)

These resources help improve conceptual understanding and expand the tutoring capability beyond previous examination questions.

---

# 🚀 Fine-Tuning Pipeline

## 1. Environment Setup

The project uses **Unsloth** for efficient GPU-based fine-tuning.

Supported environments:

- Kaggle
- Google Colab

Benefits:

- Faster training
- Lower GPU memory usage
- Easy Hugging Face integration

---

## 2. Dataset Preparation

The original JEE Advanced questions are transformed into conversational teaching examples.

Each example teaches the model to:

- Understand the student's intent
- Avoid giving direct answers
- Generate Socratic prompts
- Encourage reasoning

---

## 3. Instruction Tuning

The Gemma model is instruction-tuned to recognize pedagogical patterns.

Instead of:

> "Here is the answer."

The model responds like:

> "Which thermodynamic quantity determines spontaneity?"

> "What happens to ΔG when entropy increases?"

This encourages active learning.

---

## 4. Deployment

After fine-tuning, the model is published on Hugging Face.

**Model Repository**

```
r-karra/Gemma-2-9B-JEE-Socratic-Final
```

---

# 💻 Inference Guide

You **do not** need to convert the model to GGUF format.

The model can be loaded directly using the Hugging Face `transformers` library.

## Installation

```bash
pip install transformers accelerate torch
```

---

## Python Example

```python
from transformers import AutoModelForCausalLM, AutoTokenizer
import torch
from google.colab import userdata

# Configuration
model_id = "r-karra/Gemma-2-9B-JEE-Socratic-Final"
token = userdata.get("HF_TOKEN")

device = "cuda" if torch.cuda.is_available() else "cpu"

# Load tokenizer
print("Loading tokenizer...")
tokenizer = AutoTokenizer.from_pretrained(
    "google/gemma-2-9b-it",
    token=token
)

# Load model
print("Loading model...")
model = AutoModelForCausalLM.from_pretrained(
    model_id,
    token=token,
    device_map="auto",
    torch_dtype=torch.float16
).to(device)

def ask_tutor(question):
    prompt = (
        "System: You are an expert IIT-JEE Advanced tutor "
        "using the Socratic method.\n"
        f"User: {question}\n"
        "Model:"
    )

    inputs = tokenizer(prompt, return_tensors="pt").to(device)

    with torch.no_grad():
        outputs = model.generate(
            **inputs,
            max_new_tokens=500
        )

    response = tokenizer.decode(
        outputs[0][inputs["input_ids"].shape[1]:],
        skip_special_tokens=True
    )

    return response

print(
    ask_tutor(
        "How can I determine if a reaction is spontaneous using Gibbs free energy?"
    )
)
```

---

# 🧪 Example Interaction

### Student

> How can I determine whether a reaction is spontaneous using Gibbs free energy?

### Tutor

> Before thinking about spontaneity, can you recall the mathematical expression relating Gibbs free energy to enthalpy and entropy?

> What does a negative value of ΔG imply about the direction of a process?

> How would increasing temperature affect the entropy contribution?

---

# 🎓 Educational Philosophy

Rather than replacing teachers, this project aims to become an intelligent learning companion that:

- Encourages curiosity
- Promotes conceptual reasoning
- Builds confidence
- Helps students think like problem solvers

The emphasis is on **learning the process**, not memorizing the answer.

---

# 🧰 Technology Stack

- Python
- PyTorch
- Hugging Face Transformers
- Hugging Face Hub
- Unsloth
- Google Colab
- Kaggle
- Gemma-2-9B

---

# 📦 Model

**Model Name**

```
Gemma-2-9B-JEE-Socratic-Final
```

**Hugging Face Repository**

```
r-karra/Gemma-2-9B-JEE-Socratic-Final
```

---

# 📄 License

All IIT-JEE Advanced question papers remain the intellectual property of their respective examination authorities.

This repository is intended solely for:

- Educational purposes
- Research
- AI-assisted learning

No copyright over the original examination content is claimed.

---

# 🙏 Acknowledgements

Special thanks to:

- Google DeepMind
- Hugging Face
- Unsloth
- Kaggle
- Google Colab
- The Full-Stack GenAI Bootcamp

for providing the tools and ecosystem that made this project possible.

---

# ⭐ Citation

If you find this project useful, please consider starring the repository and citing it in your educational or research work.

```
@misc{jee-socratic-tutor,
  author = {Rajesh Kumar Karra},
  title = {Gemma-2-9B JEE Advanced Socratic Tutor},
  year = {2026},
  publisher = {Hugging Face},
  model = {r-karra/Gemma-2-9B-JEE-Socratic-Final}
}
```
