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

The PageRank of a node at step k is the probability that a *random walker* lands on the node after taking k steps.

Random walk of k steps:

- start on a random node
- choose an outgoing edge at random and follow it to the next node
- repeat k times

---

### PageRank Problem

For large enough k, F and G each have PageRank of 1/2 and all the other nodes have PageRank 0... in other words, whenever the random walk lands on F or G, it gets stuck on F and G.

---

### PageRank Solution

**Solution**: introduce "damping parameter" $\alpha$

- random walk of k steps with damping parameter $\alpha$
  - start on a random node
  - with probability $\alpha$: choose an outgoing edge at random and follow it to the next node
  - with probability $1 - \alpha$: choose a node at random and go to it
  - repeat k times

---

### Scaled Page Rank

Scaled PageRank of k steps and damping factor $\alpha$ of a node n is the probability that a random walk with damping factor $\alpha$ lands on a node n after k steps

For most networks, as k gets larger, Scaled PageRank converges to a unique value, which depends on $\alpha$

---

### Hubs and Authorities

- another way to find central nodes in a network (important nodes)
- given a query to a search engine:
  - **root**: set of highly relevant web pages (e.g. pages that contain the query string)-- *potential authorities*

    - find all pages that link to a page in the root-- *potential hubs*
  - **base**: root nodes and any node that links to a node in root
    - consider all edges connecting nodes in the base set

---

### HITS Algorithm

Computing k iterations of the HITS algorithm to assign an *authority score* and *hub score* to each node

1. Assign each node an authority and hub score of 1
2. Apply the ***Authority Update Rule***: each node's *authority score* is the sum of *hub scores* (out-degree) of each node that points to it (... then normalize scores)

---

### Comparing Centrality Measures

- in-degree centrality: how many nodes are pointing to you
- closeness centrality: how many steps does it take to reach other nodes
- betweenness centrality: shows up in the shortest path between many pairs of nodes
- pagerank: central nodes would be traversed most if many random walks were taken
- authority and hub: ...

---

## Week 4

---

### Degree Distributions

- the **degree** of a node in an undirected graph is the number of neighbors it has
- the **degree distribution** of a graph is the probability distribution of the degrees over the entire network
- plot degree distribution of network

  ```python
  degrees = G.degree()
  degree_values = sorted(set(degrees.values()))
  histogram = [list(degrees.values()).count(i) / float(nx.number_of_nodes(G)) for i in degree_values]

  import matplotlib.pyplot as plt
  plt.bar(degree_values, histogram)
  plt.xlabel('Degree')
  plt.ylabel('Fraction of Nodes')
  plt.show()
  ```  

---

### Degree Distributions (continued)

- A: **Actors**: network of 225,000 actors connected when they appear in a movie together
- B: **The Web**: network of 325,000 documents on the WWW connected by URLs
- C: **US Power Grid**: network of 4,941 generators connected by transmission lines
- Degree-distribution looks like a straight line on log-log scale
  - **Power law**: $P(k) = Ck^{-\alpha}$, where $\alpha$ and C are constants
    - $\alpha$-values -- A: 2.3, B: 2.1, C: 4

---

### Preferential Attachment Model

- start with two nodes connected by an edge
- at each time step, add a new node with an edge connecting it to an existing node
- choose the node to connect to at random with probability proportional to each node's degree
  - the probability of connecting to a node $u$ of degree $k_u$ is $\frac{k_u}{\sum_jk_j}$

---

### Preferential Attachment in NetworkX

`barabasi_albert_graph(n,m)` returns a network with n nodes. Each new node attaches to m existing nodes according to the Preferential Attachment model

```python
G = nx.barabasi_albert_graph(1000000,1)
degrees = G.degree()
degree_values = sorted(set(degrees.values()))
histogram = [list(degrees.values().count(i)) / float(nx.number_of_nodes(G)) for i in degree_values]

plt.plot(degree_values, histogram, 'o')
plt.xlabel('Degree')
plt.ylabel('Fraction of Nodes')
plt.xscale('log')
plt.yscale('log')
plt.show()
```

---

### Small World Networks

- **local clustering coefficient of a node**: fraction of pairs of the node's friends that are friends with each other
  - Facebook 2011: high average CC (decreases with degree)
- The degree distribution of small world network is not a power law because the degree of most nodes lie in the middle.
- the small world model starts with a ring lattice with nodes connected to k nearest neighbors (high local clustering), and it rewires edges with probability p

---

### Small World Networks (continued)

- can be disconnected, which is sometimes undesirable
- `connected_watts_strogatz_graph(n,k,p,t)` runs `watts_strogatz_graph(n,k,p)` up to t times, until it returns a connected network

---

### Link Prediction

- Which new edges are likely to form in this network?
- Who is likely to become friends?
- **Triadic closure**: the tendency for people who share connections in a social network to become connected

---

#### Measure 1: Common Neighbors

The number of common neighbors of nodes X and Y is :

```python
common_neigh = [(e[0], e[1], len(list(nx.common_neighbors(G, e[0], e[1]))) for e in nx.non_edges(G)]
sorted(common_neigh, key=operator.itemgetter(2), reverse=True)
print(common_neigh)
```

---

#### Measure 2: Jaccard Coefficient

Number of common neighbors normalized by the total number of neighbors

```python
L = list(nx.jaccard_coefficient(G))
L.sort(key=operator.itemgetter(2), reverse=True)
print(L)
```

---

#### Measure 3: Resource ALlocation

Fraction of a "resource" that a node can send to another through their common neighbors

```python
L = list(nx.resource_allocation_index(G))
L.sort(key=operator.itemgetter(2), reverse=True)
print(L)
```

---

#### Measure 4: Adamic-Adar Index

Similar to resource allocation index, but with log in the denominator

```python
L = list(nx.adamic_adar(G))
L.sort(key=operator.itemgetter(2), reverse=True)
print(L)
```

---

#### Measure 5: Preferential Attachment

In the preferential attachment model, nodes with high degree get more neighbors; product of the nodes' degree

```python
L = list(nx.preferential_attachment(G))
L.sort(key=operator.itemgetter(2), reverse=True)
print(L)
```

---

### Community Structure

- some measures consider the community structure of the network for link prediction
- assume the nodes in this network belong to different communities (sets of nodes)
- pairs of nodes who belong to the same community and have many common neighbors in their community are likely to form an edge

---

#### Measure 6: Community Common Neighbors

Number of common neighbors with bonus for neighbors in same community

First step: assign nodes to communities with attribute node "community"

```python
L = list(nx.cn_soundarajan_hopcroft(G))
L.sort(key=operator.itemgetter(2), reverse=True)
print(L)
```

---

#### Measure 7: Community Resource Allocation

Similar to resource allocation index, but only considering nodes in the same community

First step: assign nodes to communities with attribute node "community"

```python
L = list(nx.ra_soundarajan_hopcroft(G))
L.sort(key=operator.itemgetter(2), reverse=True)
print(L)
```
