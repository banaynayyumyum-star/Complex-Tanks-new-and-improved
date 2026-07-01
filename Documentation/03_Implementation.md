layout:

## Iteration :

### 1. Objective

### 2. Core Code Solution

### 3. Evidence of Testing



# Implementation
Design $\rightarrow$ Code $\rightarrow$ Test $\rightarrow$ Evaluate.

## Iteration 1: Custom Complex Number Math Engine

### 1. Objective
The purpose of this iteration was to build a custom structure to handle Complex number operations like addition subtraction and multiplication. Complex numbers cannot be processed normally due to their ($x + iy$) form so a custom blueprint was required to store the co-ordinates and perform transformations and modulus tracking.

### 2. Core Code Solution
``` csharp
namespace Complex_Tanks_new_and_improved
{
    internal class ComplexNumber
    {
        public double Real = 0;
        public double Imaginary = 0;

        // a custom math engine to create a complex number
        public ComplexNumber(double real, double imaginary)
        {
            Real = real;
            Imaginary = imaginary;
        }

        // a method to add 2 complex numbers together
        public ComplexNumber Add(ComplexNumber otherVector)
        {
            double newReal = this.Real + otherVector.Real;
            double newImaginary = this.Imaginary + otherVector.Imaginary;

            return new ComplexNumber(newReal, newImaginary);
        }

        // a method to subtract 2 complex numbers from eachother
        public ComplexNumber Subtract(ComplexNumber otherVector)
        {
            double newReal = this.Real - otherVector.Real;
            double newImaginary = this.Imaginary - otherVector.Imaginary;

            return new ComplexNumber(newReal, newImaginary);
        }

        // a method to multiple 2 complex numbers together
        public ComplexNumber Multiply(ComplexNumber otherVector)
        {
            double newReal = (this.Real * otherVector.Real) - (this.Imaginary * otherVector.Imaginary);
            double newImaginary = (this.Real * otherVector.Imaginary) + (this.Imaginary * otherVector.Real);

            return new ComplexNumber(newReal, newImaginary);
        }

        // a method to find the modulus of a vector 
        public double CalculateModulus()
        {
            double newModulus = Math.Sqrt((Real * Real) + (Imaginary * Imaginary));

            return newModulus;
        }
    }
}
```

### 3. Evidence of Testing
- Method Verification: To verify the math engine, I created a temporary testing block inside the Main method. I initialised two complex numbers and performed an addition operation. The ouput confirmed the sum was calculated correctly. The calculateModulus method was tested using a 3-4-5 triangle which returned the expected value of 5.0
  - ``` csharp
            // 1. Create two test complex numbers
            ComplexNumber c1 = new ComplexNumber(3, 4);
            ComplexNumber c2 = new ComplexNumber(1, 2);

            // 2. Perform a test calculation (3 + 4i) + (1 + 2i) = (4 + 6i)
            ComplexNumber result = c1.Add(c2);

            // 3. Print the result to the console to see if it works
            Console.WriteLine($"Result: {result.Real} + {result.Imaginary}i");

            // 4. Test the Modulus (sqrt of 3^2 + 4^2 = 5)
            double length = c1.CalculateModulus();
            Console.WriteLine($"Modulus: {length}");
    ```
  - Console Output Verification:
  - <img width="308" height="84" alt="image" src="https://github.com/user-attachments/assets/16619755-2e70-45d0-bad0-59706c359653" />

- Compilation and Error Log Status:
  (The solution compiles cleanly with 0 Errors and 0 Warnings)
  <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c6b4ef02-3e2e-4289-b2e6-78ee1e0426b0" /> 

--- 

## Iteration 2: Rendering the Argand Diagram Arena

### 1. Objective
The purpose of this is to create a background for the Arena so players can see what quadrant they are in and be aware of how specific gimmicks may affect them at the time. The modulus swamp is also added so players can see where they will get slowed. 

