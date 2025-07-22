<details>
<summary>Relevant source files</summary>

The following file was used as context for generating this wiki page:

- [Minesweeper.java](https://github.com/aanickode/Minesweeper-Project/blob/main/Minesweeper.java)
</details>

# User Interface

## Introduction

The Minesweeper game is a classic puzzle game where the player aims to uncover all safe tiles on a grid while avoiding hidden mines. This project's implementation of the Minesweeper game focuses on the game logic and does not include a graphical user interface (GUI). Instead, it provides a text-based interface for interacting with the game board and tracking the game state.

The main entry point for the game is the `Minesweeper` class, which encapsulates the game board, game state, and methods for manipulating the board and tracking the game's progress.

## Game Board Representation

The game board is represented as a 2D array of `Tile` objects, where each `Tile` represents a single cell on the board. The `Tile` class contains information about the tile's state (flipped or unflipped), whether it contains a bomb, and the number of neighboring bombs.

```java
private Tile[][] board;
```

Source: [Minesweeper.java:10]()

## Game State Tracking

The `Minesweeper` class maintains several variables to track the game state:

- `gameOver`: An integer value representing the game's outcome. `0` indicates the game is ongoing, `-1` indicates a loss (a bomb was flipped), and `1` indicates a win (all safe tiles have been flipped).
- `safeTiles`: An integer value representing the number of remaining safe tiles (tiles without bombs) on the board.
- `flipTrack`: A `LinkedList` that keeps track of the tiles that have been flipped during the game. This is used for the `unflip` functionality.

```java
private int gameOver;
private int safeTiles;
private LinkedList<Tile> flipTrack;
```

Source: [Minesweeper.java:12-14]()

## Game Initialization

The `Minesweeper` class provides two constructors:

1. `Minesweeper()`: This constructor initializes a new game by calling the `reset()` method.
2. `Minesweeper(boolean b)`: This constructor is used for testing purposes and calls the `resetForTest()` method.

Both `reset()` and `resetForTest()` methods create a new 8x8 game board, generate bombs and safe tiles, and initialize the game state variables.

```java
public Minesweeper() {
    reset();
}

public Minesweeper(boolean b) {
    resetForTest();
}
```

Source: [Minesweeper.java:18-23]()

The `generateBombs()` method is responsible for generating the game board with 10 randomly placed bombs and 54 safe tiles. It also calculates the number of neighboring bombs for each tile and sets up the neighbor relationships between tiles.

```java
public Tile[][] generateBombs() {
    // ... (implementation omitted for brevity)
}
```

Source: [Minesweeper.java:60-87]()

The `generateBombsforTest()` method is a separate implementation used for testing purposes, where the bombs are placed in a specific pattern (the first row) for easier testing.

```java
public Tile[][] generateBombsforTest() {
    // ... (implementation omitted for brevity)
}
```

Source: [Minesweeper.java:90-112]()

## Tile Flipping

The `flip(int r, int c)` method is responsible for flipping a tile at the specified row and column coordinates. It performs the following actions:

1. Check if the game is over or if the tile has already been flipped. If so, return `false`.
2. Flip the tile and add it to the `flipTrack` list.
3. If the flipped tile is a bomb, set `gameOver` to `-1` (loss).
4. If the flipped tile is safe and has no neighboring bombs, recursively flip all its neighbors.
5. Decrement the `safeTiles` count.
6. If all safe tiles have been flipped, set `gameOver` to `1` (win).
7. Return `true` to indicate a successful flip operation.

```java
public boolean flip(int r, int c) {
    // ... (implementation omitted for brevity)
}
```

Source: [Minesweeper.java:26-48]()

## Unflipping Tiles

The `unflip()` method allows the player to unflip the most recently flipped tile, but only if that tile contained a bomb. This functionality is useful for continuing the game after a loss. The method performs the following steps:

1. Get the last flipped tile from the `flipTrack` list.
2. Check if the tile is flipped and contains a bomb.
3. If both conditions are met, unflip the tile, remove it from `flipTrack`, and set `gameOver` to `0` (ongoing game).
4. Return `true` if the unflip operation was successful, `false` otherwise.

```java
public boolean unflip() {
    // ... (implementation omitted for brevity)
}
```

Source: [Minesweeper.java:51-59]()

## Game Result and Board Printing

The `Minesweeper` class provides methods for retrieving the current game result (`gameResult()`) and printing the game board (`printBoard()`). The `printBoard()` method displays the number of neighboring bombs for each tile on the board.

```java
public int gameResult() {
    return gameOver;
}

public void printBoard() {
    // ... (implementation omitted for brevity)
}
```

Source: [Minesweeper.java:115-124](), [Minesweeper.java:128]()

Additionally, there are helper methods for retrieving the number of remaining safe tiles (`getSafeTiles()`), printing the game outcome (`gameOutcome()`), and printing the number of safe tiles remaining (`safeTileSize()`).

```java
public int getSafeTiles() {
    return safeTiles;
}

public void gameOutcome() {
    String s = String.valueOf(gameOver);
    System.out.println(s);
}

public void safeTileSize() {
    System.out.print(" " + safeTiles);
}
```

Source: [Minesweeper.java:131-140]()

## Main Method

The `main` method in the `Minesweeper` class demonstrates the usage of the game by creating a new instance of the `Minesweeper` class, printing the board, flipping several tiles, and printing the game outcome.

```java
public static void main(String[] args) {
    Minesweeper t = new Minesweeper();
    t.printBoard();
    t.flip(7, 0);
    t.flip(7, 1);
    t.flip(7, 2);
    t.flip(7, 3);
    t.flip(7, 4);
    t.flip(7, 5);
    t.flip(7, 6);
    t.gameOutcome();
    t.flip(7, 7);
    t.gameOutcome();
}
```

Source: [Minesweeper.java:143-155]()

## Conclusion

The Minesweeper project provides a text-based implementation of the classic Minesweeper game. The `Minesweeper` class encapsulates the game board, game state, and methods for manipulating the board and tracking the game's progress. The `Tile` class represents individual cells on the board and stores information about their state and neighboring bombs. The project focuses on the game logic and does not include a graphical user interface.