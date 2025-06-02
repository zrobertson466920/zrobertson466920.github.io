---
layout: post
title: A Conservation Law in Attention - Why Syntax and Semantics Can't Evolve Independently
published: true
---

Something fundamental is happening during attention training that we've been missing. While studying the gradient dynamics of self-attention layers, I discovered a conservation law that puts strict constraints on how attention mechanisms can evolve during learning.

The result is striking: **the attention patterns (QK projections) and value transformations (V projections) cannot grow or shrink independently**. They're mathematically coupled by a conservation law that holds throughout training.

This has profound implications for a hypothesis that's been floating around the transformer community - that attention might specialize in syntactic structure while embeddings and MLPs handle semantic content. The conservation law suggests these two aspects of language understanding might be more intertwined than we thought.

## The Conservation Law

Here's the precise mathematical result. Consider a self-attention layer with ReLU activation and scalar output trained with square loss:

$$
A_W(X) = \text{relu}(X W_{QK} X^T) X W_V
$$

where $X \in \mathbb{R}^{n \times d_0}$ is the input sequence and $W_{QK} = W_Q W_K^T$ combines the query-key projections.

**Theorem:** Under gradient descent training with square loss $\mathcal{L}(A_W(X), y) = \frac{1}{2} \|y - A_W(X)\|^2$, the following quantity is conserved:

$$
\frac{1}{2} \Vert W_{QK}(t) \Vert_F^2 - \frac{1}{2} \Vert W_{QK}(0) \Vert_F^2 = \frac{1}{2} \Vert W_V(t) \Vert_V^2 - \frac{1}{2} \Vert W_V(0) \Vert_F^2
$$

In other words: **the change in attention pattern complexity exactly equals the change in value transformation complexity**.

## The Proof

The conservation law emerges from analyzing the gradient flow dynamics. Under gradient descent:

$$
\dot{W}_{QK} = -\eta \nabla_{W_{QK}} \mathcal{L}, \quad \dot{W}_V = -\eta \nabla_{W_V} \mathcal{L}
$$

Computing the gradients (using matrix calculus identities):

$$
\nabla_{W_{QK}} \mathcal{L} = X^T \cdot \text{diag}(\delta(t)) \cdot \text{relu}(\text{sign}(X W_{QK} X^T)) \cdot \text{diag}(X W_V) \cdot X
$$

$$
\nabla_{W_V} \mathcal{L} = X^T \cdot \text{relu}(X W_{QK} X^T) \cdot \delta(t)
$$

where $\delta(t) = A_W(X) - y$ is the prediction error.

The key insight is showing that the inner products are equal:

$$
\langle \dot{W}_{QK}, W_{QK} \rangle = \langle \dot{W}_V, W_V \rangle
$$

For the QK term:
$$
\langle \dot{W}_{QK}, W_{QK} \rangle = -\eta \cdot \text{Tr}(\mathbb{E}[\text{diag}(\delta(t)) \cdot \text{relu}(\text{sign}(X W_{QK} X^T)) \cdot \text{diag}(X W_V) \cdot X W_{QK}^T X^T])
$$

Expanding the trace:
$$
= -\eta \sum_{i,j} \mathbb{E}[\delta(t)_i \cdot \text{diag}(X W_V)_j \cdot \text{relu}(\text{sign}(X W_{QK} X^T))_{ij} \cdot (X W_{QK} X^T)_{ij}]
$$

$$
= -\eta \sum_{i,j} \mathbb{E}[\delta(t)_i \cdot \text{diag}(X W_V)_j \cdot \text{relu}(X W_{QK} X^T)_{ij}]
$$

For the V term:
$$
\langle \dot{W}_V, W_V \rangle = -\eta \cdot \text{Tr}(\mathbb{E}[X^T \text{relu}(X W_{QK} X^T) \delta(t) W_V^T])
$$

$$
= -\eta \cdot \text{Tr}(\mathbb{E}[\delta(t) (X W_V)^T \text{relu}(X W_{QK} X^T)])
$$

$$
= -\eta \sum_{i,j} \mathbb{E}[\delta(t)_i \cdot \text{diag}(X W_V)_j \cdot \text{relu}(X W_{QK} X^T)_{ij}]
$$

