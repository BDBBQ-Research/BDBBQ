
## 1. Dataset Overview

| Attribute | Description |
|---|---|
| Dataset Name | BDBBQ |
| Full Title | A Human-AI Collaborative Benchmark for Bengali Social Bias in LLMs |
| Task | Social Bias Evaluation in Question Answering |
| Languages | Bengali and English |
| Total Records | 61,312 |
| English Records | 30,656 |
| Bengali Records | 30,656 |
| Social Categories | 12 |
| Underlying Themes | 214 |
| Data Format | JSONL (JSON Lines) |
| Primary Domain | Natural Language Processing and Responsible AI |

BDBBQ is a bilingual question-answering benchmark developed to investigate social biases in Large Language Models (LLMs), with particular emphasis on the socio-cultural context of Bangladesh.

The benchmark extends the original BanglaBBQ undergraduate research and introduces a larger, more systematically aligned collection of English and Bengali question-answering instances. It adopts a context-sensitive evaluation framework that distinguishes between situations where an answer is explicitly supported by the available context and situations where the information is insufficient.

BDBBQ aims to facilitate culturally grounded, cross-lingual investigations of social bias and model behavior in Bengali and English.

## 2. Research Motivation

Although social bias in Large Language Models has received increasing research attention, existing benchmarks provide limited coverage of culturally grounded evaluation in Bengali, particularly within Bangladesh's socio-cultural context.

Directly translating an existing benchmark does not necessarily preserve the social meaning, contextual assumptions, or cultural relevance of its examples.

BDBBQ addresses this limitation through a culturally situated bilingual benchmark that supports systematic comparisons between English and Bengali model responses.

The benchmark builds upon the BBQ question-answering framework and incorporates elements of culturally adapted bias evaluation, extending previous BanglaBBQ research from 6,607 records across nine categories to 61,312 records across twelve categories.

Its primary research objectives are:

- Investigating stereotype-aligned model behavior.
- Evaluating how models handle ambiguous and disambiguated contexts.
- Examining differences in model behavior between Bengali and English.
- Studying the influence of social categories and question framing on model responses.
- Supporting research into culturally grounded and trustworthy language technologies.

## 3. Dataset Composition

BDBBQ contains 61,312 question-answering records, equally distributed across English and Bengali.

| Language | Number of Records | Percentage |
|---|---:|---:|
| English | 30,656 | 50% |
| Bengali | 30,656 | 50% |
| **Total** | **61,312** | **100%** |

The dataset is organized into 24 JSONL files, comprising one English and one Bengali file for each of the twelve social categories.

The collection covers 214 underlying themes. The reported record counts represent question-answering instances rather than independent social situations.

### 3.1 Social Categories

The benchmark covers twelve social categories:

| Social Category | Themes | Records per Language |
|---|---:|---:|
| Age | 20 | 2,920 |
| Gender Identity | 34 | 4,696 |
| Physical Appearance | 10 | 1,440 |
| Religion | 20 | 2,880 |
| Regional Affiliation | 10 | 1,440 |
| Educational Background | 20 | 2,880 |
| Marital Status | 20 | 2,880 |
| Health Status | 20 | 2,880 |
| Politics | 20 | 2,880 |
| Ethnicity | 10 | 1,440 |
| Occupational Background | 20 | 2,880 |
| Social Status | 10 | 1,440 |
| **Total** | **214** | **30,656** |

Health Status includes both physical and mental-health settings.

The three additional categories introduced in the expanded benchmark are Ethnicity, Occupational Background, and Social Status. Health Status, Marital Status, and Occupational Background were also expanded to twenty themes each.

## 4. Benchmark Design

BDBBQ follows a context-sensitive question-answering design that evaluates whether models distinguish between insufficient evidence and explicitly supported information.

Each record contains a short contextual passage, a question, two social-group answer options, and an Unknown option.

### 4.1 Ambiguous Context (AMB)

An ambiguous context does not provide sufficient information to determine which social-group answer is correct.

The expected answer is **Unknown**.

Selecting either social-group option without sufficient contextual evidence constitutes an unsupported choice and may reflect stereotype-aligned behavior.

### 4.2 Disambiguated Context (DIS)

A disambiguated context introduces additional information that identifies which answer is supported by the passage.

The expected answer is the social-group option supported by the contextual evidence.

This condition evaluates whether models can identify and use explicit contextual information when it is available.

### 4.3 Question Polarity

Questions are presented in both negative and non-negative forms.

This design allows the benchmark to examine model responses against the direction of the stereotype rather than relying on a single question framing.

