# LDTS - PAC-MAN

> **Project**
> <br />
> Course Unit: [Software Design and Testing Laboratory](https://sigarra.up.pt/feup/en/UCURR_GERAL.FICHA_UC_VIEW?pv_ocorrencia_id=484407 "Laboratório de Design e Testes de Software"), 2022/23
> <br />
> Faculty: **FEUP** (Faculty of Engineering of the University of Porto)
> <br />
> Project Grade: **19.4**/20

---

## Game Description

In this recreation of the classic arcade game, you will be tasked with navigating a maze and eating all the Pac-dots while avoiding the infamous ghosts: Blinky, Pinky, Inky, and Clyde.

If one of the ghosts catches you, you will lose a life unless you've eaten a power pellet, in which case you can eat the ghosts for bonus points. If you lose all three lives, it's game over. You can also play with a friend in a **2-player mode**, where one player controls Pac-Man and the other controls a ghost.

<p align="center" justify="center">
  <img src="docs/gifs/SinglePlayer.gif" width="400"/>
</p>

### Controls
* **Up/Down/Left/Right**: Move Pac-Man / Navigate Menus
* **W/A/S/D**: Move the Monster player (Multiplayer)
* **Enter**: Select menu option
* **Q**: Go back / Pause

---

## Implemented Features

- **Connected Menus** — Browse back and forth through different menus.
- **Main Menu** — Choose whether to play single or multiplayer, and navigate to other menus.
- **Level Selection Menu** — Choose the level to play.
- **Score Menu** — View the best scores.
- **Sound** — Many actions produce sounds (e.g. moving through menus, starting the game, Pac-Man eating coins or hitting monsters).
- **Player Movement** — Moves in the direction given by input and keeps moving until reaching a wall or receiving new input.
- **Monster Movement** — Monsters move according to different algorithms.
- **Multiplayer** — An additional monster player with movement similar to Pac-Man.
- **Coin Collection** — Score increases when Pac-Man collects a coin.
- **Monster Collision** — Health decreases and positions of monsters and Pac-Man reset to the beginning.
- **Power-Ups** — Makes ghosts "scared"; Pac-Man can eat them on collision for bonus points.
- **Loading Map from File** — Maps are loaded from file, allowing different maps to be used.
- **Map Reset** — After Pac-Man collects all coins, the coins and power-ups reset to their initial state.

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

---

## Quick Start

### Prerequisites
* Java 17+
* Gradle

### Running the Game

```bash
./gradlew run
```

### Running Tests

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

---

## Design

This project was developed with a strong focus on **Object-Oriented Design patterns** and **Test-Driven Development (TDD)** principles.

### GUI — Adapter Pattern

**Problem in Context**

Using Lanterna directly in all Viewer classes would create a direct violation of the Dependency Inversion Principle (DIP). Many classes would depend on Lanterna, making it difficult to change or update the external library without modifying several classes.

**The Pattern**

We used the **Adapter** pattern. A `GUI` interface exposes the methods used by our classes, avoiding direct dependency on Lanterna. If the external library were to change, only a new implementation of the `GUI` interface would be needed.

**Implementation**

<p align="center">
  <img src="docs/images/uml/LanternaAdapterPattern.jpg" width="600"/>
</p>

Relevant classes:
- [GUI](src/main/java/ldts/pacman/gui/GUI.java)
- [LanternaGUI](src/main/java/ldts/pacman/gui/LanternaGUI.java)

> The "client" in the diagram represents all classes that use the GUI (mostly Viewer classes).

**Consequences**

Promotes replaceability of the external library and respects the Single Responsibility Principle, since only `LanternaGUI` is concerned with how to draw things with Lanterna.

---

### Monster States — State Pattern

**Problem in Context**

Monsters can have different behaviours (including different movement algorithms), and the controller and viewer need to know how to handle them. Using a switch-case with flags would violate the Open/Closed Principle (OCP).

**The Pattern**

We used the **State** pattern. The [Monster](src/main/java/ldts/pacman/model/game/elements/monsters/Monster.java) abstract class has a `MonsterState` field that can change throughout the game. Different states define different movement algorithms and drawing characters. States can transition themselves or be changed externally (e.g. when Pac-Man collects a power-up).

**Implementation**

<p align="center">
  <img src="docs/images/uml/MonsterState_UML_diagram.jpg" width="700"/>
</p>

<p align="center">
  <img src="docs/images/uml/MonsterStateDiagram.jpg" width="700"/>
</p>

Relevant classes:
- [Monster](src/main/java/ldts/pacman/model/game/elements/monsters/Monster.java)
- [MonsterState](src/main/java/ldts/pacman/controller/game/monster/state/MonsterState.java)
- [ScatterState](src/main/java/ldts/pacman/controller/game/monster/state/ScatterState.java)
- [ChaseState](src/main/java/ldts/pacman/controller/game/monster/state/ChaseState.java)
- [EatenState](src/main/java/ldts/pacman/controller/game/monster/state/EatenState.java)
- [ScaredState](src/main/java/ldts/pacman/controller/game/monster/state/ScaredState.java)

**Consequences**

Allows changing monster behaviour at runtime and avoids scattered conditional logic. State transitions are explicit in the code.

---

### Factory Method

**Problem in Context**

Parent abstract classes sometimes cannot define which concrete objects to create — only the subclasses can. For example, `MonsterState` knows it needs a `MovementStrategy` but not which concrete implementation.

**The Pattern**

We used the **Factory Method** pattern. The abstract `MonsterState` delegates the choice of `MovementStrategy` to its subclasses (e.g. `ScaredState` creates a `ScaredStrategy`).

**Implementation**

<p align="center">
  <img src="docs/images/uml/FactoryMethod_createStrategy.jpg" width="900"/>
</p>

Relevant classes:
- [MonsterState](src/main/java/ldts/pacman/controller/game/monster/state/MonsterState.java)
- [ScaredState](src/main/java/ldts/pacman/controller/game/monster/state/ScaredState.java)
- [ScatterState](src/main/java/ldts/pacman/controller/game/monster/state/ScatterState.java)
- [ChaseState](src/main/java/ldts/pacman/controller/game/monster/state/ChaseState.java)
- [MovementStrategy](src/main/java/ldts/pacman/controller/game/movement/strategy/MovementStrategy.java)
- [ScaredStrategy](src/main/java/ldts/pacman/controller/game/movement/strategy/bot/ScaredStrategy.java)
- [ChasePacmanStrategy](src/main/java/ldts/pacman/controller/game/movement/strategy/bot/target/ChasePacmanStrategy.java)
- [ScatterCornerStrategy](src/main/java/ldts/pacman/controller/game/movement/strategy/bot/target/ScatterCornerStrategy.java)
- [EatenStrategy](src/main/java/ldts/pacman/controller/game/movement/strategy/bot/target/EatenStrategy.java)

**Consequences**

Delegates the algorithm choice to the concrete `MonsterState` subclasses. Avoids switch-cases by leveraging polymorphism.

---

### Strategy Pattern

**Problem in Context**

Different entities share movement algorithms. Defining movement directly in each entity would lead to code duplication.

**The Pattern**

We used the **Strategy** pattern. Movement algorithms are encapsulated in strategy classes. Each entity identifies its strategy rather than defining the algorithm itself.

**Implementation**

<p align="center">
  <img src="docs/images/uml/StrategyPattern.jpg" width="900"/>
</p>

Relevant classes:
- [MonsterState](src/main/java/ldts/pacman/controller/game/monster/state/MonsterState.java)
- [PlayerMovementStrategy](src/main/java/ldts/pacman/controller/game/movement/strategy/player/PlayerMovementStrategy.java)
- [PacmanPlayerStrategy](src/main/java/ldts/pacman/controller/game/movement/strategy/player/PacmanPlayerStrategy.java)
- [MonsterPlayerStrategy](src/main/java/ldts/pacman/controller/game/movement/strategy/player/MonsterPlayerStrategy.java)
- [BotStrategy](src/main/java/ldts/pacman/controller/game/movement/strategy/bot/BotStrategy.java)
- [ScaredStrategy](src/main/java/ldts/pacman/controller/game/movement/strategy/bot/ScaredStrategy.java)
- [TargetStrategy](src/main/java/ldts/pacman/controller/game/movement/strategy/bot/target/TargetStrategy.java)
- [ScatterCornerStrategy](src/main/java/ldts/pacman/controller/game/movement/strategy/bot/target/ScatterCornerStrategy.java)
- [EatenStrategy](src/main/java/ldts/pacman/controller/game/movement/strategy/bot/target/EatenStrategy.java)
- [ChasePacmanStrategy](src/main/java/ldts/pacman/controller/game/movement/strategy/bot/target/ChasePacmanStrategy.java)

**Consequences**

Avoids code duplication across entities that share the same movement algorithm. Strategies can be swapped at runtime.

---

### MVC (Model-View-Controller)

**Problem in Context**

Defining data, behaviour, and rendering in a single class would violate the Single Responsibility Principle. That class would have too many reasons to change.

**The Pattern**

We used the **Model-View-Controller** architectural pattern (specifically HMVC — MVC for each component). The model holds data, the controller manipulates it, and the viewer displays it.

**Implementation**

<p align="center">
  <img src="docs/images/uml/mvcPacman.jpg" width="600"/>
</p>

Example classes:
- [PacmanController](src/main/java/ldts/pacman/controller/game/PacmanController.java)
- [PacmanViewer](src/main/java/ldts/pacman/view/game/PacmanViewer.java)
- [Pacman](src/main/java/ldts/pacman/model/game/elements/Pacman.java)

**Consequences**

Each class has a single responsibility and a single reason to change.

---

### Application State — State Pattern

**Problem in Context**

Using flags and switch-cases to define application state (menus vs. game) would violate the Open/Closed Principle.

**The Pattern**

We used the **State** pattern. Different application states are represented by different subclasses of `State`. The main `Game` class holds a state reference, and any controller can trigger a transition. Each state subclass also uses a Factory Method to define its own controller and viewer.

**Implementation**

<p align="center">
  <img src="docs/images/uml/ApplicationState.jpg" width="600"/>
</p>

Relevant classes:
- [Game](src/main/java/ldts/pacman/Game.java)
- [State](src/main/java/ldts/pacman/application/state/State.java)
- [GameState](src/main/java/ldts/pacman/application/state/GameState.java)
- [MainMenuState](src/main/java/ldts/pacman/application/state/menu/MainMenuState.java)
- [ChooseLevelState](src/main/java/ldts/pacman/application/state/menu/ChooseLevelState.java)
- [SaveScoreState](src/main/java/ldts/pacman/application/state/menu/SaveScoreState.java)
- [ScoreMenuState](src/main/java/ldts/pacman/application/state/menu/ScoreMenuState.java)

**Consequences**

Existing states are explicit and easy to understand. No conditional logic is needed for state management — polymorphism handles it.

---

### Sound

Although we initially considered the Observer pattern for sound (as discussed during the presentation), we decided it was not the best fit and felt we were forcing an unnecessary pattern, so we removed it.

---

## Known Code Smells and Refactoring Suggestions

### Large Classes

Some classes contain many methods (`GUI` interface and `LanternaGUI`) or many fields (`Arena`). We judged this acceptable given their responsibilities. A potential solution would be to split the `GUI` interface into multiple smaller interfaces.

### Long Parameter List (and Data Clumps)

Present in the constructor of [Arena](src/main/java/ldts/pacman/model/game/arena/Arena.java) and the `createArena` method in [ArenaLoader](src/main/java/ldts/pacman/model/game/arena/ArenaLoader.java). The sound parameters are passed for dependency injection (so tests can mock them). A solution would be to bundle the `SoundObserver` parameters into a single object. A similar smell exists in [PlayerMovementStrategy](src/main/java/ldts/pacman/controller/game/movement/strategy/player/PlayerMovementStrategy.java), where four directional options are passed separately.

### Feature Envy (and Message Chaining)

Due to MVC, controllers primarily access their models' features, and viewers access model data for drawing. This also generates message chaining. Examples include [MainMenuController](src/main/java/ldts/pacman/controller/menu/MainMenuController.java) and [PacmanController](src/main/java/ldts/pacman/controller/game/PacmanController.java). Since this is a direct consequence of MVC and supports SRP, we did not change it.

### Refused Bequest

Some subclasses don't need all of their parent's methods. Examples include [ScoreMenuController](src/main/java/ldts/pacman/controller/menu/ScoreMenuController.java) (doesn't use its model), some methods in [EatenState](src/main/java/ldts/pacman/controller/game/monster/state/EatenState.java) (defined as no-ops), and [MovementStrategy](src/main/java/ldts/pacman/controller/game/movement/strategy/MovementStrategy.java)'s `move` method receiving `options` only used by `PlayerMovementStrategy` subclasses. This trade-off allows uniform treatment of all monsters, including the player-controlled one.

### Data Classes

Due to MVC, some classes only contain data and accessors. Examples include the [Element](src/main/java/ldts/pacman/model/game/elements) subclasses (`MovableElement`, `Pacman`, `Coin`, `PowerUp`, `Wall`) and some [Menu](src/main/java/ldts/pacman/model/menu) model classes (`MainMenu`, `SaveScore`, `ChooseLevel`). This is a natural consequence of MVC.

---

## Testing

### Coverage Report

<p align="center" justify="center">
  <img src="docs/images/reports/jacoco.png" width="800"/>
</p>

<p align="center" justify="center">
  <img src="docs/images/reports/PIT.png" width="800"/>
</p>

- [Mutation testing report](https://adriano-7.github.io/)

---

## Team

| Name | Email | 
|------|-------|
| **Adriano Machado** | up202105352@fe.up.pt | 
| **Félix Martins** | up202108837@fe.up.pt | 
| **Tomás Pereira** | up202108845@fe.up.pt | 
