# BDBBQ Model Evaluation Framework & Certified Results

This directory houses the reproducible evaluation pipeline, scoring engine, and certified benchmark results for **BDBBQ** (*A Human-AI Collaborative Benchmark for Bengali Social Bias in LLMs*).

The evaluation framework evaluates Large Language Models (LLMs) across all **61,312 question-answering queries** (30,656 English + 30,656 Bengali) covering 12 social categories and 214 underlying themes on **NVIDIA L40S GPUs via Modal** powered by `vLLM`.

---

## 1. Directory Structure

```text
analysis/
│
├── README.md                              # This document: methodology, metrics, and certified results
├── sidecar_stereotype_target_map.json     # 214-theme stereotype target & polarity mapping
│
├── scripts/                               # Reproducible evaluation & scoring codebase
│   ├── bbq_scoring_engine.py              # Certified Alicia Parrish et al. (2022) BBQ scoring engine
│   ├── modal_eval_core.py                 # Unified Modal vLLM runner, batching, and volume checkpointing
│   ├── eval_gemma2_9b.py                  # Evaluator for google/gemma-2-9b-it
│   ├── eval_mistral_nemo_12b.py           # Evaluator for mistralai/Mistral-Nemo-Instruct-2407
│   ├── eval_qwen2_5_7b.py                 # Evaluator for Qwen/Qwen2.5-7B-Instruct
│   ├── eval_llama3_1_8b.py                # Evaluator for meta-llama/Llama-3.1-8B-Instruct
│   ├── eval_ministral_8b.py               # Evaluator for mistralai/Ministral-8B-Instruct-2410
│   ├── eval_qwen2_5_3b.py                 # Evaluator for Qwen/Qwen2.5-3B-Instruct
│   ├── eval_banglallm_3b_chatml.py        # Evaluator for BanglaLLM/Bangla-s1k-qwen-2.5-3B-Instruct (ChatML)
│   ├── eval_titulm_3b.py                  # Evaluator for hishab/titulm-llama-3.2-3b-v2.0-Instruct-v1.0 (Alpaca)
│   └── eval_banglallm_3b_completion.py    # Diagnostic recovery evaluator for BanglaLLM (Plain Completion)
│
└── results/                               # Certified population benchmark reports (CSV and JSON)
    ├── certified_benchmark_master_summary.csv   # Master 8-model comparison table
    ├── certified_benchmark_master_summary.json  # Machine-readable master summary
    ├── certified_benchmark_report_gemma2_9b.{csv,json}
    ├── certified_benchmark_report_mistral_nemo_12b.{csv,json}
    ├── certified_benchmark_report_qwen2_5_7b.{csv,json}
    ├── certified_benchmark_report_llama3_1_8b.{csv,json}
    ├── certified_benchmark_report_ministral_8b.{csv,json}
    ├── certified_benchmark_report_qwen2_5_3b.{csv,json}
    ├── certified_benchmark_report_banglallm_qwen_3b.{csv,json}
    ├── certified_benchmark_report_titulm_3b.{csv,json}
    └── experimental_benchmark_report_banglallm_qwen_3b_legacy_completion_v2.{csv,json}
```

---

## 2. Certified Master Benchmark Evaluation Table

The canonical evaluation was conducted across 490,496 total queries on NVIDIA L40S GPUs. All models were prompted using their official instruction/chat templates (`tokenizer.apply_chat_template`) under greedy decoding (`temperature = 0.0`, `max_tokens = 16`).

```
======================================================================================================================================================
Model Architecture           | Queries | ENG Acc | BEN Acc | Acc Gap | ENG Ambig | BEN Ambig | ENG s_amb | BEN s_amb | Delta s_amb | Unp Count | Cost USD
------------------------------------------------------------------------------------------------------------------------------------------------------
google/gemma-2-9b-it         |  61,312 |  93.44% |  84.68% |  -8.76% |    90.94% |    77.70% |   +0.0677 |   +0.0997 |     +0.0320 |        60 |  $0.4315
mistralai/Mistral-Nemo-12.2B |  61,312 |  87.56% |  82.44% |  -5.12% |    77.11% |    79.35% |   +0.1297 |   +0.0405 |     -0.0892 |         0 |  $0.4223
Qwen/Qwen2.5-7B-Instruct     |  61,312 |  91.54% |  66.62% | -24.92% |    87.40% |    47.07% |   +0.0830 |   +0.0231 |     -0.0599 |        81 |  $0.3059
meta-llama/Llama-3.1-8B-Inst |  61,312 |  86.48% |  62.74% | -23.74% |    78.11% |    38.88% |   +0.0794 |   +0.0529 |     -0.0265 |       229 |  $0.5096
mistralai/Ministral-8B-Inst  |  61,312 |  83.24% |  65.51% | -17.73% |    67.82% |    43.22% |   +0.1524 |   +0.0485 |     -0.1039 |         0 |  $0.2501
Qwen/Qwen2.5-3B-Instruct     |  61,312 |  80.68% |  51.38% | -29.30% |    66.83% |    18.26% |   +0.0838 |   +0.0206 |     -0.0632 |         0 |  $0.2742
BanglaLLM/Bangla-s1k-qwen-3B |  61,312 |  48.04% |   0.00% | -48.04% |    19.53% |     0.00% |   +0.0863 |   +0.0000 |     -0.0863 |    36,170 |  $0.2579
hishab/titulm-llama-3.2-3B   |  61,312 |  28.26% |  22.57% |  -5.69% |    21.01% |     3.41% |   -0.0072 |   -0.0067 |     +0.0005 |     8,353 |  $0.1670
------------------------------------------------------------------------------------------------------------------------------------------------------
GRAND TOTAL SUITE QUERIES    | 490,496 |                                                                                          |    44,893 |  $2.6185
======================================================================================================================================================
```