### 4.4 Counterfactual Group Orientations and Answer Permutations

The benchmark incorporates controlled variations in social-group orientations and answer-option positions.

For the standard construction pattern, each scenario combines:

- Two counterfactual group orientations.
- Two context conditions.
- Two question polarities.
- Six permutations of the three answer options.

Three wording variants produce 144 records per theme under the standard construction pattern.

Age and Gender Identity retain category-specific designs and are not forced into this standard 144-record construction pattern.

The gold label follows the correct answer role across answer permutations, maintaining the reference answer regardless of its displayed position.

## 5. Dataset Structure and Schema

BDBBQ is distributed in JSON Lines (JSONL) format.

Each record follows a structured 15-field schema that preserves example identifiers, question indices, reference labels, context conditions, question polarity, answer choices, answer-role annotations, and construction metadata.

The principal fields include:

| Field | Description |
|---|---|
| `example_id` | Identifier associated with a benchmark example |
| `context_condition` | Indicates ambiguous or disambiguated context |
| `question_polarity` | Indicates negative or non-negative question framing |
| `category` | Social bias category |
| `type` | Question subtype, including DT, TM, and NA |
| `context` | Contextual passage presented to the model |
| `question` | Question associated with the context |
| `ans0` | First answer option |
| `ans1` | Second answer option |
| `ans2` | Third answer option |
| `answer_info` | Answer-related role information |
| `additional_metadata` | Supplementary construction and annotation metadata |
| `label` | Reference answer index |

The English and Bengali versions preserve corresponding identifiers, structural fields, and reference labels. Their contextual passages, questions, and answer choices are expressed in their respective languages.

**Evaluation precaution:** The reference `label` must not be provided to the evaluated model during inference. It is reserved for calculating evaluation metrics and comparing model predictions against the benchmark's gold answers.

## 6. Bilingual Alignment and Cultural Grounding

The English and Bengali collections are aligned through corresponding identifiers, context conditions, question polarity, answer roles, and reference-label positions.

The benchmark uses Bangladesh-specific settings and wording to represent relevant socio-cultural themes.

Theme selection is informed by Bangladeshi academic, legal, and journalistic accounts. These sources support the identification of social issues relevant to the benchmark; they do not establish that the fictional events described in individual question-answering scenarios actually occurred.

Automated agreement across corresponding metadata fields establishes structural alignment, but does not necessarily establish equivalent pragmatic meaning between English and Bengali.

Comprehensive independent assessment of Bengali fluency, cultural framing, and source-to-theme relevance remains necessary.

## 7. Dataset Validation

The benchmark underwent automated structural validation, answer-role auditing, statistical consistency testing, and human assessment of sampled gold-answer correctness.

### 7.1 Structural Validation

Automated checks examined:

- The 15-field dataset schema.
- Example identifiers and record consistency.
- Gold-answer roles.
- Answer-option permutations.
- English-Bengali metadata alignment.

An answer-role audit covered all 61,312 records across 214 themes.

Of these, 61,232 records were eligible for directional bias analysis. Eighty Gender records contained identical comparison roles and were excluded from that analysis.

No answer-role mapping errors were reported.

### 7.2 Human Annotation

Four independent annotators assessed the correctness of gold labels across a random sample of 150 instances.

The annotation process achieved:

**Fleiss' Kappa (κ) = 0.8349**

This indicates strong inter-annotator agreement, classified within the almost-perfect agreement range under the Landis and Koch descriptive scale.

This assessment measures agreement regarding sampled answer-label correctness. It should not be interpreted as comprehensive validation of Bengali fluency, cultural relevance, or every record in the dataset.

### 7.3 Statistical Consistency Testing

The benchmark underwent 220 paired statistical tests across 14 category-subtype strata per language.

The tests examined five recorded variables:

- `is_unknown`
- `is_stereotype`
- `label_position`
- `stereotype_position`
- `unknown_position`

McNemar's test was used for binary variables, while paired t-tests and Wilcoxon signed-rank tests were used to assess recorded answer positions.

The reported results were:

- All Bonferroni-adjusted p-values were 1.0.
- The smallest unadjusted p-value was 0.1297.
- The largest reported absolute effect size was 0.064.

These findings indicate no detectable systematic differences under the specified statistical tests. They do not establish equivalence or rule out unmeasured textual cues.

## 8. Intended Uses

BDBBQ is intended primarily for academic research and systematic evaluation of social bias in Large Language Models.

Potential applications include:

