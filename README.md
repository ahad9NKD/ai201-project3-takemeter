# TakeMeter

## Project Overview

TakeMeter is a text classification project that evaluates different types of discourse found in NBA online communities.

The objective is to classify NBA-related comments into one of four categories:

* Insightful
* Reactionary
* Low_Effort
* Neutral

The project compares a fine-tuned DistilBERT classifier against a zero-shot Large Language Model baseline using Groq's llama-3.3-70b-versatile.

---

# Community Choice

I chose NBA discussion communities because they contain a wide variety of discourse styles. Some users provide detailed basketball analysis, while others post emotional reactions, memes, jokes, or simple factual questions.

This makes NBA communities an interesting classification task because the differences between these discourse styles are meaningful to community members.

---

# Label Taxonomy

## Insightful

A basketball opinion supported by reasoning, strategy, statistics, matchup analysis, or specific observations.

Examples:

* "Jokic creates efficient offense because his passing forces defenses to rotate."
* "The Celtics are difficult to guard because their spacing creates mismatches on nearly every possession."

---

## Reactionary

An emotional or exaggerated opinion driven by frustration, hype, or overreaction to a recent event.

Examples:

* "Trade everyone after that loss."
* "The season is over after that game."

---

## Low_Effort

A meme, joke, insult, slogan, or one-line comment with little meaningful basketball discussion.

Examples:

* "LeFraud strikes again."
* "Ball does not lie."

---

## Neutral

A factual statement, information request, or question without a strong opinion.

Examples:

* "Who is starting at center tonight?"
* "What time does the game start?"

---

# Data Collection

The dataset contains 200 manually labeled NBA discussion examples.

Dataset distribution:

| Label       | Count |
| ----------- | ----: |
| Insightful  |    50 |
| Reactionary |    50 |
| Low_Effort  |    50 |
| Neutral     |    50 |

Total examples: 200

The dataset was created to simulate realistic NBA discussion patterns while maintaining balanced representation across all labels.

---

# Difficult Labeling Decisions

## Example 1

Comment:

> "Not my MVP."

Possible labels:

* Low_Effort
* Reactionary

Decision:

Low_Effort because it functions primarily as a meme-style slogan rather than a genuine emotional argument.

---

## Example 2

Comment:

> "LeBron is overrated because his playoff record against top seeds is below .500."

Possible labels:

* Insightful
* Reactionary

Decision:

Reactionary because the statistic is used primarily to support a provocative claim rather than a structured basketball argument.

---

## Example 3

Comment:

> "They are going nowhere because the offense looked bad for one half."

Possible labels:

* Insightful
* Reactionary

Decision:

Reactionary because the conclusion is an overreaction based on limited evidence.

---

# Fine-Tuning Approach

Base Model:

* distilbert-base-uncased

Training Configuration:

* Epochs: 3
* Learning Rate: 2e-5
* Batch Size: 16
* Weight Decay: 0.01

Dataset Split:

* Training: 140 examples
* Validation: 30 examples
* Test: 30 examples

The model was fine-tuned using HuggingFace Transformers.

---

# Baseline Model

The baseline model used Groq's:

* llama-3.3-70b-versatile

The baseline was evaluated using zero-shot prompting.

The prompt included:

* Label definitions
* Examples for each label
* Instructions to output only the label name

No task-specific training was performed.

---

# Evaluation Results

## Overall Accuracy

| Model                   | Accuracy |
| ----------------------- | -------: |
| Groq Zero-Shot Baseline |    1.000 |
| Fine-Tuned DistilBERT   |    0.733 |

The Groq baseline outperformed the fine-tuned model by 26.7 percentage points.

---

## Fine-Tuned Model Metrics

| Label       | Precision | Recall |   F1 |
| ----------- | --------: | -----: | ---: |
| Insightful  |      0.80 |   1.00 | 0.89 |
| Reactionary |      0.45 |   0.71 | 0.56 |
| Low_Effort  |      1.00 |   0.25 | 0.40 |
| Neutral     |      1.00 |   1.00 | 1.00 |

Macro Average F1:

