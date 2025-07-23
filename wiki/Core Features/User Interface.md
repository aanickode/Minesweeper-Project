<details>
<summary>Relevant source files</summary>

The following file was used as context for generating this wiki page:

- [Minesweeper.java](https://github.com/aanickode/Minesweeper-Project/blob/main/Minesweeper.java)
</details>

# User Interface

## Introduction

The Minesweeper game project provides a command-line interface for playing the classic Minesweeper game. The user interface is implemented within the `Minesweeper` class, which handles the game logic, board generation, and user interactions. The class provides methods for flipping tiles, resetting the game, and displaying the game board and outcome.

## Game Board Representation

The game board is represented by a 2D array of `Tile` objects, where each `Tile` represents a single cell on the board. The `Tile` class encapsulates the state of a cell, including whether it is a bomb, the number of adjacent bombs, and its flipped state.

```java
private Tile[][] board;
```

Sources: [Minesweeper.java:8]()

## Game Initialization

The `Minesweeper` class provides two constructors:

1. `Minesweeper()`: This constructor initializes a new game by calling the `reset()` method.
2. `Minesweeper(boolean b)`: This constructor is used for testing purposes and calls the `resetForTest()` method.

Both `reset()` and `resetForTest()` methods create a new 8x8 board, generate bombs and safe tiles, and initialize the game state.

```java
public Minesweeper() {
    reset();
}

public Minesweeper(boolean b) {
    resetForTest();
}
```

Sources: [Minesweeper.java:13-20]()

## Bomb Generation

The `generateBombs()` and `generateBombsforTest()` methods are responsible for generating the game board with bombs and safe tiles. The `generateBombs()` method randomly places 10 bombs on the board, while `generateBombsforTest()` places all bombs in the first row for testing purposes.

```java
public Tile[][] generateBombs() {
    // ... (implementation omitted for brevity)
}

public Tile[][] generateBombsforTest() {
    // ... (implementation omitted for brevity)
}
```

Sources: [Minesweeper.java:73-117]()

## Tile Flipping

The `flip(int r, int c)` method is responsible for flipping a tile at the specified row and column coordinates. It performs the following actions:

1. Check if the game is over or the tile is already flipped.
2. Flip the tile and add it to the `flipTrack` list.
3. If the flipped tile is a bomb, set the game over state to -1 (loss).
4. If the flipped tile is safe and has no adjacent bombs, recursively flip all neighboring tiles.
5. Decrement the `safeTiles` counter.
6. If all safe tiles have been flipped, set the game over state to 1 (win).

```java
public boolean flip(int r, int c) {
    // ... (implementation omitted for brevity)
}
```

Sources: [Minesweeper.java:23-46]()

## Tile Unflipping

The `unflip()` method allows the player to unflip the most recently flipped tile if it was a bomb. This method is useful for continuing the game after a loss. It performs the following actions:

1. Get the last flipped tile from the `flipTrack` list.
2. Check if the tile is flipped and is a bomb.
3. If the conditions are met, unflip the tile, remove it from the `flipTrack` list, and reset the game over state to 0 (ongoing).

```java
public boolean unflip() {
    // ... (implementation omitted for brevity)
}
```

Sources: [Minesweeper.java:49-62]()

## Game State and Outcome

The `Minesweeper` class provides methods to retrieve the current game state and display the outcome:

- `gameResult()`: Returns the current game over state (-1 for loss, 0 for ongoing, 1 for win).
- `gameOutcome()`: Prints the current game over state.
- `getSafeTiles()`: Returns the number of remaining safe tiles.
- `safeTileSize()`: Prints the number of remaining safe tiles.

```java
public int gameResult() {
    return gameOver;
}

public void gameOutcome() {
    String s = String.valueOf(gameOver);
    System.out.println(s);
}

public int getSafeTiles() {
    return safeTiles;
}

public void safeTileSize() {
    System.out.print(" " + safeTiles);
}
```

Sources: [Minesweeper.java:120-137]()

## Board Printing

The `printBoard()` method prints the game board with the number of adjacent bombs for each tile. This method is useful for debugging and testing purposes.

```java
public void printBoard() {
    for (int row = 0; row < board.length; row++) {
        for (int col = 0; col < board[0].length; col++) {
            System.out.print(" " + board[row][col].getNumBombs());
        }
        System.out.println();
    }
}
```

Sources: [Minesweeper.java:139-147]()

## Main Method

The `main()` method in the `Minesweeper` class provides an example of how to interact with the game. It creates a new `Minesweeper` instance, prints the board, flips several tiles, and prints the game outcome.

```java
public static void main(String[] args) {
    Minesweeper t = new Minesweeper();
    t.printBoard();
    t.flip(7, 0);
    t.flip(7, 1);
    // ... (additional flips omitted for brevity)
    t.gameOutcome();
    t.flip(7, 7);
    t.gameOutcome();
}
```

Sources: [Minesweeper.java:150-163]()

## Conclusion

The `Minesweeper` class provides a command-line user interface for playing the Minesweeper game. It handles game initialization, board generation, tile flipping, game state tracking, and outcome display. The class is designed to be extensible and can be integrated with a graphical user interface or other input/output mechanisms.