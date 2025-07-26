<details>
<summary>Relevant source files</summary>

The following file was used as context for generating this wiki page:

- [Minesweeper.java](https://github.com/aanickode/Minesweeper-Project/blob/main/Minesweeper.java)
</details>

# User Interface

## Introduction

The Minesweeper project is a Java implementation of the classic Minesweeper game. The user interface (UI) of the game is primarily text-based and is handled within the `Minesweeper` class. The class provides methods for interacting with the game board, flipping tiles, and tracking the game state.

Sources: [Minesweeper.java]()

## Game Board Representation

The game board is represented as a 2D array of `Tile` objects, stored in the `board` field of the `Minesweeper` class. Each `Tile` object represents a single cell on the game board and contains information about its state (flipped or not), whether it is a bomb, and the number of neighboring bombs.

```java
private Tile[][] board;
```

Sources: [Minesweeper.java:10]()

## Game State Tracking

The `Minesweeper` class maintains several fields to track the game state:

- `gameOver`: An integer value representing the game outcome (-1 for loss, 0 for ongoing, 1 for win).
- `safeTiles`: An integer value representing the number of remaining safe (non-bomb) tiles on the board.
- `flipTrack`: A `LinkedList` that keeps track of the tiles that have been flipped during the game.

```java
private int gameOver;
private int safeTiles;
private LinkedList<Tile> flipTrack;
```

Sources: [Minesweeper.java:12-14]()

## Game Initialization

The `Minesweeper` class provides two constructors:

1. `Minesweeper()`: This constructor initializes a new game by calling the `reset()` method.
2. `Minesweeper(boolean b)`: This constructor is used for testing purposes and calls the `resetForTest()` method.

```java
public Minesweeper() {
    reset();
}

public Minesweeper(boolean b) {
    resetForTest();
}
```

Sources: [Minesweeper.java:18-23]()

The `reset()` method initializes the game board, sets the `safeTiles` count, generates the bomb positions, and resets the game state and `flipTrack` list.

```java
public void reset() {
    board = new Tile[8][8];
    safeTiles = 0;
    board = generateBombs();
    gameOver = 0;
    flipTrack = new LinkedList<Tile>();
}
```

Sources: [Minesweeper.java:46-52]()

The `resetForTest()` method is similar to `reset()` but generates the bombs in a specific pattern for testing purposes.

```java
public void resetForTest() {
    board = new Tile[8][8];
    safeTiles = 0;
    board = generateBombsforTest();
    gameOver = 0;
    flipTrack = new LinkedList<Tile>();
}
```

Sources: [Minesweeper.java:54-60]()

## Tile Flipping

The `flip(int r, int c)` method is responsible for flipping a tile at the specified row and column coordinates. It performs the following actions:

1. Check if the tile is already flipped or if the game is over.
2. Flip the tile and add it to the `flipTrack` list.
3. If the flipped tile is a bomb, set `gameOver` to -1 (loss).
4. If the flipped tile is not a bomb and has no neighboring bombs, recursively flip all its neighbors.
5. Decrement the `safeTiles` count.
6. If all safe tiles have been flipped, set `gameOver` to 1 (win).

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

Sources: [Minesweeper.java:26-46]()

## Tile Unflipping

The `unflip()` method allows the player to unflip the most recently flipped tile, but only if that tile is a bomb. This feature enables the player to continue playing even after losing the game.

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

Sources: [Minesweeper.java:62-73]()

## Bomb Generation

The `generateBombs()` method is responsible for generating the game board with bombs and safe tiles. It follows these steps:

1. Randomly place 10 bombs on the board.
2. Fill the remaining tiles with safe (non-bomb) tiles.
3. Set the neighbors and the number of neighboring bombs for each tile.

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

Sources: [Minesweeper.java:75-102]()

The `generateBombsforTest()` method is similar to `generateBombs()` but generates the bombs in a specific pattern for testing purposes.

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

Sources: [Minesweeper.java:104-126]()

## Game State Retrieval

The `Minesweeper` class provides several methods for retrieving information about the game state:

- `getTile(int x, int y)`: Returns the `Tile` object at the specified row and column coordinates.
- `gameResult()`: Returns the current game outcome (-1 for loss, 0 for ongoing, 1 for win).
- `printBoard()`: Prints the game board with the number of neighboring bombs for each tile.
- `getSafeTiles()`: Returns the number of remaining safe (non-bomb) tiles on the board.
- `gameOutcome()`: Prints the current game outcome (-1 for loss, 0 for ongoing, 1 for win).
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

Sources: [Minesweeper.java:128-159]()

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

Sources: [Minesweeper.java:161-174]()

## Conclusion

The `Minesweeper` class provides a text-based user interface for playing the Minesweeper game. It handles game initialization, tile flipping, bomb generation, and game state tracking. The class also provides methods for retrieving information about the game state and printing the game board and outcome.