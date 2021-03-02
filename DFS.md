# Depth-first Search 

## Objectives 

* Depth-first search on a Graph 
* Depth-first search on a 2-D Array 

## 1. Depth-first search on a Graph 

```swift 
0-----------1
|           |
|           |
|           |
|           |
3-----------2

// 0 3 2 1
```

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
let graph = [
  [1, 2, 3, 4,],
  [5, 6, 7, 8],
  [9, 10, 11, 12],
  [13, 14, 15, 16]
]

// 1 5 9 13 14 10 6 2 3 7 11 15 16 12 8 4
```

```swift 
func dfs(_ grid: [[Int]]) {
  // we need to modify grid so make a mutable copy
  var grid = grid
  
  // call our helper dfs recursive function
  dfsUtils(&grid, 0, 0)
}

func dfsUtils(_ grid: inout [[Int]], _ row: Int, _ col: Int) {
  // we need to set up boundaries for example:
  //   have we seen this cell before?
  //   is the row within limits of the array
  //   is the col withing limits of the array
  
  let height = grid.count // number of rows in grid
  let length = grid[0].count // number of elements in a row
  
  if row < 0 || col < 0 || row >= height || col >= length || grid[row][col] == 0 {
    return
  }
  
  // print the cell
  print("\(grid[row][col])", terminator: " ")
  
  // mark the cell as visited
  // two ways to keep track of visited nodes or cells
  // 1. visited array [[Bool]]
  // 2. mark the cell with an arbitrary value
  grid[row][col] = 0
  
  // perform dfs on each direction in the grid recursively
  dfsUtils(&grid, row + 1, col)
  dfsUtils(&grid, row - 1, col)
  dfsUtils(&grid, row, col + 1)
  dfsUtils(&grid, row, col - 1)
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

## Where to go from here? 

#### Graph Notes: Review
* review adjacency list vs adjacency matrix
* revisit binary tree traversals - dfs and bfs
* read up on disconnected and connected graphs - https://www.tutorialspoint.com/connected-vs-disconnected-graphs
* review dfs on a graph and a matrix
* challenge: number of islands [(LeetCode - 200. Number of Islands)](https://leetcode.com/problems/number-of-islands/)

