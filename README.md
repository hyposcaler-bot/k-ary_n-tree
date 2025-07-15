# k-ary n-tree visualizer

This repo is primarily for a interactive utility for visualizing k-ary n-tree's

## how to use the visualizer

1. clone the repo

```
git clone https://github.com/hyposcaler-bot/k-ary_n-tree.git
```

use your browser to open the file k-ary_n-tree_viz.html file found in the root of the repo.

## Screenshot

![Alt text](images/viz-screenshot.png)

## What is a k-ary n-tree

In short it is a Fat-Tree

In 1997 Fabrizio Petrini and Marco Vanneschi from the Department of Computer Science at the University of Pisa in Italy published a paper entitled [k-ary n-trees: High Performance Networks for Massively Parallel Architectures](https://ieeexplore.ieee.org/document/580853)

In part Petrini and Vanneschi's paper built on Charles E. Leiserson's article from 1985 [Fat-Trees: Universal networks for hardware-efficient supercomputing](https://ieeexplore.ieee.org/abstract/document/6312192).  In the time between Leiserson paper, and Petrini/Vanneschi's paper, Fat-Trees had seen considerable use as interconnects in supercomputing and other distributed systems.   

While Leiserson's paper provides a general description of Fat-Trees and does a great deal of work to prove them as being useful as Universal networks, Petrini and Vanneschi's paper provides a formal definition for them. 

## Similarity to a Clos

in 1953 Charles Clos authored a paper entitled [A Study of Non- Blocking Switching Networks](https://ieeexplore.ieee.org/document/6770468).  In this paper Clos was primarily exploring optimal strategies for creating multi-stage networks from individual crossbar fabrics.   The strategies he came up with in would go on to be known as Clos networks.

It is frequently said that Fat-Trees are Clos, but not all Clos are Fat-Trees.  In particular a Clos that was built using the same value for m, n and r does indeed have sub-graphs that also happen to be k-ary n-trees, and k-ary n-trees are Fat-Trees

If you start with a 3-stage Clos and remove the ingress stage, the remaining egress stage, middle stage, and unused R ports (previously connected to the ingress) of the middle stage form a k-ary 2-tree.  likewise, you were to remove the egress stage the remaining ingress stage, middle stage and r ports that formerly connected to the egress stage also form a k-ary 2-tree.   

This is why people often equate Fat-Tree's with Clos. The ingress and middle stages of a Clos form a Fat-Tree as desribed by Petrini and Vanneschi as do the middle-stages and egress stage.

## Why are k-ary n-trees important to datacenter networking?

The combined work of Charles Clos, Charles Leiserson, F. Petrini, and M. Vanneschi would go on to inform the work of Mohammad Al-Fares, Alexander Loukissas, and Amin Vahdat's in their 2008 paper [A Scalable, Commodity Data Center Network Architecture](https://dl.acm.org/doi/abs/10.1145/1402958.1402967) which in turn would influence how modern datacenter networks have been built since.

## Definition of a k-ary n-tree/Fat-Tree

The following definitions were pulled from section 2 of Petrini and Vanneschi's paper and used as the foundation for the logic the visualizer uses.

### Fat-Tree Definition

*(Fat-Tree)*: A Fat-Tree is a collection of vertices connected by edges and is defined recursively as follows:

- A single vertex by itself is a Fat-Tree. This vertex is also the root of the Fat-Tree.
- If v₁, v₂, ..., vᵢ are vertices and T₁, T₂, ..., Tⱼ are Fat-Trees, with r₁, r₂, ..., rₖ as roots (j and k need not be equal), a new Fat-Tree is built by connecting with edges, in any manner, the vertices v₁, v₂, ..., vᵢ to the roots r₁, r₂, ..., rₖ. The roots of the new Fat-Tree are v₁, v₂, ..., vᵢ.

### k-ary n-tree Definition

*(k-ary n-tree)*: A k-ary n-tree is composed of two types of vertices:

### Components
- **Processing nodes**: N = kⁿ processing nodes
- **Communication switches**: nkⁿ⁻¹ switches of arity k × k

### Node and Switch Representation
- Each **processing node** is an n-tuple: (p₀, p₁, ..., pₙ₋₁) where pᵢ ∈ {0, 1, ..., k-1}
- Each **communication switch** is defined as an ordered pair: ⟨w, l⟩ where:
  - w ∈ {0, 1, ..., k-1}ⁿ⁻¹ (an (n-1)-tuple)
  - l ∈ {0, 1, ..., n-1} (the level)

### Connection Rules

#### Switch-to-Switch Connections
Two switches ⟨w₀, w₁, ..., wₙ₋₂, l⟩ and ⟨w₀', w₁', ..., wₙ₋₂', l'⟩ are connected by an edge if and only if:
- l' = l + 1
- wᵢ = wᵢ' for all i ≠ l

The edge is labeled with:
- w₀' on the level l vertex
- wₗ on the level l' vertex

#### Switch-to-Node Connections
There is an edge between the switch ⟨w₀, w₁, ..., wₙ₋₂, n-1⟩ and the processing node (p₀, p₁, ..., pₙ₋₁) if and only if:
- wᵢ = pᵢ for all i ∈ {0, 1, ..., n-2}

This edge is labeled with pₙ₋₁ on the level (n-1) switch.

## Mathematical Notation Summary

| Symbol | Meaning |
|--------|---------|
| k | Arity parameter (number of connections per switch) |
| n | Dimension parameter (tree height) |
| N = kⁿ | Total number of processing nodes |
| nkⁿ⁻¹ | Total number of k×k communication switches |
| ⟨w, l⟩ | Communication switch at level l with address w |
| (p₀, p₁, ..., pₙ₋₁) | Processing node address |
| {0, 1, ..., k-1}ⁿ | Set of all possible n-tuples with elements from {0, 1, ..., k-1} |
