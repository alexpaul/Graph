# Graph

A Graph is an abstract data structure that connects nodes or vertices via edges. Those edges can be weighted, example the price of an airline ticket. 

<pre> 
          $440 
New York -----> Saint Lucia 
</pre> 

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
* Directed Graph - direction 
* Undirected - bi-directional
* Graph implemenations: Edge list, Adjacency Matrix, Adjacency List

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
struct Edge {
  var source: Int
  var destination: Int
  var weight: Int
  
  init(source: Int, destination: Int, weight: Int) {
    self.source = source
    self.destination = destination
    self.weight = weight
  }
}

struct Node {
  var value: Int
  var weight: Int
  
  init(value: Int, weight: Int) {
    self.value = value
    self.weight = weight
  }
}

struct Graph {
  var adjList: [[Node]]
  
  init(edges: [Edge]) {
    self.adjList = Array(repeating: [Node](), count: edges.count)
    for edge in edges {
      let node = Node(value: edge.destination, weight: edge.weight)
      adjList[edge.source].append(node)
    }
  }
  
  func printGraph() {
    let n = self.adjList.count
    var source = 0
    
    while source < n {
      for edge in self.adjList[source] {
        print(source.description + " ---> " + edge.value.description + " (\(edge.weight))", terminator: " ")
      }
      print()
      source += 1
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

let edges = [Edge(source: 0, destination: 1, weight: 6),
             Edge(source: 0, destination: 4, weight: 7),
             
             Edge(source: 1, destination: 0, weight: 6),
             Edge(source: 1, destination: 2, weight: 6),
             Edge(source: 1, destination: 3, weight: 6),
             Edge(source: 1, destination: 4, weight: 6),
             
             Edge(source: 2, destination: 1, weight: 6),
             Edge(source: 2, destination: 3, weight: 6),
             
             Edge(source: 3, destination: 1, weight: 6),
             Edge(source: 3, destination: 2, weight: 6),
             Edge(source: 3, destination: 4, weight: 6),
             
             Edge(source: 4, destination: 0, weight: 6),
             Edge(source: 4, destination: 1, weight: 6),
             Edge(source: 4, destination: 3, weight: 6),
]


let graph = Graph(edges: edges)
graph.printGraph()


/*
 0 ---> 1 (6) 0 ---> 4 (7)
 1 ---> 0 (6) 1 ---> 2 (6) 1 ---> 3 (6) 1 ---> 4 (6)
 2 ---> 1 (6) 2 ---> 3 (6)
 3 ---> 1 (6) 3 ---> 2 (6) 3 ---> 4 (6)
 4 ---> 0 (6) 4 ---> 1 (6) 4 ---> 3 (6)
*/
```

## Resouces 

1. [Wikipedia - Graph data type](https://en.wikipedia.org/wiki/Graph_(abstract_data_type))
2. [AlgoDaily - Graphs](https://algodaily.com/lessons/implementing-graphs-edge-list-adjacency-list-adjacency-matrix)
