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
- Inheritance: each type of tank will have these base properties with each having its own unique trait which will override the default ones EG: an Aerodynamic tank will override the speed and health

### Missile
- Variables: speed (Type: double), colour (Type: Color), position (Type: ComplexNumber), direction (Type: ComplexNumber), type (Type: enum), isDestructable (Type: bool)
- Methods: move in the direction the tank is facing, bounce of walls (if it can), damage tank
- Inheritance: each missile will have these properties but specific missiles can override them EG: an imaginary missile will override the type property to 'Imaginary' and bypass Real walls

### PowerUp
- Variables: type (Type: enum), whatItAffects (Type: string / target object reference)
- Methods: apply buff 

### Obstacle/Wall
- Variables: position (Type: ComplexNumber), type (Type: enum), colour (Type: Color)
- Methods: check collision

---

## Algorithmic Design

### How the Tank moves and turns (Pseudo-Code)

// all un-set variable have been set elsewhere in the code
```text
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

## User Interface Design

- Main Menu $\rightarrow$ Game Modes $\rightarrow$ Gameplay Arena $\rightarrow$ Game OVer
- Main Menu $\leftrightarrow$ Settings
- Main Menu $\leftrightarrow$ Help

- Main Menu Screen: Includes a title ('Complex Tanks') accompanied by a 'Play' button, 'settings' button and a 'help' button. An 'Exit' button is also included slightly below.
- Game Modes Screen: Includes a list of the game modes (1v1 Local PvP, Player vs Bots, Timed Survival) and a 'back' button.
- Gameplay Arena: Background with a rendered, labeled Argand diagram intersecting at (GetScreenWidth/2, GetScreenHeight/2).
  - The center features a shaded circle representing the Modulus Swamp ($|z| < 100$).
  - Top-Left HUD tracks Player 1's data (HP, ammunition queue, Power-Up stack)
  - Top-Right HUD tracks Player 2's data
- Game Over Screen: Displays the outcome of the game (EG: "Player 1 Wins!" or "time survived: X seconds") with navigation options to the Main Menu
- Settings Screen: Shows all customisable features (tank colours, Cheat Settings)
- Help Screen: Presents you with a description of the game, its controls, and how each game mode works
