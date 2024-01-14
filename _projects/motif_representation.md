---
layout: page
title: Motif Representation
description: Pretraining a GNN model using molecule's motifs and reaction data for downstream applications.
img:
importance: 1
category: Research
related_publications: true
---

## Motivation

[Chemical-Reaction-Aware Molecule Representation Learning (MolR)](https://openreview.net/forum?id=6sh3pIzKS-) was published in ICLR 2022, utilizing reaction data (USPTO-479k) to pretrain molecular GNN.
Their proposal was to use chemical equivalence between reactants and products in a chemical reaction. In representation learning, this proposal means that the sum of embeddings of the reactants are enforced to be equal to that of embeddings of products.

$$\sum_{r\in R}h_r = \sum_{p \in P}h_p$$

More details on this paper is given in my blog [post](https://rhtmddn100.github.io/blog/2023/molr).

Such pretraining of GNN using reaction data leads to good performances in the downstream tasks. So, similar to this idea, we can also enforce equivalence that exists inside a single molecule. This equivalence, named motif-level equivalnce, makes the sum of embeddings of the motifs inside a molecule equal to the molecule's embedding. For molecule $$i$$ and the set of motifs that make up the molecule, $$M_i$$, we enforce the following equation.

$$\sum_{m \in M_i} h_m = h_i$$

Enforcing such motif-level equivalence is hypothesized to have several useful outcomes:
- It forces the representation of the whole molecule as a linear combination of chemical motifs. Thus, each chemical motif, which will inevitably shared among many different molecules, will have to accurately represent the role it plays across multiple molecules.
- The resulting molecule embedding will have good understanding of the fundamental blocks comprising it, which will be useful for downstream tasks such as retrosynthesis prediction.
- As we also train the embeddings of the motifs, these embeddings can be the starting point for developing a model for de novo molecule generation using motifs.

## Method

### Overall Training Objective


### Motif Generation



Every project has a beautiful feature showcase page.
It's easy to include images in a flexible 3-column grid format.
Make your photos 1/3, 2/3, or full width.

To give your project a background in the portfolio page, just add the img tag to the front matter like so:

    ---
    layout: page
    title: project
    description: a project with a background image
    img: /assets/img/12.jpg
    ---

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
    Caption photos easily. On the left, a road goes through a tunnel. Middle, leaves artistically fall in a hipster photoshoot. Right, in another hipster photoshoot, a lumberjack grasps a handful of pine needles.
</div>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/5.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    This image can also have a caption. It's like magic.
</div>

You can also put regular text between your rows of images, even citations {% cite einstein1950meaning %}.
Say you wanted to write a bit about your project before you posted the rest of the images.
You describe how you toiled, sweated, _bled_ for your project, and then... you reveal its glory in the next row of images.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/6.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/11.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    You can also have artistically styled 2/3 + 1/3 images, like these.
</div>

The code is simple.
Just wrap your images with `<div class="col-sm">` and place them inside `<div class="row">` (read more about the <a href="https://getbootstrap.com/docs/4.4/layout/grid/">Bootstrap Grid</a> system).
To make images responsive, add `img-fluid` class to each; for rounded corners and shadows use `rounded` and `z-depth-1` classes.
Here's the code for the last row of images above:

{% raw %}

```html
<div class="row justify-content-sm-center">
  <div class="col-sm-8 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/6.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-sm-4 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/11.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
```

{% endraw %}