### 2. Core Code Solution
``` csharp
// Inside the main game update/draw loop
    // --- ARGAND DIAGRAM AND MODULUS SWAMP ---
    // clears the screen to a white canvas
    ClearBackground(Raylib_cs.Color.White);

    // finds the centre of the screen
    int centerX = GetScreenWidth() / 2;
    int centerY = GetRenderHeight() / 2;
    Vector2 centerV = new Vector2(centerX, centerY);

    // finds the vectors for the starts and ends of each axes
    Vector2 realStart = new Vector2(0, centerY);
    Vector2 realEnd = new Vector2(GetScreenWidth(), centerY);

    Vector2 imaginaryStart = new Vector2(GetScreenWidth() / 2, 0);
    Vector2 imaginaryEnd = new Vector2(GetScreenWidth() / 2, GetScreenHeight());

    // draws real and imaginary axes
    DrawLineEx(realStart, realEnd, 3.0f, Raylib_cs.Color.LightGray);
    DrawLineEx(imaginaryStart, imaginaryEnd, 3.0f, Raylib_cs.Color.LightGray);

    // draws the modulus swamp outline and fills it in with a transparent red
    DrawRing(centerV, 117, 120, 0, 360, 0, Raylib_cs.Color.LightGray);
    Raylib_cs.Color swampColour = new Raylib_cs.Color(255, 0, 0, 50);
    DrawCircle(centerX, centerY, 117, swampColour);
```
Design modification note: when testing the original design for the modulus swamp (100 pixels out from the center) didnt affect the gameplay as much as desired so the radius was increased to 120 pixels to provide a more effective hazard zone.

### 3. Evidence of Testing
- Visual Verification: upon running, the Raylib window opens and both Real and Imaginary axes are drawn 3 pixels wide along with the outline for the modulus swamp and its transparent red filling.
- scaling test: when tested with different screen sizes, the operations are still carried out as desired with no error.
  - Figure 1: fullscreen
  - <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/79a2275b-3bf9-4e5d-ae07-871ba3f45a58" />
  - Figure 2: screen size of 800 by 600
  - <img width="813" height="646" alt="image" src="https://github.com/user-attachments/assets/eb5b3824-ee90-4898-82cf-3d826c7aa5a2" />

- Compilation and Error Log Status:
  (The solution compiles cleanly with 0 Errors and 0 Warnings)
  <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9dd75e85-583e-4c35-8551-8e32cef01f1e" />

## Iteration 3: Vector-Based Tank Mechanics

### 1. Objective
The purpose of this iteration is to get the Tank class running and have a Tank for each player moving comfortably across the map.

