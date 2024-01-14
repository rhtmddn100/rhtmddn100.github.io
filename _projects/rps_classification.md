---
layout: page
title: Rock, Paper, Scissors
description: Using data augmentation to utilize limited data for rock, paper, scissors classifcation with CNN model.
img:
importance: 3
category: Course Work
related_publications: false
---

*This project was the final project for Software and Programming for Data Science class at Seoul National University.*

## Problem Setting

The goal of the project was to train a CNN model to classify real-world rock, paper, scissors images. However, there was a catch: we were only allowed to use standardized images of rock, paper, and scissors with white background and equal orientation, while the test set contained real world bckground with diverse orientations. Use of other datasets was not allowed.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/1.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/3.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/5.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Example of rock, paper, and scissors images in the training dataset.
</div>
