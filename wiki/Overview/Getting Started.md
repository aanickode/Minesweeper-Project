<details>
<summary>Relevant source files</summary>

The following file was used as context for generating this wiki page:

- [Minesweeper.java](https://github.com/aanickode/Minesweeper-Project/blob/main/Minesweeper.java)

</details>

# Getting Started

## Introduction

The `Minesweeper` class is the core component of a Minesweeper game implementation in Java. It manages the game board, tile flipping, bomb generation, and game state tracking. The class provides methods to initialize the game, flip tiles, reset the game, and retrieve game information.

## Game Board and Tiles

The game board is represented by a 2D array of `Tile` objects, where each `Tile` represents a position on the board. The `Tile` class (not shown) likely contains properties and methods to track the tile's state (e.g., flipped, bomb, number of adjacent bombs).

```java
private Tile[][] board;
```

Source: [Minesweeper.java:10]()

## Game State

The `Minesweeper` class maintains several variables to track the game state:

- `gameOver`: An integer representing the game outcome (-1 for loss, 0 for ongoing, 1 for win).
- `safeTiles`: The number of remaining safe (non-bomb) tiles on the board.
- `flipTrack`: A linked list that keeps track of the order in which tiles were flipped, used for the "unflip" functionality.

```java
private int gameOver;
private int safeTiles;
private LinkedList<Tile> flipTrack;
```

Source: [Minesweeper.java:12-14]()

## Game Initialization

The `Minesweeper` class provides two constructors:

1. `Minesweeper()`: Initializes a new game by calling the `reset()` method.
2. `Minesweeper(boolean b)`: Initializes a new game for testing purposes by calling the `resetForTest()` method.

```java
public Minesweeper() {
    reset();
}

public Minesweeper(boolean b) {
    resetForTest();
}
```

Source: [Minesweeper.java:17-23]()

The `reset()` method initializes a new 8x8 game board, generates 10 bombs and 54 safe tiles, sets up the neighbors and bomb counts for each tile, and resets the game state variables.

```java
public void reset() {
    board = new Tile[8][8];
    safeTiles = 0;
    board = generateBombs();
    gameOver = 0;
    flipTrack = new LinkedList<Tile>();
}
```

Source: [Minesweeper.java:47-53]()

The `resetForTest()` method is similar to `reset()`, but it generates bombs in a specific pattern (all bombs on the first row) to facilitate testing.

```java
public void resetForTest() {
    board = new Tile[8][8];
    safeTiles = 0;
    board = generateBombsforTest();
    gameOver = 0;
    flipTrack = new LinkedList<Tile>();
}
```

Source: [Minesweeper.java:56-62]()

## Bomb Generation

The `generateBombs()` method generates 10 random bomb tiles and the remaining safe tiles on the 8x8 board. It also sets up the neighbors and bomb counts for each tile.

```java
public Tile[][] generateBombs() {
    Random rand = new Random();
    int count = 0;
    while (count < 10) {
        int row = rand.nextInt(8);
        int col = rand.nextInt(8);
        if (board[row][col] == null) {
            Tile tile = new Tile(row, col, board, true);
            board[row][col] = tile;
            count++;
        }
    }
    for (int row = 0; row < 8; row++) {
        for (int col = 0; col < 8; col++) {
            if (board[row][col] == null) {
                Tile tile = new Tile(row, col, board, false);
                board[row][col] = tile;
                safeTiles++;
            }
        }
    }
    for (int row = 0; row < 8; row++) {
        for (int col = 0; col < 8; col ++) {
            board[row][col].setNeighbors();
            board[row][col].setNumBombs(board[row][col].findNumBombs());
        }
    }
    return board;
}
```

Source: [Minesweeper.java:65-92]()

The `generateBombsforTest()` method generates bombs in a specific pattern (all bombs on the first row) for testing purposes.

```java
public Tile[][] generateBombsforTest() {
    int row = 0;
    for (int col = 0; col < 8; col++) {
        if (board[row][col] == null) {
            Tile tile = new Tile(row, col, board, true);
            board[row][col] = tile;
        }
    }
    for (int row1 = 0; row1 < 8; row1++) {
        for (int col = 0; col < 8; col++) {
            if (board[row1][col] == null) {
                Tile tile = new Tile(row1, col, board, false);
                board[row1][col] = tile;
                safeTiles++;
            }
        }
    }
    for (int row1 = 0; row1 < 8; row1++) {
        for (int col = 0; col < 8; col ++) {
            board[row1][col].setNeighbors();
            board[row1][col].setNumBombs(board[row1][col].findNumBombs());
        }
    }
    return board;
}
```

Source: [Minesweeper.java:95-116]()

## Tile Flipping

The `flip(int r, int c)` method flips a tile at the specified row and column positions. It performs the following actions:

1. Check if the game is over or if the tile is already flipped. If so, return `false`.
2. Flip the tile and add it to the `flipTrack` list.
3. If the flipped tile is a bomb, set `gameOver` to -1 (loss).
4. If the flipped tile is safe and has no adjacent bombs, recursively flip all its neighbors.
5. Decrement the `safeTiles` count.
6. If all safe tiles have been flipped, set `gameOver` to 1 (win).
7. Return `true` to indicate a successful flip.

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

Source: [Minesweeper.java:26-45]()

## Unflipping Tiles

The `unflip()` method allows the player to "unflip" the most recently flipped tile, but only if that tile is a bomb. This functionality enables the player to continue playing even after losing the game.

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

Source: [Minesweeper.java:118-129]()

## Game Information

The `Minesweeper` class provides several methods to retrieve information about the game state:

- `getTile(int x, int y)`: Returns the `Tile` object at the specified row and column positions.
- `gameResult()`: Returns the current game outcome (-1 for loss, 0 for ongoing, 1 for win).
- `printBoard()`: Prints the board with all tile values (likely for debugging purposes).
- `getSafeTiles()`: Returns the number of remaining safe tiles.
- `gameOutcome()`: Prints the game outcome (-1, 0, or 1).
- `safeTileSize()`: Prints the number of remaining safe tiles.
- `getBoard()`: Returns the 2D array representing the game board.

```java
public Tile getTile(int x, int y) {
    return board[x][y];
}

public int gameResult() {
    return gameOver;
}

public void printBoard() {
    for (int row = 0; row < board.length; row++) {
        for (int col = 0; col < board[0].length; col++) {
            System.out.print(" " + board[row][col].getNumBombs());
        }
        System.out.println();
    }
}

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

public Tile[][] getBoard() {
    return board;
}
```

Sources: [Minesweeper.java:131-132](), [Minesweeper.java:134-143](), [Minesweeper.java:145-146](), [Minesweeper.java:148-150](), [Minesweeper.java:152-153](), [Minesweeper.java:155-156]()

## Main Method

The `main` method in the `Minesweeper` class demonstrates the usage of the class by creating a new game instance, printing the board, flipping several tiles, and printing the game outcome.

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

Source: [Minesweeper.java:158-170]()

## Conclusion

The `Minesweeper` class provides the core functionality for a Minesweeper game, including game board initialization, bomb generation, tile flipping, game state tracking, and information retrieval. It serves as the central component for managing the game logic and state.