### 2. Code Solution
```csharp
using System.Numerics;
using static Raylib_cs.Raylib;
using Color = Raylib_cs.Color;
using KeyboardKey = Raylib_cs.KeyboardKey;
using MouseButton = Raylib_cs.MouseButton;



namespace Complex_Tanks_new_and_improved
{
    // creates the TankType data type
    public enum TankType
    {
        Default,
        Aerodynamic,
        Bulky,
        Burst,
        Multishot,
        TimedSurvival
    }
    internal class Tank
    {
        // --- VARIABLE DECLARATION ---
        private ComplexNumber tankPosition;
        private ComplexNumber tankDirection;

        // set to readonly as they wont be getting changed
        private readonly ComplexNumber rotateLeft = new ComplexNumber(0.9994, 0.0349);
        private readonly ComplexNumber rotateRight = new ComplexNumber(0.9994, -0.0349);

        private float tankSpeed;
        private int tankHP;
        private Color tankColour;
        private TankType Tanktype;
        private bool canFire;

        private KeyboardKey forwardKey;
        private KeyboardKey backwardKey;
        private KeyboardKey rotateLeftKey;
        private KeyboardKey rotateRightKey;

        // keeps all the variables encapsulated
        public Tank(double startX, double startY, int startDirectionX, int startDirectionY, float baseSpeed, int healthPoints, Color colour, TankType typeInput, bool canFireInput,
                    KeyboardKey forward, KeyboardKey backward, KeyboardKey left, KeyboardKey right)
        {
            // sets starting position as a ComplexNumber
            tankPosition = new ComplexNumber(startX, startY);

            // sets direction to face the middle depending on what side you are
            tankDirection = new ComplexNumber(startDirectionX, startDirectionY);
            tankSpeed = baseSpeed;
            tankHP = healthPoints;
            tankColour = colour;
            Tanktype = typeInput;
            canFire = canFireInput;

            // assigns all the keyboard keys
            forwardKey = forward;
            backwardKey = backward;
            rotateLeftKey = left;
            rotateRightKey = right;
        }
        
        public void Update()
        {
            // --- FORWARD MOVEMENT ---
            if (IsKeyDown(forwardKey))
            {
                // creates the movement vector to add to the tanks current position
                ComplexNumber tempMovementVector = new ComplexNumber(tankDirection.Real * tankSpeed, tankDirection.Imaginary * tankSpeed);
                tankPosition = tankPosition.Add(tempMovementVector);
            }
            // --- BACKWARD MOVEMENT ---
            if (IsKeyDown(backwardKey))
            {
                // creates the movement vector to subtract to the tanks current position
                ComplexNumber tempMovementVector = new ComplexNumber(tankDirection.Real * tankSpeed, tankDirection.Imaginary * tankSpeed);
                tankPosition = tankPosition.Subtract(tempMovementVector);
            }
            // --- ROTATIONS ---
            if (IsKeyDown(rotateLeftKey))
            {
                // multiplies the tanks direction by the constant Anit-clockwise rotation
                tankDirection = tankDirection.Multiply(rotateLeft);
            }
            if (IsKeyDown(rotateRightKey))
            {               
                // multiplies the tanks direction by the constant clockwise rotation
                tankDirection = tankDirection.Multiply(rotateRight);
            }

            // --- VECTOR RESET ---
            // divide the vector length by the current modulus to reset it back to exactly 1.0
            double temporaryModulus = tankDirection.CalculateModulus();

            // to prevent a "Divide by Zero" error, only reset it if the modulus is greater than 0
            if (temporaryModulus > 0)
            {
                tankDirection.Real /= temporaryModulus;
                tankDirection.Imaginary /= temporaryModulus;
            }
        }
        public void Draw()
        {
            // find the center of the screen
            float centerX = GetScreenWidth() / 2.0f;
            float centerY = GetScreenHeight() / 2.0f;
            float scale = 30.0f; // 30 pixels per unit

            // convert the tanks position into screen pixels
            float screenX = centerX + ((float)tankPosition.Real * scale);
            float screenY = centerY - ((float)tankPosition.Imaginary * scale);

            // creates the vectors for the position and size of the tank
            Vector2 screenPosition = new Vector2(screenX, screenY);
            Vector2 drawOffset = new Vector2(screenX - 20, screenY - 20);
            Vector2 size = new Vector2(40, 40);

            // creates the vectors for the tanks turret
            Vector2 turretStart = screenPosition;
            Vector2 turretEnd = new Vector2(
                screenX + (float)tankDirection.Real * 50,
                screenY - (float)tankDirection.Imaginary * 50);
            
            // draws the tank and then turret
            DrawRectangleV(drawOffset, size, tankColour);
            DrawLineEx(turretStart, turretEnd, 5.0f, Color.Black);
        }
    }
}
```
#### The Initializing code in the Main method
```csharp
public static void Main()
{
    // Initializes window ("Complex Tanks"), sets it to borderless windowed and sets the frames per second to 60
    InitWindow(800, 600, "Complex Tanks");
    ToggleBorderlessWindowed();
    SetTargetFPS(60);

    // --- finds start position of the tanks ---
    // finds the center X and Y position and the offset the tanks spawn from the wall
    int centerX = GetScreenWidth() / 2;
    int centerY = GetRenderHeight() / 2;
    int offset = 100;

    // calculates the start X and Y positions for each tank (top left and bottom right corners)
    float player1StartX = (offset - centerX) / 30.0f;
    float player1StartY = (centerY - offset) / 30.0f;
    float player2StartX = - player1StartX;
    float player2StartY = - player1StartY;

    float speed = 1.0f / 12.0f;
    int HP = 100;

    // player 1 and player 2 are drawn
    Tank player1 = new Tank(player1StartX, player1StartY, 1, 0, speed, HP, Color.Red, TankType.Default, true,
                   KeyboardKey.W, KeyboardKey.S, KeyboardKey.A, KeyboardKey.D);
    Tank player2 = new Tank(player2StartX, player2StartY, -1, 0, speed, HP, Color.Blue, TankType.Default, true,
                   KeyboardKey.Up, KeyboardKey.Down, KeyboardKey.Left, KeyboardKey.Right);

    // --- main loop (runs 60 times per second) ---
    while (!WindowShouldClose())
    {
        // updates the players positions as they move
        player1.Update();
        player2.Update();

        // ----------------------------------------------------------------------------------

        // if fire keys are being pressed, it calls the fire method to shoot a missile
        if (IsKeyPressed(KeyboardKey.Space))
        {
            player1.Fire();
        }
        if (IsMouseButtonPressed(MouseButton.Left))
        {
            player2.Fire();
        }

        // if power-up keys are being pressed, it calls the PowerUp method to use the power-up
        if (IsKeyPressed(KeyboardKey.E))
        {
            player1.PowerUp();
        }
        if (IsMouseButtonPressed(MouseButton.Right))
        {
            player2.PowerUp();
        }

        // ----------------------------------------------------------------------------------

        BeginDrawing();

        // ----------------------------------------------------------------------------------

        // --- ARGAND DIAGRAM AND MODULUS SWAMP ---
        // clears the screen to a white canvas
        ClearBackground(Color.White);

        // finds the centreVector of the screen with centerX and centerY from earlier
        Vector2 centerVector = new Vector2(centerX, centerY);

        // finds the vectors for the starts and ends of each axes
        Vector2 realStart = new Vector2(0, centerY);
        Vector2 realEnd = new Vector2(GetScreenWidth(), centerY);

        Vector2 imaginaryStart = new Vector2(GetScreenWidth() / 2, 0);
        Vector2 imaginaryEnd = new Vector2(GetScreenWidth() / 2, GetScreenHeight());

        // draws real and imaginary axes
        DrawLineEx(realStart, realEnd, 3.0f, Color.LightGray);
        DrawLineEx(imaginaryStart, imaginaryEnd, 3.0f, Color.LightGray);

        // draws the modulus swamp outline and fills it in with a transparent red
        Color swampColour = new Color(255, 0, 0, 50);
        DrawCircle(centerX, centerY, 118, swampColour);
        DrawRing(centerVector, 117, 120, 0, 360, 0, Color.LightGray);

        // ----------------------------------------------------------------------------------

        // draws both tanks so they keep their updated positions
        player1.Draw();
        player2.Draw();

        EndDrawing();
    }
```

