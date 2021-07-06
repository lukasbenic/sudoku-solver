# Sudoku Solver 

---

## Formulation of the Problem

---

For a given **9x9 sudoku**: 
- the set of variables **(X)** represents the squares 
- The domain **(Di)** for each variable is then numbers **1 - 9**.
- The set of constraints **(C)** are:
    - Each cell can only contain a single value
    - It is not allowed to change values in any non-empty cells from the input sudoku
    - Each row can only contain numbers 1-9 once (no duplicates)
    - Each column can only contain numbers 1-9 once (no duplicates)
    - Each square (3x3) can only contain numbers 1-9 once (no duplicates)
    
It is therefore possible to classify this problem as a **constraint satisfaction** problem. Hence, I implemented functions to satisfy these constraints as we want to reach a state with a **complete assignment** that is **consistent**. Meaning that each states with **partial assignment** also has to satisfy the constraints i.e be **consistent**. This pointed me in the direction of implementing a **depth-first-search (DFS)** that I combined with the aforementioned constraint satisfaction, resulting in the **backtracking** algorithm.


## Choice of Algorithm

---

### Backtracking

I chose to implement the backtracking algorithm as it has **time complexity of O(9^(n*n))** with early pruning. Meaning that it performs well for most of the sudokus in all difficulties with some exceptions in the hard category as they have more empty squares that have to be filled so the upper bound of the time complexity remains the same as for a naive algorithm. This is based on the fact that the algorithm will recurse a lot more for the hard category so time taken to solve the sudoku is increased exponentially for the that difficulty.

The way the algorithm works is that each state with partial assignments represents a certain point in the recursion tree and when we reach a dead end on a branch we backtrack and try a different number for the previous state until we reach a complete and consistent assignment returning the solved sudoku or returning a 9x9 numpy array filled with -1s if no solution exists or input sudoku is invalid.

### Input Sudoku Validity 

I also implemented a function that validates if an input sudoku is valid, which if evaluates to **true** runs the backtracking algorithm, otherwise returns a **9x9 numpy array filled with -1 in each cell**. The reason behind this is that sudokus of difficulty easy and above can contain invalid sudokus. This will not increase the time complexity any noticeable amount and therefore the speed of the code will remain relatively unaffected as the time complexity of this is lower than the backtracking algorithm.

### Result 

The resulting agent created can solve most sudokus with exceptions in all difficulties expcept for **hard** as the backtrackings upper bound  compexity is quite high and a solution to this would be to implement **Algorithm X** that uses **Dancing Links** for the hard difficulty.


## Function List 

---

#####  backtracking(sudoku: np.ndarray) -> bool:
    - Function that attempts to solve sudoku and returns true if solution exists otherwise returns false if no solution
##### sudoku_solver(sudoku: np.ndarray) -> np.ndarray
    - If solution exists returns the solved sudoku, otherwise returns 9x9 numpy array filled with -1s
##### valid_row_col(sudoku: np.ndarray) -> bool:
    - returns true if row and column constraints are met for initial input sudoku and false otherwise
##### valid_box(sudoku: np.ndarray) -> bool:
    - returns true if box constraint is met for initial input sudoku, otherwise false 
##### find_empty(sudoku: np.ndarray) -> (int, int)
    - returns row and column of next empty cell (contains a 0), if at the end of sudoku returns None, None
##### valid_num(sudoku: np.ndarray, num: int, row: int, col: int) -> bool
    - returns true if all constrains are met otherwise false
