## Baseline Reflection

> Model Performance:

🎯 Baseline accuracy: 0.700 (evaluated on 30/30 parseable responses)

Per-class metrics (baseline):
precision recall f1-score support

        news       1.00      0.90      0.95        10
    hot take       0.71      0.45      0.56        11
    reaction       0.50      0.78      0.61         9

    accuracy                           0.70        30
    macro avg      0.74      0.71      0.70        30
    weighted avg   0.75      0.70      0.70        30

> The baseline zero-shot classifier achieved an overall **accuracy of 70%** with a **macro F1-score of 0.70**. It performed very well on the **News** label (F1 = 0.95), suggesting that factual reports and announcements are relatively easy for the model to identify. However, it struggled with **Hot Take** (F1 = 0.56), while **Reaction** achieved a moderate F1-score of 0.61.

> The primary source of error appears to be confusion between **Hot Take** and **Reaction**. Both labels often contain emotionally charged language, making it difficult for a general-purpose language model to distinguish between an immediate emotional response and a broad, unsupported opinion. This aligns with the anticipated edge case identified during the dataset design phase.

> **Hypothesis:** Fine-tuning the model on manually annotated examples from **r/soccer** will improve its ability to distinguish between **Hot Take** and **Reaction** by learning the subtle linguistic patterns and annotation rules specific to this community. In particular, I expect improvements in the recall and F1-score for the **Hot Take** class while maintaining strong performance on **News**.