### 3. Evidence of Testing
- Evaluation: The implementation of the Tank() class successfully uses the ComplexNumber math engine and Raylib rendering. By fixing the bug, the movement system is now consistent and smooth, and ready to support the upcoming combat mechanics in Iteration 4.
- Visual Verification: Upon running, 2 tanks (one Red and one Blue) are drawn onto the Argand diagram, each 100 pixels from the corners. The red tank can be moved with W, A, S, D and the blue tank with Up, Left, Down, Right Arrows.
- Beta Test Issue: During the initial movement testing, a physics bug was found where the tanks accelerated exponentially across the screen into infinity when turning keys were held down
- Issue Fix: The bug was caused by tiny floating-point rounding errors during complex multiplication causing the direction vector's modulus to continuously increase above 1.0. A reset was implemented at the end of the Update() method to regulate the modulus and scale the vector components back to a length of 1.0 every frame to keep gameplay consistent.
  - Figure 1: upon start
  - <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/049b4156-862d-4db0-ab2d-e015798581ce" />
  - Figure 2: after movement
  - <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/23d58af9-ab82-4646-9dda-2c4724e93627" />

- Compilation and Error Log Status:
  (The solution compiles cleanly with 0 Errors and 0 Warnings)
- <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/620b38b4-31a0-4523-aeb2-bf4f214f96ff" />

