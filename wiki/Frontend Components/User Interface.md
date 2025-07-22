<details>
<summary>Relevant source files</summary>

The following file was used as context for generating this wiki page:

- [Game.java](https://github.com/aanickode/Minesweeper-Project/blob/main/Game.java)

</details>

# User Interface

## Introduction

The User Interface (UI) of the Minesweeper game is implemented using Java Swing components. It provides a graphical environment for the user to interact with the game, including displaying the game board, showing instructions, and providing controls for resetting and undoing moves. The UI is designed to be user-friendly and visually appealing, enhancing the overall gaming experience.

## Main Window

The main window of the Minesweeper game is created using a `JFrame` instance. It serves as the top-level container for all the UI components and is responsible for managing the layout and positioning of these components.

```java
// Top-level frame in which game components live
final JFrame frame = new JFrame("Minesweeper");
frame.setLocation(400, 400);
```

The main window is divided into several panels, each serving a specific purpose:

1. **Status Panel**: Located at the bottom of the window, this panel displays the current status of the game using a `JLabel`.
2. **Instructions Panel**: Positioned on the right side of the window, this panel provides instructions on how to play the game using a `JLabel` with HTML formatting.
3. **Score Panel**: Situated at the top of the window, this panel displays the leaderboard using a `JLabel`.
4. **Game Board**: The central component of the window, where the actual game board is displayed using a custom `GameBoard` component.
5. **Control Panel**: Located on the left side of the window, this panel contains buttons for resetting and undoing moves.

```java
// Status panel
final JPanel status_panel = new JPanel();
frame.add(status_panel, BorderLayout.SOUTH);
final JLabel status = new JLabel("Setting up...");
status_panel.add(status);

// Instructions panel
final JPanel instruction_panel = new JPanel();
frame.add(instruction_panel, BorderLayout.EAST);
final JLabel instructions_label = new JLabel("<html>How to play:...</html>");
instruction_panel.add(instructions_label);

// Score panel
final JPanel score_panel = new JPanel();
frame.add(score_panel, BorderLayout.NORTH);
final JLabel leaderboard = new JLabel("leaderboard");
score_panel.add(leaderboard);

// Game board
final GameBoard board = new GameBoard(status, leaderboard);
frame.add(board, BorderLayout.CENTER);

// Control panel
final JPanel control_panel = new JPanel();
frame.add(control_panel, BorderLayout.WEST);
```

Sources: [Game.java](https://github.com/aanickode/Minesweeper-Project/blob/main/Game.java)

## Control Buttons

The control panel contains two buttons: "Reset" and "Undo". These buttons are implemented using `JButton` instances and are added to the control panel using `control_panel.add(button)`.

### Reset Button

The "Reset" button is used to reset the game board and start a new game. When the button is clicked, an `ActionListener` is triggered, which calls the `reset()` method of the `GameBoard` instance.

```java
final JButton reset = new JButton("Reset");
reset.addActionListener(new ActionListener() {
    public void actionPerformed(ActionEvent e) {
        board.reset();
    }
});
control_panel.add(reset);
```

Sources: [Game.java:58-64](https://github.com/aanickode/Minesweeper-Project/blob/main/Game.java#L58-L64)

### Undo Button

The "Undo" button allows the user to undo the last move made on the game board. When clicked, an `ActionListener` is triggered, which calls the `undo()` method of the `GameBoard` instance.

```java
final JButton undo = new JButton("Undo");
undo.addActionListener(new ActionListener() {
    public void actionPerformed(ActionEvent e) {
        board.undo();
    }
});
control_panel.add(undo);
```

Sources: [Game.java:67-72](https://github.com/aanickode/Minesweeper-Project/blob/main/Game.java#L67-L72)

## Game Board

The game board is a custom component called `GameBoard`, which is responsible for rendering the actual game board and handling user interactions with the tiles. The `GameBoard` instance is created and added to the center of the main window.

```java
final GameBoard board = new GameBoard(status, leaderboard);
frame.add(board, BorderLayout.CENTER);
```

The `GameBoard` component likely contains the logic for rendering the tiles, handling user clicks, and updating the game state based on the user's actions. However, the implementation details of the `GameBoard` class are not provided in the `Game.java` file.

Sources: [Game.java:47](https://github.com/aanickode/Minesweeper-Project/blob/main/Game.java#L47)

## Window Setup and Initialization

After creating and configuring all the UI components, the main window is set up and displayed on the screen. The `pack()` method is called to ensure that the window is sized appropriately based on the components it contains.

```java
// Put the frame on the screen
frame.pack();
frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
frame.setVisible(true);
```

Finally, the `reset()` method of the `GameBoard` instance is called to start the game.

```java
// Start the game
board.reset();
```

Sources: [Game.java:76-79](https://github.com/aanickode/Minesweeper-Project/blob/main/Game.java#L76-L79)

## Conclusion

The User Interface of the Minesweeper game is implemented using Java Swing components, providing a graphical environment for the user to interact with the game. The main window is divided into several panels, each serving a specific purpose, such as displaying the game board, instructions, and control buttons. The `GameBoard` component is responsible for rendering the actual game board and handling user interactions, while the control buttons allow the user to reset and undo moves. The UI is designed to be user-friendly and visually appealing, enhancing the overall gaming experience.