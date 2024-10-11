# Graph
- A set of vertices and edges (V,E).
- Simplest graph is single vertex.
- Next simplest would be two vertexes and one edge.
- Undirected edges represent 2 way relationships such as two way roads.

# Types Of Graphs

## Directed Graph
- Direction going from A to B denoting one way path and the edge is a directed edge.
- i.e. you can go from A to B but not from B to A.
- Source: A, Destination: B.
- e.g. you reporting to your manager.

## Undirected Graph
- If n edges touches a vertex then degree of that vertex is n.
- If we can go from one node to another then we can say that there exists a path between them.
- Cycle: If we are able to reach back to vertex from where we started then we can say there exists a cycle, we can call such graph as undirected cyclic graph.
- If it is undirected and there does not exists cycle we can say that this graph is undirected Acyclic graph.
- Connected Graph: There exists a path from every node to every other node.
- A Graph with no cycle is tree(n-ary tree).
- Forest: A Disjoint set of trees (if the graph is disjoint).

## Graph Representation
- Representation plays key role in running algorithms, runtime, space and other core fundamentals
- What do we need to represent in a graph?
  - A way to model a vertex which may hold some information.
  - A way to model directed or undirected edges.
  - There a 3 standard ways that graphs can be represented.
-  Adjancency means directly connected to other vertexes (neighbors).

1. Adjacency Matrix:
2. Adjacency Set: 
3. Adjacency List:

### The Graph Interface
- Setup an interface with methods all graphs should implement the implementation can use the Adjacency Matrix, Adjacency List or Adjancency Set.
- An enum to indicate whethere the graph represents directed or undirected graph.
- An edge lies between two vertices are represented by numbers.
- Helper to get the adjacent vertices for any vertex, a method which is required for all algorithms involving graphs.
```
public interface Graph{

	enum GraphType{
		DIRECTED,
		UNDIRECTED
	}
	void addEdge(int v1, int v2);

	List<Integer> getAdjacentVertices(int v);
}
```
### Graph Using Adjacency Matrix
- Use a matrix with row and columns
- A matrix is just a table
- The row labels and the column labels represents the vertices
- Each cell represents the relationship between the vertices i.e. the edges.
- If we put 1 means there exists an edge.
- If we put 0 means there does not exists edge.
- Directed edge will be represented by row vertex as source and column vertex as destination.

```
class Graph{
	int[][] adjacencyMatrix;
	int numVertices;
}
```

Directed
```
	A 	B	C	D	E
A   0   1   1   0   0
B   0   0   0   1   0
C   0   0   0   0   1
D   0   0   0   0   0
E   0   0   0   1   0
```

Undirected
```
	A 	B	C	D	E
A   0   1   1   0   0
B   1   0   0   1   1
C   1   0   0   0   1
D   0   1   0   0   1
E   0   1   1   1   0
```

### Code for Adjacency Matrix
- This implements the graph interface, the use of Adjacency Matrix is an implmentation detail.
- Set up an V*V matrix to hold the vertices and edges relationship.
```
public class AdjacencyMatrixGraph implements Graph{
	private int[][] adjacencyMatrix;
	private GraphType graphType = GraphType.DIRECTED;
	private int numVertices = 0;

	public AdjacencyMatrixGraph(int numVertices, GraphType graphType){
		this.numVertices = numVertices;
		this.graphType = graphType;

		adjacencyMatrix = new int[numVertices][numVertices];

		for(int i=9;i<numVertices;i++){
			for(int j=0;j<numVertices;j++){
				adjacencyMatrix[i][j]=0;
			}
		}
	}

	@Override
	public void addEdge(int v1, int v2){
		if(v1>=numVertices||v1<0 ||v2>=numVertices || v2<0){
			throw new IllegalArgumentException("Vertex number is not valid");
		}

		adjacencyMatrix[v1][v2] =1;
		if(graphType==GraphType.UNDIRECTED){
			adjacencyMatrix[v2][v1]= 1;
		}
	}

	@Override
	public List<Integer> getAdjacentVertices(int v){
		if(v>=numVertices || v<0){
			throw new IllegalArgumentException("Vertex number is not valid");
		}
		List<Integer> adjacentVerticesList = new ArrayList<>();
		for(int i=0;i<numVertices;i++){
			if(adjacentMatrix[v][i]==1){
				adjacentVerticesList.add(i);
			}
		}
		Collections.sort(adjacentVerticesList);
		return adjacentVerticesList;
	}
}
```

### Adjacency List
- Each Vertex is a Node
- Each Vertex has a pointer to a linkedList
- This linked list contains all the other nodes this vertex connects directly.
- If a Vertex V has an edge leading to another vertex U, then U is present in V's Linked List.
- Adjacency Lists are not the best represetation of graphs.
- They have some major downsides.
- The order of the vertices in the Adjacency  Lists Matter
- The same graph can have multiple representations.
- Certain operations become tricky. e.g. deleting a node involves looking through all the ajacency lists to remove node from all lists.

Representation for Directed
```
A -> B -> C
B -> D
C -> E
D 
E -> B -> D
```

Representation for Undirected
```
A -> B -> C
B -> D -> E -> A
C -> E -> A
D -> B
E -> B -> D -> C
```

