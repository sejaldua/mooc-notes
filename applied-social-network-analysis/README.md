---
marp: true
theme: gaia
size: 16:9
_class: invert
paginate: true
---

# Applied Social Network Analysis in Python

---

## Week 1

---

### Definitions

- **network (or graph)** = a representation of conections among a set of items
  - items are called nodes (or vertices)
  - connections are called edges (or links or ties)

    ```python
    import networkx as nx
    G = nx.Graph()
    G.add_edge('A', 'B')
    G.add_edge('A', 'C')
    ```

---

### Definitions (continued)

- network
  - **undirected**: edges have no direction
  - **directed**: edges have direction
    - `G = nx.DiGraph()`
  - **weighted**: not all relationships are equal; some edges carry higher weight than others
    - `G.add_edge('A','B', weight=6)`
  - **signed**: a network where edges are assigned a positive or negative sign
    - `G.add_edge('A','B', sign='+')`

---

### Definitions (continued)

- **multigraphs**: a networks where multiple edges can connect the same nodes (parallel edges)

```python
G = nx.MultiGraph()
G.add_edge('A','B', relation='friend')
G.add_edge('A','B', relation='neighbor')
```

---

### Types of questions we can answer

- is a rumor likely to spread in this network?
- who are the most influential people in this organization?
- is this club likely to split into two groups?
  - if so, which nodes will go into which group?
- which airports are at highest risk for a virus spreading?
- are some parts of the world more difficult to reach?

---

### Edge Attributes

- list all edges
  - `G.edges()`
- list all edges with attributes
  - `G.edges(data=True)`
- list all edges with attribute 'relation'
  - `G.edges(data='relation')`

---

### Edge Attributes (continued)

- accessing attributes of a specific edge
  - dictionary of attributes of edge (A, B)
    - `G.edge['A']['B']`
  - weight of edge (B, C)
    - `G.edge['B']['C']['weight']`

---

### Node Attributes

```python
G = nx.Graph()
G.add_edge('A','B', weight=6, relation='family')
G.add_edge('B','C', weight=13, relation='friend')
G.add_node('A', role='trader')
G.add_node('B', role='trader')
G.add_node('C', role='manager')
```

- list all nodes
  - `G.nodes()`
- list all nodes with attributes
  - `G.nodes(data=True)`
- get role of node A
  - `G.node['A']['role']`

---

### Bipartite Graphs

- **bipartite graph**: a graph whose nodes can be split into two sets *L* and *R* and every edge connects a node in *L* with a node in *R*

  ```python
  from networkx.algorithms import bipartite
  B = nx.Graph()
  B.add_nodes_from(['A', 'B', 'C', 'D', 'E'], bipartite=0)
  B.add_nodes_from([1, 2, 3, 4], bipartite=1)
  B.add_edges_from([('A',1), ('B',1), ('C',1), ('C',3), ('D',2), ('E',3), ('E',4)])
  ```

---

#### Handy Bipartite Graph Functions

- checking if a graph is bipartite:
  - `bipartite.is_bipartite(B)`
- checking if a set of nodes is a bipartition of a graph
  - `bipartite.is_bipartite_node_set(B,X)` where `X` is a set of nodes and `B` is the bipartite graph
- getting each set of nodes of a bipartite graph
  - `bipartite.sets(B)`
    - if not bipartite, `NetworkXError: Graph is not bipartite.`

---

### L-Bipartite Graphs

- **L-Bipartite graph projection**: network of nodes in group *L* where a pair of nodes is connected if they have a common neighbor in *R* in the bipartite graph

```python
# define graph B
# add edges to B
# define set of nodes X
P = bipartite.projected_graph(B, X)
```

---

### L-Bipartite Graphs (continued)

- **L-Bipartite weighted graph projection**: an L-Bipartite graph projection with weights on the edges that are proportional to the number of common neighbors between the nodes

```python
# define graph B
# add edges to B
# define set of nodes X
P = bipartite.weighed_projected_graph(B, X)
```

---

## Week 2

---

### Clustering Coefficient

- **triadic closure** = the tendency for people who share connections in a social network to become connected
- **local clustering coefficient of a node** = fraction of pairs of the node's friends that are friends with each other
  - compute the local clustering coefficient of node C
    - $\frac{\text{number of pairs of C's friends who are friends}}{\text{number of pairs of C's friends}}$
      - denominator: $\frac{d_c(d_c-1)}{2}$
      - if demoninator is 0, assume local clustering coefficient is zero

---

### Clustering Coefficient (continued)

