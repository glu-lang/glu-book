---
title: Using Enums
category: Data Types
---

Enums (short for enumerations) are a way to define a type that can only take one of a few predefined values. They are useful for representing a set of related constants or options.

## Declaring an Enum

In Glu, you can declare an enum using the `enum` keyword followed by the enum's name, underlying type, and its cases. 

Here is an example of an enum declaration:

```glu
enum Direction: Int8 {
    NORTH,
    SOUTH,
    EAST,
    WEST,
}
```

In this example, the enum `Direction` has an underlying type of `Int8`, which means the enum will be represented as an 8-bit integer. The cases `NORTH`, `SOUTH`, `EAST`, and `WEST` are the possible values that a variable of type `Direction` can take:

```glu
let direction: Direction = Direction::NORTH;
```

When referencing a case of an enum, you must use the double colon `::` scope resolution operator, followed by the case name.

## Explicit Raw Values

You can assign explicit raw values to the cases of an enum. This is useful when you want to assign specific values to each case.

Here is an example of an enum with explicit raw values:

```glu
enum Tile: Char {
    EMPTY = ' ',
    WALL = '#',
    PLAYER = '@',
}
```

In this example, the enum `Tile` has an underlying type of `Char`, and each case has an explicit raw value assigned to it. The raw value is specified after the case name using the assignment operator `=`.

The raw values must be unique within the enum and must match the underlying type of the enum. If you don't specify raw values for the cases, the compiler will automatically assign them incrementing values starting from 0.

The raw values can be of any integer type, such as `Int`, `Int8`, `Int16`, `Int32`, `Int64`, `UInt`, `UInt8`, `UInt16`, `UInt32`, `UInt64`, or `Char`.

## Accessing Raw Values

The raw value of an enum case can be accessed using the `rawValue` property, and enum cases can be created from raw values using initializer syntax:

```glu
let tile: Tile = Tile::PLAYER;
let rawPlayer: Char = tile.rawValue;
let playerTile: Tile = { rawPlayer };
```

In this example, the raw value of the `PLAYER` case is accessed using the `rawValue` property, and a new `Tile` instance is created from the raw value using initializer syntax.

If you try to create an enum case from a raw value that doesn't match any of the cases, the program will throw a runtime error. This makes it safer to work with enums, as you can be sure that the raw value corresponds to a valid case. If this behavior is not desired, you can instead use an open enum.

## Open Enums

An open enum allows creating enum cases from raw values that don't match any of the defined cases. This can be useful when working with external data sources or when you want to allow additional cases to be added in the future.

To declare an open enum, you can use the `@open` attribute before the `enum` keyword:

```glu
@open enum Color: UInt32 {
    RED = 0xFF0000,
    GREEN = 0x00FF00,
    BLUE = 0x0000FF,
}

let yellow: Color = { 0xFFFF00 };
```

In this example, the `Color` enum is declared as open, allowing the creation of additional cases from raw values that don't match the defined cases. The `yellow` variable is assigned the color yellow, which is not explicitly defined in the enum.

When working with open enums, you should be aware that values of this type can be created from any value of the underlying type, not just the explicitly defined cases.

## Overloading Operators

Just like any other type, enums can have operators overloaded to provide custom behavior. This can be useful when you want to define custom behavior for comparison operators or arithmetic operators.

Here is an example of overloading the bitwise OR operator `|` for the `Color` enum:

```glu
operator |(a: Color, b: Color) -> Color {
    return { a.rawValue | b.rawValue };
}
```

In this example, the `|` operator is overloaded to perform a bitwise OR operation on two `Color` instances. The operator calls the bitwise OR operator on the raw values of the two colors and returns a new `Color` instance with the result.

This allows you to use the `|` operator to combine two colors:

```glu
let magenta: Color = Color::RED | Color::BLUE;
```

In this example, the `magenta` variable is assigned the color magenta, which is a combination of red and blue.

## Restricting Enums

Enums can use another enum as their underlying type, which permits creating an enum that can only take a subset of the values of the underlying enum. This can be useful when you want to restrict the possible values of an enum to a specific subset.

Here is an example of an enum that restricts the possible values to a subset of the `Color` enum:

```glu
enum PrimaryColor: Color {
    RED = Color::RED,
    GREEN = Color::GREEN,
    BLUE = Color::BLUE,
}
```

In this example, the `PrimaryColor` enum restricts the possible values to the primary colors red, green, and blue, which are defined in the `Color` enum. Because `PrimaryColor` is a closed enum (it is not defined with the `@open` attribute), it can only take the values explicitly defined in the enum: `RED`, `GREEN`, and `BLUE`.

Because `PrimaryColor` uses `Color` as its underlying type, its raw value will be of type `Color`:

```glu
let primaryColor: PrimaryColor = PrimaryColor::RED;
let color: Color = primaryColor.rawValue;
std::assert(color == Color::RED);
```

Because the raw value of `PrimaryColor::RED` is defined as `Color::RED`, the raw value of `primaryColor` will be `Color::RED`. This allows you to convert between the two enums and ensures that the values are compatible.

## Conclusion

Enums are a powerful feature in Glu that allow you to define custom types with a fixed set of values. They can be used to represent a set of related constants or options and provide type safety and clarity to your code. By using enums, you can make your code more expressive and easier to understand.
