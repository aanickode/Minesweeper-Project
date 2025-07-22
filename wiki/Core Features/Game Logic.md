<details>
<summary>Relevant source files</summary>

The following files were used as context for generating this wiki page:

- [Game.java](https://github.com/aanickode/Minesweeper-Project/blob/main/Game.java)
- [GameBoard.java](https://github.com/aanickode/Minesweeper-Project/blob/main/GameBoard.java)
- [Tile.java](https://github.com/aanickode/Minesweeper-Project/blob/main/Tile.java)
- [Minesweeper.java](https://github.com/aanickode/Minesweeper-Project/blob/main/Minesweeper.java)
- [Files/FastestTime.txt](https://github.com/aanickode/Minesweeper-Project/blob/main/Files/FastestTime.txt)

</details>

# Game Logic

## Introduction

The "Game Logic" module is responsible for managing the core gameplay mechanics and rules of the Minesweeper game. It handles the game board initialization, tile flipping, bomb placement, win/loss conditions, and game state management. The game follows a Model-View-Controller (MVC) design pattern, where the `Minesweeper` class acts as the model, `GameBoard` serves as the view and controller, and `Game` sets up the top-level GUI components.

Sources: [Game.java](https://github.com/aanickode/Minesweeper-Project/blob/main/Game.java), [GameBoard.java](https://github.com/aanickode/Minesweeper-Project/blob/main/GameBoard.java), [Minesweeper.java](https://github.com/aanickode/Minesweeper-Project/blob/main/Minesweeper.java)

## Game Board Initialization

The game board is initialized with a fixed size of 8x8 tiles. The `Minesweeper` class creates a 2D array of `Tile` objects, representing the game board.

```java
private Tile[][] board = new Tile[8][8];
```

Sources: [Minesweeper.java:14](https://github.com/aanickode/Minesweeper-Project/blob/main/Minesweeper.java#L14)

During the initialization process, the `reset()` method is called, which performs the following steps:

1. Create a new `Tile` object for each position on the board.
2. Randomly place 10 bombs on the board by setting the `bomb` flag of 10 `Tile` objects to `true`.
3. For each non-bomb tile, set its `numBombs` value based on the number of adjacent bomb tiles.

```java
public void reset() {
    // ...
    for (int row = 0; row < 8; row++) {
        for (int col = 0; col < 8; col++) {
            board[row][col] = new Tile(col, row, board, false);
        }
    }
    placeBombs();
    setNumBombs();
    // ...
}
```

Sources: [Minesweeper.java:32-41](https://github.com/aanickode/Minesweeper-Project/blob/main/Minesweeper.java#L32-L41)

## Tile Flipping

When the player clicks on a tile, the `GameBoard` class handles the mouse event and updates the game state accordingly. The `flip()` method in the `Minesweeper` class is called with the row and column indices of the clicked tile.

```java
t.flip(p.y / 50, p.x / 50);
```

Sources: [GameBoard.java:52](https://github.com/aanickode/Minesweeper-Project/blob/main/GameBoard.java#L52)

The `flip()` method performs the following actions:

1. If the clicked tile is a bomb, the game ends with a loss.
2. If the clicked tile is not a bomb and has no adjacent bombs, it recursively flips all neighboring non-bomb tiles.
3. If the clicked tile is not a bomb and has adjacent bombs, it flips the tile and displays the number of adjacent bombs.

```java
public void flip(int row, int col) {
    Tile t = board[row][col];
    if (!t.isFlipped()) {
        if (t.isBomb()) {
            gameOver = -1;
        } else {
            flipTile(row, col);
            if (t.getNumBombs() == 0) {
                flipNeighbors(row, col);
            }
        }
    }
}
```

Sources: [Minesweeper.java:77-89](https://github.com/aanickode/Minesweeper-Project/blob/main/Minesweeper.java#L77-L89)

## Win/Loss Conditions

The game ends when either all non-bomb tiles are flipped (win condition) or a bomb tile is flipped (loss condition). The `gameResult()` method in the `Minesweeper` class checks the current game state and returns an integer value representing the game result:

- `1` for a win
- `-1` for a loss
- `0` for an ongoing game

```java
public int gameResult() {
    return gameOver;
}
```

Sources: [Minesweeper.java:91](https://github.com/aanickode/Minesweeper-Project/blob/main/Minesweeper.java#L91)

The `GameBoard` class updates the game status label based on the result returned by `gameResult()`.

```java
private void updateStatus() {
    int gameStatus = t.gameResult();
    if (gameStatus == 1) {
        // ...
        status.setText("You won!!!  Time: " + gameTime + "  NumMoves: " + (numMoves + 1));
    } else if (gameStatus == -1) {
        // ...
        status.setText("You lost   Time: " + gameTime + "  NumMoves: " + (numMoves + 1));
    }
}
```

Sources: [GameBoard.java:105-116](https://github.com/aanickode/Minesweeper-Project/blob/main/GameBoard.java#L105-L116)

## Game State Management

The `GameBoard` class manages the game state, including the timer, move count, and high score tracking. It provides the following functionality:

### Timer

The game timer is implemented using a `Timer` object from the Swing library. The timer is started when the game begins and stopped when the game ends (win or loss). The elapsed time is displayed in the status label.

```java
timerDelay = 1000;
myTimer = new Timer(timerDelay, gameTimer);
startTime = System.currentTimeMillis();
gameTime = 0;
myTimer.start();
```

Sources: [GameBoard.java:44-48](https://github.com/aanickode/Minesweeper-Project/blob/main/GameBoard.java#L44-L48)

### Move Count

The `numMoves` variable keeps track of the number of moves made by the player. It is incremented whenever a non-winning or non-losing move is made and displayed in the status label.

```java
if (!(t.gameResult() == 1 || t.gameResult() == -1)) {
    numMoves++;
}
```

Sources: [GameBoard.java:54](https://github.com/aanickode/Minesweeper-Project/blob/main/GameBoard.java#L54)

### High Score Tracking

The `GameBoard` class maintains a leaderboard of high scores based on the time taken to win the game and the number of moves made. The high scores are stored in a text file (`FastestTime.txt`) and loaded into a `TreeMap` data structure during game initialization.

```java
public void updateScores() {
    // ...
    try {
        br = new BufferedReader(new FileReader("Files/FastestTime.txt"));
    } catch (FileNotFoundException e) {
        System.out.println("File not found");
        hasValue = false;
    }
    // ...
}
```

Sources: [GameBoard.java:129-135](https://github.com/aanickode/Minesweeper-Project/blob/main/GameBoard.java#L129-L135)

The top 5 high scores are displayed in the leaderboard label at the top of the game window.

```java
public String toStringHighScores() {
    String s = "Top 5 Scores: ";
    for (int i = 0; i < highscores.size(); i++) {
        s += highscores.get(i) + " " + scores.get(highscores.get(i)) + ", ";
    }
    return s;
}
```

Sources: [GameBoard.java:181-187](https://github.com/aanickode/Minesweeper-Project/blob/main/GameBoard.java#L181-L187)

## Undo Functionality

The game provides an "Undo" button that allows the player to undo the last move made. The `undo()` method in the `GameBoard` class calls the `unflip()` method in the `Minesweeper` class to revert the last tile flip.

```java
public void undo() {
    if (t.unflip()) {
        numMoves += 2;
        myTimer.start();
    }
    repaint();
    requestFocusInWindow();
}
```

Sources: [GameBoard.java:95-101](https://github.com/aanickode/Minesweeper-Project/blob/main/GameBoard.java#L95-L101)

The `unflip()` method in the `Minesweeper` class keeps track of the last flipped tile and unflips it if possible.

```java
public boolean unflip() {
    if (lastFlipped != null) {
        lastFlipped.unflipTile();
        lastFlipped = null;
        return true;
    }
    return false;
}
```

Sources: [Minesweeper.java:108-115](https://github.com/aanickode/Minesweeper-Project/blob/main/Minesweeper.java#L108-L115)

## Conclusion

The "Game Logic" module is responsible for managing the core gameplay mechanics of the Minesweeper game. It handles the game board initialization, tile flipping, bomb placement, win/loss conditions, and game state management. The module follows the Model-View-Controller design pattern, with the `Minesweeper` class acting as the model, `GameBoard` serving as the view and controller, and `Game` setting up the top-level GUI components. The game also provides additional features such as a timer, move count, high score tracking, and an undo functionality.