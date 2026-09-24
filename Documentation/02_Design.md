# Design
This game will be coded in Object oriented programming (OOP) so different tank types can inherit properties of the main tank class, or the same with missiles

## Class and Data Design
Inheritance: each type of tank will have these base properties with each having its own unique trait which will override the default ones EG: an Aerodynamic tank will override the speed and health.

### Objects
- Tank
- Missile
- PowerUp
- Obstacle/Wall

### Tank
- Variables: speed (Type: double), colour (Type: Color), position (Type: ComplexNumber), direction (Type: ComplexNumber), type (Type: enum), HP (Type: int), uniqueTrait (Type: string / custom method)
- Methods: move forward/backward, rotate left/right, fire Missile, use PowerUp
- Inheritance: subclasses (`AerodynamicTank`, `BulkyTank`) inherit movement and combat logic while overriding health and speed attributes

### Missile
- Variables: speed (Type: double), colour (Type: Color), position (Type: ComplexNumber), direction (Type: ComplexNumber), type (Type: enum), isDestructable (Type: bool)
- Methods: move in tank facing direction, bounce of walls, damage tank
- Inheritance: derived missile classes override properties (e.g., `ImaginaryMissile` overrides wall collision checks to phase through walls of the matching phase type)

### PowerUp
- Variables: type (Type: enum), whatItAffects (Type: string / target object reference)
- Methods: apply buff (e.g., execute Complex Conjugate position reflection)
  
### Obstacle/Wall
- Variables: position (Type: ComplexNumber), type (Type: enum), colour (Type: Color)
- Methods: check collision

---

## Algorithmic Design

### Frame Execution Logic Flowchart
The flowchart below shows the frame-by-frame physics loop executed inside the main program every cycle. It models how complex vector position are checked against the Modulus Swamp ($|z| < 100$) for debuffs, followed by phase-matching wall collision handing.

![System Logic Flowchart](<img width="440" height="779" alt="image" src="https://github.com/user-attachments/assets/f69a4779-f41d-4c7c-9ae2-0ea5309300ff" />)

---

### How the Tank moves and turns (Pseudo-Code)
```text
// all un-set variable have been set elsewhere in the code
tankPosition = Complex(startPosX, startPosY)
tankDirection = Complex(1, 0)
tankSpeed = baseSpeed
tankColour = colour
rotateLeft = Complex(0.9994, 0.0349)
rotateRight = Complex(0.9994, -0.0349)

if keypressed = W THEN
  tankPosition += tankDirection * tankSpeed
else if keypressed = S THEN
  tankPosition -= tankDirection * tankSpeed
else if keypressed = A THEN
  tankDirection *= rotateLeft
else if keypressed = D THEN
  tankDirection *= rotateRight
endif
```

### How the Modulus Swamp works (Pseudo-Code)
```text
// tankSpeed must be reset at the beginning of the update loop before all code
tankSpeed = baseSpeed 

centerPoint = Complex(GetScreenWidth / 2, GetScreenHeight / 2)
relativePosition = tankPosition - centerPoint
range = 100
speedDebuff = 0.5

if CalculateModulus(relativePosition) <= range THEN
  Tank.tankSpeed *= speedDebuff
endif
 ```

### How a bullet checks if its matching the phase of a wall (Pseudo-Code)
``` text
// all un-set variable have been set elsewhere in the code

distanceVector = missilePosition - wallPosition
actualDistance = CalculateModulus(distanceVector)
collisionThreshold = missileRadius + wallRadius
missileType = Missile.type
wallType = Wall.type

if actualDistance <= collisionThreshold
  if missileType = wallType THEN
    missile.isColliding = false
  else
    missile.isColliding = true
    wall.triggerImpact()
  endif
endif
```

---

## Application

### 1. Queues
- **used for:** Managing the tanks upcoming ammunition list and rendering a live ammunition HUD (Head-up display).
- **why this structure:** A queue uses a FIFO (first in first out) order. This same design can be seen by an ammunition belt: the first missile loaded is the first fired.
- **benefits:**
  - $O(1)$ constant time complexity for enqueueing and dequeueing
  - prevents the player from skipping or changing the order of the ammunition.

### 2. Stacks
- **used for:** Navigating between game menus (main menu, gameplay arena, game over, help overlay) and managing the players inventory of collected power-ups.
- **why this structure:** When a player presses 'H' during a match, the `HelpOverlayScreen` is pushed onto the LIFO (last-in, first-out) stack on top of the active `GameplayArenaScreen`. Pressing `Escape` pops the overlay off. For power-ups, pushing collected items onto a stack ensures the most recently acquired power-up is used first.
- **benefits:**
  - $O(1)$ push/pop time complexity for instant screen changing
  - eliminates complex structures getting messy and unreadable

### 3. Dictionary
- **used for:** Storing the key-binds for both players and UI navigation. Also for looking up properties for Tank/Missile types.
- **why this structure:** the structure stores data in key-value pairs to replace multiple individual fields with a single controller object.
- **benefits:**
  - ensures cleaner encapsulation and maintainability
  - $O(1)$ constant time complexity per lookup

---

## User Interface Design

### Screen Descriptions
- Main Menu Screen: Includes a title ('Complex Tanks') accompanied by a 'Play' button, and a 'help' button. An 'Exit' button is also included slightly below.
- Gameplay Arena: Background with a rendered, labeled Argand diagram intersecting at (GetScreenWidth/2, GetScreenHeight/2).
  - The center features a shaded circle representing the Modulus Swamp ($|z| < 100$).
  - Top-Left HUD tracks Player 1's data (HP, ammunition queue, Power-Up stack)
  - Top-Right HUD tracks Player 2's data
- Game Over Screen: Displays the outcome of the game (EG: "Player 1 Wins!") with navigation options to the Main Menu
- Help Screen: Presents you with a description of the game, its controls and how the complex math mechanics work

### Navigation Flow
- Main Menu $\rightarrow$ Gameplay Arena $\rightarrow$ Game Over
- Main Menu $\leftrightarrow$ Help