## Iteration 4: Combat mechanics

### 1. Objective
the goal of this iteration is to have the tanks firing missiles which get destroyed when touching an enemy tank or reaching the borders. The missiles should also do damage to the tanks and have them get destroyed when health reaches 0.
In this case the missiles do 25 damage and the tanks have 100 HP

### 2. Core Code Solution
```csharp
using System.Numerics;
using static Raylib_cs.Raylib;
using Color = Raylib_cs.Color;
using KeyboardKey = Raylib_cs.KeyboardKey;
using MouseButton = Raylib_cs.MouseButton;

namespace Complex_Tanks_new_and_improved
{
    public enum MissileType
    {
        Default,
        Real,
        Imaginary,
        Bounce
    }
    internal class Missile
    {
        // --- VARIABLE DECLARATION ---
        private ComplexNumber missilePosition;
        private ComplexNumber missileDirection;

        private float missileSpeed;
        private Color missileColour;
        private MissileType missileType;
        private bool isDestructable;
        private int missileDamage;

        //  missile position and missile damage become public so the main program can access them
        public ComplexNumber Position => missilePosition;
        public int Damage => missileDamage;

        // keeps all the variables encapsulated
        public Missile(double startX, double startY, double startDirectionX, double startDirectionY, float baseSpeed, Color colour, MissileType inputType, bool inputIsDestructable)
        {


            missilePosition = new ComplexNumber(startX, startY);
            missileDirection = new ComplexNumber(startDirectionX, startDirectionY);

            missileSpeed = baseSpeed;
            missileColour = colour;
            missileType = inputType;
            isDestructable = inputIsDestructable;

            // will set the missiles damage to different values depending on what missile type it is
            switch (inputType)
            {
                case MissileType.Default:
                    missileDamage = Constants.DEFAULT_MISSILE_DAMAGE;
                    break;
                case MissileType.Bounce:
                    missileDamage = Constants.BOUNCE_MISSILE_DAMAGE;
                    break;
                case MissileType.Real:
                    missileDamage = Constants.REAL_MISSILE_DAMAGE;
                    break;
                case MissileType.Imaginary:
                    missileDamage = Constants.IMAGINARY_MISSILE_DAMAGE;
                    break;

            }
        }

        public void Draw()
        {
            // find the center of the screen
            float centerX = GetScreenWidth() / 2.0f;
            float centerY = GetScreenHeight() / 2.0f;

            // convert the missiles position into screen pixels
            float screenX = centerX + ((float)missilePosition.Real * Constants.PIXEL_SCALE);
            float screenY = centerY - ((float)missilePosition.Imaginary * Constants.PIXEL_SCALE);

            // creates the vector for the position of the missile
            Vector2 missileSpawnPosition = new Vector2(screenX, screenY);

            // draws circular missile
            DrawCircleV(missileSpawnPosition, Constants.MISSILE_SIZE, missileColour);
        }

        public void Update()
        {
            ComplexNumber tempMovement = new ComplexNumber(missileDirection.Real * missileSpeed, missileDirection.Imaginary * missileSpeed);
            missilePosition = missilePosition.Add(tempMovement);
        }

        public bool IsOutOfBounds()
        {
            // find the center of the screen
            float centerX = GetScreenWidth() / 2.0f;
            float centerY = GetScreenHeight() / 2.0f;

            // convert the missiles position into screen pixels
            float screenX = centerX + ((float)missilePosition.Real * Constants.PIXEL_SCALE);
            float screenY = centerY - ((float)missilePosition.Imaginary * Constants.PIXEL_SCALE);

            // checks if the missile has passed any of the 4 borders
            if (screenX < 0 || screenX > GetScreenWidth() || screenY < 0 || screenY > GetScreenHeight())
            {
                return true;
            }
            return false;
        }
    }
}
```
#### The Missile logic in the Tank class methods
```csharp
public void Fire(MissileType bulletType)
{
    double turretLength = Constants.TURRET_LENGTH / Constants.PIXEL_SCALE;
    double turretLengthX = turretLength * tankDirection.Real;
    double turretLengthY = turretLength * tankDirection.Imaginary;

    double startX = tankPosition.Real + turretLengthX;
    double startY = tankPosition.Imaginary + turretLengthY;

    // creates a new missile and adds it to the list of active missiles
    Missile newMissile = new Missile(startX, startY, tankDirection.Real, tankDirection.Imaginary, tankSpeed * 3, tankColour, bulletType, true);
    activeMissiles.Add(newMissile);
}

public void Update()
{
    // --- FORWARD MOVEMENT ---
    if (IsKeyDown(forwardKey))
    {
        // creates the movement vector to add to the tanks current position
        ComplexNumber tempMovementVector = new ComplexNumber(tankDirection.Real * tankSpeed, tankDirection.Imaginary * tankSpeed);
        tankPosition = tankPosition.Add(tempMovementVector);
    }
    // --- BACKWARD MOVEMENT ---
    if (IsKeyDown(backwardKey))
    {
        // creates the movement vector to subtract to the tanks current position
        ComplexNumber tempMovementVector = new ComplexNumber(tankDirection.Real * tankSpeed, tankDirection.Imaginary * tankSpeed);
        tankPosition = tankPosition.Subtract(tempMovementVector);
    }
    // --- ROTATIONS ---
    if (IsKeyDown(rotateLeftKey))
    {
        // multiplies the tanks direction by the constant Anit-clockwise rotation
        tankDirection = tankDirection.Multiply(rotateLeft);
    }
    if (IsKeyDown(rotateRightKey))
    {
        // multiplies the tanks direction by the constant clockwise rotation
        tankDirection = tankDirection.Multiply(rotateRight);
    }

    // --- VECTOR RESET ---
    // divide the vector length by the current modulus to reset it back to exactly 1.0
    double temporaryModulus = tankDirection.CalculateModulus();

    // to prevent a "Divide by Zero" error, only reset it if the modulus is greater than 0
    if (temporaryModulus > 0)
    {
        tankDirection.Real /= temporaryModulus;
        tankDirection.Imaginary /= temporaryModulus;
    }

    for (int i = activeMissiles.Count - 1; i >= 0; i--)
    {
        activeMissiles[i].Update();
        if (activeMissiles[i].IsOutOfBounds())
        {
            activeMissiles.RemoveAt(i);
        }
    }

}
public void Draw()
{
    // find the center of the screen
    float centerX = GetScreenWidth() / 2.0f;
    float centerY = GetScreenHeight() / 2.0f;

    // convert the tanks position into screen pixels
    float screenX = centerX + ((float)tankPosition.Real * Constants.PIXEL_SCALE);
    float screenY = centerY - ((float)tankPosition.Imaginary * Constants.PIXEL_SCALE);

    // creates the vectors for the position and size of the tank
    Vector2 screenPosition = new Vector2(screenX, screenY);
    Vector2 drawOffset = new Vector2(screenX - Constants.TANK_HALF_SIZE, screenY - Constants.TANK_HALF_SIZE);
    Vector2 size = new Vector2(Constants.TANK_SIZE, Constants.TANK_SIZE);

    // creates the vectors for the tanks turret
    Vector2 turretStart = screenPosition;
    Vector2 turretEnd = new Vector2(
        screenX + (float)tankDirection.Real * Constants.TURRET_LENGTH,
        screenY - (float)tankDirection.Imaginary * Constants.TURRET_LENGTH);

    // draws the tank and then turret
    DrawRectangleV(drawOffset, size, tankColour);
    DrawLineEx(turretStart, turretEnd, 7.0f, Color.Black);

    for (int i = 0; i < activeMissiles.Count; i++)
    {
        activeMissiles[i].Draw();
    }            
}

public bool HasBeenHit(ComplexNumber tempMissilePosition)
{
    // finds the missiles relative position to the tank
    ComplexNumber missileRelativePosition = tempMissilePosition.Subtract(tankPosition);

    // calculates the complex conjugate of the tanks direction to multiply with the relative position to unrotate the missile
    ComplexNumber reverseRotation = tankDirection.ComplexConjugate();
    ComplexNumber unRotatedMissilePosition = missileRelativePosition.Multiply(reverseRotation);

    // calculates the invisible hitbox of the tank
    double halfSize = Constants.TANK_HALF_SIZE / Constants.PIXEL_SCALE;

    // checks if the missile is within its hitbox 
    if (Math.Abs(unRotatedMissilePosition.Real) <= halfSize && Math.Abs(unRotatedMissilePosition.Imaginary) <= halfSize)
    {
        return true;
    }
    return false;
}

public void TakeDamage(int damageReceived)
{
    // deals the tank damage
    tankHP -= damageReceived;

    // checks if the damage has put the health below 0, if so kill the tank and reset its health to 0 
    if (tankHP <= 0)
    {
        tankHP = 0;
        isAlive = false;
    }
}
```
#### The Initializing code in the Main method
```csharp
public static void Main()
{
    // Initializes window ("Complex Tanks"), sets it to borderless windowed and sets the frames per second to 60
    InitWindow(Constants.SCREEN_WIDTH, Constants.SCREEN_HEIGHT, "Complex Tanks");
    ToggleBorderlessWindowed();
    SetTargetFPS(60);

    // --- finds start position of the tanks ---
    // finds the center X and Y position and the offset the tanks spawn from the wall
    int centerX = GetScreenWidth() / 2;
    int centerY = GetRenderHeight() / 2;
    int offset = 100;

    // calculates the start X and Y positions for each tank (top left and bottom right corners)
    float player1StartX = (offset - centerX) / Constants.PIXEL_SCALE;
    float player1StartY = (centerY - offset) / Constants.PIXEL_SCALE;
    float player2StartX = -player1StartX;
    float player2StartY = -player1StartY;

    float speed = 1.0f / 12.0f;
    int HP = 100;

    List<Tank> aliveTanks = new List<Tank>();

    // player 1 and player 2 are drawn
    Tank player1 = new Tank(player1StartX, player1StartY, 1, 0, speed, HP, Color.Red, TankType.Default, true, true,
                   KeyboardKey.W, KeyboardKey.S, KeyboardKey.A, KeyboardKey.D);
    Tank player2 = new Tank(player2StartX, player2StartY, -1, 0, speed, HP, Color.Blue, TankType.Default, true, true,
                   KeyboardKey.Up, KeyboardKey.Down, KeyboardKey.Left, KeyboardKey.Right);

    // adds both players to a list of living playe
    aliveTanks.Add(player1);
    aliveTanks.Add(player2);

    // --- MAIN LOOP (runs 60 times per second) ---
    while (!WindowShouldClose())
    {

        // a loop to deal the tanks damage when they are hit my an enemies missile
        for (int i = 0; i < aliveTanks.Count; i++)
        {
            for (int j = aliveTanks[i].ActiveMissiles.Count - 1; j >= 0; j--)
            {
                ComplexNumber missilePos = aliveTanks[i].ActiveMissiles[j].Position;

                for (int k = 0; k < aliveTanks.Count; k++)
                {
                    if (!(i == k))
                    {
                        Tank enemyTank = aliveTanks[k];
                        if (enemyTank.HasBeenHit(missilePos))
                        {
                            int damage = aliveTanks[i].ActiveMissiles[j].Damage;

                            enemyTank.TakeDamage(damage);

                            aliveTanks[i].ActiveMissiles.RemoveAt(j);
                            break;
                        }
                    }
                }
            }
        }

        // updates the players positions as they move if they are alive
        for (int i = aliveTanks.Count - 1; i >= 0; i--)
        {
            aliveTanks[i].Update();

            if (!aliveTanks[i].IsAlive)
            {
                aliveTanks.RemoveAt(i);
            }
        }
        
        // ----------------------------------------------------------------------------------

        // --- KEY RECOGNITION ---
        // if fire keys are being pressed, it calls the fire method to shoot a missile
        if (IsKeyPressed(KeyboardKey.Space))
        {
            player1.Fire(MissileType.Default);
        }
        if (IsMouseButtonPressed(MouseButton.Left))
        {
            player2.Fire(MissileType.Default);
        }

        // if power-up keys are being pressed, it calls the PowerUp method to use the power-up
        if (IsKeyPressed(KeyboardKey.E))
        {
            player1.PowerUp();
        }
        if (IsMouseButtonPressed(MouseButton.Right))
        {
            player2.PowerUp();
        }

        // ----------------------------------------------------------------------------------

        BeginDrawing();

        // ----------------------------------------------------------------------------------

        // --- ARGAND DIAGRAM AND MODULUS SWAMP ---
        // clears the screen to a white canvas
        ClearBackground(Color.White);

        // finds the centreVector of the screen with centerX and centerY from earlier
        Vector2 centerVector = new Vector2(centerX, centerY);

        // finds the vectors for the starts and ends of each axes
        Vector2 realStart = new Vector2(0, centerY);
        Vector2 realEnd = new Vector2(GetScreenWidth(), centerY);

        Vector2 imaginaryStart = new Vector2(GetScreenWidth() / 2, 0);
        Vector2 imaginaryEnd = new Vector2(GetScreenWidth() / 2, GetScreenHeight());

        // draws real and imaginary axes
        DrawLineEx(realStart, realEnd, 3.0f, Color.LightGray);
        DrawLineEx(imaginaryStart, imaginaryEnd, 3.0f, Color.LightGray);

        // draws the modulus swamp outline and fills it in with a transparent red
        Color swampColour = new Color(255, 0, 0, 50);
        DrawCircle(centerX, centerY, 118, swampColour);
        DrawRing(centerVector, 117, 120, 0, 360, 0, Color.LightGray);

        // ----------------------------------------------------------------------------------

        // draws both tanks so they keep their updated positions
        foreach (Tank aliveTank in aliveTanks)
        {
            aliveTank.Draw();
        }

        EndDrawing();
    }
}
```

