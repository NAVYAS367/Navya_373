# Find Directly Connected Vertices

**Course:** Graph Analytics (EITC 302)
**Student:** Navya S
**Register No:** 2411021060373

## Problem Statement

Write a program that identifies all direct neighbours of a selected vertex in an undirected graph.

The program should:
- Accept an undirected graph from the user.
- Store the graph using an adjacency list.
- Ask the user to select a vertex.
- Display all vertices directly connected to it.
- Display the number of direct connections.

## Approach

The graph is stored as an **adjacency list** — a dictionary where each vertex maps to a list of its neighbours. Since the graph is undirected, every edge `(u, v)` is added to *both* `u`'s list and `v`'s list at construction time. This makes finding the direct neighbours of any vertex a simple O(1) dictionary lookup instead of scanning every edge, and the count of direct connections is just the length of that list.

## Algorithm

1. Start.
2. Read the number of vertices and their names from the user.
3. Initialize an empty adjacency list (dictionary of lists), one entry per vertex.
4. Read the number of edges. For each edge `(u, v)`, add `v` to the adjacency list of `u` and add `u` to the adjacency list of `v`, since the graph is undirected.
5. Display the complete adjacency list representation of the graph.
6. Ask the user to select a vertex to inspect.
7. If the selected vertex exists, retrieve its adjacency list — this gives all directly connected (neighbouring) vertices.
8. Display the list of directly connected vertices and their count.
9. Stop.

## Code

```python
# Program to find all directly connected vertices (neighbours) of a
# selected vertex in an undirected graph using an Adjacency List.

def build_graph():
    """Accepts an undirected graph from the user and stores it as an
    adjacency list using a dictionary of lists."""
    graph = {}

    n = int(input("Enter the number of vertices: "))
    print("Enter vertex names (e.g., A, B, C ...):")
    for _ in range(n):
        v = input("Vertex: ").strip()
        if v not in graph:
            graph[v] = []

    e = int(input("\nEnter the number of edges: "))
    print("Enter each edge as: vertex1 vertex2")
    for _ in range(e):
        u, v = input("Edge: ").split()
        u, v = u.strip(), v.strip()

        # Since the graph is undirected, add the connection on both sides
        if v not in graph[u]:
            graph[u].append(v)
        if u not in graph[v]:
            graph[v].append(u)

    return graph


def show_direct_neighbours(graph, vertex):
    """Displays all vertices directly connected to the given vertex
    and the total number of such direct connections."""
    if vertex not in graph:
        print(f"\nVertex '{vertex}' does not exist in the graph.")
        return

    neighbours = graph[vertex]
    print(f"\nVertices directly connected to '{vertex}': {neighbours}")
    print(f"Number of direct connections: {len(neighbours)}")


def main():
    print("---- Build the Undirected Graph ----")
    graph = build_graph()

    print("\nAdjacency List Representation:")
    for vertex, neighbours in graph.items():
        print(f"{vertex} -> {neighbours}")

    selected = input("\nEnter the vertex to inspect: ").strip()
    show_direct_neighbours(graph, selected)


if __name__ == "__main__":
    main()
```

## How to Run

```bash
python graph.py
```

You will be prompted to enter:
1. The number of vertices, followed by each vertex name.
2. The number of edges, followed by each edge as `vertex1 vertex2`.
3. The vertex you want to inspect.

## Sample Input / Output

**Graph used:** vertices `A, B, C, D, E` with edges `A-B, A-C, B-D, C-D, D-E`

```
---- Build the Undirected Graph ----
Enter the number of vertices: 5
Enter vertex names (e.g., A, B, C ...):
Vertex: A
Vertex: B
Vertex: C
Vertex: D
Vertex: E

Enter the number of edges: 5
Enter each edge as: vertex1 vertex2
Edge: A B
Edge: A C
Edge: B D
Edge: C D
Edge: D E

Adjacency List Representation:
A -> ['B', 'C']
B -> ['A', 'D']
C -> ['A', 'D']
D -> ['B', 'C', 'E']
E -> ['D']

Enter the vertex to inspect: D

Vertices directly connected to 'D': ['B', 'C', 'E']
Number of direct connections: 3
```

## Complexity

| | Complexity |
|---|---|
| Building the adjacency list | O(V + E) |
| Finding direct neighbours of a vertex | O(1) lookup + O(deg(v)) to read the list |
| Space | O(V + E) |

## Observation

Selecting vertex `D` correctly returns its three direct neighbours — `B`, `C`, and `E` — confirming that the undirected edges were stored symmetrically on both endpoints during graph construction.

## Repository Structure

```
.
├── graph.py     # Source code
└── README.md    # This file
```
