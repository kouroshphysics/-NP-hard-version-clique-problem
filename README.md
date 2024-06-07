# Quantum Clique Detection using D-Wave

## Overview

This repository contains a Python script to solve the NP-hard clique problem using quantum annealing on the D-Wave quantum computer. The script formulates the problem as a Quadratic Unconstrained Binary Optimization (QUBO) problem and uses D-Wave's quantum annealer to find the solution. The implementation is based on Section 2.3 of the paper "Ising formulations of many NP problems".

## Problem Description

### The Clique Problem

Given an undirected graph `G = (V, E)` and an integer `k`, the goal is to determine if there is a subset of `k` vertices forming a complete subgraph (clique) in `G`.

## QUBO Formulation

The script constructs the QUBO matrix representing the clique problem and solves it using D-Wave's quantum annealer.

### Hamiltonian Components

**Hamiltonian \( H_A \):**

\[ H_A = A \left(1 - \sum_{i=2}^{N} y_i\right)^2 + A \left(\sum_{i=2}^{N} i y_i - \sum_{v} x_v\right)^2 \]

where \( y_i \) are auxiliary binary variables and \( x_v \) are binary variables indicating vertex inclusion.

**Hamiltonian \( H_B \):**

\[ H_B = \frac{B}{2} \left( \sum_{i=2}^{N} i y_i \right) \left( -1 + \sum_{i=2}^{N} i y_i \right) - \sum_{(u,v) \in E} x_u x_v \]

where \( (u, v) \in E \) represents the edges in the graph.

**Hamiltonian \( H_C \):**

\[ H_C = -C \sum_{v} x_v \]

## Installation

1. **Install Python 3.8+**: Download and install from the [official website](https://www.python.org).
2. **Clone this repository**:
    ```bash
    git clone https://github.com/yourusername/quantum-clique-detection.git
    cd quantum-clique-detection
    ```
3. **Install required packages**:
    ```bash
    pip install dwave-ocean-sdk numpy networkx matplotlib
    ```

## Usage

1. **Set your D-Wave API token and solver**: Replace `token` and `solver` with your D-Wave API token and solver name in the script.
2. **Run the script**: Execute the provided Python script to find and visualize the clique.

```python
import numpy as np
import networkx as nx
import matplotlib.pyplot as plt
from dwave.system import DWaveSampler, EmbeddingComposite
import dimod

# Example graph adjacency matrix J
J = np.array([
    [0, 0, 1, 0, 1, 0],
    [0, 0, 1, 1, 1, 1],
    [1, 1, 0, 1, 1, 0],
    [0, 1, 1, 0, 1, 0],
    [1, 1, 1, 1, 0, 0],
    [0, 1, 0, 0, 0, 0]
])

# Desired clique size
k = 4
A = 10  # Scaling constant for interaction term
B = 1   # Scaling constant for constraint term

# Number of nodes
n = len(J)

edges = [(i, j) for i in range(n) for j in range(i + 1, n) if J[i][j] == 1]
C = 1
Q = {}
# H-A
for i in range(2, n + 1):
    Q[f'y_{i}', f'y_{i}'] = A * (-1 + i**2)
    for j in range(i + 1, n + 1):
        Q[(f'y_{i}', f'y_{j}')] = 2 * A * (1 + i * j)
for t in range(n):
    Q[(f'x_{t}', f'x_{t}')] = A
    for m in range(t + 1, n):
        Q[(f'x_{t}', f'x_{m}')] = A * 2
    for i in range(2, n + 1):
        Q[(f'y_{i}', f'x_{t}')] = - (A * i)
# H-B
for m, t in edges:
    Q[(f'x_{m}', f'x_{t}')] = -2 * B
for i in range(2, n + 1):
    Q[(f'y_{i}', f'y_{i}')] += B * (i**2 - i) / 2
    for j in range(i + 1, n + 1):
        Q[(f'y_{i}', f'y_{j}')] += B * (j * i)
# H-C
for t in range(n):
    Q[(f'x_{t}', f'x_{t}')] = -C

sampler = EmbeddingComposite(DWaveSampler(token=token, solver=solver))

# Solve the problem
response = sampler.sample_qubo(Q, num_reads=200)

# Get the best solution
best_solution = response.first.sample

# Extract the solution nodes
clique_nodes = [i for i in best_solution if best_solution[i] == 1]
print("Nodes in the clique:", clique_nodes)
largest_clique = [int(key.split('_')[1]) for key, value in best_solution.items() if key.startswith('x') and value == 1]

G = nx.Graph()

# Add nodes
for i in range(n):
    G.add_node(i)

# Add edges
for i in range(n):
    for j in range(i + 1, n):
        if J[i][j] == 1:
            G.add_edge(i, j)

# Set colors: orange for clique nodes, blue for others
node_colors = ['orange' if node in largest_clique else 'blue' for node in G.nodes()]

# Set sizes: larger size for clique nodes
node_sizes = [400 if node in largest_clique else 200 for node in G.nodes()]

# Draw the graph
pos = nx.spring_layout(G)
nx.draw(G, pos, with_labels=True, node_color=node_colors, node_size=node_sizes, edge_color='gray', font_weight='bold')

# Highlight the largest clique with a different color for edges
clique_edges = [(i, j) for i in largest_clique for j in largest_clique if i < j and J[i][j] == 1]
nx.draw_networkx_edges(G, pos, edgelist=clique_edges, edge_color='orange', width=2)

plt.title("Graph with Largest Clique Highlighted with Orange")
plt.show()

