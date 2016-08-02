---
  layout: post
  title: "Temporal Information Extraction"
  categories: projects
---

This project uses mainly regular expressions with some additional tweaking functions to extract temporal information (dates, durations, time, etc) from news articles or any other text.

Everything here should be viewed as a proof on concept. To code is not pretty and it is only targeted against the TempEval-2 Task A of the SemEval-2010 conference competition and should not to be used in a real product.

![screenshot](/images/projects/temporal.png)

The code was evaluated against SemEval-2010.

### Test set:

```text
true positives:   165
true negatives:   9336
false positives:  8
false negatives:  104

attribute type: +94.0 -4.0
attribute value: +86.0 -12.0

precision   0.95
recall      0.61
f1-measure  0.75
accuracy    0.99

attribute type       0.96
attribute value      0.88
```

### Training set:

```text
true positives:   1053
true negatives:   51246
false positives:  87
false negatives:  1064

attribute type: +546.0 -17.0
attribute value: +494.0 -69.0

precision   0.92
recall      0.50
f1-measure  0.65
accuracy    0.98

attribute type       0.97
attribute value      0.88
```

Due to time constraints, I decided to quit when reaching 50% in the training set - but I believe that this could be greatly improved if I had more time to work on it. This is only a proof of concept but my belief is that this technique could work very well given enough time and dedication.

### Paper
[Paper about this project](/files/tempex_anton_fagerberg.pdf)

### Source code

[github.com/AntonFagerberg/Temporal-Information-Extraction](https://github.com/AntonFagerberg/Temporal-Information-Extraction)

Modified "demo system" source code:

[github.com/AntonFagerberg/Temporal-Information-Extraction-Demo](https://github.com/AntonFagerberg/Temporal-Information-Extraction-Demo)
