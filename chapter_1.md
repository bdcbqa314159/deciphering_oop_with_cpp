# Chapter 1

## Compilation Model
We saw the importance of include guards, the process of compilation and how to write a simple executable and compile it.
The first exercise was not done as I use Makefile and CMake commands to compile c++ code.

## Built-in Data Types
We saw the basics of primitive data types, the data types modifiers and variable definition with some naming conventions.
Two new types of initialisation has been covered:
```cpp
auto variable_name = value;

type variable1;
decltype(variable1) variable2;
```

Note that the auto keyword needs an initialisation.

## Pointers and References
We saw pointers, references, const qualifier (+constexpr) and scope of variables.

```cpp
#include <iostream>

int global = 10;

int main(){

    int global = 20;

    std::cout<<"global: "<<::global<<std::endl; //10
    std::cout<<"local: "<<global<<std::endl; //20

    return 0;
}
```

## Control Flow Statements

We saw selection statements: if else, else if logic, the swith usage.
We saw iteration statements: for, while and do while loops.
We saw jump statements: break, continue.

## The try-catch Block

We saw the mere basic in order to understand how it does work.

## Arrays

We saw how to declare, initialise and access into one dimensional and two dimensionals arrays.

## Summary

In this chapter, we saw the basic structure and syntax of the language. We started with an overview of the compilation model, the process of transforming C++ source code into an executable program. We wrote, compiled, and ran our first program, a simple main function that successfully returns an exit/return code.

We described the built-in arithmetic types that the language offers.
We learned how to declare and define variable names, and what the difference is between references and pointers. We also saw the use of the const qualifier and its advantages.

Furthermore, we talked about control flow statements and how to exploit them to perform more complex actions.

Finally, we presented arrays and multidimensional arrays, and the operation to perform to initialize them and access their values. In the next chapter, we will learn what functions in C++ are, and how and why we should use them in our code.
