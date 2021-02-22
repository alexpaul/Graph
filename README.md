# Graph

A Graph is an abstract data structure that connects nodes or vertices via edges. Those edges can be weighted, example the price of an airline ticket. 

<pre> 
          $440 
New York -----> Saint Lucia 
</pre> 

## Examples of Grapsh in Real life 

* Social Graphs
* Path Optimization Algorithms 
* GPS Navigation Systems
* Facebook Graph API 
* Recommendation Engines  


## Objectives 

* Know graph vocabulary and terminology.
* Adjacency Matrix Implementation.
* Adjacency List Implementation.
* Breadth-First Search on a Graph (BFS).
* Depth-First Search of a Graph (DFS).

![graph notes](https://user-images.githubusercontent.com/1819208/108640224-75fe0500-7466-11eb-9d6c-0b352060560c.jpg)

As we can visualize from the above illustration we see that the `Adjacency List` representation of a graph gives a quicker visual representation of the connections or neighbors of any given vertex. This comes in very handy when doing algorithms such as BFS or breadth-first search on a graph.

## Vocabulary

* Edge - connection between two nodes (a source and destination) 
* Vertex or Node - holds a value (data)
* Directed Graph - one-way direction 
* Undirected - bi-directional
* Graph implemenations: Edge list, Adjacency Matrix, Adjacency List
* Neighbors

## Types of Graphs

* Finite Graphs 
* Infinite Grapsh 
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

```swift 
// adjacency list

/*
          (6)
     0---------1
     |       / |  \(3)
  (8)| (7)/    |    \
     |  /      |(1) / 2
     |/        |  /(3)
     4---------3/
         (10)
 
*/


struct Edge {
  public var source: Int
  public var destination: Int
  public var weight: Int
  
  init(source: Int, destination: Int, weight: Int) {
    self.source = source
    self.destination = destination
    self.weight = weight
  }
}

struct Node {
  public var value: Int
  public var weight: Int
  
  init(value: Int, weight: Int) {
    self.value = value
    self.weight = weight
  }
}

struct Graph {
  
  // adjacency list
  private var adjList: [[Node]]
  
  // our initializer takes in an array of [Edge] then creates a matrix of nodes
  init(edges: [Edge]) {
    // creates empty buckets e.g [[], [], [], []] based on how many edges (connnections)
    self.adjList = Array(repeating: [Node](), count: edges.count)
    
    for edge in edges {
      // create a node for the edge
      let node = Node(value: edge.destination, weight: edge.weight)
      
      // append the a new node for the source vertex
      adjList[edge.source].append(node)
    }
  }
  
  public func printGraph() {
    for source in 0..<adjList.count {
      for edge in adjList[source] {
        print("\(source) ---> \(edge.value) (\(edge.weight))", terminator: " ")
      }
      print()
    }
  }
}


/*
          (6)
     0---------1
     |       / |  \(3)
  (8)| (7)/    |    \
     |  /      |(1) / 2
     |/        |  /(11)
     4---------3/
         (10)
 */

let edges = [
  Edge(source: 0, destination: 1, weight: 6),
  Edge(source: 0, destination: 4, weight: 8),
  
  Edge(source: 1, destination: 0, weight: 6),
  Edge(source: 1, destination: 2, weight: 3),
  Edge(source: 1, destination: 4, weight: 7),
  Edge(source: 1, destination: 3, weight: 1),
  
  Edge(source: 2, destination: 1, weight: 3),
  Edge(source: 2, destination: 3, weight: 11),
  
  
  Edge(source: 3, destination: 1, weight: 1),
  Edge(source: 3, destination: 2, weight: 11),
  Edge(source: 3, destination: 4, weight: 10),


  Edge(source: 4, destination: 0, weight: 8),
  Edge(source: 4, destination: 1, weight: 7),
  Edge(source: 4, destination: 3, weight: 10),
]

var graph = Graph(edges: edges)

graph.printGraph()

/*
 0 ---> 1 (6) 0 ---> 4 (8)
 1 ---> 0 (6) 1 ---> 2 (3) 1 ---> 4 (7) 1 ---> 3 (1)
 2 ---> 1 (3) 2 ---> 3 (11)
 3 ---> 1 (1) 3 ---> 2 (11) 3 ---> 4 (10)
 4 ---> 0 (8) 4 ---> 1 (7) 4 ---> 3 (10)
*/
```

## Resources

1. [Wikipedia - Graph data type](https://en.wikipedia.org/wiki/Graph_(abstract_data_type))
2. [AlgoDaily - Graphs](https://algodaily.com/lessons/implementing-graphs-edge-list-adjacency-list-adjacency-matrix)
3. [GeeksForGeeks - Graph Data Structure and Algorithms](https://www.geeksforgeeks.org/graph-data-structure-and-algorithms/)
4. [Comparison between Adjacency List and Adjacency Matrix representation of Graph](https://www.geeksforgeeks.org/comparison-between-adjacency-list-and-adjacency-matrix-representation-of-graph/)
5. [Real-life applications](https://leapgraph.com/graph-data-structures-applications/)
