# Problem 1

[Simulation](Simulatio_Circuits.HTML)
## Algorithm for Equivalent Resistance Using Graph Theory

The algorithm models an electrical circuit as an undirected weighted graph:
- **Nodes**: Represent junctions or terminals in the circuit.
- **Edges**: Represent resistors, with weights equal to resistance values (in ohms, Ω).
- **Goal**: Compute the equivalent resistance $R_{eq}$ between two specified nodes (source $S$ and sink $T$) by iteratively simplifying the graph through series and parallel reductions until a single edge connects $S$ and $T$.

### Key Formulas
1. **Series Reduction**:
   - For resistors $R_1, R_2, \ldots, R_n$ connected in series (forming a chain through nodes of degree 2):
     $$
     R_{eq} = R_1 + R_2 + \cdots + R_n
     $$
   - Example: Two resistors $R_1$ and $R_2$ in series yield $R_{eq} = R_1 + R_2$.

2. **Parallel Reduction**:
   - For resistors $R_1, R_2, \ldots, R_n$ connected in parallel (multiple edges between the same two nodes):
     $$
     \frac{1}{R_{eq}} = \frac{1}{R_1} + \frac{1}{R_2} + \cdots + \frac{1}{R_n}
     $$
   - For two resistors: 
     $$
     R_{eq} = \frac{R_1 \cdot R_2}{R_1 + R_2}
     $$
   - Example: Two resistors $R_1 = 4Ω$ and $R_2 = 6Ω$ in parallel yield:
     $$
     R_{eq} = \frac{4 \cdot 6}{4 + 6} = \frac{24}{10} = 2.4Ω
     $$

### Pseudocode
The algorithm iteratively applies series and parallel reductions until the graph is reduced to a single edge between $S$ and $T$.

```pseudocode
Function ComputeEquivalentResistance(G, S, T):
    // Input: Graph G (nodes, edges with resistance weights), source node S, sink node T
    // Output: Equivalent resistance R_eq between S and T

    While G has more than two nodes or more than one edge between S and T:
        // Step 1: Series Reduction
        For each node N in G (excluding S and T):
            If degree(N) == 2:
                Let A, B be neighbors of N
                Let R1 = resistance of edge (A, N)
                Let R2 = resistance of edge (N, B)
                R_eq = R1 + R2  // Series formula
                Remove node N and edges (A, N), (N, B)
                Add edge (A, B) with resistance R_eq

        // Step 2: Parallel Reduction
        For each pair of nodes (U, V) in G:
            If multiple edges exist between U and V:
                Let R1, R2, ..., Rk be resistances of parallel edges
                R_eq = 1 / (1/R1 + 1/R2 + ... + 1/Rk)  // Parallel formula
                Remove all edges between U and V
                Add edge (U, V) with resistance R_eq

        // Step 3: Check for Progress
        If no reductions were made:
            Break  // Non-series-parallel graphs may require advanced methods

    // Final Step
    If only one edge exists between S and T:
        Return resistance of edge (S, T)
    Else:
        Return error (graph not reducible)

End Function
```

### How It Handles Nested Combinations
- **Iterative Simplification**: The algorithm alternates between series and parallel reductions, processing nested configurations (e.g., a parallel branch containing series resistors) by reducing substructures step-by-step.
- **Graph Traversal**: Uses depth-first search (DFS) or breadth-first search (BFS) to identify nodes with degree 2 (series) or multiple edges between node pairs (parallel).
- **Order Independence**: For series-parallel graphs, the final $R_{eq}$ is independent of the reduction order, ensuring correct handling of nested structures.
- **Limitations**: Assumes series-parallel graphs. Non-series-parallel graphs (e.g., Wheatstone bridge) require extensions like delta-star transformations.

### Example Applications
Below are three example circuits to demonstrate the algorithm’s application, including the graph representation, reduction steps, and final $R_{eq}$.

#### Example 1: Simple Series and Parallel Combination
**Circuit**: Two resistors in series ($R_1 = 2Ω$, $R_2 = 3Ω$) connected in parallel with a third resistor ($R_3 = 5Ω$) between nodes $S$ and $T$.

**Graph**:
- Nodes: $S, A, T$
- Edges: $(S, A, 2Ω), (A, T, 3Ω), (S, T, 5Ω)$

**Reduction Steps**:
1. **Series Reduction**:
   - Node $A$ has degree 2, with edges $(S, A, 2Ω)$ and $(A, T, 3Ω)$.
   - Apply series formula:
   
    $R_{eq} = 2 + 3 = 5Ω$.
   - Remove node $A$ and edges; add edge $(S, T, 5Ω)$.
   - Updated graph: $(S, T, 5Ω), (S, T, 5Ω)$.
2. **Parallel Reduction**:
   - Two edges between $S$ and $T$ with $5Ω$ each.
   - Apply parallel formula:
   
    $R_{eq} = \frac{5 \cdot 5}{5 + 5} = \frac{25}{10} = 2.5Ω$.
   - Replace with edge $(S, T, 2.5Ω)$.

**Result**: $R_{eq} = 2.5Ω$

#### Example 2: Nested Configuration
**Circuit**: Two resistors in parallel ($R_1 = 4Ω$, $R_2 = 6Ω$) in series with a third resistor ($R_3 = 2Ω$) between $S$ and $T$.

