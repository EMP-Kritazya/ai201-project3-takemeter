# Soccer Discussion Classification using DistilBERT

# Author - **Kritazya Upreti**

## Project Overview

This project fine-tunes DistilBERT to classify posts from **r/soccer** into one of three discourse categories:

- **News** – Factual information or reporting without expressing the author's opinion.
- **Hot Take** – A bold or controversial opinion stated confidently without supporting evidence.
- **Reaction** – An immediate emotional response or personal feeling about a specific event.

The objective was to determine whether a fine-tuned transformer model could outperform a zero-shot large language model on this domain-specific classification task.

---

# Dataset

- **Community:** r/soccer
- **Total examples:** 199
- **Training:** 139
- **Validation:** 30
- **Testing:** 30

### Training Distribution

| Label    | Count |
| -------- | ----: |
| News     |    65 |
| Hot Take |    63 |
| Reaction |    71 |

The dataset is approximately balanced across all three classes.

---

# Models Evaluated

## Baseline

**Model:** Llama-3.3-70B (Zero-shot)

### Overall Performance

- **Accuracy:** **70.0%**

### Per-class Metrics

| Label    | Precision | Recall |   F1 |
| -------- | --------: | -----: | ---: |
| News     |      1.00 |   0.90 | 0.95 |
| Hot Take |      0.71 |   0.45 | 0.56 |
| Reaction |      0.50 |   0.78 | 0.61 |

---

## Fine-Tuned Model

**Model:** DistilBERT (`distilbert-base-uncased`)

### Training Configuration

| Parameter         |    Value |
| ----------------- | -------: |
| Epochs            |        8 |
| Learning Rate     |     2e-5 |
| Batch Size        |       16 |
| Weight Decay      |     0.01 |
| Warmup Steps      |        2 |
| Best Model Metric | Accuracy |

`FineTuned Parameters`

> Warmup Steps was dropped from 10 to 2, since there weren't a lot of training data points and we couldn't afford losing training data points for warmup steps.
> Epochs was increased from 3 to 8 as the accuracy was still increasing and validation loss and training loss was decreasing in a healthy manner.

### Overall Performance

- **Accuracy:** **86.7%**
- **Macro F1:** **0.87**

### Per-class Metrics

| Label    | Precision | Recall |   F1 |
| -------- | --------: | -----: | ---: |
| News     |      1.00 |   0.90 | 0.95 |
| Hot Take |      0.89 |   0.80 | 0.84 |
| Reaction |      0.75 |   0.90 | 0.82 |

The fine-tuned model improved substantially over the zero-shot baseline, particularly on the **Hot Take** class.

---

# Confusion Matrix

| True Label   | Predicted News | Predicted Hot Take | Predicted Reaction |
| ------------ | -------------: | -----------------: | -----------------: |
| **News**     |              9 |                  0 |                  1 |
| **Hot Take** |              0 |                  8 |                  2 |
| **Reaction** |              0 |                  1 |                  9 |

---

# Error Analysis

The fine-tuned model made **4 incorrect predictions** out of 30 test examples.

## Example 1

**Post**

> Messi's goal shouldn't have stood because of a foul in the build up of the goal

- **True Label:** Reaction
- **Predicted:** Hot Take
- **Confidence:** 0.48

### Analysis

This sentence sits directly on the boundary between **Reaction** and **Hot Take**. It expresses an opinion about a specific match incident without providing evidence. While the annotation considered it an immediate reaction to an event, the model interpreted the unsupported judgment ("shouldn't have stood") as a Hot Take. This demonstrates the inherent ambiguity between these two labels.

---

## Example 2

**Post**

> For me it's that Erling Haaland is quite rubbish, don't get me wrong he is good but he simply cannot perform against the good teams.

- **True Label:** Hot Take
- **Predicted:** Reaction
- **Confidence:** 0.55

### Analysis

This post contains a broad unsupported opinion about a player, making it a Hot Take. However, the first-person phrasing ("For me...") resembles conversational reactions that frequently appear in the training data. The model appears to associate first-person emotional language with the Reaction class.

