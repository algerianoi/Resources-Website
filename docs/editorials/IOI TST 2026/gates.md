# Toll Gates

## Problem Statement

- [English](statements/gates(en).pdf)
- [French](statements/gates(fr).pdf)
- [Arabic](statements/gates(ar).pdf)

## Solution

#### Important definitions

- Let there be a directed graph $G = (V, E)$ such that:
    - $|V| = N, |E|=M$
    - $\forall u, v \in V, (u, v) \in E \Rightarrow (v, u) \notin E$.
- A path on this graph is defined as a sequence of edges $(u_i, v_i)$ such that:
    - $v_{i-1} = u_i$ for $i \geq 1$
    - $u_i, v_i \in V$ and either $(u_i, v_i) \in E$ or $(v_i, u_i) \in E$.
    - We define a path between $a$ and $b$ to be any such path where $u_0 = a$ and $v_{n-1} = b$, such that $n = |P|$ is the number of edges in the path.
    - We define $P[:i]$ to be the first $i$ edges of the path. 
- We define $S(u, v)$ as the score of an edge between $u, v$, where $S(u, v) = 1$ if $(u, v) \in E$ and $S(u, v) = -1$ if $(v, u) \in E$ (it is not defined otherwise). The former are referred to as "positive" edges, the latter as "negative" edges.
- We define the score of a path $P$ to be the sum of the scores of all its edges.

You are given $Q$ pairs $(s_i, t_i)$ such that $s_i, t_i \in V$. Your goal is to determine, for each given instance of $G$, if there exists a path $P$ between each $s_i$ and $t_i$ such that $\forall i < |P|: S(P[:i]) \ge 0$ and $S(P) = 0$.

### Subtask 1: $G$ is a path graph, $Q = 1$

One can observe that there is a forced path that has to be taken to go from any $s_i$ to its corresponding $t_i$. Any additional "detour" will not affect the score as it will have to be reversed. For example, in this graph, there is no way to go from $2$ to $5$ without having a final score of $1$: there is the forced path $2 {\color{blue}\rightarrow} 3 {\color{red}\rightarrow} 4 {\color{blue}\rightarrow} 5$, and trying to go to $6$ will not fix the score as the path $5 {\color{blue}\rightarrow} 6 {\color{red}\rightarrow} 5$ has a score of $0$. <br> <br>
The solution comes naturally: do a DFS/BFS from your only $s_i$ to your $t_i$ updating score and making sure the path is valid. If the traversal reaches $t_i$ and the score of the path is $0$, then there is a valid path between $s_i$ and $t_i$. Otherwise, there isn't. This takes $O((N + M)*Q)$ time, which solves this subtask.

### Subtask 2: $G$ is a rooted spider graph, all edges are oriented towards the root

Since a spider graph is a tree, there is still the same idea that between any two nodes, there is only one path. Additionally, notice two things: 

- If two nodes are on the same row, then obviously one cannot reach the other, because they are only doors of the same color between them.
- Paths between two nodes of two different rows can be considered "unimodal", in the sense that the path is a sequence of blue doors then red doors.

We thus know that a node can reach the other if this sequence has the same amount of blue doors than red doors. Finally, we know that the amount of blue doors is the distance of the first node from the root, and the amount of red doors is the distance of the second node from the root. Consequently, to solve this subtask, you merely need to check if the distance of the two nodes from the root is equal. <br> <br>
This can be preprocessed with a BFS in $O(N + M)$ time. Each query can be handled in $O(1)$ time, which results in a final complexity of $O(N + M + Q)$ time.

### Subtask 3: In any path $P$ over $G$, $\forall i < |P|, 0 \leq S(P[:i]) \leq 1$. $G$ is connected.

