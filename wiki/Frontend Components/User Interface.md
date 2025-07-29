<details>
<summary>Relevant source files</summary>

The following file was used as context for generating this wiki page:

- [Game.java](https://github.com/aanickode/Minesweeper-Project/blob/main/Game.java)

</details>

# User Interface

## Introduction

The User Interface (UI) of the Minesweeper game is built using Java Swing components and follows a Model-View-Controller (MVC) design pattern. The `Game` class serves as the entry point for the application and sets up the top-level frame and various UI components. It instantiates the `GameBoard` class, which handles the game's view and controller functionality, and interacts with the game's model (not shown in the provided file).

## Main Frame

The main frame of the application is created as a `JFrame` instance with the title "Minesweeper". It is positioned at (400, 400) on the screen.

```java
final JFrame frame = new JFrame("Minesweeper");
frame.setLocation(400, 400);
```

The frame is divided into several panels, each serving a specific purpose:

1. **Status Panel**: Located at the bottom of the frame, this panel displays the current status of the game using a `JLabel`.

```java
final JPanel status_panel = new JPanel();
frame.add(status_panel, BorderLayout.SOUTH);
final JLabel status = new JLabel("Setting up...");
status_panel.add(status);
```

2. **Instructions Panel**: Located on the right side of the frame, this panel provides instructions on how to play the game using an HTML-formatted `JLabel`.

```java
final JPanel instruction_panel = new JPanel();
frame.add(instruction_panel, BorderLayout.EAST);
final JLabel instructions_label = new JLabel("<html>How to play:...</html>");
instruction_panel.add(instructions_label);
```

3. **Score Panel**: Located at the top of the frame, this panel displays a leaderboard using a `JLabel`.

```java
final JPanel score_panel = new JPanel();
frame.add(score_panel, BorderLayout.NORTH);
final JLabel leaderboard = new JLabel("leaderboard");
score_panel.add(leaderboard);
```

4. **Game Board**: Located in the center of the frame, this panel displays the actual game board and is an instance of the `GameBoard` class.

```java
final GameBoard board = new GameBoard(status, leaderboard);
frame.add(board, BorderLayout.CENTER);
```

5. **Control Panel**: Located on the left side of the frame, this panel contains buttons for resetting and undoing moves in the game.

```java
final JPanel control_panel = new JPanel();
frame.add(control_panel, BorderLayout.WEST);
```

Sources: [Game.java](https://github.com/aanickode/Minesweeper-Project/blob/main/Game.java)

## Control Buttons

The control panel contains two buttons: "Reset" and "Undo".

### Reset Button

The "Reset" button is created as a `JButton` instance and is added to the control panel. When clicked, it triggers the `reset()` method of the `GameBoard` instance, effectively resetting the game.

```java
final JButton reset = new JButton("Reset");
reset.addActionListener(new ActionListener() {
    public void actionPerformed(ActionEvent e) {
        board.reset();
    }
});
control_panel.add(reset);
```

Sources: [Game.java:49-55](https://github.com/aanickode/Minesweeper-Project/blob/main/Game.java#L49-L55)

### Undo Button

The "Undo" button is created as a `JButton` instance and is added to the control panel. When clicked, it triggers the `undo()` method of the `GameBoard` instance, allowing the user to undo their last move.

```java
final JButton undo = new JButton("Undo");
undo.addActionListener(new ActionListener() {
    public void actionPerformed(ActionEvent e) {
        board.undo();
    }
});
control_panel.add(undo);
```

Sources: [Game.java:57-62](https://github.com/aanickode/Minesweeper-Project/blob/main/Game.java#L57-L62)

## Application Lifecycle

The `Game` class implements the `Runnable` interface, and its `run()` method is responsible for setting up the UI components and initializing the game.

```java
public void run() {
    // Setup UI components
    // ...

    // Put the frame on the screen
    frame.pack();
    frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    frame.setVisible(true);

    // Start the game
    board.reset();
}
```

The `main()` method is the entry point of the application and invokes the `run()` method of the `Game` instance on the Swing event dispatch thread using `SwingUtilities.invokeLater()`.

```java
public static void main(String[] args) {
    SwingUtilities.invokeLater(new Game());
}
```

Sources: [Game.java:64-83](https://github.com/aanickode/Minesweeper-Project/blob/main/Game.java#L64-L83), [Game.java:88-91](https://github.com/aanickode/Minesweeper-Project/blob/main/Game.java#L88-L91)

## Summary

The User Interface of the Minesweeper game is built using Java Swing components and follows the Model-View-Controller design pattern. The `Game` class sets up the main frame and various UI components, including the game board, status panel, instructions panel, score panel, and control buttons for resetting and undoing moves. The `GameBoard` class, which is instantiated by the `Game` class, handles the game's view and controller functionality, and interacts with the game's model.