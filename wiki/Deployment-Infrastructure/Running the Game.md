<details>
<summary>Relevant source files</summary>

The following file was used as context for generating this wiki page:

- [Minesweeper.java](https://github.com/aanickode/Minesweeper-Project/blob/main/Minesweeper.java)

</details>

# Running the Game

## Introduction

The `Minesweeper` class is the core component of the Minesweeper game implementation. It manages the game board, tile flipping, bomb generation, and game state tracking. The class provides methods to initialize the game, flip tiles, reset the game, and retrieve game status information.

## Game Board Initialization

### Board Generation

The game board is represented as a 2D array of `Tile` objects, initialized in the `generateBombs()` and `generateBombsforTest()` methods.

```java
public Tile[][] generateBombs() {
    // ...
    // Generate 10 bomb tiles randomly
    // Generate remaining safe tiles
    // Set neighbors and bomb counts for each tile
    return board;
}

public Tile[][] generateBombsforTest() {
    // ...
    // Generate bombs in the first row for testing purposes
    // Generate remaining safe tiles
    // Set neighbors and bomb counts for each tile
    return board;
}
```

The `generateBombs()` method randomly places 10 bomb tiles on the 8x8 board, while `generateBombsforTest()` places all bombs in the first row for testing purposes.

Sources: [Minesweeper.java:52-92]()

### Game Initialization

The game is initialized in the `reset()` and `resetForTest()` methods, which create a new board and reset the game state.

```java
public void reset() {
    board = new Tile[8][8];
    safeTiles = 0;
    board = generateBombs();
    gameOver = 0;
    flipTrack = new LinkedList<Tile>();
}

public void resetForTest() {
    board = new Tile[8][8];
    safeTiles = 0;
    board = generateBombsforTest();
    gameOver = 0;
    flipTrack = new LinkedList<Tile>();
}
```

The `flipTrack` is a linked list that keeps track of the tiles flipped during the game.

Sources: [Minesweeper.java:36-43, 45-52]()

## Tile Flipping

The `flip(int r, int c)` method is responsible for flipping a tile at the given row and column coordinates.

```java
public boolean flip(int r, int c) {
    if (board[r][c].isFlipped() || gameOver == -1 || gameOver == 1) {
        return false;
    }
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
    return true;
}
```

The method performs the following steps:

1. Check if the tile is already flipped, or if the game is already over. If so, return `false`.
2. Flip the tile and add it to the `flipTrack`.
3. If the flipped tile is a bomb, set `gameOver` to -1 (loss).
4. If the flipped tile is safe and has no neighboring bombs, recursively flip all its neighbors.
5. Decrement the `safeTiles` count.
6. If there are no safe tiles remaining, set `gameOver` to 1 (win).
7. Return `true` to indicate a successful flip.

Sources: [Minesweeper.java:15-38]()

### Recursive Neighbor Flipping

When a safe tile with no neighboring bombs is flipped, the `flip()` method recursively flips all its neighbors.

```java
if (board[r][c].getNumBombs() == 0) {
    ArrayList<Tile> neighbors = board[r][c].getNeighbors();
    for (Tile t : neighbors) {
        int x = t.getXPos();
        int y = t.getYPos();
        flip(x, y);
    }
}
```

This behavior is achieved by retrieving the neighbors of the current tile using `board[r][c].getNeighbors()`, and then calling `flip(x, y)` for each neighbor tile.

Sources: [Minesweeper.java:28-32]()

## Unflipping Tiles

The `unflip()` method allows the player to unflip the most recently flipped tile if it was a bomb, effectively undoing the loss condition.

```java
public boolean unflip() {
    Tile tile = flipTrack.getLast();
    if (!tile.isFlipped()) {
        return false;
    }
    if (tile.isBomb()) {
        tile.unflipTile();
        flipTrack.remove(tile);
        gameOver = 0;
        return true;
    }
    return false;
}
```

The method retrieves the last flipped tile from the `flipTrack`, checks if it is a bomb, and if so, unflips the tile, removes it from the `flipTrack`, and resets the `gameOver` state to 0 (ongoing game).

Sources: [Minesweeper.java:40-51]()

## Game State Tracking

The `Minesweeper` class maintains the game state using the following variables:

- `gameOver`: An integer representing the game outcome (-1 for loss, 0 for ongoing game, 1 for win).
- `safeTiles`: The number of remaining safe tiles on the board.
- `flipTrack`: A linked list that keeps track of the tiles flipped during the game.

The `gameResult()` method returns the current value of `gameOver`, indicating the game outcome.

```java
public int gameResult() {
    return gameOver;
}
```

The `getSafeTiles()` method returns the number of remaining safe tiles.

```java
public int getSafeTiles() {
    return safeTiles;
}
```

Sources: [Minesweeper.java:6-8, 93, 97]()

## Board Printing and Debugging

The `Minesweeper` class provides methods for printing the game board and debugging purposes.

### Printing the Board

The `printBoard()` method prints the entire game board, displaying the number of neighboring bombs for each tile.

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

Sources: [Minesweeper.java:99-106]()

### Printing Game Outcome

The `gameOutcome()` method prints the current value of `gameOver`, indicating the game outcome.

```java
public void gameOutcome() {
    String s = String.valueOf(gameOver);
    System.out.println(s);
}
```

Sources: [Minesweeper.java:109-112]()

### Printing Safe Tile Count

The `safeTileSize()` method prints the number of remaining safe tiles.

```java
public void safeTileSize() {
    System.out.print(" " + safeTiles);
}
```

Sources: [Minesweeper.java:115-118]()

## Main Method

The `main()` method in the `Minesweeper` class provides an example of how to run the game and demonstrates various method calls.

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

1. A new `Minesweeper` instance is created.
2. The initial board is printed using `printBoard()`.
3. A series of tile flips are performed on the last row.
4. The game outcome is printed using `gameOutcome()`.
5. Another tile flip is performed, and the game outcome is printed again.

This `main()` method is primarily for testing and demonstration purposes and may not represent the actual game flow in a production environment.

Sources: [Minesweeper.java:120-132]()