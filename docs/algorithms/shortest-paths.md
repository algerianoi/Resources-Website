# Shortest Paths

*Written by Haithem Djefel*

---

## 1. Unweighted Edges

> **Problem Statement: Motivation Problem**  
> [Piggyback (USACO)](http://www.usaco.org/index.php?page=viewproblem2&cpid=491)

> **Definition: Breadth-First Search (BFS)**  
> Used for unweighted graphs. Finds shortest paths in $O(V + E)$.
>
> ```text
> Algorithm: BFS
> Q <- empty queue; dist[v] <- infinity for all v
> dist[s] <- 0; Q.enqueue(s)
> while Q is not empty:
>     u <- Q.dequeue()
>     for each neighbor v of u:
>         if dist[v] == infinity:
>             dist[v] <- dist[u] + 1
>             Q.enqueue(v)
> ```

[Example using BFS](https://cses.fi/problemset/task/1666)

**Layer Property:** BFS processes the graph in discrete layers $L_0, L_1, L_2, \dots$, where $L_i$ contains all nodes at distance exactly $i$ from the source. It exhaustively explores all nodes in $L_k$ before moving to any node in $L_{k+1}$.

**Proof sketch:** Assume all vertices at distance $k$ are reached via their shortest path. Since BFS explores all neighbors of these vertices before moving to distance $k+2$, any vertex at distance $k+1$ will be discovered through a vertex at distance $k$. By induction, the first path found to any vertex is the shortest.

**Note on Multi-Source:** In the case of having multiple source nodes, we do not need to run BFS multiple times. We simply initialize $dist[s_i] = 0$ for all starting nodes $s_i$ and push them all into the initial Queue. The algorithm will naturally find the shortest distance from the *nearest* source to every other node in a single pass.

---

## 2. 0 - 1 Weighted Edges

> **Problem Statement: Motivation Problem**  
> [Tracks in the Snow (Baltic OI)](https://oj.uz/problem/view/BOI13_tracks)

> **Definition: 0-1 BFS**  
> Used when weights are only $0$ or $1$. Uses a **Deque** in $O(V+E)$.
>
> ```text
> Algorithm: 0-1 BFS
> D <- empty deque; dist[v] <- infinity; dist[s] <- 0
> D.push_back(s)
> while D is not empty:
>     u <- D.pop_front()
>     for each neighbor v of u with weight w:
>         if dist[v] > dist[u] + w:
>             dist[v] <- dist[u] + w
>             if w == 0:
>                 D.push_front(v)
>             else:
>                 D.push_back(v)
> ```

---

## 3. Weighted Edges (Non-Negative)

> **Problem Statement: Motivation Problem**  
> [Flight Discount (CSES)](https://cses.fi/problemset/task/1195)

> **Definition: Dijkstra's Algorithm**  
> Greedy approach for non-negative weights. $O((V+E) \log V)$.
>
> ```text
> Algorithm: Dijkstra
> PQ <- priority queue; dist[v] <- infinity; dist[s] <- 0
> PQ.push({0, s})
> while PQ is not empty:
>     {d, u} <- PQ.pop_min()
>     if d > dist[u]:
>         continue
>     for each neighbor v of u with weight w:
>         if dist[u] + w < dist[v]:
>             dist[v] <- dist[u] + w
>             PQ.push({dist[v], v})
> ```

**Intuition:** Dijkstra can be viewed as a generalization of BFS. If all weights $w$ were integers, we could replace each edge with $w$ unit-length edges and run BFS. The Priority Queue in Dijkstra effectively simulates this by always jumping to the next closest "real" vertex.

[Example using Dijkstra](https://cses.fi/problemset/task/1671)

---

## 4. Space State Graphs

Sometimes a problem does not mention a graph, but can be solved as one by defining a **State-Space Graph**.

> **Definition: State-Space Representation**  
> * **Nodes:** Each node represents a unique state of the problem (e.g., your current position, the amount of fuel left, and which items you have collected).  
> * **Edges:** An edge exists between two states if you can move from one to the other in a single step/action.  
> * **Weights:** The "cost" of taking that action (time, distance, or fuel).

**Common Bijection Examples:**
* **Grid Problems:** A cell $(x, y)$ is a node. Edges exist to $(x\pm1, y)$ and $(x, y\pm1)$.
* **Dijkstra with "States":** If you can perform a special move (like "skipping" an edge) up to $K$ times, your state is $(u, k)$, representing: *"I am at node $u$ and I have $k$ skips remaining."*
* **BFS on Numbers:** If you can multiply a number by 2 or subtract 1 to reach a target, each number is a node, and the operations are edges.

At first it is quite hard and unintuitive to come up with such ideas, as it requires creativity and being familiar with this type of problems, but at the same time this technique is very powerful, as sometimes the problem may seem impossible to approach, but once bijected onto the right graph, becomes a simple shortest path problem, we will see a demonstration with the following example:

> **Problem: The Two Buttons Puzzle**  
> You are given two integers $n$ and $m$. Your goal is to reach $m$ starting from $n$ using the minimum number of operations. You have two buttons:
> * **Red Button:** Multiplies the current number by 2 ($x \to 2x$).
> * **Blue Button:** Subtracts 1 from the current number ($x \to x - 1$).
> 
> Find the minimum number of "clicks" required.

> **The Shortest Path Bijection**  
> **State Space:** Each integer represents a node in a graph.  
> **Transitions:**
> 1. An edge from $u$ to $2u$ with weight 1.
> 2. An edge from $u$ to $u-1$ with weight 1.
> 
> **Observation:** Since all weights are equal, this is a shortest path problem on an **implicit graph**. We solve it by running **BFS** starting at $n$ and stopping as soon as we reach $m$.

---

## 5. Problemset

Problems are sorted in ascending order of their difficulty.

### Practice Problems

**Level: $\star$**
* [Labyrinth (CSES)](https://cses.fi/problemset/task/1193)
* [Monsters (CSES)](https://cses.fi/problemset/task/1194)

---

**Level: $\star\star$**
* [Labyrinth (CF 1063B)](https://codeforces.com/contest/1063/problem/B)
* [Milk Pails (USACO)](https://usaco.org/index.php?page=viewproblem2&cpid=620)

---

**Level: $\star\star\star$**
* [Cow At Large (USACO)](https://usaco.org/index.php?page=viewproblem2&cpid=790)
* [Shortcut (USACO)](https://usaco.org/index.php?page=viewproblem2&cpid=899)