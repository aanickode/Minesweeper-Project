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

The "Game Logic" module is responsible for managing the core gameplay mechanics and rules of the Minesweeper game. It handles the game board initialization, tile flipping, bomb placement, win/loss conditions, and game state management. The primary classes involved in this module are `Minesweeper`, `GameBoard`, and `Tile`.

Sources: [Minesweeper.java](), [GameBoard.java](), [Tile.java]()

## Game Board Initialization

The game board is initialized with a fixed size of 8x8 tiles. The `Minesweeper` class creates a 2D array of `Tile` objects, representing the game board. It then randomly places 10 bombs on the board, ensuring that no tile starts with a bomb initially.

```java
// Minesweeper.java
public Minesweeper() {
    board = new Tile[8][8];
    initBoard();
    placeBombs();
}
```

After initializing the board, the `initBoard()` method creates `Tile` objects for each position and sets their neighbors. The `placeBombs()` method randomly selects 10 tiles and marks them as bombs.

Sources: [Minesweeper.java:15-27](), [Minesweeper.java:39-55](), [Tile.java:10-19](), [Tile.java:23-42]()

## Tile Flipping

When the player clicks on a tile, the `GameBoard` class handles the event and updates the game state accordingly. The `flip()` method in `Minesweeper` is called with the row and column indices of the clicked tile.

```java
// GameBoard.java
t.flip(p.y / 50, p.x / 50);
```

The `flip()` method in `Minesweeper` performs the following actions:

1. If the clicked tile is a bomb, the game ends with a loss.
2. If the clicked tile is not a bomb and has not been flipped before, it flips the tile and recursively flips all neighboring tiles with zero bombs around them.
3. If all non-bomb tiles have been flipped, the game ends with a win.

```mermaid
graph TD
    A[flip(row, col)] --> B{Tile is bomb?}
    B -->|Yes| C[Game Over - Loss]
    B -->|No| D{Tile already flipped?}
    D -->|Yes| E[Do nothing]
    D -->|No| F[Flip tile]
    F --> G[Get neighbors]
    G --> H{Any neighbor is bomb?}
    H -->|Yes| I[Set numBombs]
    H -->|No| J[Recursively flip neighbors]
    J --> K{All non-bomb tiles flipped?}
    K -->|Yes| L[Game Over - Win]
    K -->|No| M[Continue game]
```

Sources: [Minesweeper.java:57-92](), [Tile.java:23-42](), [Tile.java:44-51]()

## Game State Management

The `GameBoard` class manages the overall game state, including the timer, move count, and win/loss conditions. It updates the status label and handles the reset and undo functionality.

```java
// GameBoard.java
private void updateStatus() {
    int gameStatus = t.gameResult();
    if (gameStatus == 1) {
        myTimer.stop();
        status.setText("You won!!!  Time: " + gameTime + "  NumMoves: " + (numMoves + 1));
    } else if (gameStatus == -1) {
        myTimer.stop();
        status.setText("You lost   Time: " + gameTime + "  NumMoves: " + (numMoves + 1));
    }
}
```

The `reset()` method resets the game board, timer, and move count, and updates the high scores. The `undo()` method allows the player to undo the last move if possible.

```java
// GameBoard.java
public void reset() {
    if (t.gameResult() == 1) {
        write();
    }
    updateScores();
    updateHighScores();
    leaderBoard.setText(toStringHighScores());
    t.reset();
    startTime = System.currentTimeMillis();
    gameTime = 0;
    myTimer.restart();
    numMoves = 0;
    repaint();
    requestFocusInWindow();
}

public void undo() {
    if (t.unflip()) {
        numMoves += 2;
        myTimer.start();
    }
    repaint();
    requestFocusInWindow();
}
```

Sources: [GameBoard.java:123-144](), [GameBoard.java:146-156](), [GameBoard.java:158-174](), [Minesweeper.java:94-105](), [Minesweeper.java:107-118]()

## High Scores

The game keeps track of high scores based on the time taken to win and the number of moves made. The high scores are stored in a text file (`FastestTime.txt`) and loaded into a `TreeMap` data structure.

```java
// GameBoard.java
public void updateScores() {
    BufferedReader br = null;
    boolean hasValue = true;
    TreeMap<Integer, Integer> temp = new TreeMap<Integer, Integer>();
    try {
        br = new BufferedReader(new FileReader("Files/FastestTime.txt"));
    } catch (FileNotFoundException e) {
        System.out.println("File not found");
        hasValue = false;
    }
    // ... (read and parse file contents into temp TreeMap)
    scores = temp;
}
```

The `updateHighScores()` method sorts the scores and selects the top 5 to display on the leaderboard.

```java
// GameBoard.java
public void updateHighScores() {
    Object[] temp = scores.keySet().toArray();
    LinkedList<Integer> tempscores = new LinkedList<Integer>();
    Arrays.sort(temp);
    int i = 0;
    while (i < 5 && i < temp.length) {
        int x = (int) temp[i];
        tempscores.add(x);
        i++;
    }
    highscores = tempscores;
}
```

Sources: [GameBoard.java:177-212](), [GameBoard.java:214-227](), [Files/FastestTime.txt]()

## Conclusion

The "Game Logic" module is responsible for managing the core gameplay mechanics of the Minesweeper game. It handles the game board initialization, tile flipping, bomb placement, win/loss conditions, and game state management. The module also keeps track of high scores based on the time taken to win and the number of moves made. The primary classes involved in this module are `Minesweeper`, `GameBoard`, and `Tile`.

Sources: [Minesweeper.java](), [GameBoard.java](), [Tile.java]()