I'll start by making a foundational observation to the cases in this subtask: if the score of every path always tends between $0$ and $1$, this means that there is no way to see two edges of the same sign on any path. Notice how this means that a node is either connected only to positive edges or only to negative edges. Furthermore, notice that the graph is connected, so there is necessarily a path between any two nodes. <br> <br>
These two ideas combined allow us to make a bipartite coloring $(L, R)$ of the nodes of $G$ where the nodes in set $L$ only have positive edges, and the nodes in set $R$ only have negative edges. Naturally, no path can start at the nodes of $R$ because they will immediately cause a score of $-1$ when reaching any adjacent edge. However, the nodes of $L$ can reach each other because the paths between them start at $0$, go to $1$ when hitting any nodes in $R$, and then back to $0$ when heading for an edge in $L$. There is always a path of edges between each of the nodes of $L$ because the graph is connected. Thus, all nodes in $L$ can reach each other, andthe nodes in $R$ can't reach anything but themselves. 
<br> <br>
The coloring can be done in $O(M)$ time by reading the input and assigning $L$ to the nodes with positive edges and $R$ to the nodes with negative edges. Answering each query can be done in $O(1)$ time by checking if $s_i, t_i$ are in $L$ or $s_i = t_i$. Final complexity is $O(M + Q)$.

### Subtask 4: $G$ is a path graph.

Building on the fact that a path graph is basically an array, we can quickly find the solution to this subtask: lay the nodes of $G$ along an array and then do an iteration starting from the first edge of $G$ to the last, assigning each node a value $p[i]$, which is the score resulting from a path starting at said first edge and ending at $i$. We can thus observe that if $p[s_i] = p[t_i]$, the path between them has a score of $0$, which validates one of the conditions for checking that the path is valid. <br> <br>
However, there is another condition to verify, which is that the path does not have a negative score anywhere. To do this, we need to check that there exists no $w$ between $s_i$ and $t_i$ such that $p[w] < p[s_i]$, because if that were the case, this would mean that there is such a path with negative score. To find such a $w$, suffices to find the minimum of the score on the path between $s_i$ and $t_i$. Notice how this is identical to the RMQ problem, which can be solved efficiently using a segment tree. <br> <br>
Constructing a segment tree takes $O(n\log n)$ time, and each query on a segment tree takes $O(\log n)$ time. Thus, the final complexity is $O((N + Q) \log N)$ time. 

### Subtask 5: $G$ is a tree

Fundamentally, the same ideas hold from the previous subtask, except some changes have to be made: instead of calculating $p[i]$ from the first node, we can just simply take any node as the root of the tree, and calculate $p[i]$ relative to it. We still check if $p[s_i] = p[t_i]$. <br> <br>
What is different this time around is that we need to answer the RMQ problem on trees now instead of arrays, and that requires a different technique; binary lifting: We check that, on paths $s_i \rightarrow LCA(s_i, t_i)$ and $LCA(s_i, t_i) \rightarrow t_i$, there is no $p[w] < p[s_i]$, and this by simply keeping track of the minimum up to the $2^k$th ancestor for each node and applying binary lifting. <br> <br>
Considering that the amount of information tracked per node is at most $log(h)$ with $h$ being the distance between the root and the lowest leaf ($N$ in the worst case) we have to do at most $O(n\log n)$ calculations. Answering each query still takes $O(\log n)$ time, so we end up with a final complexity of $O((N + Q)\log N)$.


### Subtask 6: $N \leq 250$