- compute local clustering coefficient via `networkx`

  ```python
  nx.clustering(G, 'F')
  ```

- compute global clustering coefficient
  - *Approach 1*: average local clustering coefficient over all nodes in the graph

      ```python
      nx.average_clustering(G)
      ```

---

### Clustering Coefficient (continued)

- *Approach 2*: percentage of "open triads" that are triangles in a network
  - **triangles** = 3 nodes connected by 3 edges
  - **open triads** = 3 nodes that are connected by 2 edges
  - NOTE: a triangle (continued)ains 3 open triads

  - **transitivity** = ratio of number of triangles and number of "open triads" in a network
- both approaches measure the tendency for edges to form triangles
  - transitivity weights nodes with large degree higher

---

### Distance Measures

- **path** = a sequence of nodes connected by an edge
  - A - B - C - E - H (4 "hops")
  - distance between two nodes = the length of the shortest path between them
  - `nx.shortest_path(G, 'A', 'H')`
  - `nx.shortest_path_length(G, 'A', 'H')`
- **breadth-first search** = a systematic and efficient procedure for computing distances from a node to all other nodes in a large network by "discovering" nodes in layers
  - `T = nx.bfs_tree(G, 'A')`

---

### Distance Measures (continued)

- take average distance between every pair of nodes
  - `nx.average_shortest_path_length(G)`
- **diameter** = maximum distance between any pair of nodes
  - `nx.diameter(G)`
- **eccentricity** = the largest distance between node n and all other nodes
  - `nx.eccenricity(G)`
  - `out: {'A': 5, 'B': 4, 'C': 3}`

---

### Distance Measures (continued)

- **radius** = mimimum eccentricity
  - `nx.radius(G)`
- **periphery** = the set of nodes that have eccentricity equal to the diameter
  - `nx.periphery(G)`
  - `out: ['A', 'K', 'J']`
- **center** = the set of nodes that have eccentricity equal to the radius

---

### Connected Components

- an undirected graph is **connected** if, for every pair of nodes, there is a path between them
  - `nx.is_connected(G)`
- **connected component** = a subset of nodes such that:
  - 1. every node in the subset has a path to every other node
  - 2. no other node has a path to any node in the subset

---

#### `networkx` Functions

- `nx.number_connected_components(G)`
- `sorted(nx.connected_components(G))`
- `nx.node_connected_component(G, 'M')`
  
---

### Connected Components (continued)

- connectivity in directed graphs
  - a directed graph is **strongly connected** if, for every pair of nodes u and v, there is a directed path from u to v and a directed path from v to u
    - `nx.is_strongly_connected(G)`
  - a directed graph is **weakly connected** if replacing all directed edges with undirected edges produces a connected undirected graph
    - `nx.is_weakly_connected(G)`

---

### Connected Components (continued)

- **strongly connected component** = a subset of nodes such that:
  - 1. every node in the subset has a *directed* path to every other node
  - 2. no other node has a *directed* path to and from every node in the subset
- **weakly connected component** = the connected components of the graph after replacing all directed edges with undirected edges

---

### Network Robustness

- **network robustness** = the ability of a network to maintain its general structural properties when it faces failures or attacks
- **types of attacks** = removal of nodes or edges
- **structural properties** = connectivity
- **examples**: airport closures, internet router failures, power line failures
- disconnecting a graph

---

### Network Robustness (continued)

- what is the smallest number of nodes that can be removed from this graph in order to disconnect it?
  - `nx.node_connectivity(G_un)`
- which node?
  - `nx.minimum_node_cut(G_un)`
- what is the smallest number of edges that can be removed from this graph in order to disconnect it?
  - `nx.edge_connectivity(G_un)`
- which edges?
  - `nx.minimum_edge_cut(G_un)`
- *robust networks have large minimum node and edge cuts*

---

### Network Robustness (continued)

- simple paths
  - node G wants to send a message to node L
    - `sorted(nx.all_simple_paths(G, 'G', 'L'))`
  - if we wanted to block the message from G to L by removing nodes from the network, how many nodes would we need to remove?
    - `nx.node_connectivity(G, 'G', 'L')`
    - `nx.minimum_node_cut(G, 'G', 'L')`
  - same idea for edges...

---

## Week 3

---

### Degree and Closeness Centrality

- **Degree Centrality**
  - assumption: important nodes have many connections
  - measure: number of neighbors
  - undirected networks: use degree
    - $C_{deg}(v) = \frac{d_v}{|N|-1}$

      ```python
      G = nx.karate_club_graph()
      G = nx.convert_node_labels_to_integers(G, first_label=1)
      degCent = nx.degree_centrality(G)
      ```