---

## Example 3

**Post**

> From sideshow to goldmine: the rise of (sell on) clauses

- **True Label:** News
- **Predicted:** Reaction
- **Confidence:** 0.61

### Analysis

Unlike most News examples, this post is written as a headline rather than a complete factual statement. It lacks explicit reporting verbs such as "confirmed" or "announced," making it appear less informational. This suggests the model relies heavily on linguistic cues commonly found in news reporting.

---

# Overall Failure Patterns

After reviewing all incorrect predictions, several common patterns emerged.

- The majority of errors involved confusion between **Hot Take** and **Reaction**.
- Both categories contain subjective language, making the boundary inherently difficult.
- First-person expressions such as _"For me..."_ or _"I have always said..."_ frequently caused the model to predict **Reaction** even when the post was expressing a broader unsupported opinion.
- Headline-style News posts lacking explicit factual language were more difficult than traditional news reports.

Initially, I suspected that short posts or sarcastic comments were responsible for most mistakes. After manually reviewing every error, neither pattern consistently appeared. Instead, the primary challenge was the semantic overlap between unsupported opinions and immediate reactions.

To improve the classifier further, I would collect more examples illustrating the boundary between **Hot Take** and **Reaction**, particularly examples that share similar wording but belong to different labels.

---

# Sample Classifications

| Post                                                   | Predicted Label | Confidence |
| ------------------------------------------------------ | --------------- | ---------: |
| FIFA confirms expanded Club World Cup schedule.        | News            |       0.85 |
| For me it's that Erling Haaland is quite rubbish...    | Reaction        |       0.49 |
| Messi's goal shouldn't have stood because of a foul... | Hot Take        |       0.48 |
| What a finish! Absolutely unbelievable goal.           | Reaction        |       0.82 |
| Chelsea have completed the signing of João Pedro.      | News            |       0.91 |

The prediction for **"Chelsea have completed the signing of João Pedro."** is reasonable because the post reports a factual transfer announcement without expressing any personal opinion or emotional response, matching the definition of the News label.

---

# Reflection

The original goal was to distinguish between factual reporting, unsupported opinions, and emotional reactions. Overall, the model learned this distinction well, achieving an accuracy of **86.7%** on the held-out test set.

However, the model's decision boundary differs slightly from the intended label definitions. Rather than focusing purely on the intent of a post, it relies heavily on linguistic patterns. First-person statements were frequently interpreted as Reactions, while headline-style News posts without explicit reporting language occasionally appeared subjective. This indicates that the model learned lexical and stylistic cues in addition to the semantic distinctions defined during annotation.

---

# Specification Reflection

## How the specification helped

The specification emphasized creating labels that were **mutually exclusive** and defining explicit decision rules for ambiguous cases before annotation. This significantly improved annotation consistency, especially for the difficult boundary between Hot Take and Reaction.

## Where my implementation diverged

The original plan included an **Analysis** label. After collecting examples from r/soccer, I found that genuine analytical posts were relatively rare and difficult to distinguish consistently from Hot Takes. I therefore removed this label and focused on three categories that were both more common and easier to annotate reliably.

---

# AI Usage

AI tools were used throughout the project to improve both the annotation process and the final evaluation.

## 1. Annotation Assistance

Claude was used to pre-label a large batch of collected Reddit posts based on the label definitions in `planning.md`. I manually reviewed every generated label and corrected examples that violated the decision rules, particularly those on the boundary between Hot Take and Reaction.

## 2. Label Stress Testing

ChatGPT was used to generate borderline examples that could reasonably belong to multiple labels. These synthetic examples helped refine the annotation guidelines and establish explicit decision rules before labeling the dataset.

## 3. Failure Analysis

After evaluating the fine-tuned model, ChatGPT was used to identify common patterns among the incorrect predictions. Rather than accepting these observations directly, I manually reviewed each misclassified example to verify which patterns were genuine and which hypotheses (such as post length or sarcasm) did not hold.

---
