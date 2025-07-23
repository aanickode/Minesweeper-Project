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

The "Game Logic" module is responsible for managing the core gameplay mechanics and rules of the Minesweeper game. It handles the game board initialization, tile flipping, bomb placement, win/loss conditions, and game state management. The game follows a Model-View-Controller (MVC) design pattern, where the `Minesweeper` class serves as the model, `GameBoard` acts as the view and controller, and `Game` sets up the top-level GUI.

Sources: [Game.java](https://github.com/aanickode/Minesweeper-Project/blob/main/Game.java), [GameBoard.java](https://github.com/aanickode/Minesweeper-Project/blob/main/GameBoard.java), [Minesweeper.java](https://github.com/aanickode/Minesweeper-Project/blob/main/Minesweeper.java)

## Game Board Initialization

The game board is initialized with a fixed size of 8x8 tiles. The `Minesweeper` class creates a 2D array of `Tile` objects, representing the game board.

```java
private Tile[][] board = new Tile[8][8];
```

Sources: [Minesweeper.java:10](https://github.com/aanickode/Minesweeper-Project/blob/main/Minesweeper.java#L10)

During the initialization process, the `reset()` method is called, which performs the following steps:

1. Creates a new 2D array of `Tile` objects.
2. Randomly places 10 bombs on the board.
3. Sets the number of neighboring bombs for each non-bomb tile.

```mermaid
graph TD
    A[reset()] --> B[Create new 2D Tile array]
    B --> C[Place 10 random bombs]
    C --> D[Set neighboring bomb counts]
```

Sources: [Minesweeper.java:18-38](https://github.com/aanickode/Minesweeper-Project/blob/main/Minesweeper.java#L18-L38)

## Tile Flipping

When the user clicks on a tile, the `flip()` method in the `Minesweeper` class is called with the row and column indices of the clicked tile. The method performs the following actions:

```mermaid
graph TD
    A[flip(row, col)] --> B[Check if tile is already flipped]
    B -->|No| C[Check if tile is a bomb]
    C -->|No| D[Flip tile]
    D --> E[Recursively flip neighbors if tile has 0 bombs around]
    C -->|Yes| F[Game over, return -1]
    B -->|Yes| G[Return 0]
```

If the clicked tile is not a bomb, it is flipped, and if it has zero neighboring bombs, all its neighbors are recursively flipped as well. If the clicked tile is a bomb, the game ends with a loss.

Sources: [Minesweeper.java:40-65](https://github.com/aanickode/Minesweeper-Project/blob/main/Minesweeper.java#L40-L65)

## Game State and Win/Loss Conditions

The `gameResult()` method in the `Minesweeper` class checks the current game state and returns an integer value representing the game's outcome:

- 0: Game is ongoing
- 1: Player has won
- -1: Player has lost

```java
public int gameResult() {
    // Check if all non-bomb tiles are flipped
    for (int row = 0; row < 8; row++) {
        for (int col = 0; col < 8; col++) {
            if (!board[row][col].isBomb() && !board[row][col].isFlipped()) {
                return 0; // Game is ongoing
            }
        }
    }

    // If all non-bomb tiles are flipped, player wins
    return 1;
}
```

Sources: [Minesweeper.java:67-79](https://github.com/aanickode/Minesweeper-Project/blob/main/Minesweeper.java#L67-L79)

The `GameBoard` class updates the game status based on the result from `gameResult()` and displays the appropriate message to the user.

Sources: [GameBoard.java:121-134](https://github.com/aanickode/Minesweeper-Project/blob/main/GameBoard.java#L121-L134)

## Undo Functionality

The game provides an "Undo" button that allows the user to undo the last move. The `unflip()` method in the `Minesweeper` class is responsible for this functionality:

```mermaid
graph TD
    A[unflip()] --> B[Check if any tiles are flipped]
    B -->|No| C[Return false]
    B -->|Yes| D[Find last flipped tile]
    D --> E[Unflip last flipped tile]
    E --> F[Return true]
```

If there are flipped tiles, the method finds the last flipped tile and unflips it, returning `true` to indicate a successful undo operation. Otherwise, it returns `false`.

Sources: [Minesweeper.java:81-95](https://github.com/aanickode/Minesweeper-Project/blob/main/Minesweeper.java#L81-L95)

## Game Timer and Move Counter

The `GameBoard` class manages a game timer and a move counter to track the player's progress. The timer starts when the game begins and stops when the game ends (win or loss). The move counter increments with each valid tile flip.

```java
private int timerDelay;
private final Timer myTimer;
private long startTime;
private long gameTime;
private int numMoves;
```

The timer is implemented using a `Timer` object and an `ActionListener`. The `numMoves` variable keeps track of the number of moves made by the player.

Sources: [GameBoard.java:24-26](https://github.com/aanickode/Minesweeper-Project/blob/main/GameBoard.java#L24-L26), [GameBoard.java:72-80](https://github.com/aanickode/Minesweeper-Project/blob/main/GameBoard.java#L72-L80)

## Leaderboard and High Scores

The game maintains a leaderboard that displays the top 5 high scores based on the time taken to win the game and the number of moves made. The high scores are stored in a file named `FastestTime.txt`.

```java
private TreeMap<Integer, Integer> scores;
private LinkedList<Integer> highscores = new LinkedList<Integer>();
```

The `updateScores()` method reads the `FastestTime.txt` file and populates the `scores` `TreeMap` with the time taken (key) and the number of moves (value) for each game.

The `updateHighScores()` method sorts the `scores` `TreeMap` by time and selects the top 5 entries to populate the `highscores` list.

The `toStringHighScores()` method converts the `highscores` list into a formatted string for display in the leaderboard panel.

Sources: [GameBoard.java:29-30](https://github.com/aanickode/Minesweeper-Project/blob/main/GameBoard.java#L29-L30), [GameBoard.java:165-209](https://github.com/aanickode/Minesweeper-Project/blob/main/GameBoard.java#L165-L209)

## Game Board Rendering

The `GameBoard` class is responsible for rendering the game board and updating the game status. The `paintComponent()` method draws the grid lines, flipped tiles (with bomb or number indicators), and unflipped tiles.

```java
@Override
public void paintComponent(Graphics g) {
    super.paintComponent(g);

    // Draws board grid
    // ...

    // Draws X's and O's
    for (int row = 0; row < 8; row++) {
        for (int col = 0; col < 8; col++) {
            Tile tile = t.getTile(row, col);
            if (tile.isFlipped() && !tile.isBomb()) {
                String s = String.valueOf(tile.getNumBombs());
                g.drawString(s, 50 * col + 25, 50 * row + 25);
            }
            if (tile.isFlipped() && tile.isBomb()) {
                g.drawLine(50 * col, 50 * row, 50 * col + 50, 50 * row + 50);
                g.drawLine(50 * col, 50 * row + 50, 50 * col + 50, 50 * row);
            }
        }
    }
}
```

Sources: [GameBoard.java:137-165](https://github.com/aanickode/Minesweeper-Project/blob/main/GameBoard.java#L137-L165)

## Conclusion

The "Game Logic" module is the core component of the Minesweeper game, responsible for managing the game board, handling user interactions, enforcing game rules, and tracking the game state. It follows the Model-View-Controller design pattern, separating the game logic from the user interface and providing a structured approach to game development.

Sources: [Game.java](https://github.com/aanickode/Minesweeper-Project/blob/main/Game.java), [GameBoard.java](https://github.com/aanickode/Minesweeper-Project/blob/main/GameBoard.java), [Minesweeper.java](https://github.com/aanickode/Minesweeper-Project/blob/main/Minesweeper.java), [Tile.java](https://github.com/aanickode/Minesweeper-Project/blob/main/Tile.java)