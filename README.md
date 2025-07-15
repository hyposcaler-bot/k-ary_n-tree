# k-ary n-tree visualizer

## What is a k-ary n-tree

In 1997 F. Petrini and M. Vanneschi from Department of Computer Science, University of Pisa, Pisa, Italy published a paper entitled [k-ary n-trees: High Performance Networks for Massively Parallel Architectures](https://ieeexplore.ieee.org/document/580853)

In part Petrini and Vanneschi's paper builds on Charles E. Lierseron's [Fat-trees: Universal networks for hardware-efficient supercomputing](https://ieeexplore.ieee.org/abstract/document/6312192)

## Definitions

The following are taken from Petrini and M. Vanneschi's paper [k-ary n-trees: High Performance Networks for Massively Parallel Architectures](https://ieeexplore.ieee.org/document/580853)

### Fat-tree

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

## Properties

### Structural Properties
- k-ary n-trees are fat-trees according to Definition 2.1
- Level 0 switches ⟨w, 0⟩ are the roots of k-ary n-trees whose subtrees are (n-1)-dimensional k-ary n-trees
- The labeling scheme makes k-ary n-trees **delta networks**: any path from a level 0 switch to a given node (p₀, p₁, ..., pₙ₋₁) traverses the same sequence of edge labels (p₀, p₁, ..., pₙ₋₁)

### Routing Properties
- **Minimal routing** between any pair of nodes can be accomplished by:
  1. **Ascending phase**: Send message up to one of the nearest common ancestors
  2. **Descending phase**: Send message down the unique path to destination
- There exists a unique minimal path between any pair of processing nodes

### Special Cases
- A k-ary n-tree of dimension n = 0 is composed of a single processing node
- k-ary n-trees generalize the topology of k-ary n-butterflies for the internal switch structure

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