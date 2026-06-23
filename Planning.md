## Community Chosen

> Soccer (r/soccer): ` This is an active online community where users discuss football through news, match threads, tactical dicussions, opinions, and reactions`

## Labels

- News - `Reports factual information or events without adding substantial personal opinion, interpretation, or reasoning.`
  Two example Posts:
  1. The President of Turkish Football Federation has called on the Justice Minister to imprison those who criticize the national team.
  2. Cape Verde manager Bubista presented gifts to Uruguay manager Marcelo Bielsa before the match

- Hot Take - `a bold, confident opinion stated without supporting evidence. The claim might be true, but the post asserts rather than argues.`
  Two example Posts:
  1. That group would be designated as a terrorist organization
  2. Messi's goal shouldn't have stood. Clear foul in the build up.

- Reaction - `Expresses an emotional, humorous, or personal response to an event, post, or discussion without presenting structured reasoning.`
  Two example Posts:
  1. I have never seen Uruguay play under Bielsa before the tournament but I was still excited because I loved his Leeds side. But man this not that exciting.
  2. Of all the things that could've happened to me today, listening to Yaya talking about Hazard's bum was one of the least likely.

### One Post That I Am Not Sure About

**Post:**

> "This referee has completely ruined the game."

This post could reasonably be labeled as either **Reaction** or **Hot Take**. It expresses a strong negative opinion, but it may also represent an immediate emotional response to a specific match event.

**Decision Rule:**  
If the statement is directed at a specific match or event (e.g., criticizing a referee's decisions during the current game), it will be labeled **Reaction**, as it reflects an immediate emotional response. If the statement makes a broad or absolute judgment about a person, team, or topic beyond the current event (e.g., _"This referee is the worst in Europe."_), it will be labeled **Hot Take**, since it expresses a strong opinion without supporting evidence.

Using this decision rule, **"This referee has completely ruined the game."** is labeled **Reaction** because the criticism is directed at the referee's impact on a specific match rather than making a broader claim about the referee's overall ability.

## Why These Labels Matter?

`Members of r/soccer use the community for different purposes, including staying informed about football news, discussing tactical aspects of matches, debating controversial opinions, and sharing emotional reactions during games.
Distinguishing these discourse types helps capture how users communicate rather than whether their opinions are correct, making the labels suitable for training a text classification model that identifies different forms of online discussion.`

## Data Collection Plan:

> I will collect data from r/soccer on Reddit by sampling posts and comments from a variety of discussions, including match threads, post-match threads, transfer news, and general discussion threads. Sampling from different types of discussions will help capture all four discourse labels: News, Hot Take, Reaction, and Analysis (if encountered) or, in your case, News, Hot Take, and Reaction.

> My goal is to collect approximately 200 examples, aiming for a balanced distribution across the labels. Ideally, I will annotate around 65–70 examples each for News, Hot Take, and Reaction.

> If one label is underrepresented after collecting 200 examples, I will perform targeted sampling by searching for posts or comments where that discourse type is more common. For example, I will collect additional examples from news posts if News is underrepresented or from live match threads if Reaction is underrepresented. This targeted approach will help maintain a more balanced dataset for training the classification model.

## Evaluation Metrics

> I will evaluate the classifier using accuracy, precision, recall, F1-score, and a confusion matrix. While accuracy provides an overall measure of how often the model predicts the correct label, it does not indicate how well the model performs on individual classes, especially if one label is more common than the others.

> Precision and recall will help evaluate how reliably the model identifies each discourse type. Precision measures how often the model's predictions for a label are correct, while recall measures how many examples of that label the model successfully identifies. I will also use the F1-score because it balances precision and recall, making it a better indicator of overall performance for each class. Finally, a confusion matrix will help identify which labels are most frequently confused with one another, such as Reaction and Hot Take, allowing me to analyze the model's weaknesses.

## Definition of Success

> A successful classifier should correctly distinguish between News, Hot Take, and Reaction for the majority of posts while minimizing confusion between similar discourse types. I would consider the model successful if it achieves an overall accuracy of at least 80% and an F1-score of 0.80 or higher for each label. This level of performance would make the classifier useful for automatically categorizing discussions, assisting moderators, or organizing content within an online football community, even though occasional misclassifications are expected due to the subjective nature of some posts.

## AI Tool Plan

### Label Stress-Test

> Before beginning the annotation process, I will use ChatGPT to evaluate the robustness of my label definitions. I will provide the definitions for News, Hot Take, and Reaction, along with my annotation guidelines, and ask it to generate 5–10 posts that intentionally sit at the boundary between two labels. If I cannot confidently assign a single label to these examples, I will refine my label definitions or decision rules before annotating the full dataset.

### Annotation Assistance

> I will primarily use Claude to assist with the annotation workflow. Claude will help pre-label small batches of posts based on my annotation guidelines, allowing me to review the suggested labels more efficiently. Every example will be manually reviewed, and I will make the final labeling decision.

### Failure Analysis

> After training and evaluating the classifier, I will use Claude to analyze the model's incorrect predictions and identify common patterns of misclassification. For example, I will ask Claude to determine whether certain wording, sentence length, or conversational style causes confusion between Reaction and Hot Take. I will manually verify every suggested pattern by reviewing the original posts and comparing them with my annotation guidelines before including any conclusions in the final report.
