# Disjoint-Set Union (Union-Find) & Percolation

This repository contains the implementation and experiments for **Problem 11** of the Algorithms and Data Structures assignment.

The project explores how connectivity algorithms evolve from basic graph traversal to an efficient Disjoint-Set Union (DSU), also known as **Union-Find**. It also demonstrates how Union-Find can be used to simulate percolation in a grid.

## Project Overview

The project explores five approaches to solving dynamic connectivity problems:

| Version | Algorithm | Main Idea |
|---|---|---|
| V1 | Graph BFS/DFS | Search the graph whenever connectivity is queried |
| V2 | Quick-Find | Store a component ID for every element |
| V3 | Naive Union-Find | Represent components using parent pointers |
| V4 | Union by Rank | Attach shorter trees below taller trees |
| V5 | Union by Rank + Path Compression | Keep trees shallow and flatten paths during Find operations |

The main objective is to compare their performance and understand why V5 is significantly more efficient for repeated connectivity operations.

## Experiments

### Experiment 1: Connectivity Benchmarking

**Objective:** Compare the performance of the five connectivity approaches using an adversarial sequence of union operations.

**Methodology:**

- Construct a chain using the operations:

  `Union(0,1), Union(1,2), ..., Union(n-2,n-1)`

- Test the algorithms with the following input sizes:

  `n = 500, 1000, 2000, 5000`

- Perform connectivity queries after constructing the chain.
- Measure execution time and average parent-pointer hops per Find operation.
- Compare the tree depths produced by the different Union-Find implementations.

**What this experiment demonstrates:**

- V1 repeatedly searches through the graph to answer connectivity queries.
- V2 has fast Find operations but expensive Union operations because it updates component IDs.
- V3 can produce a long chain, resulting in slow Find operations.
- V4 prevents unnecessarily tall trees through union by rank.
- V5 further improves Find operations by compressing paths.

**Key observations:**

- At `n = 5000`, V3 produces a tree with a maximum depth of 4999.
- V4 and V5 maintain a maximum depth of 1 for this particular chain sequence.
- V4 and V5 require approximately one parent-pointer hop per query on this test.
- The optimized versions demonstrate the benefit of controlling tree height.

### Experiment 2: Monte Carlo Percolation Simulation

**Objective:** Investigate how the probability of percolation changes as the fraction of open cells increases.

**Methodology:**

- Generate square grids of different sizes:

  `L = 40, 80, 160`

- Randomly open cells with different probabilities:

  `p = 0.45 to 0.75`, increasing in steps of `0.01`.

- Run 100 independent trials for each probability and grid size.
- Use Union-Find with virtual top and bottom nodes to determine whether the grid percolates.
- Calculate the empirical percolation probability:

  `P(percolates) = Number of successful trials / Total trials`

- Plot the empirical percolation probability against the probability of opening a cell.

**What this experiment demonstrates:**

- At low opening probabilities, it is unlikely that a continuous open path connects the top and bottom.
- As the opening probability increases, the chance of percolation rises.
- The transition occurs around the critical probability of approximately `0.593` for a sufficiently large square lattice.
- Larger grids show a sharper transition around the critical probability.

## Complexity Comparison

| Version | Union | Find / Connected | Auxiliary Space |
|---|---|---|---|
| V1 – BFS/DFS | O(1) | Θ(n + e) | Θ(n + e) |
| V2 – Quick-Find | Θ(n) | Θ(1) | Θ(n) |
| V3 – Naive Union-Find | O(n) worst case | O(n) worst case | Θ(n) |
| V4 – Union by Rank | O(log n) | O(log n) | Θ(n) |
| V5 – Rank + Path Compression | O(α(n)) amortized | O(α(n)) amortized | Θ(n) |

Here, `n` is the number of elements, `e` is the number of graph edges, and `α(n)` is the inverse Ackermann function, which grows extremely slowly.

## Technologies Used

- **Python** – Algorithm implementation and simulations
- **Jupyter Notebook** – Running experiments and recording results
- **Matplotlib** – Visualizing benchmark and percolation results

## How to Run

1. Clone the repository:

   ```bash
   git clone <YOUR_REPOSITORY_URL>
   ```

2. Navigate to the project directory:

   ```bash
   cd <YOUR_PROJECT_FOLDER>
   ```

3. Install the required packages:

   ```bash
   pip install matplotlib numpy jupyter
   ```

4. Start Jupyter Notebook:

   ```bash
   jupyter notebook
   ```

5. Open and execute the experiment notebooks.

   - Run the connectivity benchmarking notebook for Experiment 1.
   - Run the Monte Carlo percolation notebook for Experiment 2.

*Note: Update the notebook names and installation commands above if your repository uses different filenames or dependencies.*

## Conclusion

The experiments demonstrate how choosing an appropriate data structure can significantly improve algorithm performance.

Union by Rank prevents the formation of unnecessarily tall trees, while Path Compression makes future Find operations faster by connecting visited nodes directly to their root.

The percolation simulation demonstrates a practical application of Union-Find and illustrates how connectivity emerges as the probability of open cells increases.

Together, these experiments show why **Union-Find with Path Compression and Union by Rank** is an efficient solution for dynamic connectivity problems.

---

**Academic Project:** Algorithms and Data Structures – Problem 11  
**Topic:** Disjoint-Set Union (Union-Find) & Percolation