The expressions are identical, proving $\langle \dot{W}_{QK}, W_{QK} \rangle = \langle \dot{W}_V, W_V \rangle$.

Since $\frac{d}{dt}\frac{1}{2}\|W\|_F^2 = \langle \dot{W}, W \rangle$, integrating gives the conservation law. $\square$

## Why This Matters for Syntax vs. Semantics

This conservation law directly connects to Aryaman Arora's [intriguing hypothesis](https://twitter.com/aryaman2020/status/1591947909886464000) about functional specialization in transformers: *"It would be so interesting if it turns out attention holds the syntactic information in the model and the embeddings + MLPs hold semantic info.."*

If attention primarily captures syntactic relationships (which tokens should attend to which other tokens), while value transformations extract semantic content (what information to extract from attended tokens), then the conservation law tells us something profound:

**Syntactic structure learning and semantic extraction are fundamentally coupled during training.**

You can't learn more complex attention patterns without simultaneously learning more complex value transformations, and vice versa. The two processes are mathematically constrained to evolve together.

## The Mathematical Intuition

The coupling emerges because both gradients depend on the same underlying attention pattern activations. The gradient with respect to the QK projection involves the attention pattern multiplied by the value projections, while the gradient with respect to V involves the attention pattern multiplied by the prediction error.

This shared dependence creates the coupling: when attention patterns become more complex (higher $\|W_{QK}\|_F$), the gradients necessarily drive value transformations to become more complex as well (higher $\|W_V\|_F$).

## Implications for Transformer Architecture

This result suggests several important implications:

**1. Syntax-Semantics Coupling:** If Aryaman's hypothesis is correct, then syntactic and semantic learning are more intertwined than a clean separation would suggest. The model can't learn complex syntactic attention patterns without simultaneously developing complex semantic extraction capabilities.

**2. Initialization Constraints:** The conservation law depends on initial conditions. The relative initialization of QK vs. V projections constrains their relative complexity throughout training.

**3. Training Dynamics:** The coupling suggests that techniques for controlling attention pattern complexity (like attention regularization) will necessarily affect semantic processing capabilities.

## Limitations and Extensions

This result has several important limitations:

**Simplified Setting:** The analysis assumes ReLU activations, scalar outputs, and square loss. Real transformers use softmax attention, multiple heads, and cross-entropy loss.

**Single Layer:** The analysis is for a single attention layer. Multi-layer dynamics could be more complex.

**Norm vs. Function:** The result is about Frobenius norms, not necessarily about functional complexity or interpretability.

**Extensions:** The key question is whether similar conservation laws hold for softmax attention and more realistic loss functions. The coupling mechanism (shared dependence on attention activations) suggests this might generalize.

## Connection to Mechanistic Interpretability

The result connects to mechanistic interpretability research trying to understand what different transformer components do. If attention patterns and value transformations are coupled during training, this might explain why attention heads often perform coherent "functions" - the coupling naturally leads to coherent attention-value pairs.

This suggests interpretability research should analyze attention patterns and value transformations as coupled systems rather than independently.

## Open Questions

The conservation law opens several research directions:

1. **Empirical validation:** Do real transformers show evidence of this coupling during training?

2. **Softmax extension:** How does the conservation law change with softmax attention?

3. **Multi-head dynamics:** How do different attention heads interact under this coupling constraint?

4. **Functional implications:** Does the norm coupling translate to functional coupling between syntax and semantics?

## Conclusion

The conservation law reveals a fundamental mathematical constraint in attention training: syntactic pattern learning (QK projections) and semantic extraction (V projections) cannot evolve independently. While functional specialization might still occur, it happens within this coupling constraint.

This provides a new lens for understanding transformer training dynamics and suggests that the relationship between syntax and semantics in these models is more mathematically constrained than previously appreciated.

The coupling might be a feature, not a bug - forcing the model to develop coherent attention-value pairs that perform meaningful linguistic functions. Understanding these mathematical principles will be crucial as we build more sophisticated language models.

---

*Thanks to conversations with mechanistic interpretability researchers for inspiring this investigation.*
