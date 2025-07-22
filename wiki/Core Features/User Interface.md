<details>
<summary>Relevant source files</summary>

The following file was used as context for generating this wiki page:

- [Minesweeper.java](https://github.com/aanickode/Minesweeper-Project/blob/main/Minesweeper.java)
</details>

# User Interface

## Introduction

The Minesweeper project is a Java implementation of the classic Minesweeper game. The user interface (UI) is primarily text-based and is handled within the `Minesweeper` class. The class provides methods for interacting with the game board, flipping tiles, resetting the game, and displaying the board state. While the current implementation lacks a graphical user interface (GUI), the core game logic and board manipulation functionality are encapsulated within this class.

## Game Board Representation

The game board is represented as a 2D array of `Tile` objects, stored in the `board` field of the `Minesweeper` class. Each `Tile` object represents a single cell on the board and contains information about its state (flipped or not), whether it is a bomb, and the number of neighboring bombs.

```java
private Tile[][] board;
```

The `Tile` class is likely a separate class that encapsulates the state and behavior of individual tiles on the board. The `Minesweeper` class interacts with the `Tile` objects to manipulate the board state and game logic.

## Game Initialization

The `Minesweeper` class provides two constructors:

1. `Minesweeper()`: This constructor initializes a new game by calling the `reset()` method.
2. `Minesweeper(boolean b)`: This constructor is likely used for testing purposes and calls the `resetForTest()` method.

Both `reset()` and `resetForTest()` methods create a new 8x8 board and generate the bomb positions using the `generateBombs()` and `generateBombsforTest()` methods, respectively.

```java
public Minesweeper() {
    reset();
}

public Minesweeper(boolean b) {
    resetForTest();
}
```

The `generateBombs()` method randomly places 10 bombs on the board and initializes the remaining tiles as safe tiles. It then calculates the number of neighboring bombs for each tile using the `setNeighbors()` and `findNumBombs()` methods of the `Tile` class.

```java
public Tile[][] generateBombs() {
    // ... (implementation omitted for brevity)
}
```

The `generateBombsforTest()` method is likely used for testing purposes and places all bombs in the first row of the board.

```java
public Tile[][] generateBombsforTest() {
    // ... (implementation omitted for brevity)
}
```

Sources: [Minesweeper.java:10-22](), [Minesweeper.java:26-29](), [Minesweeper.java:33-74](), [Minesweeper.java:78-119]()

## Tile Flipping

The `flip(int r, int c)` method is responsible for flipping a tile at the specified row and column coordinates. It performs the following actions:

1. Check if the tile is already flipped or if the game is over. If so, return `false`.
2. Flip the tile by calling the `flipTile()` method of the `Tile` class.
3. Add the flipped tile to the `flipTrack` list for tracking purposes.
4. If the flipped tile is a bomb, set the `gameOver` flag to -1 (indicating a loss).
5. If the flipped tile is not a bomb and has no neighboring bombs, recursively flip all neighboring tiles by calling `flip(x, y)` for each neighbor.
6. Decrement the `safeTiles` counter if the flipped tile is not a bomb.
7. If all safe tiles have been flipped, set the `gameOver` flag to 1 (indicating a win).
8. Return `true` if the flip operation was successful.

```java
public boolean flip(int r, int c) {
    // ... (implementation omitted for brevity)
}
```

Sources: [Minesweeper.java:124-149]()

## Unflipping Tiles

The `unflip()` method allows the player to unflip the most recently flipped tile, but only if that tile is a bomb. This method is useful when the player accidentally flips a bomb and wants to continue playing. The method performs the following actions:

1. Get the last flipped tile from the `flipTrack` list.
2. Check if the tile is flipped. If not, return `false`.
3. If the tile is a bomb, unflip it by calling the `unflipTile()` method of the `Tile` class, remove it from the `flipTrack` list, and set the `gameOver` flag to 0 (indicating the game is still in progress).
4. Return `true` if the unflip operation was successful, `false` otherwise.

```java
public boolean unflip() {
    // ... (implementation omitted for brevity)
}
```

Sources: [Minesweeper.java:152-165]()

## Game State and Result

The `Minesweeper` class provides methods to retrieve and display the current game state and result:

- `gameResult()`: Returns the value of the `gameOver` flag, which indicates the game result (-1 for loss, 0 for in progress, 1 for win).
- `printBoard()`: Prints the board with the number of neighboring bombs for each tile.
- `getSafeTiles()`: Returns the number of remaining safe tiles.
- `gameOutcome()`: Prints the value of the `gameOver` flag, indicating the game outcome.
- `safeTileSize()`: Prints the number of remaining safe tiles.
- `getBoard()`: Returns the 2D array of `Tile` objects representing the game board.

These methods can be used to display the game state and result to the user or for testing and debugging purposes.

Sources: [Minesweeper.java:168-185](), [Minesweeper.java:188-192]()

## Main Method

The `main` method in the `Minesweeper` class is likely used for testing and demonstration purposes. It creates a new `Minesweeper` instance, prints the board, flips several tiles, and displays the game outcome.

```java
public static void main(String[] args) {
    Minesweeper t = new Minesweeper();
    t.printBoard();
    t.flip(7, 0);
    t.flip(7, 1);
    // ... (additional tile flips omitted for brevity)
    t.gameOutcome();
    t.flip(7, 7);
    t.gameOutcome();
}
```

Sources: [Minesweeper.java:195-208]()

## Conclusion

The `Minesweeper` class in the provided source file encapsulates the core logic and functionality of the Minesweeper game. It handles game initialization, board generation, tile flipping, game state tracking, and basic text-based output. While the current implementation lacks a graphical user interface, it provides a solid foundation for the game mechanics and can be extended or integrated with a GUI component for a more user-friendly experience.