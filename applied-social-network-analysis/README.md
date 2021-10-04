# Applied Social Network Analysis in Python

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
