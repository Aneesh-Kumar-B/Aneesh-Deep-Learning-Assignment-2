## Overview

This project explores the progression of computer vision models from basic Feedforward Networks (FFN) to Convolutional Neural Networks (CNN), efficient MobileNet architectures, and finally a multi-task object detection system.

The assignment is divided into four parts:

1. Feedforward Network (Dimensionality Crisis)
2. CNN Baseline & Transfer Learning
3. MobileNet & Efficiency
4. Object Detection (Team 4 – Medical Imaging: Blood Cell Detection)


## Part 1: Feedforward Network (FFN)

### Implementation

* A 3-layer FFN with 512 hidden units per layer
* Input: Flattened CIFAR-10 images (32×32×3 → 3072)

### Experiment: Translation Sensitivity

* A test image was shifted by 4 pixels to the right

### Results

| Metric              | Value  |
| ------------------- | ------ |
| Original Confidence | 0.7226 |
| Shifted Confidence  | 0.4542 |

### Observation

The confidence drops significantly after shifting, showing that FFNs are **not translation invariant**, as they treat pixel positions independently.


## Part 2: CNN Baseline

### Implementation

* 3 Convolution layers (3×3, stride 1)
* Global Average Pooling (GAP)

### Results

| Model      | Accuracy |
| ---------- | -------- |
| Custom CNN | 63.29%   |

### Observation

CNN performs better than FFN due to:

* Spatial feature learning
* Translation invariance


## Transfer Learning (ResNet-18)

### Implementation

* Pretrained ResNet-18
* Final layer modified for CIFAR-10

### Results

| Model     | Accuracy |
| --------- | -------- |
| ResNet-18 | 81.73%   |

### Observation

Transfer learning significantly improves performance due to:

* Pre-learned feature representations
* Faster convergence


## Part 3: MobileNet & Efficiency

### Implementation

* Replaced standard convolutions with:

  * Depthwise Convolution
  * Pointwise (1×1) Convolution

### Parameter Comparison

| Layer Type                   | Parameters |
| ---------------------------- | ---------- |
| Standard Conv (3×3, 128→256) | 294,912    |
| Depthwise Separable Conv     | 33,920     |

### Reduction

~ **88.5% fewer parameters**

### Observation

MobileNet achieves similar performance with significantly reduced computational cost.


## Part 4: Object Detection (BCCD Dataset)

### Dataset

* Blood Cell Count and Detection (BCCD)
* Images resized to 96×96
* Bounding boxes normalized to [0,1]


### Model Design

* Backbone: MobileNet-style CNN
* Output: Bounding box regression (x, y, w, h)
* Activation: Sigmoid

### Loss Function

* Mean Squared Error (MSE)


## Results

### Mean IoU

| Metric   | Value |
| -------- | ----- |
| Mean IoU | 0.188 |


## Visualization

* Red Box → Predicted Bounding Box
* Green Box → Ground Truth

### Observations

* In some cases, predicted boxes overlap well with ground truth
* In others, predictions are offset due to:

  * Multiple objects per image
  * Simplified single-object training strategy


## Final Comparison

| Model     | Accuracy / Metric | Observation                     |
| --------- | ----------------- | ------------------------------- |
| FFN       | Confidence Drop   | Sensitive to translation        |
| CNN       | 63.29%            | Learns spatial features         |
| ResNet    | 81.73%            | Best classification performance |
| MobileNet | Efficient         | Reduced parameters              |
| Detection | IoU: 0.188        | Partial localization success    |


## Limitations

* Detection model trained on single bounding box per image
* Multiple objects in dataset reduce accuracy
* Limited epochs for training


## Conclusion

* FFNs fail to generalize spatial transformations
* CNNs improve performance through spatial awareness
* Transfer learning provides significant accuracy boost
* MobileNet reduces computational cost efficiently
* Object detection works but requires more advanced handling for multiple objects