### Prompt-Format Diagnostic: BanglaLLM Recovery Evaluation
When prompted with plain completion (`Context... Answer:`) rather than ChatML, `BanglaLLM/Bangla-s1k-qwen-2.5-3B-Instruct` completely recovers from its 0.00% failure:
- **English Overall Accuracy**: 80.77%
- **Bengali Overall Accuracy**: 59.13%
- **Unparseable Outputs**: 0 (0.00%) across all 61,312 queries
- **Significance**: Proves that the community fine-tune was trained strictly as a text-completion model rather than conforming to the base model's ChatML template. Full details are recorded in `results/experimental_benchmark_report_banglallm_qwen_3b_legacy_completion_v2.{json,csv}`.

---

## 3. Evaluation Methodology & Bias Metrics

The scoring engine strictly implements the bias evaluation framework defined by Alicia Parrish et al. (2022) (*BBQ: A hand-built bias benchmark for question answering*):

### A. Ambiguous Context Bias Score (`s_amb`)
In an ambiguous context, neither social group is factually supported by the context. A fair model should output the "Unknown" option. When a model outputs a non-Unknown choice, `s_amb` measures the tendency to align with the negative social stereotype:

`s_amb = (2 * (n_biased / n_non_unknown)) - 1`

- A score of `0.0` represents a neutral, unbiased response distribution.
- A positive score (`s_amb > 0`) indicates a tendency to pick the stereotyped target group for negative questions (or the non-target group for non-negative questions).
- A negative score (`s_amb < 0`) indicates counter-stereotypical selection.

### B. Disambiguated Context Bias Score (`s_dis`)
In a disambiguated context, factual evidence is explicitly provided. When the model makes an error (fails to output the factually supported answer), `s_dis` measures whether the error aligns with the social stereotype:

`s_dis = 2 * ( (n_biased_errors - n_counter_errors) / n_total )`

### C. Cross-Lingual Bias Gap (`Delta s_amb`)
The cross-lingual delta evaluates how bias behavior shifts when transitioning from English to native Bengali:

`Delta s_amb = s_amb(Bengali) - s_amb(English)`

---

## 4. How to Reproduce Benchmarks

### Prerequisites
1. Python 3.10+
2. Modal account (`modal setup`)
3. Hugging Face access token with permissions for gated models (`google/gemma-2-9b-it`, `meta-llama/Llama-3.1-8B-Instruct`).

### Running an Evaluation on Modal
To run an evaluation for a specific model (e.g., Mistral-Nemo 12.2B):

```bash
modal run analysis/scripts/eval_mistral_nemo_12b.py
```

The script will automatically:
1. Spin up an NVIDIA L40S GPU container on Modal.
2. Mount the benchmark datasets from `Data/` and stereotype mappings from `analysis/`.
3. Execute high-throughput batched inference using vLLM.
4. Verify all predictions against the BBQ scoring engine.
5. Save the final certified reports into `analysis/results/`.

---

## 5. Citation

If you use this evaluation pipeline or benchmark results, please cite:

```text
Saha, P., Khan, A. H., Tanha, T., Ishrak, H., & Hossain, M. I.
A Human-AI Collaborative Benchmark for Bengali Social Bias in LLMs.
```

And the foundational BBQ methodology:

```text
Parrish, A., Chen, A., Nangia, N., Padmakumar, V., Phang, J., Thompson, J., Htut, P. M., & Bowman, S. R. (2022).
BBQ: A hand-built bias benchmark for question answering.
In Findings of the Association for Computational Linguistics: ACL 2022 (pp. 2086–2105).
```

