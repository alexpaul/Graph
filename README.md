# Graph

A Graph is an abstract data structure that connects nodes or vertices via edges. Those edges can be weighted, example below the edge also shows the price associated with an airline ticket from the "New York" vertex to the destination vertex, "Saint Lucia". 

<pre> 
          $440 
New York -----> Saint Lucia 
</pre> 

## Examples of Graphs in Real life 

* Social Graphs
* Path Optimization Algorithms 
* GPS Navigation Systems
* Facebook Graph API 
* Recommendation Engines  


## Objectives 

- [x] Know graph vocabulary and terminology.
- [x] Adjacency Matrix Implementation.
- [x] Adjacency List Implementation.
- [x] Depth-First Search of a Graph (DFS).
- [ ] Breadth-First Search on a Graph (BFS).

![graph notes](https://user-images.githubusercontent.com/1819208/108704410-fb6ecd00-74d9-11eb-92b0-8d22ddb354fb.jpg)

As we can visualize from the above illustration we see that the `Adjacency List` representation of a graph gives a quicker visual representation of the connections or neighbors of any given vertex. This comes in very handy when when performing such algorithms on graphs like breadth-first (BFS). 

## Challenge

![](https://cdn.programiz.com/sites/tutorial2program/files/graph-vertices-edges.jpg)

Using the illustration above. Draw an **Adjacency Martix** and an **Adjacency List** representation of the graph.

<details> 
  <summary>Solution</summary> 
          
  ![solution](https://user-images.githubusercontent.com/1819208/108721833-73e08880-74f0-11eb-8751-93598690c677.jpg)        
          
</details> 

## Vocabulary

* Edge - connection between two nodes (a source and destination) 
* Vertex or Node - holds a value (data)
* Directed Graph (_digraph)_ - has direction
* Undirected - does not have direction
* Weighted Graph - a weight is associated with the edge e.g price of airline ticket between two airports (nodes) 
* Unweighed Graph - edge DOES not have weight
* Graph implemenations: Adjacency Matrix, Adjacency List
* Neighbors - a given node's adjacent nodes

## Types of Graphs

* Finite Graphs 
* Infinite Graphs
* Trivial Graph 
* Simple Graph 
* Multi Graph
* Null Graph 
* Complete Graph 

![graph types](https://media.geeksforgeeks.org/wp-content/uploads/simplegraph.png)

More on [GeeksForGeeks](https://www.geeksforgeeks.org/graph-types-and-applications/)


## Adjacency Matrix Implementation 

> Wikipedia: A two-dimensional matrix, in which the rows represent source vertices and columns represent destination vertices. Data on edges and vertices must be stored externally. Only the cost for one edge can be stored between each pair of vertices.


```swift 
// adjacency matrix

/*

 0---------1
 |       / |  \
 |    /    |    \
 |  /      |    / 2
 |/        |  /
 4---------3/
 
   0 1 2 3 4
0: 0 1 0 0 1
1: 1 0 1 1 1
2: 0 1 0 1 0
3: 0 1 1 0 1
4: 1 1 0 1 0
 
*/

struct Graph {
  
  // number of vertices
  private var vertices: Int
  
  // adjacency matrix
  // we will use a [[Bool]] 2-D array to represent source -> destination relationships
  // the rows represent the source
  // the columns represent the destinations
  private var adjMatrix: [[Bool]]
  
  init(vertices: Int) {
    self.vertices = vertices
    
    // we will build a matrix base on the number of vertices passed into the initializer
    // all connections will be marked "false"
    self.adjMatrix = Array(repeating: Array(repeating: false, count: vertices), count: vertices)
  }
  
  public mutating func addEdge(source: Int, destination: Int) {
    // here we are assuming connections are part of an "undirected" graph
    adjMatrix[source][destination] = true
    adjMatrix[destination][source] = true
  }
  
  public func printGraph() {
    // here we are building a String represenation of the graph
    var graphDescription = "   "
    for i in 0..<vertices {
      graphDescription.append("\(i) ")
    }
    graphDescription.append("\n")
    
    for vertexIndex in 0..<adjMatrix.count {
      graphDescription.append("\(vertexIndex): ")
      for dest in adjMatrix[vertexIndex] {
        graphDescription.append(dest ? "1" : "0")
        graphDescription.append(" ")
      }
      graphDescription.append("\n")
    }
    print(graphDescription)
  }
  
}

var graph = Graph(vertices: 5)

graph.addEdge(source: 0, destination: 1)
graph.addEdge(source: 0, destination: 4)
graph.addEdge(source: 1, destination: 3)
graph.addEdge(source: 1, destination: 4)
graph.addEdge(source: 1, destination: 2)
graph.addEdge(source: 2, destination: 3)
graph.addEdge(source: 3, destination: 4)

graph.printGraph()

/*
   0 1 2 3 4
0: 0 1 0 0 1
1: 1 0 1 1 1
2: 0 1 0 1 0
3: 0 1 1 0 1
4: 1 1 0 1 0
*/
```

## Adjacency List Implementation

> Wikipedia: Vertices are stored as records or objects, and every vertex stores a list of adjacent vertices.

```swift 
// adjacency list

/*
     0---------1
     |       / |  \
     |    /    |    \
     |  /      |    / 2
     |/        |  /
     4---------3/
         
0 ---> [1, 4]
1 ---> [0, 2, 4, 3]
2 ---> [1, 3]
3 ---> [1, 2, 4]
4 ---> [0, 1, 3]
*/

struct Edge {
  public var source: Int
  public var destination: Int  
  public var weight: Int?   
}

struct Node {
  public var value: Int
  public var weight: Int?   
}

struct Graph {
  // adjacency list
  private var adjList: [[Node]]
  
  // our initializer takes in an array of [Edge] then creates a matrix of nodes
  init(edges: [Edge]) {
    // creates empty buckets e.g [[], [], [], []] based on how many edges (connnections)
    self.adjList = Array(repeating: [Node](), count: edges.count)
    
    for edge in edges {
      // create the destination node for the current edge
      let destinationNode = Node(value: edge.destination, weight: edge.weight)
      
      // append the new destination node to the source node
      adjList[edge.source].append(destinationNode)
    }
  }
  
  public mutating func addEdge(source: Int, destination: Int, weight: Int? = nil) {
    adjList[source].append(Node(value: destination, weight: weight))
    adjList[destination].append(Node(value: source, weight: weight))
  }
  
  public func printGraph() {
    for source in 0..<adjList.count {
      for edge in adjList[source] {
        print("\(source) ---> \(edge.value)", terminator: " ")
      }
      print()
    }
  }
}

/*
     0---------1
     |       / |  \
     |    /    |    \
     |  /      |    / 2
     |/        |  /
     4---------3/
*/

let edges = [
  Edge(source: 0, destination: 1),
  Edge(source: 0, destination: 4),
  
  Edge(source: 1, destination: 0),
  Edge(source: 1, destination: 2),
  Edge(source: 1, destination: 4),
  Edge(source: 1, destination: 3),
  
  Edge(source: 2, destination: 1),
  Edge(source: 2, destination: 3),
  
  Edge(source: 3, destination: 1),
  Edge(source: 3, destination: 2),
  Edge(source: 3, destination: 4),

  Edge(source: 4, destination: 0),
  Edge(source: 4, destination: 1),
  Edge(source: 4, destination: 3),
]

var graph = Graph(edges: edges)

graph.printGraph()

/*
 0 ---> 1 0 ---> 4
 1 ---> 0 1 ---> 2 1 ---> 4 1 ---> 3
 2 ---> 1 2 ---> 3 
 3 ---> 1 3 ---> 2 3 ---> 4 
 4 ---> 0 4 ---> 1 4 ---> 3 
*/
```

## Challenge 

Refactor the `printGraph()` method above for the **Adjacency List** implementation to print out the graph as follows, print the vertices in ascending order.  

```swift 
0 ---> [1, 4]
1 ---> [0, 2, 4, 3]
2 ---> [1, 3]
3 ---> [1, 2, 4]
4 ---> [0, 1, 3]
```

<details> 
  <summary>Solution</summary> 
      
```swift 
func printGraph() {
  var dict: [Int: [Node]] = [:]
  for srcIndex in 0..<adjList.count {
    for destNode in adjList[srcIndex] {
      if let nodes = dict[srcIndex] {
        dict[srcIndex] = nodes + [destNode]
      } else {
        dict[srcIndex] = [destNode]
      }
    }
  }
  let sortedVertices = dict.sorted { $0.key < $1.key }
  for (srcIndex, nodes) in sortedVertices {
    print("\(srcIndex) ---> \(nodes.map{$0.value})")
  }
}
```
      
</details>

## Breadth-first Traversal (BFS) 

#### Pseudocode 

<pre> 
start at source node
create a queue
create a visited set
add source node to queue
add source node to visited nodes (set)
iterate queue while its not empty
  dequeue front node
  print node
  iterate through sources's adjacency nodes (neighbors)
    if edge is not yet visited
      add to queue
      add to visited nodes (set)
</pre> 

```swift 
/*
 0---------1
 |       / |  \
 |    /    |    \
 |  /      |    / 2
 |/        |  /
 4---------3/
 
 BFS: 2 1 3 0 4
*/

extension Graph {
  func bfs(source: Int) {
    var visited: Set<Int> = []
    var queue = [Int]()
    visited.insert(source)
    queue.append(source)
    while !queue.isEmpty {
      let sourceIndex = queue.removeFirst()
      print("\(sourceIndex)", terminator: " ")
      for node in adjList[sourceIndex] {
        if !visited.contains(node.value) {
          queue.append(node.value)
          visited.insert(node.value)
        }
      }
    }
  }
}

graph.bfs(source: 2)
// 2 1 3 0 4 
```


## Depth-first Traversal (DFS)

#### Pseudocode 

<pre> 
start at the source node
create a stack
create a visited set
add the souce node to the stack
add the source to visited nodes
iterate through the stack while it's not empty
  pop the source from the stack
  print the source
  iterate through the source's neighbours
    if the the neighbour has not been marked visited
      push neighbor onto the stack
      mark neighbor visited
</pre> 


```swift 
/*
 0---------1
 |       / |  \
 |    /    |    \
 |  /      |    / 2
 |/        |  /
 4---------3/
 
 DFS: 2 3 4 0 1
 
*/


extension Graph {
  func dfs(source: Int) {
    var stack = [Int]()
    var visited: Set<Int> = []
    stack.append(source)
    visited.insert(source)
    while !stack.isEmpty {
      let source = stack.removeLast()
      print("\(source)", terminator: " ")
      for node in adjList[source] {
        if !visited.contains(node.value) {
          visited.insert(node.value)
          stack.append(node.value)
        }
      }
    }
  }
}

graph.dfs(source: 2)
// 2 3 4 0 1
```

## Resources

1. [Wikipedia - Graph data type](https://en.wikipedia.org/wiki/Graph_(abstract_data_type))
2. [AlgoDaily - Graphs](https://algodaily.com/lessons/implementing-graphs-edge-list-adjacency-list-adjacency-matrix)
3. [GeeksForGeeks - Graph Data Structure and Algorithms](https://www.geeksforgeeks.org/graph-data-structure-and-algorithms/)
4. [Comparison between Adjacency List and Adjacency Matrix representation of Graph](https://www.geeksforgeeks.org/comparison-between-adjacency-list-and-adjacency-matrix-representation-of-graph/)
5. [Real-life applications](https://leapgraph.com/graph-data-structures-applications/)
6. [Graph mapping using Core Data](https://yageek.github.io/blog/2018/02/19/a-taste-of-core-data-0-a-graph-framework/)
7. [Wikipedia - BFS](https://en.wikipedia.org/wiki/Breadth-first_search)
8. [Wikipedia - DFS](https://en.wikipedia.org/wiki/Depth-first_search)
