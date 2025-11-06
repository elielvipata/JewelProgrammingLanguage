Jewel is a systems programming langauge with a focus on speed, memory safety and consequent free abstraction.

Jewel's key feature is consequent free abstraction. This will be covered in the abstraction mechanisms section of this doc.

The following are the language features in development and are designed to make the language powerful and easy to use.
- Haskell-like functional type signatures for type checking
-  Frames: an entity used to wrap 
    -  Closures
    -  Functions
    -  Scope/Lifetimes
- Custom Error Messaging/ Errors as values
- Explicit move and copy semantics based on variable types
- Pattern Matching
- Guarded Heap Allocation
- User defined datatype management
- Inter operation with assembly and c

```
// Hello world example:
main::[string]->int
main args {
    print("hello world");
    end(0);
}

```

### Native Datatypes
These are datatypes/values that all expressions will eventually evaluate to. 
They are used for preservation and progression checks when evaluating and typechecking expressions and abstractions.
The following are native datatypes in Jewel.
- int (varying by size up to 64 bits)
- char ( a symbolic representation of integers)
- float ( precision values by size up to 64 bits, encompassing a double)
- boolean ( integer value 1 or 0) 


### Native Datastructures
All native datatypes in Jewel can be used in the native datastructures, these are the following datastructures that are supported or will be supported in Jewel
- List (analogous to arrays): Used to store data in a sequence of memory. With support for constant time access
- String: A list of characters
- Heap: A list with logarithmic access and insertion
- Map: Key based Heap access and insertion

### Frame: The building block of Jewel 
A frame is body of execution that has its own address space and can be executed asynchronously or on demand.

Functions are frames with a type signature. Each frame can also be considered a region, i.e a basic amount of memory is allocated during the second parsing phase when building an AST.
Anonoymous functions are frames within a function that are run when encountered during execution. Anonoymous functions can run on seperate threads or as a child of the parent function they execute in. They can return a value or process data

They are used to build closures and can return custom runtime errors
They are also used to determine lifetimes of variables using scope

### Explicit Value Semantics
Jewel requires the programmer to explicitly specify whether they intend for the values they use to be mutable or immutable. These rules will determine how the data will be accessed, copied, moved or deleted. It also determines the owner and lifetime of a variable.

More details can be found in the type system section 

### Type System

Jewel's type system operates on two levels, high and low each with their own value and reference semantics. 


#### High Level Type System:
     Jewel's high level type system consists of expressions and their operations. When an operation is applied to an expression it produces another expression. The expressions can be evaluated to values that are passed to the low level type system.
     
#### Low Level Type System:
    Jewel has a low level type system that consists of discrete values with specific sizes in bytes that can be moved, copied or manipulated to create new values based on their operations.
    
#### Static and Mutable Types
    All expressions and values can either be mutable or static and must be explicitly declared as such by the programmer.
    
    Static Values are  constant, they cannot be changed or moved. They are accessed by value, and if assigned to a variable, are considered global to the frame the variable is declared in. 
    Locality plays a huge role in the semantics of static variables, an anonymous function can refer to a value in any supercedeing frame that calls it but it does so by coping the value in that address and deleting the value when execution exits its scope.
    
    Mutable values are values that are accessed by reference, they are local to the frame they are declared in and managaed on the heap. Useful for transfering state, any sort of imperative operation that requires the variables to change state(loops) would find these useful. 
    
    Explicit value semantics make behavior of the program more transparent to the programmer. The goal for this feature is to avoid hiding value semantics and its details behind data structures and place them directly in the values themselves. If a programmer wishes to use a mutable datastructure, all they have to do is declare it as such and vice versa. 

#### Lifetimes and Ownership
    Transparency of behavior is the ideal goal for Jewel. This also applies to memory allocation and dellocation. Inorder to maintain the goal of execution speed and efficient memory management, Jewel will be using scope to determine the lifetime of variables created on the stack or heap. Since frames will have their own memory addresses and execution bodies, any variables created within them are automatically deleted when execution exits the frame.  
    
#### Expressions and Operations

Values in Jewels can be used explicitly or placed in a variable.
If used explicitly, they are placed in the program stack. Otherwise if placed in a variable, will go on the stack if static and on the heap if mutable. 

#### Sample Programs
[Functions](functions.md)


### Applications

Web servers
Command line interfaces
Low level/memory intensive applications