* 0.71

Overall Accuracy:

* 0.733

---

## Baseline Metrics

| Label       | Precision | Recall |   F1 |
| ----------- | --------: | -----: | ---: |
| Insightful  |      1.00 |   1.00 | 1.00 |
| Reactionary |      1.00 |   1.00 | 1.00 |
| Low_Effort  |      1.00 |   1.00 | 1.00 |
| Neutral     |      1.00 |   1.00 | 1.00 |

Overall Accuracy:

* 1.000

---

# Confusion Matrix

| True \ Predicted | Insightful | Reactionary | Low_Effort | Neutral |
| ---------------- | ---------: | ----------: | ---------: | ------: |
| Insightful       |          8 |           0 |          0 |       0 |
| Reactionary      |          2 |           5 |          0 |       0 |
| Low_Effort       |          0 |           6 |          2 |       0 |
| Neutral          |          0 |           0 |          0 |       7 |

## Interpretation

The confusion matrix shows that the model perfectly classified Insightful and Neutral comments.

However, six of eight Low_Effort comments were misclassified as Reactionary.

This suggests that the model learned emotional language patterns but struggled to recognize basketball memes and slang.

---

# Error Analysis

## Error 1

Comment:

> Someone call security.

True Label:

Low_Effort

Predicted:

Reactionary

Confidence:

0.26

Analysis:

The model interpreted a meme-style joke as an emotional reaction.

---

## Error 2

Comment:

> They should waive the backup guard after that turnover.

True Label:

Reactionary

Predicted:

Insightful

Confidence:

0.28

Analysis:

The model confused basketball terminology with actual analysis.

---

## Error 3

Comment:

> Ball does not lie.

True Label:

Low_Effort

Predicted:

Reactionary

Confidence:

0.26

Analysis:

The model did not recognize this common basketball meme and instead focused on its emotional tone.

---

# Sample Classifications

| Comment                                                                        | Prediction  | Confidence |
| ------------------------------------------------------------------------------ | ----------- | ---------: |
| Who is starting at center tonight?                                             | Neutral     |       0.99 |
| Jokic creates efficient offense because his passing forces defenses to rotate. | Insightful  |       0.94 |
| Trade everyone after that loss.                                                | Reactionary |       0.92 |
| Ball does not lie.                                                             | Reactionary |       0.26 |
| Someone call security.                                                         | Reactionary |       0.26 |

Example Explanation:

The prediction for "Who is starting at center tonight?" is reasonable because the comment is a factual question and contains no opinion or emotional judgment.

---

# Reflection

The model successfully learned the distinction between analytical discussion and factual questions. It achieved perfect recall on Insightful and Neutral comments.

However, it struggled to distinguish between Low_Effort memes and Reactionary opinions. Many basketball memes contain emotional language, causing the model to learn surface-level patterns instead of the intended conceptual distinction.

This demonstrates that the model learned linguistic signals rather than deeper cultural knowledge of NBA discourse.

---

# Spec Reflection

One way the project specification helped was by emphasizing label design before model training. Defining precise labels significantly improved consistency during annotation.

One way the implementation diverged from the original plan was in data collection. Rather than collecting all examples from live community discussions, I created a balanced dataset designed to reflect realistic NBA discourse patterns.

---

# AI Usage

## Instance 1

I used ChatGPT to help refine label definitions and identify ambiguous boundary cases between Reactionary and Low_Effort comments.

I revised several definitions after reviewing generated examples.

## Instance 2

I used AI assistance to generate and organize examples that matched the label taxonomy.

I manually reviewed all labels before training and adjusted examples that did not fit the intended category.

## Instance 3

I used AI assistance during error analysis to identify common failure patterns in the model's predictions.

The final conclusions were verified manually using the confusion matrix and misclassified examples.

---

# Repository Structure

```text
ai201-project3-takemeter/
│
├── planning.md
├── README.md
│
├── data/
│   └── labeled/
│       └── nba_comments_labeled.csv
│
└── outputs/
    ├── evaluation_results.json
    └── confusion_matrix.png
```
