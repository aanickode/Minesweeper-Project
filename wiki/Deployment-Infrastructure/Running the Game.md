<details>
<summary>Relevant source files</summary>

The following file was used as context for generating this wiki page:

- [Minesweeper.java](https://github.com/aanickode/Minesweeper-Project/blob/main/Minesweeper.java)

</details>

# Running the Game

## Introduction

The Minesweeper class is the core component of the Minesweeper game implementation. It manages the game board, tile flipping logic, game state, and other essential functionalities. This page provides an overview of how the game is run and the key components involved in the process.

## Game Setup

### Board Generation

The game board is represented by a 2D array of `Tile` objects, initialized in the `generateBombs()` and `generateBombsforTest()` methods. These methods randomly place 10 bomb tiles and create the remaining safe tiles on an 8x8 board.

```java
public Tile[][] generateBombs() {
    // ...
    while (count < 10) {
        int row = rand.nextInt(8);
        int col = rand.nextInt(8);
        if (board[row][col] == null) {
            Tile tile = new Tile(row, col, board, true); // Create a bomb tile
            board[row][col] = tile;
            count++;
        }
    }
    // ...
}
```

After placing the bomb tiles, the method creates safe tiles for the remaining positions and sets up the neighbor relationships and bomb counts for each tile.

Sources: [Minesweeper.java:86-117](), [Minesweeper.java:123-147]()

### Game Initialization

The game is initialized in the `Minesweeper` constructor, which calls the `reset()` method to set up a new game board and reset the game state.

```java
public Minesweeper() {
    reset();
}
```

The `reset()` method creates a new 8x8 board, generates the bomb and safe tiles, and initializes the game state variables.

```java
public void reset() {
    board = new Tile[8][8];
    safeTiles = 0;
    board = generateBombs();
    gameOver = 0;
    flipTrack = new LinkedList<Tile>();
}
```

Sources: [Minesweeper.java:25-27](), [Minesweeper.java:76-82]()

## Tile Flipping

The `flip(int r, int c)` method is responsible for flipping a tile at the specified row and column positions. It performs the following actions:

1. Check if the game is already over or if the tile has been flipped before. If so, return without any action.
2. Flip the tile and add it to the `flipTrack` list for tracking.
3. If the flipped tile is a bomb, set the `gameOver` flag to -1 (game lost).
4. If the flipped tile is a safe tile with no neighboring bombs, recursively flip all its neighbors.
5. Decrement the `safeTiles` counter.
6. If all safe tiles have been flipped, set the `gameOver` flag to 1 (game won).

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

Sources: [Minesweeper.java:31-56]()

### Unflipping Tiles

The `unflip()` method allows the player to unflip the most recently flipped tile, but only if it was a bomb tile. This functionality enables the player to continue playing even after losing the game.

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

Sources: [Minesweeper.java:59-71]()

## Game State

The `gameOver` variable keeps track of the game state:

- `gameOver = 0`: Game is in progress
- `gameOver = -1`: Game is lost (a bomb tile was flipped)
- `gameOver = 1`: Game is won (all safe tiles have been flipped)

The `gameResult()` method returns the current value of the `gameOver` variable, indicating the game's outcome.

```java
public int gameResult() {
    return gameOver;
}
```

Sources: [Minesweeper.java:151]()

## Utility Methods

The `Minesweeper` class provides several utility methods for debugging, testing, and accessing game data:

- `printBoard()`: Prints the board with all tile values (bomb counts) shown.
- `getSafeTiles()`: Returns the number of remaining safe tiles.
- `gameOutcome()`: Prints the outcome of the game (0, -1, or 1).
- `safeTileSize()`: Prints the number of remaining safe tiles.
- `getBoard()`: Returns the 2D array representing the game board.

Sources: [Minesweeper.java:154-168](), [Minesweeper.java:171]()

## Main Method

The `main` method in the `Minesweeper` class demonstrates a sample game execution. It creates a new `Minesweeper` instance, prints the board, flips several tiles, and prints the game outcome.

```java
public static void main(String[] args) {
    Minesweeper t = new Minesweeper();
    t.printBoard();
    t.flip(7, 0);
    t.flip(7, 1);
    // ... (flipping more tiles)
    t.gameOutcome();
    t.flip(7, 7);
    t.gameOutcome();
}
```

Sources: [Minesweeper.java:174-186]()

## Conclusion

The `Minesweeper` class orchestrates the game flow by managing the board generation, tile flipping logic, game state tracking, and utility methods for debugging and testing. It serves as the central component for running the Minesweeper game and provides the necessary functionality for players to interact with the game board and track the game's progress.