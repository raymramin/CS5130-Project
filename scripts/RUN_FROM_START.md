# Run LLMollama from the beginning (Command Prompt / PowerShell)

Follow these steps in order. Run everything from the **project root** (`CS5130-Project`).

---

## Step 1: Open terminal at project root

```cmd
cd c:\Users\Ray\Desktop\CS5130\CS5130-Project\CS5130-Project
```

---

## Step 2: (Optional) Test without a real image or model — dry run

Uses fake probabilities and mock LLM output. No Ollama, no trained model, no image file.

```cmd
python scripts/run_llmollama_example.py --dry-run
```

You should see:
- `example_xray_input.json` created
- `example_explanation.json` created
- `example_personalized_report.txt` created

---

## Step 3: (Optional) Test with Ollama but no image — example data

Uses fake probabilities and **real** Ollama for the report. No image file, no trained model.

1. Start Ollama (pull and run a model once):
   ```cmd
   ollama run llama3.1:8b
   ```
   (Type a message to confirm it works, then you can close that chat or leave it open.)

2. In a **new** Command Prompt, from project root:
   ```cmd
   cd c:\Users\Ray\Desktop\CS5130\CS5130-Project\CS5130-Project
   python scripts/run_llmollama_example.py
   ```
   This creates the example JSON and calls Ollama to generate the explanation and report.

---

## Step 4: Run on your own x-ray image (with a trained model)

You need:
- **An x-ray image file** (e.g. a `.png` or `.jpg`).
- **A trained model** from this repo — either:
  - **Ray models**: trained with `models/ray.py` → weights in `models/resnet34_chexpert.pt` or `models/effb4_chexpert.pt`, or
  - **SK models**: weights in `sk/tuned_models/best_<model>_model.pth` (e.g. `best_resnet_model.pth`).

**If you have Ray weights (e.g. `models/resnet34_chexpert.pt`):**

```cmd
cd c:\Users\Ray\Desktop\CS5130\CS5130-Project\CS5130-Project
python scripts/connect_models_to_llmollama.py --image "C:\path\to\your\xray.png" --backend ray --model resnet34 --output report.txt --explanation-out explanation.json
```

Replace `C:\path\to\your\xray.png` with your actual image path. Use quotes if the path has spaces.

**If you have SK weights (e.g. `sk/tuned_models/best_resnet_model.pth`):**

```cmd
python scripts/connect_models_to_llmollama.py --image "C:\path\to\your\xray.png" --backend sk --model resnet --output report.txt --explanation-out explanation.json
```

**If you don’t have any trained weights yet:**

- You can still test the **LLM-only** part with the example JSON (Step 3).
- To train Ray models first:
  ```cmd
  python models/ray.py --mode train --epochs 2
  ```
  (Requires `data/processed_chexpert/` with `train_clean.csv`, `val_clean.csv`, `test_clean.csv`.)

---

## Step 5: Where the output goes

- **Report (prose):** `report.txt` (or whatever you passed to `--output`).
- **Structured explanation (JSON):** `explanation.json` (if you used `--explanation-out`).
- The script also prints the report to the console.

---

## Quick reference

| Goal                         | Command |
|-----------------------------|---------|
| Dry run (no Ollama, no image) | `python scripts/run_llmollama_example.py --dry-run` |
| Example + real Ollama       | `python scripts/run_llmollama_example.py` |
| Your image + Ray model      | `python scripts/connect_models_to_llmollama.py --image "path\to\xray.png" --backend ray --model resnet34 --output report.txt` |
| Your image + SK model      | `python scripts/connect_models_to_llmollama.py --image "path\to\xray.png" --backend sk --model resnet --output report.txt` |

Run everything from: `c:\Users\Ray\Desktop\CS5130\CS5130-Project\CS5130-Project`.