```
class Node{
	int vertexId;
	Node next;
}

or

class Node{
	int vertexId;
	List<Node> nodes;
}

class Graph{
	List<Node> vertices;
}
```

### Adjacency Set
- Very similar to adjacency List.
- Insted of list we use set.
- Has advantages like iterating through set.
- A class which represents a vertex.
- Each Node holds a set of adjacent vertices
- Each vertex has an index or unique number associated with it.
- Helper method to add an edge with this node as the source.
- Get the adjacent vertices for this node. 
Representation for Directed
```
A  B  C
B  D
C  E
D 
E  B  D
```

Representation for Undirected
```
A  B  C
B  D  E  A
C  E  A
D  B
E  B  D  C
```


```
class Node{
	int vertexId;
	Set<Node> nodes;
}

class Graph{
	List<Node> vertices;
}

public static class Node{
	private int vertexNumber;
	private set<Integer> adjacencySet = new HashSet<>();

	public Node(int vertexNumber){
		this.vertexNumber = vertexNumber;
	}

	public int getVertexNumber(){
		return vertexNumber;
	}

	public void addEdge(int vertexNumber){
		adjacencySet.add(vertexNumber);
	}

	public List<Integer> getAdjacentVertices(){
		List<Integer> sortexList = new ArrayList<>(adjacencySet);
		Collections.sort(sortedList);
		return sortedList;
	}
}
```

- This implements the graph interface the use of adjacency set is an implementation detail
- Setup a list of vertex nodes, remember each node holds a set of adjacent vertices.
- This can be directed or undirected Graph.
- Initialize the vertex List, And other Information in the constructor.
```
public class AdjacencySetGraph implements Graph{
	private List<Node> vertexList = new ArrayList<>();
	private GraphType graphType = GraphType.DIRECTED;
	private int numVertices = 0;

	public AdjacencySetGraph(int numVertices, GraphType graphType){
		this.numVertices = numVertices;
		for(int i=0;i<numVertices;i++){
			vertexList.add(new Node(i));
		}
		this.graphType = graphType;
	}

	@Override
	public void addEdge(int v1, int v2){
		if(v1>=numVertices||v1<0 || v2>=numVertices || v2<0){
			throw new IllegalArgumentException("Vertex Number is not valid: " + v1 +", "+v2);
		}
		vertexList.get(v1).addEdge(v2);
		if(graphType==GraphType.UNDIRECTED){
			vertexList.get(v2).addEdge(v1);
		}
	}
}
```

### Comparison of Graph Representation

| Adjacency Matrix | Adjacency List/Set |
|------------------|--------------------|
|This works well when the graph is well connected i.e. many nodes are connected with many other nodes.| A sparse graph with few connections between nodes might be more efficiently represented using adjacency list or set.|
|The Overhead of V*V space is worth is when the number of connections are large.| Does not have overhead of V*V space.|

### Common Operations
- E: Number of edges
- V: Number of vertices

| Factor | Adjacency Matrix | Adjacency List | Adjacency Set  |
|--------|------------------|----------------|----------------|
|Space   | V^2              | E + V          |    E + V       |
|Is Edge Present| 1         | Degree of V    |Log(Degree Of V)|
|Iterate over Edges on a vertex| V | Degree Of V | Degree Of V |

## Graph Traversal (DFS/BFS)
- Graph Traversal almost same as tree traversal
- In a tree there is only one path from the root to a specific node.
- In a graph there are multiple paths that can go from one node to other.
- Graph can have cycles.
- In order to avoid infinite looping in graph we need to keep track of the nodes previously visited.
### DFS
- We use a visited boolean list to keep track of nodes already visited.
- Other than this everything is same as tree.
```
public static void depthFirstTraversal(Graph graph, int[] visited, int currentVertex){
	if(visited[currentVertex]==1){
		return;
	}
	visited[currentVertex] = 1;
	List<Integer> list = graph.getAdjacentVertices(currentVertex);
	for(int vertex: List){
		depthFirstTraversal(graph, visited, vertex);
	}
	System.out.print(currentVertex + "->");
}
```
#### Unconnected Graph.
- Iterate through all nodes and start the DFS at every node to ensure that even unconnected nodes are covered.
```
for(int i=0;i<N;i++){
	depthFirstTraversal(graph, visited, i);
}
```
### BFS
- Visit all neighbors first.
- Go level wise from the first node.
- Add non-visited child notes to a Queue.
- Visited = True Means the node has been seen before.
- Use a visited list to keep track of nodes already visited.
```
public static void breadthFirstTraversal(Graph graph, int[] visited, int currentVertex) throws Queue.QueueOverflowException, Queue.QueueUnderflowException{

	Queue<Integer> queue = new Queue<>(Integer.class);
	queue.enqueue(currentVertex);

	while(!queue.isEmpty()){
		int vertex = queue.dequeue();
		if(visited[vertex] == 1){
			continue;
		}
		System.out.print(vertex + "->");
		visited[vertex] = 1;
		List<Integer> list = graph.getAdjacentVertices(vertex);
		for(int v: list){
			if(visisted[v]!=1){
				queue.enqueue(v);
			}
		}
	}
}

```
#### Unconnected Graphs
for(int i=0;i<N;i++){
	breadthFirstTraversal(graph, visited, i);
}
