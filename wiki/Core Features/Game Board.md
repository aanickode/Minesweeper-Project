<details>
<summary>Relevant source files</summary>

The following files were used as context for generating this wiki page:

- [GameBoard.java](https://github.com/aanickode/Minesweeper-Project/blob/main/GameBoard.java)
- [Tile.java](https://github.com/aanickode/Minesweeper-Project/blob/main/Tile.java)
</details>

# Game Board

## Introduction

The "Game Board" is a crucial component of the Minesweeper game project. It represents the graphical user interface (GUI) where the game is displayed and played. The `GameBoard` class extends the `JPanel` class from the Java Swing library, allowing it to be integrated into the application's window. It handles rendering the game board, updating the game status, and responding to user interactions such as mouse clicks.

The game board consists of a grid of tiles, each representing a cell in the Minesweeper game. The tiles can be either safe or contain a mine (bomb). The objective of the game is to uncover all safe tiles without detonating any mines.

Sources: [GameBoard.java](https://github.com/aanickode/Minesweeper-Project/blob/main/GameBoard.java), [Tile.java](https://github.com/aanickode/Minesweeper-Project/blob/main/Tile.java)

## Game Board Initialization

The `GameBoard` class is initialized with a `JLabel` for displaying the game status and another `JLabel` for displaying the leaderboard. It sets up the game board dimensions, creates an instance of the `Minesweeper` model, and adds a mouse listener to handle user clicks.

```java
public GameBoard(JLabel statusInit, JLabel leaderboardInit) {
    // ...
    t = new Minesweeper(); // initializes model for the game
    status = statusInit; // initializes the status JLabel
    leaderBoard = leaderboardInit;
    // ...
}
```

Sources: [GameBoard.java:32-44](https://github.com/aanickode/Minesweeper-Project/blob/main/GameBoard.java#L32-L44)

## Game Board Rendering

The `paintComponent` method is responsible for rendering the game board. It draws the grid lines and displays the tiles based on their state (flipped or not, bomb or not). If a tile is flipped and not a bomb, it displays the number of adjacent bombs. If a tile is flipped and a bomb, it draws an "X" symbol.

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

Sources: [GameBoard.java:100-125](https://github.com/aanickode/Minesweeper-Project/blob/main/GameBoard.java#L100-L125)

## Game Board Interaction

The `GameBoard` class listens for mouse click events using a `MouseAdapter`. When a user clicks on a tile, the `flip` method of the `Minesweeper` model is called with the corresponding row and column indices. The game status is then updated, and the board is repainted.

```java
addMouseListener(new MouseAdapter() {
    @Override
    public void mouseClicked(MouseEvent e) {
        Point p = e.getPoint();
        
        // updates the model given the coordinates of the mouseclick
        t.flip(p.y / 50, p.x / 50);
        if (!(t.gameResult() == 1 || t.gameResult() == -1)) {
            numMoves++;
        }
        updateStatus(); // updates the status JLabel
        repaint(); // repaints the game board
    }
});
```

Sources: [GameBoard.java:38-52](https://github.com/aanickode/Minesweeper-Project/blob/main/GameBoard.java#L38-L52)

## Game Timer

The `GameBoard` class includes a timer that updates the game status label with the elapsed time and the number of moves made by the player. The timer is started when the game board is initialized and stopped when the game is won or lost.

```java
ActionListener gameTimer = new ActionListener() {
    @Override
    public void actionPerformed(ActionEvent e) {
        gameTime = (System.currentTimeMillis() - startTime) / 1000; 
        status.setText("Keep going!   Time: " + gameTime + "  NumMoves: " + numMoves);
    }
};
```

Sources: [GameBoard.java:56-63](https://github.com/aanickode/Minesweeper-Project/blob/main/GameBoard.java#L56-L63)

## Game Reset

The `reset` method is called to reset the game to its initial state. It updates the leaderboard with the current game's score if the game was won, resets the game model, and restarts the timer.

```java
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
    
    // Makes sure this component has keyboard/mouse focus
    requestFocusInWindow();
}
```

Sources: [GameBoard.java:67-84](https://github.com/aanickode/Minesweeper-Project/blob/main/GameBoard.java#L67-L84)

## Undo Move

The `undo` method allows the player to undo the last move. It calls the `unflip` method of the `Minesweeper` model to revert the last tile flip, updates the move count, and repaints the board.

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

Sources: [GameBoard.java:86-92](https://github.com/aanickode/Minesweeper-Project/blob/main/GameBoard.java#L86-L92)

## Game Status Update

The `updateStatus` method updates the game status label based on the current game result (win, lose, or ongoing). If the game is won or lost, the timer is stopped, and the final time and move count are displayed.

```java
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

Sources: [GameBoard.java:94-104](https://github.com/aanickode/Minesweeper-Project/blob/main/GameBoard.java#L94-L104)

## Tile Class

The `Tile` class represents an individual tile on the game board. It encapsulates the state of a tile, including whether it is a bomb, flipped, and the number of adjacent bombs. The `Tile` class provides methods for finding neighboring tiles, determining the number of adjacent bombs, and flipping or unflipping the tile.

```java
public class Tile {
    private int numBombs;
    private boolean flipped;
    private boolean bomb;
    private Tile[][] board;
    private int xPos;
    private int yPos;
    private ArrayList<Tile> neighbors;
    private int displayValue;

    // Constructor, getters, setters, and other methods
    // ...
}
```

Sources: [Tile.java](https://github.com/aanickode/Minesweeper-Project/blob/main/Tile.java)

## Conclusion

The "Game Board" component is a crucial part of the Minesweeper game project. It handles rendering the game board, responding to user interactions, updating the game status, and integrating with the game model (`Minesweeper` class). The `GameBoard` class works in tandem with the `Tile` class to represent and manage the state of individual tiles on the board. Together, these components provide the user interface and gameplay experience for the Minesweeper game.