---

- **Degree Centrality (continued)**
  - directed networks: use in-degree or out-degree
    - $C_{indeg}(v) = \frac{d_v^{in}}{|N|-1}$

      ```python
      indegCent = nx.in_degree_centrality(G)
      indegCent['A']
      ```

    - NOTE: can do the same for out-degree centrality

---

- **Closeness Centrality**
  - assumption: important nodes are close to other nodes
  - $C_{close}(v) = \frac{|N|-1}{\sum_{u \in N, v} d(v, u)}$
    - $N$ = set of nodes in the network
    - $d(v,u)$ = length of shortest path from v to u

  ```python
  nx.closeness_centrality(G)
  ```

---

#### Disconnected Nodes

- how to measure the closeness centrality of a node when it cannot reach all other nodes?
  - *option 1*: consider only nodes that node L can reach
    - $C_{close}(L) = \frac{|R(L)|}{\sum_{u \in R(L)} d(L,u)}$
  - *option 2*: consider only nodes that L can reach and normalize by the fraction of nodes L can reach
    - $C_{close}(L) = \frac{|R(L)|}{|N - 1|}\frac{|R(L)|}{\sum_{u \in R(L)} d(L,u)}$

    ```python
    nx.closeness_centrality(G, normalized=True)
    ```

---

### Other Centrality Measures

- betweenness centrality
- load centrality
- page rank
- Katz centrality
- percolation centrality

---

### Betweenness Centrality

- assumption: important nodes connect other nodes
- $C_{btw}(v) = \sum_{s,t \in N}\frac{\sigma_{s,t}(v)}{\sigma_{s,t}}$
  - $\sigma_{s,t}$ = the number of shortest paths between nodes s and t
  - $\sigma_{s,t}(v)$ = the number of shortest baths between nodes s and t *that pass through node v*
- endpoints: we can either include or exclude node *v* as node *s* and *t* in the computation of $C_{btw}(v)$

---

### Betweenness Centrality (continued)

- endpoints: we can either include or exclude node *v* as node *s* and *t* in the computation of $C_{btw}(v)$
  - if we exclude node *v*, we have
    - $C_{btw}(B) = \frac{\sigma_{A,D}(B)}{\sigma_{A,D}} + \frac{\sigma_{A,C}(B)}{\sigma_{A,C}} + \frac{\sigma_{C,D}(B)}{\sigma_{C,D}} = \frac{1}{1} + \frac{1}{1} + \frac{0}{1} = 2$
  - if we include node *v*, we have
    - $C_{btw}(B) = \frac{\sigma_{A,B}(B)}{\sigma_{A,B}} + \frac{\sigma_{A,C}(B)}{\sigma_{A,C}} + \frac{\sigma_{A,D}(B)}{\sigma_{A,D}} + \frac{\sigma_{B,C}(B)}{\sigma_{B,C}} + \frac{\sigma_{B,D}(B)}{\sigma_{B,D}} + \frac{\sigma_{C,D}(B)}{\sigma_{C,D}}= \frac{1}{1} + \frac{1}{1} +  \frac{1}{1} + \frac{1}{1} + \frac{1}{1} + \frac{0}{1} = 5$

---

### Betweenness Centrality - Normalization

- **Normalization**: betweenness centrality values will be larger in graphs with many nodes
  - to control for this, we divide centrality values by the number of pairs of nodes in the graphs (excluding *v*)

$\frac{1}{2}(|N|-1)(|N|-2)$ in undirected graphs

$(|N|-1)(|N|-2)$ in directed graphs

```python
btwnCent = nx.betweenness_centrality(G, normalized=True, endpoints=False)
```

---

### Betweenness Centrality - misc

- can use approximation to save on computational power
- can compute for subsets
- can use to find important edges instead of nodes

---

### Basic Page Rank

> Developed by Google founders to measure the importance of webpages from the hyperlink network structure

PageRank assigns a score of importance to each node. Important nodes are those with many in-links from important pages.

Works best for directed networks.

---

### Page Rank (continued)

- n = number of nodes in the network
- k = number of steps

1. Assign all nodes a PageRank of $\frac{1}{n}$
2. Perform the `Basic PageRank Update Rule` k times

`Basic PageRank Update Rule`:  
Each node gives an equal share of its current PageRank to all the nodes it links to. The new PageRank of each node is the sum of all the PageRank it received from other nodes.

---

### Scaled Page Rank
