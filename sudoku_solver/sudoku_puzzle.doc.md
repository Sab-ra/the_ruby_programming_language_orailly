# Sudoku Puzzle

This code appears to be a Ruby module Sudoku that implements a Sudoku puzzle solver. Here's a breakdown of its structure and functionality:

1. Module Structure:
* The module Sudoku contains three classes: Puzzle, Invalid, and Impossible.
* It also includes two methods: scan and solve.


1. Class Puzzle:
* This class handles the creation and manipulation of Sudoku puzzles.
* It initializes the puzzle by parsing input lines and converting them into a grid representation.
* It ensures the grid has the correct size and does not contain illegal characters.
* The to_s method converts the grid back to a human-readable string.
* Methods like dup, [], and []= help in duplicating the puzzle, accessing specific cells, and updating cell values.
* Methods like each_unknown, has_duplicates?, and possible are used to iterate through the puzzle, check for duplicate values, and determine possible values for cells.


1. Error Classes:
* Invalid and Impossible are custom exception classes that are raised for specific error conditions (Invalid for incorrect puzzle states and Impossible for unsolvable puzzles).


5. Sudoku Solver:
* Sudoku.scan and Sudoku.solve are methods used for solving Sudoku puzzles.
* Sudoku.scan iterates through the puzzle, attempting to solve it by finding cells with only one possible value.
* Sudoku.solve is a recursive method that tries to solve the puzzle by making educated guesses and backtracking if necessary. It uses scan internally to make initial guesses and tries different values for empty cells.
The code employs various helper methods (rowdigits, coldigits, boxdigits) to assist in analyzing rows, columns, and boxes within the Sudoku grid.

Overall, this code encapsulates the logic required to represent, validate, and solve Sudoku puzzles in Ruby.

## Class Diagram

``` Mermaid
classDiagram
    class Puzzle {
        - ASCII: String
        - BIN: String
        + initialize(lines)
        + to_s(): String
        + dup(): Puzzle
        + [](row, col): Integer
        + []=(row, col, newvalue)
        + each_unknown()
        + has_duplicates?(): Boolean
        + possible(row, col, box): Array
        - rowdigits(row): Array
        - coldigits(col): Array
        - boxdigits(b): Array
    }

    class Invalid
    class Impossible

    class Sudoku {
        + scan(puzzle)
        + solve(puzzle)
    }

    Puzzle --|> Invalid
    Puzzle --|> Impossible
    Sudoku --|> Puzzle

```

This representation illustrates the relationships between the classes Puzzle, Invalid, Impossible, and Sudoku. The Puzzle class contains various methods and private helper functions to handle Sudoku puzzles, while Invalid and Impossible are used as custom exception classes. Sudoku is a module containing methods to scan and solve Sudoku puzzles, interacting with the Puzzle class.

## Sequence Diagram

This simplified diagram focuses on the interaction between the Sudoku module's solve method and the Puzzle class:

``` Mermaid
sequenceDiagram
    participant Sudoku
    participant Puzzle

    activate Sudoku
    Puzzle->>Sudoku: solve(puzzle)
    activate Puzzle
    alt Puzzle is solved
        Puzzle-->>Sudoku: Puzzle
    else Puzzle is not solvable
        alt Impossible condition
            Puzzle--x Sudoku: Impossible
        else Guessing required
            loop for each guess
                Puzzle->>Puzzle: make a guess
                Puzzle-->>Sudoku: Puzzle with guess
                alt Guess leads to solvable puzzle
                    Puzzle-->>Sudoku: Solved Puzzle
                else Guess doesn't solve puzzle
                    Puzzle--x Puzzle: Undo guess
                end
            end
        end
    end

    deactivate Puzzle
    deactivate Sudoku

```

This sequence diagram demonstrates the flow of the solve method from the Sudoku module, interacting with the Puzzle class. It shows a high-level overview of the solving process, including the possibilities of reaching a solved puzzle, encountering an unsolvable scenario (Impossible condition), or requiring educated guesses to progress towards a solution.

## State Machine Diagram

``` Mermaid
stateDiagram
    [*] --> Unsolved

    state Unsolved {
        [*] --> Scan
        Scan --> Solved: Puzzle is solvable
        Scan --> Guess: Guess required
        Guess --> Solved: Puzzle solved with guess
        Guess --> Guess: Make another guess
        Solved --> [*]: Puzzle is solved
    }

```

This state machine diagram represents a simplified view of the Puzzle class' states during the Sudoku solving process. Initially, the puzzle starts in the Unsolved state. It transitions to the Scan state where the code attempts to solve the puzzle directly or moves to the Guess state if guessing is required. From the Guess state, the process might loop through making guesses until the puzzle is solved, transitioning to the Solved state. Finally, when the puzzle is solved, it goes back to the initial state.