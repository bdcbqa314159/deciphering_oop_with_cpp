# Chapter 1 - Understanding Basic C++ Assumptions

## Reviewing basic C++ language syntax

### Comment styles

The two most common are:

- ```cpp /* */``` for multiple lines.
- ```cpp //``` for single lines.

### Variable declarations and standard data types

Variables may be of any length, and may consist of letters, digits, and underscores. Variables are case
sensitive and must begin with a letter or an underscore. Standard data types in C++ include the following:

- int: to store whole numbers
- float: to store floating point numbers
- double: to store double precisiton floating point numbers
- char: to store a single character
- bool: for boolean values of true or false

An example in action:

```cpp
int x = 5;
int a = x;
float y = 9.87;
float y2 = 10.76f; // optional 'f' suffix on float literal
float b = y;
double yy = 123456.78;
double c = yy;
char z = 'Z';
char d = z;
bool test = true;
bool e = test;
bool f = !test;
```