<details>
<summary>Relevant source files</summary>

The following files were used as context for generating this wiki page:

- [Game.java](https://github.com/aanickode/Minesweeper-Project/blob/main/Game.java)
- [Minesweeper.java](https://github.com/aanickode/Minesweeper-Project/blob/main/Minesweeper.java)
- [Tile.java](https://github.com/aanickode/Minesweeper-Project/blob/main/Tile.java)

</details>

# User Interaction

## Introduction

The "User Interaction" component of this Minesweeper game project handles the graphical user interface (GUI) and user input processing. It provides a visual representation of the game board, displays relevant information, and allows users to interact with the game through various controls and actions.

The main class responsible for setting up the GUI and handling user interactions is `Game.java`. It creates the top-level frame, game board, status panel, instructions panel, score panel, and control buttons. The `GameBoard` class, instantiated within `Game.java`, manages the game board's rendering and user input handling.

Sources: [Game.java](https://github.com/aanickode/Minesweeper-Project/blob/main/Game.java), [Minesweeper.java](https://github.com/aanickode/Minesweeper-Project/blob/main/Minesweeper.java)

## GUI Components

### Top-Level Frame

The top-level frame is created using the `JFrame` class and serves as the main window for the game. It is initialized with the title "Minesweeper" and positioned at specific coordinates on the screen.

```java
final JFrame frame = new JFrame("Minesweeper");
frame.setLocation(400, 400);
```

Sources: [Game.java:24-25](https://github.com/aanickode/Minesweeper-Project/blob/main/Game.java#L24-L25)

### Status Panel

The status panel is a `JPanel` located at the bottom of the frame. It displays the current status of the game using a `JLabel`.

```java
final JPanel status_panel = new JPanel();
frame.add(status_panel, BorderLayout.SOUTH);
final JLabel status = new JLabel("Setting up...");
status_panel.add(status);
```

Sources: [Game.java:29-32](https://github.com/aanickode/Minesweeper-Project/blob/main/Game.java#L29-L32)

### Instructions Panel

The instructions panel is a `JPanel` located on the right side of the frame. It displays the game instructions using a `JLabel` with HTML formatting.

```java
final JPanel instruction_panel = new JPanel();
frame.add(instruction_panel, BorderLayout.EAST);
final JLabel instructions_label = new JLabel("<html>How to play:<br/>This game is similar " +
                                             "to the classic minesweeper game.<br/> There are exactly 10 bombs on the board.<br/>" +
                                             "Flip all the tiles that aren't bombs to win! <br/>" +
                                             "The reset button will restart the game. <br/>The undo " +
                                             "button will undo a move if you ever make a mistake. <br/>The timer at the bottom" +
                                             "indicates how much time you are taking to win the game.<br/> The numMoves " +
                                             "tells you how many moves you have used to win the game.<br/> Try to win in the " +
                                             "fastest time possible with the least amount of moves!<html>");
instruction_panel.add(instructions_label);
```

Sources: [Game.java:35-44](https://github.com/aanickode/Minesweeper-Project/blob/main/Game.java#L35-L44)

### Score Panel

The score panel is a `JPanel` located at the top of the frame. It displays a label for the leaderboard, but the implementation of the leaderboard functionality is not present in the provided source files.

```java
final JPanel score_panel = new JPanel();
frame.add(score_panel, BorderLayout.NORTH);
final JLabel leaderboard = new JLabel("leaderboard");
score_panel.add(leaderboard);
```

Sources: [Game.java:47-50](https://github.com/aanickode/Minesweeper-Project/blob/main/Game.java#L47-L50)

### Game Board

The game board is an instance of the `GameBoard` class, which is responsible for rendering the game board and handling user input. It is added to the center of the frame.

```java
final GameBoard board = new GameBoard(status, leaderboard);
frame.add(board, BorderLayout.CENTER);
```

Sources: [Game.java:54](https://github.com/aanickode/Minesweeper-Project/blob/main/Game.java#L54)

### Control Buttons

The control panel is a `JPanel` located on the left side of the frame. It contains two buttons: "Reset" and "Undo".

The "Reset" button is created with an `ActionListener` that calls the `reset()` method of the `GameBoard` instance when clicked.

```java
final JButton reset = new JButton("Reset");
reset.addActionListener(new ActionListener() {
    public void actionPerformed(ActionEvent e) {
        board.reset();
    }
});
control_panel.add(reset);
```

Sources: [Game.java:61-67](https://github.com/aanickode/Minesweeper-Project/blob/main/Game.java#L61-L67)

The "Undo" button is created with an `ActionListener` that calls the `undo()` method of the `GameBoard` instance when clicked.

```java
final JButton undo = new JButton("Undo");
undo.addActionListener(new ActionListener() {
    public void actionPerformed(ActionEvent e) {
        board.undo();
    }
});
control_panel.add(undo);
```

Sources: [Game.java:70-75](https://github.com/aanickode/Minesweeper-Project/blob/main/Game.java#L70-L75)

## User Interaction Flow

The user interaction flow for the Minesweeper game can be summarized as follows:

1. The `Game` class sets up the GUI components, including the game board, status panel, instructions panel, score panel, and control buttons.
2. The `GameBoard` instance is responsible for rendering the game board and handling user input, such as tile clicks.
3. When the user clicks on a tile, the `GameBoard` class processes the click and updates the game state accordingly, potentially triggering a win or loss condition.
4. The "Reset" button allows the user to restart the game by calling the `reset()` method of the `GameBoard` instance.
5. The "Undo" button allows the user to undo the last move by calling the `undo()` method of the `GameBoard` instance.
6. The status panel displays the current game status, such as "Setting up..." or the outcome of the game (win or loss).
7. The instructions panel provides information on how to play the game.
8. The score panel is intended to display a leaderboard, but the implementation is not present in the provided source files.

## Conclusion

The "User Interaction" component of the Minesweeper game project provides a graphical user interface for players to interact with the game. It includes various panels and components for displaying the game board, status, instructions, and score. Users can interact with the game by clicking on tiles, resetting the game, and undoing moves using the provided control buttons. The implementation follows a Model-View-Controller design pattern, where the `Game` class sets up the view, the `GameBoard` class handles user input and rendering, and the `Minesweeper` class serves as the game model.

Sources: [Game.java](https://github.com/aanickode/Minesweeper-Project/blob/main/Game.java), [Minesweeper.java](https://github.com/aanickode/Minesweeper-Project/blob/main/Minesweeper.java), [Tile.java](https://github.com/aanickode/Minesweeper-Project/blob/main/Tile.java)