- Evaluating social bias in Bengali and English LLMs.
- Comparing cross-lingual model performance.
- Investigating stereotype-aligned answer selection.
- Evaluating ambiguity handling and contextual reasoning.
- Analyzing category-specific model behavior.
- Studying the relationship between accuracy and directional bias.
- Supporting research into bias mitigation and trustworthy AI.

The benchmark can be used to evaluate open-weight and closed-source language models, subject to their respective access and usage conditions.

## 9. Evaluation Considerations

BDBBQ distinguishes between overall answer accuracy and stereotype-aligned directional bias.

Accuracy is reported overall and separately for ambiguous and disambiguated contexts.

The benchmark also adopts BBQ-style signed bias measures:

- `s_AMB`: Directional bias in ambiguous contexts, scaled by the ambiguous error rate.
- `s_DIS`: Directional bias in disambiguated contexts.

The scores range from -1 to +1.

These measures capture directional preferences among eligible non-Unknown responses and should be interpreted alongside accuracy, response validity, and category-level results.

A lower aggregate directional score does not necessarily establish that a model is less harmful or fairer. Opposing effects across themes, invalid responses, and category-specific patterns can influence aggregate results.

Researchers should avoid relying exclusively on overall accuracy or a single aggregate bias score when interpreting benchmark performance.

## 10. Limitations and Known Gaps

BDBBQ has several limitations that should be considered when using or interpreting its results.

**Limited independent linguistic and cultural validation:** Although structural checks and sampled gold-label annotation were conducted, comprehensive independent human validation of Bengali fluency, cultural framing, and domain relevance remains outstanding.

**Theme-level dependence:** The 61,312 records represent 214 underlying themes. Many records are generated through related wording variants, polarity changes, and answer-option permutations. Therefore, individual records should not be treated as statistically independent observations.

**Potential residual positional cues:** Some inherited positional patterns, particularly in Health Status, remain under review.

**Bilingual pragmatic equivalence:** Structural alignment between English and Bengali does not guarantee that corresponding examples convey precisely equivalent implications.

**Scope of social representation:** The benchmark covers selected socio-cultural themes and does not represent the full diversity of Bangladeshi society.

**Interpretation of bias scores:** Aggregate directional scores can conceal category-specific effects and should not be interpreted as direct measures of overall model fairness or safety.

These limitations should guide future dataset refinement, independent validation, and replication efforts.

## 11. Ethical Considerations

BDBBQ addresses potentially sensitive social and cultural topics, including age, gender identity, religion, ethnicity, health status, politics, and other dimensions of social identity.

The benchmark is intended to examine model behavior and identify potential stereotypical associations. Its examples should not be interpreted as factual claims about demographic groups or as evidence that particular stereotypes are valid.

Researchers and developers should:

- Avoid using benchmark examples to reinforce discriminatory assumptions.
- Interpret results within their relevant socio-cultural context.
- Consider the potential consequences of reproducing sensitive examples.
- Avoid using benchmark scores as the sole basis for real-world decisions about individuals or communities.
- Recognize that benchmark performance does not establish the absence of social bias.

## 12. Data Availability and Licensing

**Project Repository:**  
https://github.com/BDBBQ-Research/BDBBQ

**GitHub Organization:**  
https://github.com/BDBBQ-Research

**Data Format:** JSONL

**Dataset License:** To be determined.

Dataset redistribution and licensing are subject to the research team's ownership, permission, and distribution requirements.

The dataset and its associated evaluation code may require separate licensing arrangements. No open-source or open-data license should be inferred unless explicitly specified in the repository.

## 13. Citation

If you use BDBBQ in academic research, please cite the associated research paper.

**Manuscript title:**

*BDBBQ: A Human-AI Collaborative Benchmark for Bengali Social Bias in LLMs*

Full publication and citation metadata will be provided following publication.

## 14. Acknowledgements

BDBBQ builds upon the original BanglaBBQ research and draws on established work in question-answering-based social bias evaluation, including BBQ and culturally adapted benchmarks.

The expanded benchmark is developed as a collaborative research project by researchers from the Department of Computer Science and Engineering, BRAC University.

## 15. Project Information

**Project:** BDBBQ Research  
**Institution:** BRAC University  
**Research Area:** Natural Language Processing, Social Bias Evaluation, and Trustworthy AI

**GitHub Organization:**  
https://github.com/BDBBQ-Research

**Repository:**  
https://github.com/BDBBQ-Research/BDBBQ

---

*BDBBQ aims to support more culturally grounded and language-sensitive evaluation of social bias in Large Language Models, contributing to ongoing research in responsible and trustworthy AI.*
