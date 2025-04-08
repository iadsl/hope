---
layout: page
permalink: /irregular-time-series-papers/
title: Irregular Time-Series Papers
# description: Papers that includes techniques for handling irregular time-series data. 
nav: false
nav_order: 3
---

## [Time Series as Images: Vision Transformer for Irregularly Sampled Time Series](https://arxiv.org/pdf/2303.12799) ([Code](https://github.com/Leezekun/ViTST))

There are numerous algorithms (LSTM, TCN, Transformer) for time series modeling. (mainly for regular intervals and fixed-size numerical inputs)
Models have been developed for irregular time series but they are highly specialized, requires substantial prior knowledge and efforts in model architecture selection and algorithm design

- **Main idea:**
  - Transform irregularly sampled multivariate time series into line graphs
  - organize them into a RGB image format
  - Fine-tune a pre-trained vision transformer for classification using those images

- Has superior performance over SoTA methods specifically designed for irregularly sampled time series.
- Has strong robustness to missing observations. Surpasses the previous leading solution by 42% in absolute F1 score when half of the variables are masked in the test set.

- **Approach**
  - Transform multivariate time series into a concatenated line graph image
  - Use a pre-trained vision transformer as an image classifier.

Data = $\\{( S_i, y_i  )|i = 1, · · · , N \\}$, $N$ is the number of classes, $S_i$ is a Data sample (Can contain at most D types of observations, some may have no observation)
$y_i ∈ \\{1, · · · , C\\},$ $C$ is no. of classes

- **Image creation**
  - Plot the line graph for each variable (The scales of each line graph $g_{i,d}$ are kept the same across different time series $S_i$)
  - Used grid size of $l \times l$ or $l \times (l+1)$ based on the maximum number of variables present
  - Any grid not occupied by a line graph is kept empty
  - They studied the effects of different marker , line thickness, line types in the graph, the order of variables in the graph, and the colors used to represent lines for different variables.

- **Vision Transformer for Time series modeling**
  - Vision Transformer (ViT) - Originally adopted from NLP
  - Input image is split into fixed-sized patches, and each patch is linearly embedded and augmented with position embedding.
  - The paper used Swin Transformer to reduce computational complexity.
  - They mention that the Swin transformer could transfer knowledge obtained from pre-training on natural images to their synthetic time series line graph, as the performance was drastically low without pre-training.

- **Tasks:**
  - Sepsis prediction on P19 dataset
  - Mortality Prediction on P12 dataset 
  - physical activity classification on PAM dataset.

Static features like demographic information, weight, were first converted to natural language sentences and then encoded using a text encoder( RoBERTa-base), and then concatenated with the image embeddings obtained from the vision transformer to perform classification.

---
