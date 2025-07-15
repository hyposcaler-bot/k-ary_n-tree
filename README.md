# k-ary n-tree visualizer

This repo is primarily for a interactive utility for visualizing k-ary n-tree's

## how to use the visualizer

1. clone the repo

```
git clone https://github.com/hyposcaler-bot/k-ary_n-tree.git
```

open the file k-ary_n-tree_viz.html found in the root of the repo with your browser.

## What is a k-ary n-tree

In 1997 Fabrizio Petrini and Marco Vanneschi from the Department of Computer Science at the University of Pisa in Italy published a paper entitled [k-ary n-trees: High Performance Networks for Massively Parallel Architectures](https://ieeexplore.ieee.org/document/580853)

In part Petrini and Vanneschi's paper built on Charles E. Leiserson's article from 1985 [Fat-trees: Universal networks for hardware-efficient supercomputing](https://ieeexplore.ieee.org/abstract/document/6312192).  In the time between Leiserson paper, and Petrini/Vanneschi's paper, Fat-trees had seen considerable use as interconnects in supercomputing.   

While Leiserson's paper provides a general description of Fat-trees and does a great deal of work to prove them as being useful as Universal networks, Petrini and Vanneschi's paper provides a formal definition for them. 

## Similarity to a Clos

A k-ary n-tree, as formally defined by Petrini and Vanneschi, can be thought of as forming one half of a complete Clos. 

A k-ary n-tree inherently has unused upward ports at its root. To complete the Clos topology, you reflect the entire tree structure across these root switches, creating a symmetric bidirectional fabric.

As an example to get from a 2-ary 2-tree to a 3 stage Clos

- The original k-ary n-tree forms the ingress & middle stages
- Root switches become the middle stage of the Clos
- The mirrored k-ary n-tree forms as the middle and egress stages

This creates the characteristic "folded" Clos topology where packets can traverse: Host → Ingress Tree → Middle Stage → Egress Tree → Host.  

This is why people often equate Fat-Tree's with Clos. The ingrees and middle stage of a Clos form a fat-tree as desribed by Petrini and Vanneschi as does the middle-stage and egress stage.

Not all Clos are fat-tree, but a Clos where m = n = r, can be thought of as a Fat-tree.

## Why are they important to datacenter networking?

The combined work of Charles Clos, Charles Lieserson, F. Petrini, M. Vanneschi would go on to inform the work of Mohammad Al-Fares, Alexander Loukissas, and Amin Vahdat's in thier 2008 paper [A Scalable, Commodity Data Center Network Architecture](https://cseweb.ucsd.edu/~vahdat/papers/sigcomm08.pdf) which in turn would lay the foundation upon which most modern datacenter networks have been built since.

## Definitions

The following are taken from Petrini and Vanneschi's paper [k-ary n-trees: High Performance Networks for Massively Parallel Architectures](https://ieeexplore.ieee.org/document/580853)


## Vizualizing

The following definitions were pulled from Petrini and Vanneschi's paper and used as the foundation for the visualizer

### Fat-tree Definition

*(Fat-tree)*: A fat-tree is a collection of vertices connected by edges and is defined recursively as follows:

- A single vertex by itself is a fat-tree. This vertex is also the root of the fat-tree.
- If v₁, v₂, ..., vᵢ are vertices and T₁, T₂, ..., Tⱼ are fat-trees, with r₁, r₂, ..., rₖ as roots (j and k need not be equal), a new fat-tree is built by connecting with edges, in any manner, the vertices v₁, v₂, ..., vᵢ to the roots r₁, r₂, ..., rₖ. The roots of the new fat-tree are v₁, v₂, ..., vᵢ.

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