<details>
<summary>Relevant source files</summary>

The following file was used as context for generating this wiki page:

- [Minesweeper.java](https://github.com/aanickode/Minesweeper-Project/blob/main/Minesweeper.java)
</details>

# Running the Game

## Introduction

The `Minesweeper` class is the core component of the Minesweeper game implementation. It manages the game board, tile states, and game logic. The class provides methods to initialize the game, flip tiles, reset the game, and track the game's outcome.

## Game Board Initialization

### Generating the Board

The game board is represented as a 2D array of `Tile` objects, where each tile can either be a safe tile or a bomb. The board is initialized using the `generateBombs()` method, which randomly places 10 bombs on the 8x8 board and creates safe tiles for the remaining positions.

```mermaid
graph TD
    A[generateBombs()] -->|1| B[Create 2D Tile array]
    B --> |2| C[Place 10 random bombs]
    C --> |3| D[Create safe tiles for remaining positions]
    D --> |4| E[Set neighbors and bomb counts for each tile]
    E --> |5| F[Return initialized board]
```

The `generateBombsforTest()` method is a separate method used for testing purposes, which places all bombs in the first row of the board.

Sources: [Minesweeper.java:57-97](), [Minesweeper.java:101-124]()

### Tile Initialization

Each `Tile` object is initialized with its row and column position, a reference to the game board, and a flag indicating whether it is a bomb or a safe tile. The `setNeighbors()` method is called to set the neighboring tiles for each tile, and the `findNumBombs()` method calculates the number of adjacent bombs for safe tiles.

Sources: [Minesweeper.java:57-97](), [Minesweeper.java:101-124]()

## Game Logic

### Flipping Tiles

The `flip(int r, int c)` method is responsible for flipping a tile at the specified row and column. It performs the following actions:

1. Check if the game is over or if the tile is already flipped. If so, return without any action.
2. Flip the tile and add it to the `flipTrack` list for tracking.
3. If the flipped tile is a bomb, set the game over state to a loss (-1).
4. If the flipped tile is a safe tile with no adjacent bombs, recursively flip all its neighboring tiles.
5. Decrement the `safeTiles` counter.
6. If all safe tiles have been flipped, set the game over state to a win (1).

```mermaid
graph TD
    A[flip(r, c)] -->|1| B{Tile already flipped or game over?}
    B -->|Yes| C[Return false]
    B -->|No| D[Flip tile and add to flipTrack]
    D -->|2| E{Tile is bomb?}
    E -->|Yes| F[Set gameOver = -1]
    E -->|No| G{Tile has 0 adjacent bombs?}
    G -->|Yes| H[Recursively flip neighbors]
    H --> I[Decrement safeTiles]
    G -->|No| I
    F --> I
    I -->|3| J{All safe tiles flipped?}
    J -->|Yes| K[Set gameOver = 1]
    J -->|No| L[Return true]
    K --> L
```

Sources: [Minesweeper.java:18-48]()

### Unflipping Tiles

The `unflip()` method allows the player to unflip the most recently flipped tile if it was a bomb, effectively undoing the loss condition. It checks if the last tile in the `flipTrack` list is a bomb, and if so, unflips the tile, removes it from the list, and resets the game over state to 0 (ongoing).

Sources: [Minesweeper.java:51-56]()

### Game Reset

The `reset()` method resets the game by creating a new 8x8 board, resetting the `safeTiles` counter, generating a new board with bombs and safe tiles, clearing the `flipTrack` list, and setting the game over state to 0 (ongoing).

Sources: [Minesweeper.java:59-64](), [Minesweeper.java:101-124]()

## Game State

### Game Outcome

The `gameResult()` method returns the current game over state, which can be -1 (loss), 0 (ongoing), or 1 (win).

Sources: [Minesweeper.java:127-129]()

### Remaining Safe Tiles

The `getSafeTiles()` method returns the number of remaining safe tiles that have not been flipped.

Sources: [Minesweeper.java:133-135]()

## Utility Methods

### Printing the Board

The `printBoard()` method prints the game board, displaying the number of adjacent bombs for each safe tile and a special character for bombs.

Sources: [Minesweeper.java:137-144]()

### Accessing Tiles

The `getTile(int x, int y)` method returns the `Tile` object at the specified row and column position on the board.

Sources: [Minesweeper.java:125-126]()

### Main Method

The `main()` method is a simple example of how to play the game by flipping tiles and checking the game outcome.

Sources: [Minesweeper.java:147-160]()

In summary, the `Minesweeper` class manages the game board, tile states, and game logic for the Minesweeper game. It provides methods to initialize the game, flip tiles, reset the game, and track the game's outcome.