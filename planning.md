# Community

I chose Reddit career and IT communities because I am a first-generation college graduate from an immigrant family. Growing up, my family did not have experience with technology careers, cybersecurity, engineering, or IT. Much of my career knowledge came from online communities where people shared advice, experiences, and lessons learned.

This community is a strong fit for a classification task because discussions vary significantly. Some posts ask questions, some provide detailed career advice, some share personal experiences, and others express strong opinions. These distinctions create meaningful categories that a machine learning model can learn to recognize.

# Labels

## actionable_advice

Definition:
A post whose primary purpose is giving specific recommendations, instructions, or career guidance.

Examples:

* Tailor your resume for every job application.
* Practice Active Directory and ticketing systems before applying for help desk positions.

## personal_experience

Definition:
A post whose primary purpose is sharing the author's own career journey, success, failure, or lesson learned.

Examples:

* I applied to over 50 jobs before receiving my first interview.
* My internship taught me more practical skills than my classes.

## hot_take

Definition:
A strong opinion expressed with little or no supporting evidence.

Examples:

* Certifications matter more than college degrees.
* Most entry-level jobs are fake postings.

## question_request

Definition:
A post whose primary purpose is asking for information, clarification, or guidance.

Examples:

* How do I get my first cybersecurity internship?
* Is Security+ worth earning before graduation?

# Hard Edge Cases

Example:

"I got my first IT job after building projects, so everyone should build projects."

Possible labels:

* personal_experience
* actionable_advice

Decision Rule:

If the main purpose is telling the author's story, label it personal_experience.

If the main purpose is advising others what action to take, label it actionable_advice.

# Data Collection Plan

Data will be collected from public Reddit career and technology communities.

Target dataset size:

* 60 actionable_advice
* 60 personal_experience
* 60 hot_take
* 60 question_request

Total: 240 examples

If one label becomes underrepresented, I will intentionally search for additional posts matching that category until the dataset is balanced.

# Evaluation Metrics

The model will be evaluated using:

* Accuracy
* Precision
* Recall
* F1 Score
* Confusion Matrix

Accuracy measures overall correctness.

Precision and Recall help determine whether specific labels are being confused with one another.

F1 Score is especially important because it balances Precision and Recall.

The confusion matrix will help identify which labels the model struggles to distinguish.

# Definition of Success

The classifier will be considered successful if it achieves:

* At least 70% overall accuracy
* At least 0.60 F1 score for every label
* No label with Recall below 50%

A model meeting these thresholds would provide useful classification support for real career community discussions.

# AI Tool Plan

## Label Stress Testing

I will use ChatGPT and Claude to generate ambiguous posts that sit between two labels. If multiple labels appear reasonable, I will revise the definitions before annotation begins.

## Annotation Assistance

I may use ChatGPT to suggest preliminary labels for some examples. However, every example will be manually reviewed and corrected before being included in the final dataset.

Any AI-assisted labels will be disclosed in the README.

## Failure Analysis

After evaluation, I will provide misclassified examples to ChatGPT and Claude and ask for possible error patterns.

I will manually verify all suggested patterns before including them in the final evaluation report.

# Bonus Point Preparation

To maximize bonus points, I plan to:

* Perform Inter-Annotator Reliability testing on at least 30 examples.
* Analyze confidence calibration by comparing prediction confidence to actual accuracy.
* Identify systematic error patterns across labels.
* Build a simple interface allowing users to submit a post and receive a predicted label and confidence score.
