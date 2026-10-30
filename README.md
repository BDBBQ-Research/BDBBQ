# BDBBQ: A Human-AI Collaborative Benchmark for Bengali Social Bias in LLMs

**BDBBQ** is a bilingual benchmark designed to investigate social biases in Large Language Models (LLMs) through controlled question-answering scenarios in **Bengali and English**.

The benchmark extends the BanglaBBQ research initiative by expanding its social categories, underlying themes, and bilingual evaluation resources. Its design draws inspiration from established question-answering bias benchmarks, with an emphasis on examining culturally grounded social biases in the Bengali language context.

---

## Overview

| Attribute | Description |
|---|---|
| Dataset | BDBBQ (BanglaBBQ) |
| Research Domain | Natural Language Processing, Responsible AI |
| Task | Social Bias Evaluation in Question Answering |
| Languages | Bengali and English |
| Total Records | 61,312 |
| English Records | 30,656 |
| Bengali Records | 30,656 |
| Social Categories | 12 |
| Underlying Themes | 214 |
| Data Format | JSONL |

BDBBQ uses controlled question-answering examples to examine how language models respond to social-group-related questions under different contextual conditions.

## Key Features

- **Bilingual design:** Parallel English and Bengali datasets with structurally aligned examples.
- **Expanded social coverage:** 12 social categories representing 214 underlying themes.
- **Controlled contexts:** Ambiguous and disambiguated contexts allow examination of model responses when supporting information is absent or provided.
- **Question polarity:** Both negative and non-negative question formulations are included.
- **Counterfactual variations:** Controlled variations in social-group references and answer positions support systematic evaluation.
- **Unknown option:** Each question includes an Unknown answer option to examine whether models appropriately recognize insufficient contextual information.

## Social Categories

The benchmark covers the following 12 categories:

1. Age
2. Gender Identity
3. Physical Appearance
4. Religion
5. Regional Affiliation
6. Educational Background
7. Marital Status
8. Health Status
9. Politics
10. Ethnicity
11. Occupational Background
12. Social Status

## Dataset Composition

The dataset contains 61,312 question-answering records, equally divided between English and Bengali.

Each language contains 30,656 records across the 12 social categories.

The dataset includes two contextual conditions:

- **Ambiguous (AMB):** The context does not provide sufficient information to determine which social-group-related answer is supported.
- **Disambiguated (DIS):** The context provides additional information that supports an answer.

Each example also includes question polarity, answer-role information, and answer-position variations.

## Repository Structure

```text
BDBBQ/
│
├── README.md
├── DATASET_CARD.md
│
├── data/
│   ├── raw/
│   │   ├── english/
│   │   └── bengali/
│   │
│   ├── processed/
│   │   ├── english/
│   │   └── bengali/
│   │
│   └── sample/
│
├── analysis_scripts/
│   ├── preprocessing/
│   ├── evaluation/
│   └── statistical_analysis/
│
├── models/
│   ├── open_source/
│   └── closed_source/
│
├── results/
│   ├── english/
│   ├── bengali/
│   └── comparative_analysis/
│
└── supplemental/
    ├── annotation_guidelines/
    ├── category_definitions/
    └── additional_materials/
```

*The structure above describes the planned organization of the repository. Additional directories and resources will be added as the project develops.*

## Data Format

The dataset is distributed in JSON Lines (`.jsonl`) format.

Each record follows a structured schema containing fields such as:

| Field | Description |
|---|---|
| `example_id` | Unique identifier for the example |
| `context_condition` | Ambiguous or disambiguated context |
| `question_polarity` | Negative or non-negative question |
| `category` | Social category |
| `type` | Example subtype |
| `context` | Contextual information |
| `question` | Question presented to the model |
| `ans0`, `ans1`, `ans2` | Three answer options |
| `answer_info` | Answer-role annotations |
| `additional_metadata` | Supplementary construction metadata |
| `label` | Gold answer index |

**Important:** The `label` field represents the reference answer and must not be included in the input when evaluating a language model.

For the complete schema, construction methodology, and validation details, see [`DATASET_CARD.md`](DATASET_CARD.md).

## Dataset Validation

The benchmark underwent several validation procedures, including:

- Automated structural and consistency checks across the dataset.
- Answer-role auditing across the 61,312 records.
- Human review of a random sample of 150 records for gold-label correctness.
- Inter-annotator agreement measurement using Fleiss' Kappa.
- Statistical analyses of selected dataset properties across language and category strata.

The human annotation review achieved a Fleiss' Kappa score of **0.8349**.

These procedures provide evidence of structural consistency and sampled label reliability. They do not replace comprehensive independent linguistic and cultural validation of the Bengali dataset.

Further details and statistical methodology are documented in the dataset card.

## Research Paper

**Title:** *A Human-AI Collaborative Benchmark for Bengali Social Bias in LLMs*

**Authors:** Provat Saha, Ashika Habib Khan, Tahseen Tanha, Hasin Ishrak, and Muhammad Iqbal Hossain.

For the research methodology, dataset construction, evaluation framework, and findings, please refer to the accompanying research paper.

## Intended Use

BDBBQ is intended to support research in:

- Social bias evaluation in Large Language Models.
- Bengali-language NLP and responsible AI.
- Cross-lingual and bilingual bias analysis.
- Culturally grounded benchmark development.
- Analysis of model responses under ambiguous and disambiguated contexts.

The benchmark is a research and diagnostic resource. Its scores should not be interpreted as a comprehensive measure of a model's fairness, safety, or real-world behavior.

## Limitations

Users should consider the following limitations:

- The benchmark covers 214 underlying themes and does not represent every dimension of social bias in Bengali-speaking communities.
- Structural alignment between English and Bengali examples does not necessarily guarantee complete linguistic or pragmatic equivalence.
- Comprehensive independent Bengali linguistic, cultural, and community-based validation remains an area for further work.
- Multiple variations of the same underlying theme are related examples and should not automatically be treated as statistically independent observations.
- Aggregate bias scores may conceal important category-specific patterns.

## Citation

If you use BDBBQ in your research, please cite the accompanying paper:

```text
Saha, P., Khan, A. H., Tanha, T., Ishrak, H., & Hossain, M. I.
A Human-AI Collaborative Benchmark for Bengali Social Bias in LLMs.
```

Please update this citation with the final publication venue, year, and DOI or proceedings URL when available.

## License and Dataset Access

Dataset licensing, redistribution permissions, and public data availability are subject to confirmation.

Please refer to this repository for the latest release and access information. Do not assume that the dataset is available for unrestricted redistribution or commercial use unless an explicit license and release statement are provided.

## Acknowledgements

We acknowledge the academic guidance and contributions that supported the development of this research benchmark.

---

**BDBBQ Research Team**  
*Advancing Bengali NLP and culturally grounded evaluation of responsible AI.*