For this subtask, a drastic change in solving strategy has to occur for a functional solution. The reason for this being that we no longer have the guarantee that there is only one path between two nodes, which means that $p[i]$ can't be fixed for all instances of the graph. <br> <br>
We will thus present a different way of solving the problem: if we know that there exists a $w$ where the graph has a path of score $1$ between $s_i$ and $w$ and a path of score $-1$ between $w$ and $t_i$, we know that there exists a solution between $s_i$ and $t_i$. To find the existence of such a path, we will use dynamic programming, where there are three states $dpl[s][t]$, $dpr[s][t]$ and $dpc[s][t]$ each corresponding to paths of score $1$, score $-1$ and score $0$. <br> <br>
The base case is that, for each node of our graph, $dp[u][u] = 1$, and if we have $u \rightarrow v$, we set $dpl[u][v] := 1$ and $dpr[v][u] := 1$. We then solve by checking, for each state $(u, v)$ if there exists a $w$ such that $dpl[u][v] = 1$ and $dpr[v][w] = 1$ or $dpl[w][u] = 1$ and $dpr[u][v] = 1$. We also propagate $dpc[u][v] = 1$, to other nodes: we update states $dpl[w][u], dpr[v][w], dpc[w][u]$ and $dpc[v][w]$ for each $w$. <br> <br>
Since there are $N^2$ states, and each state needs to check $N$ different values of $w$, we have a final complexity of $O(N^3)$.
### Full solution
How do we optimize the solution even further? One more observation can be made: if there is a path between $s$ and $t$, the inverse path is also valid, thus there is a path between $t$ and $s$. This also means that whatever score is obtainable in $s$ can also be obtained in $t$. We can thus treat $s$ and $t$ as the same node. In technical terms, we simply merge nodes $s$ and $t$ whenever we find a valid path between them, which is whenever $s$ and $t$ are both adjacent to the same node/cluster of nodes. <br> <br>
Merging can be done using a Union-find data structure in $O(N\alpha(N) + M)$ time, where $\alpha(x)$ is the inverse ackermann function. Queries can also be answered in $O(\alpha(N))$ time, which results in a final time complexity of $O((N + Q)\alpha(N) + M)$ time, solving our problem under the given constraints. <br> <br>

Here is a sample implementation:

```cpp
#include <bits/stdc++.h>
using namespace std;

#define vi vector<int>

void make_set(vi &par, vi &size, int node){
    par[node] = node;
    size[node] = 1;
}

int find_set(vi &par, int node){
    if(node == par[node]) return node;
    return par[node] = find_set(par, par[node]);
}

void union_set(vi &par, vi &size, int u, int v){
    
    int pu = find_set(par, u), pv = find_set(par, v);
    if(pu == pv) return;
    
    if(size[pu] >= size[pv]){
        par[pv] = pu;
        size[pu] += size[pv];
    }
    else{
        par[pu] = pv;
        size[pv] += size[pu];
    }
    
}

int main()
{
    
    int n, m, q;
    cin >> n >> m;
    
    vector<vi> adj(n+1);
    vi p(n+1, 0), sz(n+1, 0);
    
    queue<int> dsq;
    int a, b;
    
    for(int i = 1; i <= n; ++i) make_set(p, sz, i);
    for(int i = 0; i < m; ++i){
        
        char c;
        
        cin >> a >> b >> c;
        
        if(c == 'B'){
            adj[b].push_back(a);
            if(adj[b].size() == 2) dsq.push(b);
        }
        else{
            adj[a].push_back(b);
            if(adj[a].size() == 2) dsq.push(a);
        }
        
    }
    
    while(!dsq.empty()){
        
        int u = dsq.front();
        dsq.pop();
        
        set<int> ele;
        for(auto i : adj[u]) ele.insert(find_set(p, i));
        int x = *(ele.begin());

        if(ele.size() > 1){
            
            if(x == u) x = *(ele.begin()++);

            for(auto i : ele){
                
                union_set(p, sz, x, i);
                if(find_set(p, x) == i) swap(x, i); 

                if(i == x) continue;
                if(i == u) adj[x].push_back(x);
                else{
                    for(auto e : adj[i]) adj[x].push_back(e);
                    adj[i].clear();
                }
                
            }
            
            if(adj[x].size() > 1) dsq.push(x);
        }
        
        if(ele.find(u) == ele.end() || ele.size() == 1){
            adj[u].clear();
            adj[u].push_back(x);
        }
        
    }
    
    cin >> q;

    for(int i = 0; i < q; ++i){
        cin >> a >> b;
        if(find_set(p, a) == find_set(p, b)) cout << "1\n";
        else cout << "0\n";
    }

    return 0;
}
```