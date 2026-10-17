# NSE Stock Market LoRA Fine-Tune — An Honest Report

Fine-tuned Qwen2.5-0.5B-Instruct on NSE stock data using Unsloth LoRA.
Honest result: the model learned the answer *format*, not the *facts*.

## What I built
- Dataset: 200 Q&A pairs from NSE daily stock data (2016–2017, Kaggle)
- Three question types: closing price, high/low, traded quantity
- Training: Unsloth LoRA on Qwen2.5-0.5B-Instruct, 3 epochs, 60 steps
- Loss: 1.64 → 0.42 (8.8M trainable params, 1.75% of the model)
- Exported to GGUF (q8_0), served locally via Ollama as `nse-tuned`

## Honest result
Asked "What was 3MINDIA's close on 2017-06-28?", the model answered confidently —
and got the number completely wrong. Format perfect, facts invented.

## Why: the Qwen 0.5B point
**500M parameters isn't enough for factual memorization.** With 200 examples, a 0.5B
model learns the *style* of answers ("X closed at Y on Z") but cannot retain exact
numbers. Memorization needs bigger models and far more data. The tuning worked —
the model is just too small for the job.

## Takeaway
LoRA fine-tuning teaches behavior, not knowledge — especially at small scale.
The pipeline (data prep → training → GGUF export → Ollama serving) works end to end.
The lesson is knowing what fine-tuning can and cannot do.

**Bottom line: the tuning worked, the 0.5B model is too small to memorize facts.**

## Stack
Python, Unsloth, Qwen2.5-0.5B-Instruct, Ollama, Colab (T4)
