# Implementation

## Iteration 1: Custom Complex Number Math Engine
### 1. Objective
The purpose of this iteration was to build a custom structure to handle Complex number operations like addition subtraction and multiplication. Complex numbers cannot be processed normally due to their ($x + iy$) form so a custom blueprint was required to store the co-ordinates and perform transformations and modulus tracking.

### 2. Core Code Solution
``` text
namespace Complex_Tanks_new_and_improved
{
    internal class ComplexNumber
    {
        public double Real = 0;
        public double Imaginary = 0;

        public ComplexNumber(double real, double imaginary)
        {
            Real = real;
            Imaginary = imaginary;
        }

        public ComplexNumber Add(ComplexNumber otherVector)
        {
            double newReal = this.Real + otherVector.Real;
            double newImaginary = this.Imaginary + otherVector.Imaginary;

            return new ComplexNumber(newReal, newImaginary);
        }

        public ComplexNumber Subtract(ComplexNumber otherVector)
        {
            double newReal = this.Real - otherVector.Real;
            double newImaginary = this.Imaginary - otherVector.Imaginary;

            return new ComplexNumber(newReal, newImaginary);
        }

        public ComplexNumber Multiply(ComplexNumber otherVector)
        {
            double newReal = (this.Real * otherVector.Real) - (this.Imaginary * otherVector.Imaginary);
            double newImaginary = (this.Real * otherVector.Imaginary) + (this.Imaginary * otherVector.Real);

            return new ComplexNumber(newReal, newImaginary);
        }

        public double CalculateModulus()
        {
            double newReal = this.Real;
            double newImaginary = this.Imaginary;
            double newModulus = Math.Sqrt((newReal * newReal) + (newImaginary * newImaginary));

            return newModulus;
        }
    }
}
```

### 3. Evidence of Testing
- Method Verification: To verift the math engine, I created a temporary testing block insinde the Main method. I initialised to two complex numbers and performed an addition operation. The ouput confirmed the sum was calculated correctly. The calculateModulus method was tested using a 3-4-5 triangle which returned the expected value of 5.0
  - ```testing
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
  - <img width="308" height="84" alt="image" src="https://github.com/user-attachments/assets/16619755-2e70-45d0-bad0-59706c359653" />



<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c6b4ef02-3e2e-4289-b2e6-78ee1e0426b0" /> 
0 Errors, 0 Warnings
