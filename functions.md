## Functions

### Structure
A function in jewel has two parts:
- Type Signature: The name, types of arguments and the return value(last type). Jewel requires that a functions type signature be set/declared before the actual function code is written. One thing I would like to explore is if functions can be declared and then defined later on. I don't have any use cases for this other than libraries or modules but its something that will be explored after the base language is released.

- Definition: The frame body of the function preceded by the name of the function and the parameters corresponding to the types defined in the type signature.

### End Keyword
The end keyword is the equivalent of a return statement in most programming languages when used in a function. Can also be used as the equivalent of break in a loop.

### Loops
Jewel has one type of loop and called `loop` that can be structured in two ways: 
```
loop if(condition)* {
    // statements
};

loop {
    // statements
} until(condition);

```

First loop is similar to a while loop. Its basically an if statement what will execute its true branch until the condition is false.

Second loop will always run atleast once and until the condition specified in the `until` clause is false.

Using the `end loop` statement will break out of a loop when hit.


### Sample Program

```rust
// link: https://adventofcode.com/2015/day/1
// Returns the final floor number after following instructions.
// '(' -> up one floor; ')' -> down one floor.
// function compute_floor(instructions: string) -> integer:
//    floor ← 0
//    for each char in instructions:
//        if char == '(':
//            floor ← floor + 1
//        else if char == ')':
//            floor ← floor - 1
//        # ignore any other characters (whitespace/newline)
//    return floor

compute_floor::[char]::int->int
compute_floor instructions length {
    let num_floors:int = 0;
    let index:int = 0;
    loop {
        if(instructions[index] == '('){
            num_floors = num_floors + 1;
        } 
        else {
            num_floors = num_floors - 1;
        }
        index = index+1;
    } until(index == length);

    end(num_floors);
}


main::[string]->int
main args {
    let list1:[char] = ['(', '(', '('];
    let list2:[char] = ['(', '(', ')', '(', '(', ')', '('];
    let list3:[char] =  [')', ')', '(', '(', '(', '(', '('];
    let list4:[char] =  ['(', ')', ')'];
    let list5:[char] =  [')', ')', '('];
    let list6:[char] =  [')', ')', ')'];
    let list7:[char] =  [')', '(', ')', ')', '(', ')', ')'];

    let num_floors1 = compute_floor(list1,3);
    let num_floors2 = compute_floor(list2,7);
    let num_floors3 = compute_floor(list3,7);
    let num_floors4 = compute_floor(list4,3);
    let num_floors5 = compute_floor(list5,3);
    let num_floors6 = compute_floor(list6,3);
    let num_floors7 = compute_floor(list7,7);

    print("Output");
    print(num_floors1);
    print(num_floors2);
    print(num_floors3);
    print(num_floors4);
    print(num_floors5);
    print(num_floors6);
    print(num_floors7);

    end(0);
}
```
