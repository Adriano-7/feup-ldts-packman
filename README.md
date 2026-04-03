
# LDTS - PAC-MAN

> **Project**
> <br />
> Course Unit: [Software Design and Testing Laboratory](https://sigarra.up.pt/feup/en/UCURR_GERAL.FICHA_UC_VIEW?pv_ocorrencia_id=484407 "Laboratório de Design e Testes de Software"), 2022/23
> <br />
> Faculty: **FEUP** (Faculty of Engineering of the University of Porto)
> <br />
> Project Grade: **19.4**/20
> <br />
> Report: [Detailed Design Documentation](./docs/README.md)

---

## Game Description
In this recreation of the classic arcade game, you will be tasked with navigating a maze and eating all the Pac-dots while avoiding the infamous ghosts: Blinky, Pinky, Inky, and Clyde.

If one of the ghosts catches you, you will lose a life unless you've eaten a power pellet, in which case you can eat the ghosts for bonus points. If you lose all three lives, it's game over. You can also play with a friend in a **2-player mode**, where one player controls Pac-Man and the other controls a ghost.

### Controls
* **Up/Down/Left/Right**: Move Pac-Man / Navigate Menus
* **W/A/S/D**: Move the Monster player (Multiplayer)
* **Enter**: Select menu option
* **Q**: Go back / Pause

<p align="center" justify="center">
  <img src="docs/gifs/SinglePlayer.gif" width="400"/>
</p>


---

## Technical Highlights
This project was developed with a strong focus on **Object-Oriented Design patterns** and **Test-Driven Development (TDD)** principles.

### Architecture & Design Patterns
* **MVC (Model-View-Controller)**: Used to decouple game data, rendering logic, and user input handling, significantly improving maintainability.
* **State Pattern**: Implemented to manage both the **Application State** (Menus vs. Game) and individual **Monster Behavior** (Chase, Scatter, Scared, Eaten), avoiding complex conditional logic.
* **Strategy Pattern**: Used to define flexible movement algorithms for different types of entities (Pac-Man, Monster Bots, and Monster Players).
* **Adapter Pattern**: Used to interface with the **Lanterna** GUI library, allowing for potential framework migration without affecting game logic.
* **Factory Method**: Employed to handle the creation of various game strategies and state-specific logic.

### Quality Assurance
* **Unit Testing**: Extensive test coverage for controllers, models, and file manipulation.
* **Mutation Testing**: Used **PIT** to ensure high-quality test suites that go beyond mere line coverage.
* **Coverage**: Achieved thorough branch and statement coverage (refer to [docs/README.md](./docs/README.md) for full reports).

---

## Quick Start

### Prerequisites
* Java 17+
* Gradle

### Running the Game
To run the game, simply execute the following command in the project root:

```bash
./gradlew run
```

### Running Tests
To verify the system and run the test suite:

```bash
./gradlew test
```

---

## Project Structure
```text
├── docs/           # Detailed reports, UML diagrams, and testing results
├── src/
│   ├── main/       # Source code (Controller, Model, View, GUI, Sound)
│   └── test/       # Unit tests using JUnit and Mockito
└── resources/      # Game levels, fonts, and high-score storage
```

### IMPLEMENTED FEATURES
>- **Connected Menus** - Browsing back and forth through different Menus
>- **Main Menu** to browse to different menus and to choose whether to play single or multiplayer
>- **Menu** to choose the level to play
>- **Menu** to see the best scores
>- **Sound** - many actions produce sounds. Ex: moving through menus, starting the game, pacman eating coins/hitting monsters
>- **Player movement** - moves in direction given by input and keeps moving until reaches wall or receives input
>- **Monster movement** - monster move according to different algorithms
>- **Multiplayer** - additional monster player with movement similar to pacman 
>- **Pacman collects coins** - score increases when he collects one
>- **Pacman collides with monsters** - health decreased and positions of monsters and pacman reset to beginning
>- **PowerUps** for pacman - makes ghosts "scared" and is able to eat them on collision for points
>- **Loading map from file** - map is loaded from file thus allowing different maps to be used
>- **Resetting map** - after pacman collects all coins of the map, the coins and powerUps get reset back to how they were in the start of the level


<p align="left" justify="left">
  <img src="docs/gifs/SinglePlayer.gif" width="400"/>
  <img src="docs/gifs/MultiPlayer.gif" width="400"/>
</p>
<p align="left">
  <b><i>Gif 1. Single Player Game</i></b>
    &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;
    &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;
    &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;
  <b><i>Gif 2. Multiplayer Game</i></b>
</p>

<p align="left" justify="left">
  <img src="docs/gifs/MainMenu.gif" height="325"/>
  <img src="docs/gifs/ChooseLevelMenu.gif" height="325"/>
  <img src="docs/images/ScoreMenu.png" height="325"/>
</p>
<p align="left">
  <b><i>Gif 3. Main Menu</i></b>
    &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;
    &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;
  <b><i>Gif 4. Choose Level</i></b>
    &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;
    &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;
  <b><i>Fig 1. Scores</i></b>
</p>

For a more detailed versin of this description, click [here](./docs/README.md).

---

## Team
- **Adriano Machado** (up202105352@fe.up.pt)
- **Félix Martins** (up202108837@fe.up.pt)
- **Tomás Pereira** (up202108845@fe.up.pt)

