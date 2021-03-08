# Number of Islands 

[LeetCode](https://leetcode.com/problems/number-of-islands/)

```swift 
class Solution {
    
    /*
    **main function 
    create counter
    iterate through the grid
      if the cell is equal to "1"
        perform dfs on the cell using a helper function 
        keep count of how many islands we have from helper function
    return number of islands
    
    ** helper function (dfs)
    perform dfs on current cell 
      check for array boundaries and if cell has been visited 
        if false return 0 
    run dfs recursively 
      return 1
    */
    
    
    func numIslands(_ grid: [[Character]]) -> Int {
        var count = 0 
        var grid = grid // mutable copy of grid
        // iterate through the grid 
        for row in 0..<grid.count {
            for col in 0..<grid[row].count {
                if grid[row][col] == "1" {
                    // call dfs function
                    count += dfs(&grid, row, col)
                }
            }
        }
        return count
    }
    
    func dfs(_ grid: inout [[Character]], _ row: Int, _ col: Int) -> Int {
        let height = grid.count
        let length = grid[0].count
        
        // check boundaries 
        if row < 0 || col < 0 || row >= height || col >= length || grid[row][col] == "0" {
            return 0
        }
        
        // mark cell as visited 
        // 1. use a boolean array initialized with "false" values and mark "true" if 
        //    if current cell is visited 
        // 2. mark current cell with an arbitrary value e.g that's anything but "1"
        grid[row][col] = "0"
        
        // call dfs recursively
        dfs(&grid, row + 1, col)
        dfs(&grid, row - 1, col) 
        dfs(&grid, row, col + 1)
        dfs(&grid, row, col - 1)
        
        return 1
    }
}
```
