
# TakeMeter – Planning Document

## Community

For this project, I chose NBA online discussion communities. NBA discussions contain a wide variety of discourse styles, including detailed basketball analysis, emotional reactions to games, memes, jokes, and factual questions. This makes the community a strong candidate for a text classification task because the differences between discourse types are meaningful to community members.

## Labels

### Insightful

A basketball opinion supported by reasoning, strategy, statistics, matchup analysis, or specific observations.

Examples:

* "Jokic creates efficient offense because his passing forces defenses to rotate."
* "The Celtics are difficult to guard because their spacing creates mismatches on nearly every possession."

### Reactionary

An emotional or exaggerated opinion driven by frustration, hype, or overreaction to a recent event.

Examples:

* "Trade everyone after that loss."
* "The season is over after that game."

### Low_Effort

A meme, joke, insult, or one-line comment with little meaningful basketball discussion.

Examples:

* "LeFraud strikes again."
* "Ball does not lie."

### Neutral

A factual statement, question, or information request without a strong opinion.

Examples:

* "Who is starting at center tonight?"
* "What time does the game begin?"

## Hard Edge Cases

One difficult edge case is distinguishing between reactionary and low_effort comments.

Example:

"Not my MVP."

This could be interpreted as a low-effort joke or an emotional reaction.

Decision rule:
If the comment primarily expresses an emotional judgment or criticism, it will be labeled reactionary. If it is mostly a meme, joke, or slogan with little reasoning, it will be labeled low_effort.

Another difficult edge case is distinguishing insightful from reactionary comments.

Example:

"He is not a superstar because he missed the final shot."

Although it references a basketball event, it provides no meaningful evidence or reasoning. Therefore it is labeled reactionary rather than insightful.

## Data Collection Plan

Examples were collected and created to simulate realistic NBA community discourse patterns.

Target distribution:

* Insightful: 50 examples
* Reactionary: 50 examples
* Low_Effort: 50 examples
* Neutral: 50 examples

Total dataset size: 200 examples.

If a label became underrepresented, additional examples would be created for that category until the distribution was balanced.

## Evaluation Metrics

The primary metric is accuracy because the task is a multi-class classification problem.

Additional metrics:

* Precision
* Recall
* F1-score
* Confusion Matrix

Accuracy alone is not sufficient because a model could perform well on one class while failing on others. Precision, recall, and F1 provide a more complete view of performance for each label.

## Definition of Success

A useful classifier should achieve:

* Accuracy above 70%
* Per-class F1 score above 0.65
* No single class with near-zero recall

For deployment in a real community moderation or analytics tool, a model with approximately 75–80% accuracy would be considered acceptable.

## AI Tool Plan

### Label Stress Testing

I used AI assistance to examine whether the proposed labels were sufficiently distinct. I reviewed examples that could plausibly belong to multiple labels and refined the decision boundaries.

### Annotation Assistance

AI assistance was used to help generate and organize examples consistent with the label definitions. All labels were reviewed manually before training.

### Failure Analysis

After training, I reviewed incorrect predictions and used AI assistance to identify common patterns. These findings were verified manually through inspection of model outputs.

## Expected Outcome

I expect the model to learn broad distinctions between analysis, emotional reactions, memes, and neutral comments. The most difficult distinction will likely be between reactionary and low_effort comments because both are often short and emotionally charged.
