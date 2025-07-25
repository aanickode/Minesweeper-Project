<details>
<summary>Relevant source files</summary>

The following file was used as context for generating this wiki page:

- [Minesweeper.java](https://github.com/aanickode/Minesweeper-Project/blob/main/Minesweeper.java)

</details>

# Running the Game

## Introduction

The `Minesweeper` class is the core component of the Minesweeper game implementation. It manages the game board, tile flipping, game state, and other essential functionalities. This wiki page covers the process of running the game, including board initialization, tile interactions, game outcome determination, and relevant data structures and methods.

## Game Board Initialization

The game board is represented by a 2D array of `Tile` objects, where each `Tile` represents a cell on the board. The board is initialized in the `generateBombs()` and `generateBombsforTest()` methods.

### Board Generation

The `generateBombs()` method is responsible for generating the game board with randomly placed bombs and safe tiles.

```java
public Tile[][] generateBombs() {
    // ...
    // Randomly place 10 bombs on the board
    // ...
    // Fill remaining tiles as safe tiles
    // ...
    // Set neighbors and number of adjacent bombs for each tile
    // ...
    return board;
}
```

The `generateBombsforTest()` method is a variation of `generateBombs()` that places all bombs in the first row, making it easier for testing purposes.

```java
public Tile[][] generateBombsforTest() {
    // ...
    // Place all bombs in the first row
    // ...
    // Fill remaining tiles as safe tiles
    // ...
    // Set neighbors and number of adjacent bombs for each tile
    // ...
    return board;
}
```

Sources: [Minesweeper.java:68-103](), [Minesweeper.java:106-132]()

## Tile Flipping

The `flip(int r, int c)` method is responsible for flipping a tile at the specified row and column coordinates.

```java
public boolean flip(int r, int c) {
    // ...
    // Check if the tile is already flipped or the game is over
    // ...
    // Flip the tile and add it to the flip track
    // ...
    // If the tile is a bomb, end the game
    // ...
    // If the tile is safe and has no adjacent bombs, recursively flip neighboring tiles
    // ...
    // Decrement the safe tile count
    // ...
    // If all safe tiles are flipped, end the game as a win
    // ...
    return true;
}
```

The method also handles the recursive flipping of neighboring tiles if the flipped tile is safe and has no adjacent bombs.

Sources: [Minesweeper.java:23-48]()

## Unflipping Tiles

The `unflip()` method allows the player to unflip the most recently flipped tile, but only if it was a bomb. This functionality enables the player to continue playing even after losing the game.

```java
public boolean unflip() {
    // ...
    // Get the last flipped tile
    // ...
    // If the tile is a bomb, unflip it and reset the game state
    // ...
    return true;
}
```

Sources: [Minesweeper.java:51-62]()

## Game State Management

The `Minesweeper` class maintains the game state using the following variables:

- `gameOver`: An integer representing the game outcome (-1 for loss, 0 for ongoing, 1 for win).
- `safeTiles`: The number of remaining safe tiles on the board.
- `flipTrack`: A linked list that keeps track of the flipped tiles.

The game state is updated based on the tile flipping actions and the remaining safe tiles.

Sources: [Minesweeper.java:10-12](), [Minesweeper.java:23-48](), [Minesweeper.java:51-62]()

## Game Reset

The `reset()` and `resetForTest()` methods are used to reset the game board and game state.

```java
public void reset() {
    // ...
    // Initialize a new board
    // Reset safe tile count
    // Generate bombs and safe tiles
    // Reset game state and flip track
}

public void resetForTest() {
    // ...
    // Initialize a new board
    // Reset safe tile count
    // Generate bombs in the first row for testing
    // Reset game state and flip track
}
```

Sources: [Minesweeper.java:65-67](), [Minesweeper.java:104-105]()

## Game Outcome

The `gameOutcome()` method prints the current game outcome (-1 for loss, 0 for ongoing, 1 for win).

```java
public void gameOutcome() {
    System.out.println(gameOver);
}
```

Sources: [Minesweeper.java:145-147]()

## Utility Methods

The `Minesweeper` class also provides utility methods for retrieving tiles, the game board, the number of remaining safe tiles, and printing the board for debugging purposes.

```java
public Tile getTile(int x, int y) { ... }
public Tile[][] getBoard() { ... }
public int getSafeTiles() { ... }
public void printBoard() { ... }
public void safeTileSize() { ... }
```

Sources: [Minesweeper.java:140-142](), [Minesweeper.java:148-151](), [Minesweeper.java:153-161]()

## Main Method

The `main` method in the `Minesweeper` class demonstrates how to run the game by creating a new `Minesweeper` instance, flipping tiles, and printing the game outcome.

```java
public static void main(String[] args) {
    Minesweeper t = new Minesweeper();
    t.printBoard();
    // Flip several tiles
    t.gameOutcome();
    // Flip another tile
    t.gameOutcome();
}
```

Sources: [Minesweeper.java:163-175]()

## Conclusion

The `Minesweeper` class provides the core functionality for running the Minesweeper game, including board initialization, tile flipping, game state management, and outcome determination. It utilizes various data structures and methods to handle the game logic and interactions. Understanding the implementation details covered in this wiki page is crucial for developers working on or extending the Minesweeper project.