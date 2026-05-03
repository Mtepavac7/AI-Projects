# Build a simple LLM chatbot in Python

This walkthrough sets up a minimal **Python** chatbot using [Hugging Face Transformers](https://huggingface.co/docs/transformers) and Microsoft’s small instruct model **[Phi-3-mini-4k-instruct](https://huggingface.co/microsoft/Phi-3-mini-4k-instruct)** so you can run local text generation from a prompt.

If you clone this repository, open a terminal in the **`llm-chatbot`** folder and continue from the dependency step (or follow from the beginning in a fresh directory).

---

## 1. System packages (Debian / Ubuntu)

Update package lists and install **pip**:

```bash
sudo apt update
sudo apt install python3-pip -y
```

Optional (recommended for isolated installs):

```bash
sudo apt install python3-venv -y
```

---

## 2. Project directory

Create a project directory. 

```bash
mkdir llm-chatbot
cd llm-chatbot
```

---

## 3. Required libraries

To run the chatbot you need at least:

| Package | Purpose |
|--------|---------|
| **transformers** | Loads and runs state-of-the-art NLP / LLM models. |
| **torch** | Deep learning runtime used by the model and Transformers. |
| **accelerate** | Helps with device placement and efficient loading (e.g. `device_map="auto"`). |
| **einops** | Flexible tensor shape operations used by parts of the stack. |
| **jinja2** | Templating used by tooling and some model stacks. |

---

## 4. Create `requirements.txt`

Create a pinned dependency file (exact versions for reproducible installs):

```bash
cat > requirements.txt << 'EOF'
transformers==4.48.3
torch==2.6.0
accelerate==1.8.1
einops==0.8.1
jinja2==3.1.6
EOF
```

*(This repo already includes the same `requirements.txt` if you prefer not to use `cat`.)*

### PyTorch

`torch==2.6.0` installs the default wheel for your platform from PyPI (often CPU). For a specific **CUDA** build, see [PyTorch — Get Started](https://pytorch.org/get-started/locally/) and adjust `requirements.txt` or install `torch` separately before the rest.

---

## 5. Install dependencies

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt
```

---

## 6. Choose and load the model

We use Microsoft’s compact instruct model:

**[microsoft/Phi-3-mini-4k-instruct](https://huggingface.co/microsoft/Phi-3-mini-4k-instruct)**

From **transformers** we use:

- **`AutoModelForCausalLM`** — loads the causal language model weights.
- **`AutoTokenizer`** — loads the matching tokenizer.

---

## 7. Create `llm-chatbot.py`

You can build the file step by step with shell heredocs (as below), or copy the ready-made **`llm-chatbot.py`** from this repository.

### Imports

```bash
cat > llm-chatbot.py << 'EOF'
from transformers import AutoModelForCausalLM, AutoTokenizer
EOF
```

### Load the model with `from_pretrained`

```bash
cat >> llm-chatbot.py << 'EOF'
revision_id = "0a67737cc96d2554230f90338b163bc6380a2a85"
model = AutoModelForCausalLM.from_pretrained(
    "microsoft/Phi-3-mini-4k-instruct",
    revision=revision_id,
    device_map="auto",
    torch_dtype="auto",
    trust_remote_code=True,
    )
EOF
```

### Load the tokenizer

`AutoTokenizer.from_pretrained` loads the tokenizer for the same model (and revision).

```bash
cat >> llm-chatbot.py << 'EOF'
tokenizer = AutoTokenizer.from_pretrained("microsoft/Phi-3-mini-4k-instruct", revision=revision_id)
EOF
```

### Text-generation pipeline

```bash
cat >> llm-chatbot.py << 'EOF'
from transformers import pipeline

generator = pipeline(
    "text-generation",
    model=model,
    tokenizer=tokenizer,
    return_full_text=False,
    max_new_tokens=500,
    do_sample=False
    )
EOF
```

### Prompt the user and read input

```bash
cat >> llm-chatbot.py << 'EOF'
print("What do you want?")
EOF
```

```bash
cat >> llm-chatbot.py << 'EOF'
user_input = input()
EOF
```

### Run generation and print the reply

```bash
cat >> llm-chatbot.py << 'EOF'
messages = [{"role":"user", "content": user_input}]
response = generator(messages)
EOF
```

```bash
cat >> llm-chatbot.py << 'EOF'
print(response[0]["generated_text"])
EOF
```

That’s it: a minimal program that asks for text, sends it to the model as a user message, and prints the generated continuation.

---

## 8. Run the chatbot

First download can take a while (model weights from Hugging Face). Ensure you have disk space and, if the Hub requires it, [log in with the Hugging Face CLI](https://huggingface.co/docs/huggingface_hub/quick-start) (`huggingface-cli login`).

```bash
python3 llm-chatbot.py
```

---

## License

Add your preferred license if you publish this project on GitHub.