**Graph**:
- Nodes: $S, A, T$
- Edges: $(S, A, 4Ω), (S, A, 6Ω), (A, T, 2Ω)$

**Reduction Steps**:
1. **Parallel Reduction**:
   - Edges $(S, A, 4Ω)$ and $(S, A, 6Ω)$ are parallel.
   - Apply parallel formula:
   
    $R_{eq} = \frac{4 \cdot 6}{4 + 6} = \frac{24}{10} = 2.4Ω$.
   - Replace with edge $(S, A, 2.4Ω)$.
   - Updated graph: $(S, A, 2.4Ω), (A, T, 2Ω)$.
2. **Series Reduction**:
   - Node $A$ has degree 2, with edges $(S, A, 2.4Ω)$ and $(A, T, 2Ω)$.
   - Apply series formula:
   
    $R_{eq} = 2.4 + 2 = 4.4Ω$.
   - Replace with edge $(S, T, 4.4Ω)$.

**Result**: $R_{eq} = 4.4Ω$

#### Example 3: Complex Graph with Multiple Cycles
**Circuit**: A square circuit with resistors on each edge ($R_1 = 1Ω$, $R_2 = 2Ω$, $R_3 = 3Ω$, $R_4 = 4Ω$) and a diagonal resistor ($R_5 = 5Ω$) between opposite corners $S$ and $T$.

**Graph**:
- Nodes: $S, A, B, T$
- Edges: $(S, A, 1Ω), (A, T, 2Ω), (S, B, 3Ω), (B, T, 4Ω), (S, T, 5Ω)$

**Reduction Steps**:
1. **Series Reduction (Path S-A-T)**:
   - Edges $(S, A, 1Ω)$ and $(A, T, 2Ω)$ form a series path through $A$.
   - Apply series formula:
   
    $R_{eq} = 1 + 2 = 3Ω$.
   - Replace with edge $(S, T, 3Ω)$.
   - Updated graph: $(S, T, 3Ω), (S, B, 3Ω), (B, T, 4Ω), (S, T, 5Ω)$.
2. **Series Reduction (Path S-B-T)**:
   - Edges $(S, B, 3Ω)$ and $(B, T, 4Ω)$ form a series path through $B$.
   - Apply series formula: 
   
   $R_{eq} = 3 + 4 = 7Ω$.
   - Replace with edge $(S, T, 7Ω)$.
   - Updated graph: $(S, T, 3Ω), (S, T, 7Ω), (S, T, 5Ω)$.
3. **Parallel Reduction**:
   - Three edges between $S$ and $T$: $3Ω, 7Ω, 5Ω$.
   - Apply parallel formula:
     $$
     \frac{1}{R_{eq}} = \frac{1}{3} + \frac{1}{7} + \frac{1}{5} = \frac{35 + 15 + 21}{105} = \frac{71}{105}
     $$
     $$
     R_{eq} = \frac{105}{71} \approx 1.48Ω
     $$
   - Replace with edge $(S, T, \approx 1.48Ω)$.

**Result**: $R_{eq} \approx 1.48Ω$

### Efficiency Analysis
- **Time Complexity**:
  - **Series Reduction**: Checking nodes for degree 2 is $O(V)$ per iteration. Each reduction removes a node, so at most $V$ iterations.
  - **Parallel Reduction**: Checking for multiple edges is $O(E)$ per iteration. Edge updates are constant with adjacency lists.
  - **Total**: $O(V + E)$ per iteration, with up to $V$ iterations, yielding $O(V \cdot (V + E))$. For sparse graphs ($E \approx V$), this is $O(V^2)$.


- **Space Complexity**: $O(V + E)$ for the graph (adjacency list) plus $O(V)$ for traversal stack/queue.

- **Practical Notes**: Efficient for small to medium circuits. Large graphs with many iterations may benefit from optimizations.

### Potential Improvements
1. **Optimized Detection**: Use a priority queue to prioritize low-degree nodes or parallel edges, reducing traversal overhead.
2. **Advanced Transformations**: Add delta-star transformations for non-series-parallel graphs:
   - Delta to star conversion for a triangle with resistances $R_a, R_b, R_c$:
     $$
     R_1 = \frac{R_b \cdot R_c}{R_a + R_b + R_c}, \quad R_2 = \frac{R_a \cdot R_c}{R_a + R_b + R_c}, \quad R_3 = \frac{R_a \cdot R_b}{R_a + R_b + R_c}
     $$
3. **Graph Libraries**: Use NetworkX (Python) for efficient graph operations.

4. **Matrix Methods**: For complex graphs, use the Laplacian matrix to compute $R_{eq}$ numerically, though this is less intuitive.

### Conclusion
The algorithm systematically computes equivalent resistance by reducing a circuit graph using series 

($R_{eq} = R_1 + R_2$) and parallel ($\frac{1}{R_{eq}} = \frac{1}{R_1} + \frac{1}{R_2}$) formulas. It handles nested configurations through iterative reductions, as shown in the three examples. While efficient for series-parallel graphs, extensions like delta-star transformations or matrix methods can address more complex cases. The approach demonstrates the power of graph theory in simplifying circuit analysis.
