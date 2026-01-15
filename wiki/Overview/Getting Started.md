<details>
<summary>Relevant source files</summary>

The following file was used as context for generating this wiki page:

- [Minesweeper.java](https://github.com/aanickode/Minesweeper-Project/blob/main/Minesweeper.java)
</details>

# Getting Started

## Introduction

The `Minesweeper` class is the core component of a Minesweeper game implementation in Java. It manages the game board, tile flipping logic, bomb generation, and game state tracking. This class provides methods to initialize the game, flip tiles, reset the game, and retrieve game information.

## Game Board and Tiles

The game board is represented by a 2D array of `Tile` objects, where each `Tile` represents a single cell on the board. The `Tile` class (not shown in the provided code) likely contains properties and methods to track the tile's state (e.g., flipped, bomb, number of adjacent bombs).

### Board Initialization

The `generateBombs()` method is responsible for initializing the game board with bombs and safe tiles. It follows these steps:

1. Randomly place 10 bomb tiles on the board.
2. Fill the remaining tiles as safe tiles.
3. Set the neighbors and calculate the number of adjacent bombs for each tile.

```java
public Tile[][] generateBombs() {
    // ...
    // Place 10 bomb tiles randomly
    // ...

    // Fill remaining tiles as safe tiles
    // ...

    // Set neighbors and calculate adjacent bombs for each tile
    // ...

    return board;
}
```

There is also a `generateBombsforTest()` method that initializes the board with a predefined bomb pattern for testing purposes.

### Tile Flipping

The `flip(int r, int c)` method is the core logic for flipping a tile on the board. It performs the following actions:

1. Check if the game is over or the tile is already flipped.
2. Flip the tile and add it to the `flipTrack` list.
3. If the flipped tile is a bomb, set the game state to "lost" (`gameOver = -1`).
4. If the flipped tile is safe and has no adjacent bombs, recursively flip all its neighbors.
5. Decrement the `safeTiles` counter.
6. If all safe tiles have been flipped, set the game state to "won" (`gameOver = 1`).

```java
public boolean flip(int r, int c) {
    // ...
    board[r][c].flipTile();
    flipTrack.add(board[r][c]);

    if (board[r][c].isBomb()) {
        gameOver = -1;
    } else {
        if (board[r][c].getNumBombs() == 0) {
            ArrayList<Tile> neighbors = board[r][c].getNeighbors();
            for (Tile t : neighbors) {
                int x = t.getXPos();
                int y = t.getYPos();
                flip(x, y);
            }
        }
        safeTiles--;
    }

    if (safeTiles == 0) {
        gameOver = 1;
    }
    // ...
}
```

The `unflip()` method allows the player to unflip the most recently flipped tile if it was a bomb, effectively allowing them to continue playing after losing.

## Game State and Outcome

The `Minesweeper` class tracks the game state using the `gameOver` variable, which can have the following values:

- `0`: Game is in progress
- `-1`: Game is lost (a bomb was flipped)
- `1`: Game is won (all safe tiles have been flipped)

The `gameResult()` method returns the current game state, and the `gameOutcome()` method prints the game outcome.

## Utility Methods

The `Minesweeper` class provides several utility methods:

- `reset()`: Resets the game by generating a new board.
- `getTile(int x, int y)`: Returns the tile at the specified position.
- `printBoard()`: Prints the board with all tile values shown (for debugging purposes).
- `getSafeTiles()`: Returns the number of remaining safe tiles.
- `getBoard()`: Returns the game board (2D array of `Tile` objects).

## Main Method

The `main` method demonstrates the usage of the `Minesweeper` class by creating an instance, printing the board, flipping several tiles, and printing the game outcome.

```java
public static void main(String[] args) {
    Minesweeper t = new Minesweeper();
    t.printBoard();
    t.flip(7, 0);
    t.flip(7, 1);
    // ...
    t.gameOutcome();
    t.flip(7, 7);
    t.gameOutcome();
}
```

Sources: [Minesweeper.java]()