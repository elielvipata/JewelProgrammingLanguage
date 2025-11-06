Jewel is a systems programming langauge with a focus on speed and memory safety. Still underheavy development the language is meant to be a great way to build:

- Web servers
- Command line interfaces
- Low level/memory intensive applications

The language is immediately recognizable to programmers who have used Haskell, Rust, C, C++ or python. It imposes a rigid structure that the programmer has to follow, has the same feel as a functional scripting language due to its expressiveness  and provides the speed of a low level language based on a strict type system meant to evaluate and impose memory safety.

``` rust
// Hello world example:
main::[string]->int
main args {
    print("hello world");
    end(0);
}

```


## Type System
### Native Datatypes
These are datatypes/values that all expressions will eventually evaluate to. 
They are used for preservation and progression checks when evaluating and typechecking expressions and abstractions:
- int (varying by size up to 64 bits)
- char ( a symbolic representation of integers)
- float ( precision values by size up to 64 bits, encompassing a double)
- boolean ( integer value 1 or 0) 


### Frame: The building block of Jewel 
A frame is body of execution that has its own address space and can be executed asynchronously or on demand.

- Functions are frames with a type signature. Each frame can also be considered a region, i.e a basic amount of memory is allocated during the second parsing phase when building an AST.
- Anonoymous functions are frames within a function that are run when encountered during execution. Anonoymous functions can run on seperate threads or as a child of the parent function they execute in. They can return a value or process data
- They are used to build closures and can return custom runtime errors
- They are also used to determine lifetimes of variables using scope

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
    
#### Memory Management
- Jewel technically has automatic memory management using scopes and strict lifetime evaluation at compile time. A new memory management system is currently being worked on  called Principle of First Ownership and will ship in version 2 of the language.

#### Compiler Stack

As mentioned above, Jewel is meant to be a systems programming language so its not interpreted. I originally designed it as an interpreted language but consequent free abstraction makes more sense as a compiler optimization technique(ish) so it makes more sense to have it be compiled.
This is the current stack:
- [YACC](https://pubs.opengroup.org/onlinepubs/7908799/xcu/yacc.html) for an experimental parser
- [LLVM](https://llvm.org/docs/index.html) for code generation
- [MiMalloc](https://microsoft.github.io/mimalloc) as the memory allocator
- [Glibc](https://www.gnu.org/software/libc/) for its standard library

C++ is the main development language with some bash scripts to automate certain processes.
The parser will remain in Yacc while the language undergoes severe refactoring/design but will eventually be a simple recursive parser when the language hits 1.0 and goes public (January 2025).

Note: A development roadmap will also release with 1.0.



#### Sample Programs
[Functions](functions.md)

Sample Programs will be used for more in-depth documentation and explanations of how the lanugage works.



