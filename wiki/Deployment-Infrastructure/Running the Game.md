<details>
<summary>Relevant source files</summary>

The following file was used as context for generating this wiki page:

- [Minesweeper.java](https://github.com/aanickode/Minesweeper-Project/blob/main/Minesweeper.java)

</details>

# Running the Game

## Introduction

The `Minesweeper` class is the core component of the Minesweeper game implementation. It manages the game board, tile flipping, game state, and other essential functionalities. This wiki page provides an overview of how to run and interact with the Minesweeper game.

## Game Setup

### Initialization

The game is initialized by creating an instance of the `Minesweeper` class. There are two constructors available:

1. `Minesweeper()`: This constructor sets up a new game with a randomly generated board.
2. `Minesweeper(boolean b)`: This constructor is used for testing purposes and generates a specific board configuration.

```java
// Initialize a new game with a random board
Minesweeper game = new Minesweeper();

// Initialize a game for testing
Minesweeper testGame = new Minesweeper(true);
```

Sources: [Minesweeper.java:14-20]()

### Board Generation

The game board is represented by a 2D array of `Tile` objects, where each `Tile` represents a position on the board. The `generateBombs()` method is responsible for generating the board with 10 randomly placed bombs and 54 safe tiles.

```java
public Tile[][] generateBombs() {
    // ... (implementation omitted for brevity)
}
```

For testing purposes, the `generateBombsforTest()` method generates a specific board configuration with all bombs placed in the first row.

```java
public Tile[][] generateBombsforTest() {
    // ... (implementation omitted for brevity)
}
```

Sources: [Minesweeper.java:58-102]()

## Game Mechanics

### Flipping Tiles

The `flip(int r, int c)` method is used to flip a tile at the specified row and column coordinates. It performs the following actions:

1. Check if the game is already over or if the tile has already been flipped.
2. Flip the tile and add it to the `flipTrack` list.
3. If the flipped tile is a bomb, set the game state to "lost" (`gameOver = -1`).
4. If the flipped tile is safe and has no neighboring bombs, recursively flip all its neighbors.
5. Decrement the `safeTiles` counter.
6. If all safe tiles have been flipped, set the game state to "won" (`gameOver = 1`).

```java
public boolean flip(int r, int c) {
    // ... (implementation omitted for brevity)
}
```

Sources: [Minesweeper.java:22-45]()

### Unflipping Tiles

The `unflip()` method allows the player to unflip the most recently flipped tile, but only if that tile was a bomb. This functionality enables the player to continue playing even after losing the game.

```java
public boolean unflip() {
    // ... (implementation omitted for brevity)
}
```

Sources: [Minesweeper.java:47-56]()

### Game State

The `gameOver` variable keeps track of the game state:

- `gameOver = 0`: Game is in progress.
- `gameOver = -1`: Game is lost (a bomb was flipped).
- `gameOver = 1`: Game is won (all safe tiles have been flipped).

The `gameResult()` method returns the current game state.

```java
public int gameResult() {
    return gameOver;
}
```

Sources: [Minesweeper.java:104-106]()

## Utility Methods

### Printing the Board

The `printBoard()` method prints the game board with all tile values (number of neighboring bombs) revealed.

```java
public void printBoard() {
    // ... (implementation omitted for brevity)
}
```

Sources: [Minesweeper.java:108-115]()

### Getting Tiles

The `getTile(int x, int y)` method retrieves the `Tile` object at the specified row and column coordinates.

```java
public Tile getTile(int x, int y) {
    return board[x][y];
}
```

Sources: [Minesweeper.java:118-120]()

### Remaining Safe Tiles

The `getSafeTiles()` method returns the number of remaining safe tiles that have not been flipped.

```java
public int getSafeTiles() {
    return safeTiles;
}
```

Sources: [Minesweeper.java:122-124]()

### Printing Game Outcome

The `gameOutcome()` method prints the current game state (`gameOver` value).

```java
public void gameOutcome() {
    String s = String.valueOf(gameOver);
    System.out.println(s);
}
```

Sources: [Minesweeper.java:126-129]()

### Printing Safe Tile Count

The `safeTileSize()` method prints the number of remaining safe tiles.

```java
public void safeTileSize() {
    System.out.print(" " + safeTiles);
}
```

Sources: [Minesweeper.java:131-134]()

## Main Method

The `main` method in the `Minesweeper` class demonstrates how to run the game and interact with its methods.

```java
public static void main(String[] args) {
    Minesweeper game = new Minesweeper();
    game.printBoard();
    game.flip(7, 0);
    game.flip(7, 1);
    // ... (additional flips omitted for brevity)
    game.gameOutcome();
    game.flip(7, 7);
    game.gameOutcome();
}
```

This example creates a new game instance, prints the board, flips several tiles, and prints the game outcome after each flip.

Sources: [Minesweeper.java:136-148]()

## Summary

The `Minesweeper` class provides the core functionality for running and interacting with the Minesweeper game. It manages the game board, tile flipping, game state tracking, and various utility methods for printing and retrieving game information. By creating an instance of the `Minesweeper` class and using its methods, developers can implement a user interface or other components to create a complete Minesweeper game experience.