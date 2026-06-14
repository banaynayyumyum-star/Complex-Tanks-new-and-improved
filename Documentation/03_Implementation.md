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

## Iteration 3.




















