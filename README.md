# Vyne-Language
Definition for the Vyne Programming language. The Vyne language is an imaginary programming language. There are no known compilers that exist yet.

## Discord

[![Discord](https://img.shields.io/discord/530598289813536771.svg)](https://discord.gg/dHkA4bP)

## Goals

* The Vyne language is a general purpose programming language.
* It should be cross-platform.
* Compile parameters should be included within the source code.

## Notes

The syntax shown in this document is tentative. Only used as a proof of concept.

## Features

### Comments

Support for single line comments and nested multiline comments.

The current syntax for single line comments:

```csharp
// Hello World!
```

The current syntax for multiline comments:

```rust
/*
    This is inside the comment
    /*
        You can insert something here.
    */
    This is a comment since the nested comment is parsed correctly.
*/
```

There is also a way to break out of nested comments:

```rust
/*
    /*
        /*
            Comment here
*//
```

Loose multiline comment terminators are ignored as whitespace:

```
*/*/*/
```

### Casting

Casting is done after the value. Given two types `A` and `B` where `B` exposes a function called `Foo`.

```csharp
let a: A;

a:B.Foo();
```

### Blocks

There are 3 different types of code blocks.

#### :small_orange_diamond: Basic

Starts a local scope. The scope is cleaned up when the end of the scope is reached.

```
{

}
```

#### :small_orange_diamond: Deferred

Starts a local scope. The scope is cleaned up when the parent scope is cleaned up.

```rust
{+>

}
```

#### :small_orange_diamond: Paralleled

Doesn't start a new scope. Memory is cleaned up when the current scope is cleaned up. This block is brought to the top of the current scope to be executed first either sequencially or in parallel with the other parallel blocks in the scope.

```rust
{|>

}
```

Can be used like this:

```rust
{
   let c = a + b;

   {|>
      let a = 0;
   }
   {|>
      let b = 10;
   }

   {
      let e = a + d;

      {|>
         let d = 20 + c;
      }
   }
}
```

### Block Chaining

Blocks can be chained using the `else` and `then` keywords.

#### :small_orange_diamond: Else

The `else` keyword is used to execute a block when the first block was not executed.

```rust
{
    // This gets executed.
}
else {
    // This never gets executed.
}
```

#### :small_orange_diamond: Then

The `then` keyword is used to always execute a block when the first block was executed.

```vb
{
    // This gets executed.
}
then {
    // This gets executed.
}
```

### Choices

#### :small_orange_diamond: If

```rust
if condition {

}
else if condition {

}
else {

}
```

#### :small_orange_diamond: Switch

Works like other languages. Will be closer to functional languages with pattern matching.

### Loops

#### :small_orange_diamond: Loop

An infinite loop that requires manual breaking out.

```rust
loop {

}
```

#### :small_orange_diamond: While

The `while` loop has extra features compared to other languages.

```rust
while condition {

}
else while condition {

}
else loop {
   if condition break;
}
```

The Vyne while loop works like an `if` statement.

It starts by checking the first condition. If it is true, it will enter that branch until the condition becomes false.

If the first condition was false, it will check the second condition. If it is true, it will enter that branch until the condition becomes false.

If the second condition was also false, it will execute the final else loop. The else loop here is an infinite loop that requires manual breaking out.

This new `while` loop can be mixed with other statements such as the `if` statement. It makes it possible to have this syntax:

```rust
if condition {

}
else while condition {

}
else if condition {

}
else {

}
```

Or to clean up after a loop:

```rust
while condition {

}
then {
    // Loop cleanup.
}
else {
    // The loop never got executed.
}
```

#### :small_orange_diamond: For

Works like other languages.

#### :small_orange_diamond: Do While

Can be done using `loop`.

```rust
loop {
    // Some code here.

    if condition {
        break;
    }
}
```

#### :small_orange_diamond: Foreach

Most likely will work other languages.

### General Statements

#### :small_orange_diamond: Delay Expression

The delay expression is used to delay the execution of a block. It can be used to create code comments:

```rust
~{
    // Some code.
    // It will never be executed.
    // Can be useful for code that you still want the compiler to check and throw errors on.
    // It would be optimized out in the final assembly if the block isn't caught.
}
```

It is also possible to catch the definition in a variable to execute it later:

```rust
let Point = ~{+>
    let X = 10;
    let Y = 20;
};

let a = Point!;
let b = Point!;

a.X = 15;
```

This can be used to define reusable code.

Can also be used like this:

```rust
let a = ~1;
let b = a!;
```

#### :small_orange_diamond: Label

It is possible to add labels to some statements.

```rust
while :outer condition {
    while :inner condition {

    }
}
```

#### :small_orange_diamond: Break

A break is used to exit out of a loop.

```rust
loop {
    break;
}
// We end up here after the break.
```

In nested loops, it is possible to specify which loop to break out of using labels.

```rust
while :outer condition {
    while :middle condition {
        while :inner condition {
            break middle;
        }
    }
    // We end up here after the break.
}
```

#### :small_orange_diamond: Continue

A continue is used to skip to the end of a loop iteration.

```rust
while condition {
    continue;

    // Some code that is never reached.

    // We end up here after the continue.
}
```

The continue can also be used with labels.

```rust
while :outer condition {
    while :middle condition {
        while :inner condition {
            continue middle;
        }
        // We end up here after the continue.
    }
}
```

### Subroutines

#### :small_orange_diamond: Function

Doesn't have the ability to produce side effects. Takes read-only input parameters and returns write-only output parameters. If the same variable is passed as an input and output, then some optimizations can be applied. For example a variable could end up being passed as a reference, or it could be passed by value with deep copy. Control flow is returned back to the caller.

For example, the following function takes 1 input variable and returns 1 output variable:

```rust
let a = 1;

let addTwo = ~{
    in b += 2;
    out b;
}

let c = addTwo(a)!;
```

The original variable `a` is not modified. It is passed by value.

The variable `c` is write-only from the function's point of view.

```rust
let a = 1;

let addTwo = ~{
    in b += 2;
    out b;
}

a = addTwo(a)!;
```

In the example above, the caller gives explicit permission to the function to modify `a`. As such it is passed by reference.

```rust
let a = 1;
let b = 2;

let swap = ~{
    in c, d;
    out d, c;
}

a, b = swap(a, b)!;
```

This last one could be used to swap variables.

Combined with the delay expression and a deferred block, it's possible to get something similar to a class.

```rust
let Point = ~{+>
    in X;
    in Y;
};

let a = Point(10, 20)!;
```

### Boolean operators

Currently proposed boolean operators:

```
==
!=
<
>
<=
>=
!<
!>
```

`!<` and `!>` are equivalent to `>=` and `<=`. In some cases, it is useful to represent logic using one or the other to make an algorithm's purpose clearer.

Boolean operators have syntactic sugar to make it easier to write common logic using `&` and `|`:

```
0 < i &< 10
becomes
0 < i && i < 10
```
```
0 < i |< 10
becomes
0 < i || i < 10
```

### Scope

The concept of a scope is very important in the Vyne language. Where does something exist? Where something lives needs to always be explicit. A global variable would only be a variable that is made explicitly accessible within other scopes. It is possible to name scopes and pass them as function parameters.

#### Scope dependency

It is possible to define a scope as dependent on external factors. This makes it possible for a scope to access variables that are external to itself. It's up to the parent scope to satisfy those dependencies.
