# Delegates in C#: Bridging OOP with Functional and Event-Driven Programming

## Table of Contents
1. [Introduction](#introduction)
2. [Functional Programming with Delegates](#functional-programming-with-delegates)
3. [Event-Driven Programming with Delegates](#event-driven-programming-with-delegates)
4. [Alternatives in Other Languages](#alternatives-in-other-languages)

## Introduction

C# is primarily an object-oriented programming language. However, with the introduction of delegates, C# can also implement functional programming and event-driven programming paradigms. This README explores the versatility of delegates in C# and their applications.

## Functional Programming with Delegates

Delegates in C# allow for functional programming concepts by enabling:

1. Storing functions in variables
2. Passing functions as parameters (callbacks)
3. Returning functions from other functions

### Example: Storing and Passing Functions

```csharp
// Declare a delegate type
public delegate int MathOperation(int x, int y);

// Method that uses a delegate
public static void PerformOperation(int a, int b, MathOperation operation)
{
    int result = operation(a, b);
    Console.WriteLine($"Result: {result}");
}

// Usage
MathOperation add = (x, y) => x + y;
PerformOperation(5, 3, add);  // Output: Result: 8

MathOperation multiply = (x, y) => x * y;
PerformOperation(5, 3, multiply);  // Output: Result: 15
```

## Event-Driven Programming with Delegates

Delegates are fundamental to event-driven programming in C#. They allow objects to:

1. Subscribe to events
2. Notify subscribers when an event occurs

### Example: Ball Game Notification System

```csharp
public class Ball
{
    // Declare a delegate for the event
    public delegate void PositionChangedEventHandler(int x, int y);

    // Declare the event using the delegate
    public event PositionChangedEventHandler PositionChanged;

    private int _x, _y;

    public void Move(int newX, int newY)
    {
        _x = newX;
        _y = newY;
        // Trigger the event
        PositionChanged?.Invoke(_x, _y);
    }
}

public class Player
{
    public string Name { get; }

    public Player(string name)
    {
        Name = name;
    }

    public void OnBallMoved(int x, int y)
    {
        Console.WriteLine($"{Name} noticed the ball moved to ({x}, {y})");
    }
}

// Usage
Ball ball = new Ball();
Player player1 = new Player("Alice");
Player player2 = new Player("Bob");

// Subscribe players to the ball's event
ball.PositionChanged += player1.OnBallMoved;
ball.PositionChanged += player2.OnBallMoved;

// Move the ball
ball.Move(10, 20);

// Output:
// Alice noticed the ball moved to (10, 20)
// Bob noticed the ball moved to (10, 20)
```

In this example, the `Ball` class uses an event (which is essentially a delegate) to notify `Player` objects when its position changes. This demonstrates how delegates enable loosely coupled, event-driven systems.

## Alternatives in Other Languages

For languages that don't have built-in delegate-like features (such as Java), you can implement similar functionality using design patterns:

1. **Strategy Pattern**: For functional programming-like behavior (passing behavior as a parameter)
2. **Observer Pattern**: For event-driven programming


# Delegates in C#: Functions as First-Class Citizens

## Table of Contents
1. [Introduction](#introduction)
2. [Delegates vs. JavaScript Functions](#delegates-vs-javascript-functions)
3. [Delegate Syntax and Usage](#delegate-syntax-and-usage)
4. [Example: Counting Uppercase Characters](#example-counting-uppercase-characters)
5. [Delegates Under the Hood](#delegates-under-the-hood)
6. [Visual Representation](#visual-representation)

## Introduction

In C#, delegates provide a way to treat methods as first-class citizens, allowing functions to be stored in variables, passed as parameters, and returned from other functions. This functionality bridges the gap between object-oriented and functional programming paradigms within C#.

## Delegates vs. JavaScript Functions

In JavaScript, functions are loosely typed and can be easily stored in variables:

```javascript
let myFunction = function(str) {
    // function body
};
```

In C#, to achieve similar functionality, we use delegates. A delegate is a type-safe function pointer:

```csharp
public delegate int StringFuncDelegate(string str);
StringFuncDelegate myFunction = SomeMethod;
```

## Delegate Syntax and Usage

Defining a delegate:

```csharp
public delegate int StringFuncDelegate(string str);
```

This creates a new type that can reference methods with a matching signature (in this case, methods that take a string and return an int).

Using a delegate:

```csharp
StringFuncDelegate processor = StringFunctions.GetCountOfUpperCaseChars;
int result = processor("Hello World");
```

## Example: Counting Uppercase Characters

Here's an example of a method that counts uppercase characters in a string, which can be referenced by a delegate:

```csharp
public class StringFunctions
{
    public static int GetCountOfUpperCaseChars(string str)
    {
        int counter = 0;
        if (!string.IsNullOrEmpty(str))
        {
            for (int i = 0; i < str.Length; i++)
            {
                if (char.IsUpper(str[i]))
                {
                    counter++;
                }
            }
        }
        return counter;
    }
}

// Usage
StringFuncDelegate processor = StringFunctions.GetCountOfUpperCaseChars;
int result = processor("Hello World");
Console.WriteLine(result);  // Output: 2
```

## Delegates Under the Hood

When you define a delegate in C#, the compiler represents it as a class in the IL (Intermediate Language) code. This class has the following characteristics:

1. It derives from the `System.MulticastDelegate` class.
2. It has a constructor that takes an object (for the target) and a method (for the function to be called).
3. It has an `Invoke` method that matches the signature of the delegate.

Delegates can be thought of as type-safe function pointers. They can reference a single method or multiple methods (multicast delegates).

By default, delegates have internal access, but you can explicitly make them public:

```csharp
public delegate int StringFuncDelegate(string str);
```

## Visual Representation

Here's a simple visualization of how delegates work in C#:

```
┌─────────────────┐
│   Delegate      │
│  (Class Type)   │
└─────────────────┘
         │
         │ References
         ▼
┌─────────────────┐
│    Method(s)    │
│    Address(es)  │
└─────────────────┘
```

In this diagram:
- The delegate is represented as a class type.
- It holds a reference (or references in case of multicast delegates) to the address(es) of the method(s) it can invoke.
- When invoked, the delegate calls the method(s) it references.

This structure allows C# to provide type-safe "function pointers" within its object-oriented framework, enabling functional programming paradigms while maintaining type safety and object-oriented principles.



By leveraging delegates, C# provides powerful tools for implementing both functional and event-driven programming paradigms within its object-oriented framework. For languages without native delegate support, design patterns offer alternative ways to achieve similar flexibility and decoupling in software design.
