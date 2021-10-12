# Applied Social Network Analysis in Python

## Week 1

- definitions
  - **network (or graph)** = a representation of conections among a set of items
    - items are called nodes (or vertices)
    - connections are called edges (or links or ties)

    ```python
    import networkx as nx
    G = nx.Graph()
    G.add_edge('A', 'B')
    G.add_edge('A', 'C')
    ```

    - **undirected**: edges have no direction
    - **directed**: edges have direction
      - `G = nx.DiGraph()`
    - **weighted**: not all relationships are equal; some edges carry higher weight than others
      - `G.add_edge('A','B', weight=6)`
    - **signed**: a network where edges are assigned a positive or negative sign
      - `G.add_edge('A','B', sign='+')`

- **multigraphs**: a networks where multiple edges can connect the same nodes (parallel edges)

```python
G = nx.MultiGraph()
G.add_edge('A','B', relation='friend')
G.add_edge('A','B', relation='neighbor')
```

- types of questions we can answer
  - is a rumor likely to spread in this network?
  - who are the most influential people in this organization?
  - is this club likely to split into two groups?
    - if so, which nodes will go into which group?
  - which airports are at highest risk for a virus spreading?
  - are some parts of the world more difficult to reach?

- edge attributes
  - list all edges
    - `G.edges()`
  - list all edges with attributes
    - `G.edges(data=True)`
  - list all edges with attribute 'relation'
    - `G.edges(data='relation')`
  - accessing attributes of a specific edge
    - dictionary of attributes of edge (A, B)
      - `G.edge['A']['B']`
    - weight of edge (B, C)
      - `G.edge['B']['C']['weight']`

- node attributes

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

- bipartite graphs
  - **bipartite graph**: a graph whose nodes can be split into two sets *L* and *R* and every edge connects a node in *L* with a node in *R*

    ```python
    from networkx.algorithms import bipartite
    B = nx.Graph()
    B.add_nodes_from(['A', 'B', 'C', 'D', 'E'], bipartite=0)
    B.add_nodes_from([1, 2, 3, 4], bipartite=1)
    B.add_edges_from([('A',1), ('B',1), ('C',1), ('C',3), ('D',2), ('E',3), ('E',4)])
    ```

  - checking if a graph is bipartite:
    - `bipartite.is_bipartite(B)`
  - checking if a set of nodes is a bipartition of a graph
    - `bipartite.is_bipartite_node_set(B,X)` where `X` is a set of nodes and `B` is the bipartite graph
  - getting each set of nodes of a bipartite graph
    - `bipartite.sets(B)`
      - if not bipartite, `NetworkXError: Graph is not bipartite.`

  - **L-Bipartite graph projection**: network of nodes in group *L* where a pair of nodes is connected if they have a common neighbor in *R* in the bipartite graph

    ```python
    # define graph B
    # add edges to B
    # define set of nodes X
    P = bipartite.projected_graph(B, X)
    ```

  - **L-Bipartite weighted graph projection**: an L-Bipartite graph projection with weights on the edges that are proportional to the number of common neighbors between the nodes

    ```python
    # define graph B
    # add edges to B
    # define set of nodes X
    P = bipartite.weighed_projected_graph(B, X)
    ```

## Week 2

- **Clustering Coefficient**
  - **triadic closure** = the tendency for people who share connections in a social network to become connected
  - **local clustering coefficient of a node** = fraction of pairs of the node's friends that are friends with each other
    - compute the local clustering coefficient of node C
      - \# of pairs of C's friends who are friends / \# of pairs of C's friends
        - denominator: dc(dc-1)/2
        - if demoninator is 0, assume local clustering coefficient is zero
    - compute local clustering coefficient via `networkx`

        ```python
        nx.clustering(G, 'F')
        ```

  - compute global clustering coefficient
    - *Approach 1*: average local clustering coefficient over all nodes in the graph

        ```python
        nx.average_clustering(G)
        ```

    - *Approach 2*: percentage of "open triads" that are triangles in a network
      - **triangles** = 3 nodes connected by 3 edges
      - **open triads** = 3 nodes that are connected by 2 edges
      - NOTE: a triangle contains 3 open triads

      - **transitivity** = ratio of number of triangles and number of "open triads" in a network
    - both approaches measure the tendency for edges to form triangles
      - transitivity weights nodes with large degree higher

- **Distance Measures**
