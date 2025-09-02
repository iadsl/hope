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

## [ContiFormer: Continuous-Time Transformer for Irregular Time Series Modeling](https://openreview.net/pdf?id=YJDz4F2AZu) ([Code](https://github.com/microsoft/SeqML/tree/main/ContiFormer)) ([Summary](https://seqml.github.io/contiformer/))

- **Main contributions:**
  - Incorporate a continuous-time mechanism into attention calculation in the Transformer; This captures the continuity of the underlying system of the irregularly sampled time-series data.
  - Proposes a reparameterization technique that allows for the execution of the continuous-time attention in different time ranges in parallel.
  - Provides a general framework for other Transformer variants as a special case of ContiFormer.
  - Outperforms existing models in time-series interpolation, classification, and prediction.

- **How?**
  - takes as input an irregular time series X and the sampled time ω and outputs a latent continuous trajectory that captures the dynamic change of the underlying system.

### Continuous-Time Multi-Head Attention Mechanism

- Transform input irregular time series $ X $ into: $ Q = [Q_1; Q_2; \ldots; Q_N]  \text{(queries)}, K = [K_1; K_2; \ldots; K_N] \quad \text{(keys)}, V [V_1; V_2; \ldots; V_N] \quad \text{(values)} $
- Use ODEs to define **latent trajectories** for each observation.
- Define keys and values as:

$$
k_i(t_i) = K_i, \qquad
k_i(t) = k_i(t_i) + \int_{t_i}^{t} f\left( \tau, k_i(\tau); \theta_k \right) d\tau \tag{1}
$$

$$
v_i(t_i) = V_i, \qquad
v_i(t) = v_i(t_i) + \int_{t_i}^{t} f\left( \tau, v_i(\tau); \theta_v \right) d\tau
$$

  where $ t \in [t_1, t_N] $, and $ k_i(\cdot), v_i(\cdot) \in \mathbb{R}^d $ represent ordinary differential equations (ODEs) for the $ i $-th observation with parameters $ \theta_k $ and $ \theta_v $. The function $ f(\cdot): \mathbb{R}^{d+1} \rightarrow \mathbb{R}^d $ controls the change in dynamics.

- Define a closed-form continuous-time interpolation function with knots at $ t_1, \ldots, t_N $ is used, we define: $q(t_i) = Q_i$ as an approximation of the underlying process.

- Scaled Dot Product

$$
\alpha_i(t) = \frac{1}{t - t_i} \int_{t_i}^{t} q(\tau) \cdot k_i(\tau)^\top d\tau \tag{3}
$$

- Expected Value

$$
\hat{v}_i(t) = \mathbb{E}_{\tau \sim [t_i, t]}[v_i(\tau)] = \frac{1}{t - t_i} \int_{t_i}^{t} v_i(\tau) \, d\tau \tag{5}
$$

#### Multi-Head Attention

Given the predefined queries, keys, and values in continuous-time space, the continuous-time attention at query time $ t $ is:

$$
\text{CT-ATTN}(Q, K, V, \omega)(t) = \sum_{i=1}^{N} \hat{\alpha}_i(t) \cdot \hat{v}_i(t)
$$

Where:

$$
\hat{\alpha}_i(t) = \frac{\exp\left( \alpha_i(t) / \sqrt{d_k} \right)}{\sum_{j=1}^{N} \exp\left( \alpha_j(t) / \sqrt{d_k} \right)}
$$

To incorporate multiple heads:

$$
\text{CT-MHA}(Q, K, V, \omega)(t) = \text{Concat}(\text{head}^{(1)}(t), \ldots, \text{head}^{(H)}(t)) W^O \tag{7}
$$

Each head is defined as:

$$
\text{head}^{(h)}(t) = \text{CT-ATTN}(Q W_Q^{(h)}, K W_K^{(h)}, V W_V^{(h)}, \omega)(t)
$$

Where $ W^O, W_Q^{(h)}, W_K^{(h)}, W_V^{(h)} $ are learnable projection matrices, and $ h \in [1, H] $ is the head index.

- Continuous-Time Transformer Layer

$$
\tilde{z}^{l}(t) = \text{LN}\left( \text{CT-MHA}(X^l, X^l, X^l, \omega^l)(t) + x^l(t) \right) \tag{8}
$$

$$
z^l(t) = \text{LN}\left( \text{FFN}(\tilde{z}^l(t)) + \tilde{z}^l(t) \right)
$$

Where:

- $ z^l(t) $ is the output from the $ l $-th ContiFormer layer at time $ t $
- $ x^l(t) $ is a continuous interpolation of the discrete input $ X^l $

### Sampling Process

To stack ContiFormer layers:

- Reference time points are chosen for each layer's output
- These points can either be input timestamps or task-specific time points
- They are used to discretize the continuous outputs from each layer

### Complexity Analysis

To approximate attention integrals efficiently:

- Reparameterize the time domain to a fixed interval $[-1, 1]$
- Apply Gauss-Legendre quadrature for fast numerical integration

### Experiment Setup

- Use natural cubic splines to interpolate the query function into continuous time

- Tasks:
  - Interpolation and extrapolation of time series
  - Irregular time series classification
  - Event prediction (Marked Temporal Point Processes)
  - Regular time series forecasting

#### Modeling a Continuous-Time Function

- 300 two-dimensional spirals were generated
- Each spiral was sampled at 150 evenly spaced time points
- 50 irregular time points were randomly sampled per spiral
- ContiFormer significantly outperformed both Transformer and Latent ODE on this task

#### Irregular Time Series Classification

- 20 datasets from the UEA Time Series Classification Archive were used
- 30%, 50%, and 70% of observations were randomly dropped to simulate irregularity
- ContiFormer outperformed all baselines on all three stages

#### Predicting Irregular Event Sequences (MTPP)

- Synthetic Dataset
  - 10 event types, each with 3 properties
  - Predict the occurrence time of the next event and the type of the next event

- Neonate
  - Predict when the next seizure will occur

- Traffic (PeMS)
  - Predict when a traffic spike or drop will occur
  - Predict whether the change is upward or downward

- MIMIC
  - Likely task: predict the time and type of the next clinical event

- BookOrder
  - Predict the time of the next stock trade (buy/sell)

- StackOverflow
  - Predict the type of badge a user will receive and when

Dataset link: [Google Drive](https://drive.google.com/drive/folders/0BwqmV0EcoUc8UklIR1BKV25YR1U?resourcekey=0-OrlU87jyc1m-dVMmY5aC4w)

#### Regular Time Series Forecasting

- Datasets: ETT, Exchange, Weather, and ILI
- ContiFormer performs competitively with other state-of-the-art models on regularly sampled time series as well