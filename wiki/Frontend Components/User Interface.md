<details>
<summary>Relevant source files</summary>

The following file was used as context for generating this wiki page:

- [Game.java](https://github.com/aanickode/Minesweeper-Project/blob/main/Game.java)

</details>

# User Interface

The User Interface (UI) of the Minesweeper game is built using Java Swing components and follows a Model-View-Controller (MVC) design pattern. The `Game` class serves as the entry point and sets up the top-level frame and various UI components.

## Main Game Window

The main game window is created as a `JFrame` instance with the title "Minesweeper". It is positioned at (400, 400) coordinates on the screen. The window is divided into several panels arranged using `BorderLayout`:

1. **Status Panel** (SOUTH): Displays the current game status using a `JLabel`.
2. **Instructions Panel** (EAST): Provides instructions on how to play the game using an HTML-formatted `JLabel`.
3. **Score Panel** (NORTH): Displays a leaderboard label.
4. **Game Board** (CENTER): The main game board where the Minesweeper game is played, represented by the `GameBoard` class.
5. **Control Panel** (WEST): Contains the "Reset" and "Undo" buttons.

Sources: [Game.java:21-57]()

## Game Board

The `GameBoard` class is responsible for rendering and handling the game logic. It is added to the center of the main window.

```java
final GameBoard board = new GameBoard(status, leaderboard);
frame.add(board, BorderLayout.CENTER);
```

Sources: [Game.java:54]()

## Control Buttons

The control panel on the left side of the window contains two buttons:

1. **Reset Button**: Resets the game board to its initial state when clicked.

```java
final JButton reset = new JButton("Reset");
reset.addActionListener(new ActionListener() {
    public void actionPerformed(ActionEvent e) {
        board.reset();
    }
});
control_panel.add(reset);
```

Sources: [Game.java:61-67]()

2. **Undo Button**: Undoes the last move made by the player when clicked.

```java
final JButton undo = new JButton("Undo");
undo.addActionListener(new ActionListener() {
    public void actionPerformed(ActionEvent e) {
        board.undo();
    }
});
control_panel.add(undo);
```

Sources: [Game.java:70-75]()

## Game Initialization

The `Game` class implements the `Runnable` interface, and its `run()` method is responsible for setting up the UI components and initializing the game board.

```java
public void run() {
    // ... (UI setup code)

    // Start the game
    board.reset();
}
```

Sources: [Game.java:18-87]()

The `main()` method is the entry point of the application and invokes the `Game` class on the Swing event dispatch thread using `SwingUtilities.invokeLater()`.

```java
public static void main(String[] args) {
    SwingUtilities.invokeLater(new Game());
}
```

Sources: [Game.java:92-95]()

In summary, the User Interface of the Minesweeper game consists of a main window with various panels for displaying the game board, status, instructions, and control buttons. The UI is set up and initialized in the `Game` class, following the MVC design pattern.