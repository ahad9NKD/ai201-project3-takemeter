# TakeMeter

## Project Overview

TakeMeter is a text classification project that evaluates the type and quality of discourse found in NBA online communities.

The goal is to classify NBA-related comments into one of four categories:

* Insightful
* Reactionary
* Low_Effort
* Neutral

This project compares a fine-tuned DistilBERT model against a zero-shot large language model baseline using Groq's llama-3.3-70b-versatile.

---

## Community

NBA communities contain a mixture of detailed basketball analysis, emotional reactions, memes, jokes, and factual discussion. These distinctions are meaningful to community members and provide a useful classification task.

---

## Label Definitions

### Insightful

A basketball opinion supported by reasoning, context, statistics, strategy, or specific observations.

Example:

"Jokic creates efficient offense because his passing forces defenses to rotate."

### Reactionary

An emotional or exaggerated opinion based on frustration, hype, or overreaction.

Example:

"Trade everyone after that loss."

### Low_Effort

A meme, joke, insult, slogan, or one-line comment with little meaningful basketball discussion.

Example:

"LeFraud strikes again."

### Neutral

A factual statement, information request, or question without a strong opinion.

Example:

"Who is starting at center tonight?"

---

## Dataset

The dataset contains 200 synthetic NBA discussion examples manually created and labeled to simulate common discourse patterns found in online NBA communities.

Label Distribution:

| Label       | Count |
| ----------- | ----: |
| Insightful  |    50 |
| Reactionary |    50 |
| Low_Effort  |    50 |
| Neutral     |    50 |

Total Examples: 200

---

## Model

Base Model:

* distilbert-base-uncased

Training Configuration:

* Epochs: 3
* Learning Rate: 2e-5
* Batch Size: 16
* Weight Decay: 0.01

The model was fine-tuned using HuggingFace Transformers and evaluated on a held-out test set.

Dataset Split:

* Training: 140 examples
* Validation: 30 examples
* Test: 30 examples

---

## Baseline

A zero-shot baseline was implemented using:

* llama-3.3-70b-versatile (Groq)

The baseline received the label definitions and classified each test example without any task-specific training.

---

## Evaluation Results

| Model                   | Accuracy |
| ----------------------- | -------: |
| Zero-shot Groq Baseline |    1.000 |
| Fine-tuned DistilBERT   |    0.733 |

The Groq baseline outperformed the fine-tuned DistilBERT model on this dataset.

---

## Error Analysis

The fine-tuned model made 8 mistakes out of 30 test examples.

### Example 1

Text:

"Someone call security."

True Label:

low_effort

Predicted Label:

reactionary

Analysis:

The model confused a meme-style comment with an emotional reaction.

### Example 2

Text:

"They should waive the backup guard after that turnover."

True Label:

reactionary

Predicted Label:

insightful

Analysis:

The model interpreted basketball-specific language as evidence of analysis even though the statement was primarily emotional.

### Example 3

Text:

"Ball does not lie."

True Label:

low_effort

Predicted Label:

reactionary

Analysis:

The model treated a common basketball meme as an emotional opinion.

---

## Reflection

This project demonstrated that label design is often more important than model training. Defining precise and mutually exclusive labels required more effort than running the actual fine-tuning pipeline.

The fine-tuned DistilBERT model achieved 73.3% accuracy, indicating that it learned broad distinctions between analysis, reactions, memes, and neutral discussion. However, it struggled with short comments and basketball-specific meme language.

The Groq baseline achieved perfect accuracy on this dataset. This suggests that large modern language models can outperform a specialized fine-tuned model when the training dataset is small and the classification boundaries are clearly defined.

Future improvements would include collecting real NBA comments, increasing dataset size, and adding more ambiguous examples to improve robustness.

---

## Files

Project Repository Contents:

* planning.md
* README.md
* data/labeled/nba_comments_labeled.csv
* outputs/confusion_matrix.png
* outputs/evaluation_results.json

---

## AI Usage Disclosure

AI tools were used for:

* Label design assistance
* Example generation
* Annotation support
* Error analysis
* Report drafting

All final decisions regarding labels, dataset structure, evaluation, and interpretation were reviewed and approved manually.
