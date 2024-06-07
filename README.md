NP-hard Version of the Clique Problem Using Quantum Annealing
Overview
This repository contains a Python implementation for solving the Clique Decision Problem and finding the largest clique in a given graph using quantum annealing on the D-Wave quantum computer. A clique in a graph is a subset of vertices such that every two distinct vertices are adjacent. Finding the largest clique is an NP-hard problem, involving identifying the maximum complete subgraph within a given graph.

Problem Description
The Clique Decision Problem
The Clique Decision Problem is a fundamental problem in graph theory and computer science. Given an undirected graph 
𝐺
=
(
𝑉
,
𝐸
)
G=(V,E) and an integer 
𝑘
k, the problem is to determine whether there exists a subset of 
𝑘
k vertices that form a complete subgraph (clique) in 
𝐺
G.

Largest Clique Problem
A clique in an undirected graph 
𝐺
=
(
𝑉
,
𝐸
)
G=(V,E) is a subset of vertices such that every two distinct vertices are connected by an edge. The largest clique is the clique of maximum size in the graph.

Quantum Annealing Approach
Quantum annealing is a method used to find the global minimum of a given objective function over a set of candidate solutions, particularly useful for solving combinatorial optimization problems like the largest clique problem.

Formulation as a QUBO
The problem can be formulated as a Quadratic Unconstrained Binary Optimization (QUBO) problem, expressed through a Hamiltonian composed of three parts:

Hamiltonian 
𝐻
𝐴
H 
A
​
 : Ensures the sum of the binary variables reflects the size constraints of the clique.
Hamiltonian 
𝐻
𝐵
H 
B
​
 : Ensures the selected vertices form a complete subgraph.
Hamiltonian 
𝐻
𝐶
H 
C
​
 : Maximizes the number of vertices included in the clique.
These Hamiltonians are combined to form the QUBO matrix 
𝑄
Q, where the goal is to minimize the energy to find the largest clique.

Hamiltonian Equations
Hamiltonian 
𝐻
𝐴
H 
A
​
 :

𝐻
𝐴
=
𝐴
(
1
−
∑
𝑖
=
2
𝑁
𝑦
𝑖
)
2
+
𝐴
(
∑
𝑖
=
2
𝑁
𝑖
𝑦
𝑖
−
∑
𝑣
𝑥
𝑣
)
2
H 
A
​
 =A(1− 
i=2
∑
N
​
 y 
i
​
 ) 
2
 +A( 
i=2
∑
N
​
 iy 
i
​
 − 
v
∑
​
 x 
v
​
 ) 
2
 
where 
𝑦
𝑖
y 
i
​
  are auxiliary binary variables representing the possible sizes of the clique, and 
𝑥
𝑣
x 
v
​
  are binary variables indicating whether vertex 
𝑣
v is included in the clique.

Hamiltonian 
𝐻
𝐵
H 
B
​
 :

𝐻
𝐵
=
𝐵
[
1
2
(
∑
𝑖
=
2
𝑁
𝑖
𝑦
𝑖
)
(
−
1
+
∑
𝑖
=
2
𝑁
𝑖
𝑦
𝑖
)
−
∑
(
𝑢
,
𝑣
)
∈
𝐸
𝑥
𝑢
𝑥
𝑣
]
H 
B
​
 =B 
​
  
2
1
​
 ( 
i=2
∑
N
​
 iy 
i
​
 )(−1+ 
i=2
∑
N
​
 iy 
i
​
 )− 
(u,v)∈E
∑
​
 x 
u
​
 x 
v
​
  
​
 
where 
(
𝑢
,
𝑣
)
∈
𝐸
(u,v)∈E represents the edges in the graph.

Hamiltonian 
𝐻
𝐶
H 
C
​
 :

𝐻
𝐶
=
−
𝐶
∑
𝑣
𝑥
𝑣
H 
C
​
 =−C 
v
∑
​
 x 
v
​
 
Files
clique_problem.py: Contains the implementation for solving the NP-hard version of the clique problem using quantum annealing.
graph_plot.py: Contains the implementation for visualizing the graph and the largest clique found.
