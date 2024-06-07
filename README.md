Clique Problem using Quantum Annealing
This repository contains a Python implementation for solving the NP-hard version of the clique problem using D-Wave's quantum annealer. The formulation is based on the Ising model and Quadratic Unconstrained Binary Optimization (QUBO) as described in Section 2.3 of the paper Ising formulations of many NP problems.

Problem Description
The clique problem is a classical NP-hard problem in graph theory. Given a graph, the objective is to find the largest clique (a subset of vertices, all adjacent to each other).

Implementation Details
The implementation uses the D-Wave quantum annealer to solve the problem by formulating it as a QUBO problem. The following steps are included:

Adjacency Matrix: Define the graph using an adjacency matrix J.
QUBO Formulation: Construct the QUBO matrix Q that represents the clique problem constraints and objective.
Quantum Annealing: Use D-Wave's EmbeddingComposite and DWaveSampler to find the solution.
Visualization: Plot the graph and highlight the largest clique.
Requirements
Python 3.x
D-Wave Ocean SDK
NumPy
NetworkX
Matplotlib
You can install the required packages using:

bash
Copy code
pip install dwave-ocean-sdk numpy networkx matplotlib
Usage
Define the Problem
Edit the adjacency matrix J and the desired clique size k in the code:

python
Copy code
# Example graph adjacency matrix J 
J = np.array([
    [0, 0, 1, 0,1,0],
    [0, 0, 1, 1,1,1],
    [1, 1, 0, 1,1,0],
    [0, 1, 1, 0,1,0],
    [1,1,1,1,0,0],
    [0,1,0,0,0,0]
])

# Desired clique size
k = 4
Run the Code
Ensure you have set your D-Wave API token and solver in the environment variables or directly in the script. Then, execute the script:

bash
Copy code
python clique_problem.py
Output
The script will output the nodes that form the largest clique and visualize the graph highlighting the clique.

Example
Given the adjacency matrix:

csharp
Copy code
J = [
    [0, 0, 1, 0, 1, 0],
    [0, 0, 1, 1, 1, 1],
    [1, 1, 0, 1, 1, 0],
    [0, 1, 1, 0, 1, 0],
    [1, 1, 1, 1, 0, 0],
    [0, 1, 0, 0, 0, 0]
]
and a desired clique size of k = 4, the script will find the largest clique and output:

less
Copy code
Nodes in the clique: [list_of_nodes]
A graph visualization will be displayed with the clique nodes highlighted in orange.

