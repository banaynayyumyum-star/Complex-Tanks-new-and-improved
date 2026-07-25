# Complex Tanks
A 2D tank game coded on C# with Raylib, where mechanics and movement are driven by **complex numbers** and the map is an **Argand diagram**.

---

## Table of Contents
1. [Overveiw](#overview)
2. [How to Play & Controls](#how-to-play--controls)
3. [Key Features](#key-features)
4. [Game Modes](#game-modes)
5. [Tank Archetypes](#tank-archetypes)
6. [Power-Ups & Buffs](#power-ups--buffs)
7. [Project Documentation Links](#project-documentation-links)

---

## Overview
*Complex Tanks* is a basic 2D shooter game with many interesting gimmicks leading to strategic gameplay. The biggest standout from other tank games is that the movement and turning is calculated with Complex numbers (i) and the map is an argand diagram.

---

## How to Play & Controls
use strategic maneuvers, mathematical position and buffs to obliterate your enemy.

| Action | Keybind P1 | keybind P2 |
| :--- | :--- | :--- |
| Move Forward | W | Up Arrow |
| Move Backward | S | Down Arrow |
| Rotate Left | A | Left Arrow |
| Rotate Right | D | Right Arrow |
| Fire Missile | Spacebar | Left Mouse Button |

---

## Key features
- **The Modulus Swamp:** if a tank gets within 100 pixels of the origin, its speed will be reduced.
- **Real vs. Imaginary Obstacles:**  specific missiles can clip through specific walls depending on their propeties.

---

## Game Modes
- **Classic 1v1:** 2 player duel to the death.
- **Player vs Bots:** Single-player practice against AI tanks.
- **Timed Survival:** Survive as long as possible against random bots with a disable turret.
- **Waves (stretch goal):** Solo or Co-op waves of enemy bots.

---

## Tank Archetypes
Using Object-Oriented Inheritance, the game features distinct tank classes:
- **Aerodynamic Tank:** Lower health, higher movement speed.
- **Bulky Tank:** Higher health, lower movement speed.
- **Burst Tank:** Default stats, can shoot 3 missiles in a quick burst.
- **Multishot Tank:** Default stats, shoots 3 diverging missiles forwards simultaneously.

---

## Power-Ups & Buffs
- **Complex conjugate ($\bar{z}$):** Instantly reflect the player's tank across the Real axis to dodge an attack.
- **Scalar Multiplier ($kz$):** Temporarily mltiplies the tank's velocity vector by a scalar constant $k$.
- **The $i$-Shield (Rotation):** Rotates all incoming enemy velocity vectors by $90^\circ$ ($\times i$), deflecting missiles.

---

## Project Documentation Links
* [01_Analysis.md](./Documentation/01_Analysis.md)
* [02_Design.md](./Documentation/02_Design.md)
* [03_Implementation.md](./Documentation/03_Implementation.md)
* [04_Testing.md](./Documentation/04_Testing.md)
* [05_Evaluation.md](./Documentation/05_Evaluation.md)

--- 

## Project Progress
- [x] create the Complex number math engine
- [x] create the window with the Raylib import
- [x] draw on the Argand Diagram and modulus swamp
- [x] create the Tank class and get tanks drawn
- [x] create the update method in the Tank class to get them moving
- [x] create the Missile class
- [x] get the missiles moving and destroying tanks
- [ ] setup the health UI
- [ ] setup tank and wall collisions
- [ ] setup the game screens (menu, settings, gamemodes)
