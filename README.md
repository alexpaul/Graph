# Graph

# Graph

A Graph is an abstract data structure that connects nodes or vertices via edges. Those edges can be weighted, example the price of an airline ticket. 

<pre> 
          $440 
New York -----> Saint Lucia 
</pre> 

## Objectives 

* Know graph vocabulary and terminaology
* Adjacency Matrix Implementation 
* Adjacency List Implementation 
* Breadth-First Search on a Graph (BFS)
* Depth-First Search of a Graph (DFS) 

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
  var vertices: Int
  var adjMatrix: [[Bool]]
  
  init(vertices: Int) {
    self.vertices = vertices
    self.adjMatrix = Array(repeating: Array(repeating: false, count: vertices), count: vertices)
  }
  
  mutating func addEdge(source: Int, destination: Int) {
    adjMatrix[source][destination] = true
    adjMatrix[destination][source] = true
  }
  
  func printGraph() {
    var description = ""
    for index in 0..<vertices {
      description.append(index.description + ":")
      for j in adjMatrix[index] {
        description.append(j ? "1" : "0")
        description.append(" ")
      }
      description.append("\n")
    }
    print(description)
  }
}

var graph = Graph(vertices: 5)

graph.addEdge(source: 0, destination: 1)
graph.addEdge(source: 0, destination: 4)
graph.addEdge(source: 1, destination: 4)
graph.addEdge(source: 1, destination: 3)
graph.addEdge(source: 1, destination: 2)
graph.addEdge(source: 2, destination: 3)
graph.addEdge(source: 3, destination: 4)

graph.printGraph()

/*
 0:0 1 0 0 1
 1:1 0 1 1 1
 2:0 1 0 1 0
 3:0 1 1 0 1
 4:1 1 0 1 0
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
