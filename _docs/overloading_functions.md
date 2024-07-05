---
 title: Overloading Functions
 category: Polymorphisme
---

In GLU, function overloading allows you to define multiple functions with the same name but different parameters. This enables polymorphism, where the correct function to call is determined by the arguments passed. Here's how you can overload functions in GLU.

## Basic Syntax

To overload a function, you define multiple versions of the same function name with different parameter lists. The compiler determines which function to call based on the number and types of arguments. To make an example we are goinf to overload a function named `calculateArea`.


* Function to Calculate the Area of a Rectangle with Int value

```glu
func calculateArea(length: Int, width: Int) -> Int {
    return length * width;
}
```

* Function to Calculate the Area of a Rectangle with Floats value

```glu
func calculateArea(length: Float, width: Float) -> Float {
    return length * width;
}
```

* Function to Calculate the Area of a Circle
```glu
func calculateArea(radius: Float) -> Float {
    return 3.14159 * radius * radius;
}
```

## Usage Example

When you call the `calculateArea` function, GLU determines which version to execute based on the provided arguments.

```glu
let rectangleArea = calculateArea(5, 3);  // Calls the first version, returns 15

let rectangleArea = calculateArea(5.3, 3.8);  // Calls the first version, returns 20.14

let circleArea = calculateArea(radius: 4.0);                // Calls the second version, returns 50.26544
```

### Key Points

* Type Matching: GLU selects the function based on the type and number of arguments.

* Polymorphism: This feature enables polymorphism by allowing functions to behave differently based on input types.

* Clarity: Function overloading improves code readability by keeping related operations under the same function name.

Conclusion
Function overloading in GLU is a powerful feature that enhances code flexibility and maintainability. By defining multiple versions of a function with different parameter lists, you can handle various data types and use cases efficiently, leveraging polymorphism to create more dynamic and versatile programs.
