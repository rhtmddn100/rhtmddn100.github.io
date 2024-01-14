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

