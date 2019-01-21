# Vyne-Language
Definition for the Vyne Programming language.

## Goals

* The Vyne language is a general purpose programming language.
* It should be cross-platform, or platform agnostic.
* Compile parameters should be included within the source code.

## Notes

The syntax shown in this document is tentative. Only used as a proof of concept.

## Features

### Choice

#### - If

Works like other languages.

#### - Switch

Works like other languages. Will be closer to functional languages with pattern matching.

### Loops

#### - While

The while loop has extra features compared to other languages.

```csharp
while (condition) {

}
else while (condition) {

}
else {

}
```

The Vyne while loop works like an if statement.

It starts by checking the first condition. If it is true, it will enter that branch until the condition becomes false.

If the first condition was false, it will check the second condition. If it is true, it will enter that branch until the condition becomes false.

If the second condition was also false, it will execute the else. The else here is an infinite loop that requires manual breaking out.

#### - For

Works like other languages.

#### - Do While

Works like other languages.

#### - Loop

An infinite loop that requires manual breaking out.

```csharp
loop {

}
```

#### - Foreach

Most likely will work like other languages.
