# Depth-first Search 

## Objectives 

* Depth-first search on a Graph 
* Depth-first search on a 2-D Array 

## 1. Depth-first search on a Graph 

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
struct Edge {
  var source: Int
  var destination: Int
  var weight: Int? = nil
}

struct Node {
  var value: Int
  var weight: Int? = nil
}

struct Graph {
  private var adjList: [[Node]]
  private var vertices: Int = 0
  
  init(vertices: Int) {
    self.vertices = vertices
    self.adjList = Array(repeating: [Node](), count: vertices)
  }
  
  init(edges: [Edge]) {
    self.adjList = Array(repeating: [Node](), count: edges.count)
    
    for edge in edges {
      let destinationNode = Node(value: edge.destination)
      
      adjList[edge.source].append(destinationNode)
    }
  }
  
  func dfs(source: Int) {
    var visited: Set<Int> = []
    var stack = [Int]()
    stack.append(source)
    visited.insert(source)
    while !stack.isEmpty {
      let source = stack.removeLast()
      print("\(source)", terminator: " ")
      for neighborNode in adjList[source] {
        if !visited.contains(neighborNode.value) {
          visited.insert(neighborNode.value)
          stack.append(neighborNode.value)
        }
      }
    }
  }
}

let edges = [
  Edge(source: 0, destination: 1),
  Edge(source: 0, destination: 3),
  
  Edge(source: 1, destination: 0),
  Edge(source: 1, destination: 2),
  
  Edge(source: 2, destination: 1),
  Edge(source: 2, destination: 3),
  
  Edge(source: 3, destination: 0),
  Edge(source: 3, destination: 2),
]

var graph = Graph(edges: edges)

graph.dfs(source: 0)
// 0 3 2 1 
```

## 2. Depth-first search on a 2-D Array 

```swift 
func dfs(grid: [[Int]]) {
  let height = grid.count
  guard height > 0 else { return }
  let length = grid[0].count
  
  // create an array and set all values to "false" not yet visited
  var visited = Array(repeating: Array(repeating: false, count: height), count: length)
  
  // start dfs at row 0, col 0
  dfsUtil(grid: grid, row: 0, col: 0, visited: &visited)
}

func dfsUtil(grid: [[Int]], row: Int, col: Int, visited: inout [[Bool]]) {
  let height = grid.count
  let length = grid[0].count
  
  // check for boundaries and if already visited
  if row < 0 || col < 0 || row >= height || col >= length || visited[row][col] {
    return
  }
  
  visited[row][col] = true
  
  print(grid[row][col], terminator: " ")
  
  dfsUtil(grid: grid, row: row + 1, col: col, visited: &visited) // go right
  dfsUtil(grid: grid, row: row - 1, col: col, visited: &visited) // go left
  dfsUtil(grid: grid, row: row, col: col + 1, visited: &visited) // go down
  dfsUtil(grid: grid, row: row, col: col - 1, visited: &visited) // go up
}


let graph = [
  [1, 2, 3, 4,],
  [5, 6, 7, 8],
  [9, 10, 11, 12],
  [13, 14, 15, 16]
]

dfs(grid: graph)

// 1 5 9 13 14 10 6 2 3 7 11 15 16 12 8 4
```
