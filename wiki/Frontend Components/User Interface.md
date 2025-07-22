<details>
<summary>Relevant source files</summary>

The following file was used as context for generating this wiki page:

- [Game.java](https://github.com/aanickode/Minesweeper-Project/blob/main/Game.java)
</details>

# User Interface

## Introduction

The User Interface (UI) of the Minesweeper game is built using Java Swing components and follows the Model-View-Controller (MVC) design pattern. The `Game` class serves as the entry point and sets up the top-level frame and widgets for the GUI. It initializes the game board, status panel, instructions panel, score panel, and control panel with buttons for resetting and undoing moves.

The `GameBoard` class, which is instantiated within the `Game` class, handles the main game logic, including the game's view and controller functionality. It also instantiates a `TicTacToe` object (not provided in the given source file) to serve as the game's model.

## UI Components

### Top-Level Frame

The main window of the Minesweeper game is created as a `JFrame` instance with the title "Minesweeper". The frame is positioned at (400, 400) coordinates on the screen.

```java
final JFrame frame = new JFrame("Minesweeper");
frame.setLocation(400, 400);
```

Source: [Game.java:27-28]()

### Status Panel

The status panel is a `JPanel` instance added to the bottom of the frame using the `BorderLayout.SOUTH` layout constraint. It contains a `JLabel` that displays the current status of the game, initially set to "Setting up...".

```java
final JPanel status_panel = new JPanel();
frame.add(status_panel, BorderLayout.SOUTH);
final JLabel status = new JLabel("Setting up...");
status_panel.add(status);
```

Source: [Game.java:32-35]()

### Instructions Panel

The instructions panel is a `JPanel` instance added to the right side of the frame using the `BorderLayout.EAST` layout constraint. It contains a `JLabel` that displays the game instructions in HTML format.

```java
final JPanel instruction_panel = new JPanel();
frame.add(instruction_panel, BorderLayout.EAST);
final JLabel instructions_label = new JLabel("<html>How to play:<br/>This game is similar to the classic minesweeper game.<br/> There are exactly 10 bombs on the board.<br/> Flip all the tiles that aren't bombs to win! <br/>The reset button will restart the game. <br/>The undo button will undo a move if you ever make a mistake. <br/>The timer at the bottom indicates how much time you are taking to win the game.<br/> The numMoves tells you how many moves you have used to win the game.<br/> Try to win in the fastest time possible with the least amount of moves!<html>");
instruction_panel.add(instructions_label);
```

Source: [Game.java:38-45]()

### Score Panel

The score panel is a `JPanel` instance added to the top of the frame using the `BorderLayout.NORTH` layout constraint. It contains a `JLabel` displaying the text "leaderboard", which presumably will show the high scores or leaderboard for the game.

```java
final JPanel score_panel = new JPanel();
frame.add(score_panel, BorderLayout.NORTH);
final JLabel leaderboard = new JLabel("leaderboard");
score_panel.add(leaderboard);
```

Source: [Game.java:48-51]()

### Game Board

The game board is an instance of the `GameBoard` class, which is added to the center of the frame using the `BorderLayout.CENTER` layout constraint. The `GameBoard` instance is initialized with the `status` and `leaderboard` labels, likely for updating them during gameplay.

```java
final GameBoard board = new GameBoard(status, leaderboard);
frame.add(board, BorderLayout.CENTER);
```

Source: [Game.java:54-55]()

### Control Panel

The control panel is a `JPanel` instance added to the left side of the frame using the `BorderLayout.WEST` layout constraint. It contains two buttons: "Reset" and "Undo".

```java
final JPanel control_panel = new JPanel();
frame.add(control_panel, BorderLayout.WEST);
```

Source: [Game.java:58-59]()

#### Reset Button

The "Reset" button is a `JButton` instance added to the control panel. When clicked, it calls the `reset()` method of the `GameBoard` instance, likely to reset the game state.

```java
final JButton reset = new JButton("Reset");
reset.addActionListener(new ActionListener() {
    public void actionPerformed(ActionEvent e) {
        board.reset();
    }
});
control_panel.add(reset);
```

Source: [Game.java:63-69]()

#### Undo Button

The "Undo" button is a `JButton` instance added to the control panel. When clicked, it calls the `undo()` method of the `GameBoard` instance, likely to undo the last move made by the player.

```java
final JButton undo = new JButton("Undo");
undo.addActionListener(new ActionListener() {
    public void actionPerformed(ActionEvent e) {
        board.undo();
    }
});
control_panel.add(undo);
```

Source: [Game.java:72-77]()

### Frame Setup and Game Start

After setting up all the UI components, the frame is packed and made visible on the screen. The `setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE)` method ensures that the application exits when the frame is closed.

```java
frame.pack();
frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
frame.setVisible(true);
```

Source: [Game.java:80-82]()

Finally, the `board.reset()` method is called to start the game.

```java
board.reset();
```

Source: [Game.java:85]()

## Conclusion

The User Interface of the Minesweeper game is built using Java Swing components and follows the Model-View-Controller (MVC) design pattern. The `Game` class sets up the top-level frame and various UI components, including the game board, status panel, instructions panel, score panel, and control panel with buttons for resetting and undoing moves. The `GameBoard` class, which is instantiated within the `Game` class, handles the main game logic and interacts with the game's model (`TicTacToe` object).

Sources: [Game.java]()