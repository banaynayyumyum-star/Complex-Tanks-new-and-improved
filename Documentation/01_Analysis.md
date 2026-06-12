# Analysis

## The Purpose
I am making this game to create an enticing and enjoyable game which applies the topic of Complex numbers to a real time situation. This game can also help develop trajectory planning, spatial reasoning, distance calculation, and angle adjustment. Because the game requires real-time physics simulations, complex multiplications for rotations must happen 60 times per second. This makes a computational solution necessary as humans cannot manually calculate the complex vector transformations this quickly.

---

## The client
This game is targeted at any student or gaming enthusiast with an interest in maths and fast strategic gameplay. It may be good for those beginning to develop their understanding of Complex numbers in A-level or those just trying to learn more about maths as a whole. Even if you have no interest in maths the unique gimmicks which make this game different would be fun for anyone to try.

---

## Research

### Case Study 1: Tank! (Kee Games, 1974)
- Overview: A classic 2-player 2D arcade game where players navigate a maze while attempting to avoid landmines in order to destroy their opponent within a time limit.
- Control Schema: A dual-joystick is used for each tank, pushing both joysticks forward will result in forwards movement and both backward for the tank to reverse, opposing movements rotate the tank in place or while moving.
- Input/Output:
  - Inputs: Dual-joysticks with a fire button on the right stick to fire
  - Outputs: Monochromatic CRT raster graphics tracking score, static maze obstacles, tank positions and stationary landmines.
 
### Case study 2: Wii Play: Tanks! (Nintendo, 2006)
- Overview: A top-down tank combat game where the players control a small tank through several stages and fight enemy tanks to reach stage 20 with land mines and shells. There are also 100 missions to be completed for medals.
- Control Schema: the tanks are controlled using a D-pad or Nunchuk's analog stick, while the gun turret is independently moved by aming the Wii Remote at the sensor bar.
- Input/Output:
  - Inputs: D-pad or nunchuck thumstick for movement, A button to deploy a mine, B button to fire a shell, + button to pause the game, point Wii remote at screen to aim the turret.
  - Outputs:  standard-definition video (up to 480p) and analog stereo.

### Case Study 3: Diep.io (Miniclip, 2016)
- Overview: A multiplayer 2D top-down mobile game where the objective is to kill other tanks with bullets of various sizes, weight, and other factors depending on what you have upgraded. The wide choice of game modes allows a lot of competitive gameplay. The main goal is to get on the top rank of the leaderboard and stay there for as long as possible.
- Control Schema:
  - PC: W, A, S, D or arrow keys for movement, mouse movement for aiming the turret, fire with left click or spacebar and lock auto-fire with E, right click to point drones.
  - Mobile: Left joystick for movement, Right joystick to aim the turret and fire at the same time, 2 toggle buttons for standard firing behavior and other for special secondary class skills.
- Inputs/Outputs:
  - Inputs: keyboard keys for movement and mouse keys for aiming and abilities or touch coordinates for mobile, left joystick and right joystick, on screen buttons.
  - Outputs: Real-time 2D vector-style web graphics, a dynamic on screen leaderboard and a player stat/upgrade UI, network data packets sent via WebSockets to synchronize other players movements on the server.
 
---

## Comparison

| Game | Tank! (Kee Games, 1974) | Wii Play: Tanks (Nintendo, 2006) | Diep.io (Miniclip 2016) | Complex Tanks (my project) |
| :--- | :--- | :--- | :--- | :--- |
| **Perspective** | Top-down 2D | Top-down 2.5D | Top-down 2D | **Top-down 2D** |
| **Movement Physics** | dual-track arcade simulation (linear $x,y$) | standard $x,y$ vector movement | standard $x,y$ vector movement | **Complex number tranformations ($z = x + iy$)** |
| **Obstacles** | static, indestructible walls & landmines | destructible wooden walls and bounce walls | destructible geometry shapes | **Real or Imaginary phase shifting walls** |
| **Aiming Mechanism** | turret locked in chassis direction | independent aiming with Wii remote | independent aiming with mouse | **independent turret rotation with Complex number multiplication** |
| **Game Modes** | 1v1 local PvP | progressive single player waves | Free-for-all, teams, survival, maze | **1v1, player vs bots, bot waves, timed survival** |
| **Progression** | none (only 1v1, PvP) | 100 missions with 20 stages of enemies | upgrades and levels | **various tanks and special upgrades** |
| **Target Audience** | none (pure arcade) | none (pure arcade) | players who enjoy fast paced gameplay | A-level / Further maths students (Education & strategic) |

---

## User Requirements
these requirements were collected from several interviews with classmates in Computer Science and Further Maths.

### Functional Requirements (FR)
- FR1: The system must calculate tank positions using the complex form $z = x + iy$.
- FR2: The system must calculate rotations using Complex multiplication.
- FR3: The system must fire missiles towards the turret's aiming angle when the fire button is pressed.
- FR4: The player must win when there are no enemies left or when all waves are cleared
- FR5: The players record must be set when they die in timed survival
- FR6: The Modulus Swamp gimmick must reduce a tank's velocity by 50% if its position satisfies the condition $|z| < 100$ pixels from the origin.
- FR7: The Complex Conjugate power-up must reflect the user's position across the Real axis (transforming $z = x + iy$ into $\bar{z} = x - iy$).

### Non-Functional Requirements (NFR)
- NFR1: The game must contain a stable update loop of 60 frames per second (fps) to ensure smooth, real-time calculations.
- NFR2: Inputs with Raylib using key presses must be processed within a certain timeframe so movement feels responsive.
- NFR3: The game should consistently handle physics collisions without crashing or letting tanks pass through boundaries.

### Usability Requirements (UR)
- UR1: The game background must display a visible Argand coordinate grid with clearly labeled Real (Re) and Imaginary (Im) axes, divided into the four standard quadrants.
- UR2: The heads-up display (HUD) must render the current HP, ammunition queue, and power-up stack for each player on their respective sides of the screen.

### System Requirements (SR)
- SR1: The minimum specifications needed: dual-core processor, 4GB RAM, standard 1080p monitor.
- SR2: The necessary operating system and runtimes required are: Windows 10/11 and .NET 8.0 Runtime.
- SR3: The tools used to build the game are: Visual studio 2022, C#, Raylib-cs library, GitHub for version control.
