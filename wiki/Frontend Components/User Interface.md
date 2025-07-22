<details>
<summary>Relevant source files</summary>

The following file was used as context for generating this wiki page:

- [Game.java](https://github.com/aanickode/Minesweeper-Project/blob/main/Game.java)

</details>

# User Interface

## Introduction

The User Interface (UI) of the Minesweeper game is implemented using Java Swing components. It provides a graphical environment for players to interact with the game, including displaying the game board, showing instructions, and providing controls for resetting and undoing moves. The UI is designed to be user-friendly and visually appealing, with clear instructions and feedback to enhance the overall gaming experience.

## Main Frame

The main frame of the application is created using the `JFrame` class. It serves as the top-level container for all other UI components and is titled "Minesweeper". The frame is positioned at (400, 400) coordinates on the screen.

Sources: [Game.java:25](https://github.com/aanickode/Minesweeper-Project/blob/main/Game.java#L25)

## Status Panel

The status panel is a `JPanel` located at the bottom of the main frame. It displays a `JLabel` that shows the current status of the game, such as "Setting up..." or other relevant messages.

Sources: [Game.java:29-32](https://github.com/aanickode/Minesweeper-Project/blob/main/Game.java#L29-L32)

## Instructions Panel

The instructions panel is a `JPanel` located on the right side of the main frame. It contains a `JLabel` with HTML-formatted instructions on how to play the Minesweeper game. The instructions cover various aspects, including the objective, the number of bombs, winning conditions, and the functionality of the reset and undo buttons.

Sources: [Game.java:35-45](https://github.com/aanickode/Minesweeper-Project/blob/main/Game.java#L35-L45)

## High Scores Panel

The high scores panel is a `JPanel` located at the top of the main frame. It contains a `JLabel` labeled "leaderboard", which is likely intended to display the high scores or leaderboard for the game.

Sources: [Game.java:48-50](https://github.com/aanickode/Minesweeper-Project/blob/main/Game.java#L48-L50)

## Game Board

The game board is a custom component called `GameBoard` that extends `JPanel`. It is added to the center of the main frame and is responsible for rendering the actual game board and handling user interactions.

Sources: [Game.java:53](https://github.com/aanickode/Minesweeper-Project/blob/main/Game.java#L53)

## Control Panel

The control panel is a `JPanel` located on the left side of the main frame. It contains two buttons: "Reset" and "Undo".

### Reset Button

The reset button is a `JButton` labeled "Reset". When clicked, it triggers the `reset()` method of the `GameBoard` instance, which likely resets the game to its initial state.

Sources: [Game.java:59-65](https://github.com/aanickode/Minesweeper-Project/blob/main/Game.java#L59-L65)

### Undo Button

The undo button is a `JButton` labeled "Undo". When clicked, it triggers the `undo()` method of the `GameBoard` instance, which likely undoes the last move made by the player.

Sources: [Game.java:68-74](https://github.com/aanickode/Minesweeper-Project/blob/main/Game.java#L68-L74)

## UI Setup and Initialization

The `Game` class sets up the main frame and all its components, including the status panel, instructions panel, high scores panel, game board, and control panel. It also packs the frame, sets the default close operation, and makes the frame visible.

Finally, the `reset()` method of the `GameBoard` instance is called to start the game.

Sources: [Game.java:77-83](https://github.com/aanickode/Minesweeper-Project/blob/main/Game.java#L77-L83)

## Main Method

The `main` method is the entry point of the application. It uses the `SwingUtilities.invokeLater` method to create an instance of the `Game` class and run its `run` method on the Event Dispatch Thread (EDT), which is required for proper Swing UI rendering and event handling.

Sources: [Game.java:90-93](https://github.com/aanickode/Minesweeper-Project/blob/main/Game.java#L90-L93)

In summary, the User Interface of the Minesweeper game is built using Java Swing components and follows a typical layout with a main frame, game board, status panel, instructions panel, high scores panel, and control panel. The UI provides a user-friendly environment for players to interact with the game, with clear instructions and controls for resetting and undoing moves.