### 3. Evidence of Testing
- Evaluation: The addition of the Missile() class uses the ComplexNumber engine and Raylib rendering successfully. A bug was found in the initial drawing of the missile as its actual position was 40 pixels ahead of its visual position due to a mixup with the Complex Units and Screen Pixels.
- Visual Verification: Upon startup, both tanks can fire with their given fire keys (space for Red, left click for Blue), when the enemy tank has taken 4 hits it disappears and can no longer take inputs.
- Beta Test Issue: During the initial drawing of the Missile, a bug was found where the missiles visual position was 40 pixels ahead of its actual position, so the missile would pass through the borders and enemy tanks by 40 pixels and only then would it disappear.
- Issue Fix: The reason for the issue was that when trying to make the missile spawn at the end of the turret (the turret was 40 pixels long) I just added 40 without converting it. Converting the pixel length into complex units before adding it to the tanks position by dividing it by the conversion scale was enough to fix the issue.
  - Figure 1: whilst firing
  - <img width="957" height="535" alt="image" src="https://github.com/user-attachments/assets/b22de7b1-2ffb-4cc5-9bad-472557ea7a2b" />
  - Figure 2: after eliminating blue
  - <img width="1920" height="1060" alt="image" src="https://github.com/user-attachments/assets/6381e247-e224-492a-9378-f785c074618a" />
  
- Compilation and Error Log Status:
  (The solution compiles cleanly with 0 Errors and 0 Warnings)
- <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d6d15ce5-8a6d-457b-8c06-ba69c4de3ac3" />

