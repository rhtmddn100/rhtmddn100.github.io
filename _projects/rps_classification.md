---
layout: page
title: Rock, Paper, Scissors
description: Using data augmentation to utilize limited data for rock, paper, scissors classifcation with CNN model.
img:
importance: 3
category: Course Work
related_publications: false
---

*This project was the final project for Software and Programming for Data Science (Fall 2021) class at Seoul National University.*

## Problem Setting

The goal of the project was to train a CNN model to classify real-world rock, paper, scissors images. However, there was a catch: we were only allowed to use standardized images of rock, paper, and scissors with white background and equal orientation, while the test set contained real world bckground with diverse orientations. Use of other datasets was not allowed.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/rps/rock_data.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/rps/paper_data.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/rps/scissors_data.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Example of rock, paper, and scissors images in the training dataset.
</div>

## Data Augmentation

In order to overcome the limitation in data, I used several data augmentation strategies to mimic the real-world rock, paper, and scissors images.
The differences in the given train dataset and the real world test data set were as follows:
- Different background (white vs. real world)
- Position (upright vs. rotated)
- Orientation (left hand vs. mixed)
- Hand shape ()