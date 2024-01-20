# Chapter 2 - Adding Languages Necessities

## Using the const and constexpr qualifiers

The use of const and constexpr can enable values to be initialized, yet never again modified. Functions can advertise that they will not
modify their input parameters, or that their return value may only be captured (but not modified) by
using const or constexpr. These qualifiers help make C++ a more secure language. 

### const and constexpr variables

A const qualified variable is a variable that must be initialized, and may never be assigned a new
value. The keyword const is added to the variable declaration.

Similarly, a variable declared using constexpr is a constant qualified variable – one that may be
initialized and never assigned a new value. The usage of constexpr is becoming preferred whenever
its use is possible.

In some situations, the value of a constant is not known at compile time. An example might be if
user input or the return value of a function is used to initialize a constant. A const variable may be
easily initialized at runtime. A constexpr can often, but not always be initialized at runtime. We
will consider various scenarios in our example.

```cpp
#include <iostream>
#include <iomanip>
#include <cstring> // though, we'll prefer std:: string,
// char [ ] demos the const qualifier easily in cases below

using std::cout; // preferable to: using namespace std;
using std::cin;
using std::endl;
using std::setw;
// simple const variable declaration and initialization
// Convention will capitalize those known at compile time
// (those taking the place of a former macro #define)
const int MAX = 50;
// simple constexpr var. declaration and init. (preferred)
constexpr int LARGEST = 50;
constexpr int Minimum(int a, int b)
// function definition w formal parameters
{
    return (a < b)? a : b; // conditional operator ?:
}

int main()
{
    int x = 0, y = 0;
    // Since 'a', 'b' could be calculated at runtime
    // (such as from values read in), we will use lowercase
    constexpr int a = 10, b = 15;// both 'a', 'b' are const
    cout << "Enter two <int> values: ";
    cin >> x >> y;
    // const variable initialized w return val. of a fn.
    const int min = Minimum(x, y);
    cout << "Minimum is: " << min << endl;
    // constexpr initialized with return value of function
    constexpr int smallest = Minimum(a, b);
    cout << "Smallest of " << a << " " << b << " is: "
    << smallest << endl;
    char bigName[MAX] = {""}; // const used to size array
    char largeName[LARGEST] = {""}; // same for constexpr
    cout << "Enter two names: ";
    cin >> setw(MAX) >> bigName >> setw(LARGEST) >>
    largeName;
    const int namelen = strlen(bigName);
    cout << "Length of name 1: " << namelen << endl;
    cout << "Length of name 2: " << strlen(largeName) <<
    endl;
    return 0;
}

```

### const qualification with functions

The keywords const and constexpr can also be used in conjunction with functions. These
qualifiers can be used among parameters to indicate that the parameters themselves will not be modified.
This is a useful feature—the caller of the function will understand that the function will not modify input parameters qualified in these manners. However, because non-pointer (and non-reference) variables are passed by value to functions as copies of the actual parameters on the stack, const or constexpr qualifying these inherent copies of parameters does not serve a purpose. 
Hence, const or constexpr qualifying parameters that are of standard data types is not necessary.
The same principle applies to return values from functions. A return value from a function can be
const or constexpr qualified; however, unless a pointer (or reference) is returned, the item
passed back on the stack as the return value is a copy. For this reason, const qualified return values are more meaningful when the return type is a pointer to a constant object (which we will cover in Chapter 3, Indirect Addressing: Pointers, and beyond). Note that a constexpr qualified return value is required of a function whose return value will be used to initialize a constexpr variable, as we have seen in our previous example.

## Working with function prototypes

Prior to proceeding to function prototypes, let’s take a moment to review some necessary programming terms.
A function definition refers to the body of code comprising a function, whereas a declaration
of a function (also known as a forward declaration) merely introduces a function name with its
return type and argument types. Forward declarations allow the compiler to perform strong type
checking between the function call and its definition by instead comparing the call with the forward declaration. Forward declarations are useful because function definitions do not always appear in a file prior to a function call; sometimes, function definitions appear in a separate file from their calls.

### Defining function prototypes

A function prototype is a forward declaration of a function that describes how a function should be correctly invoked. A prototype ensures strong type checking between a function call and its definition.
A simple function prototype consists of the following:
- Function’s return type
- Function’s name
- Function’s type and number of arguments

```cpp
#include <iostream>
using std::cout; // preferred to: using namespace std;
using std:: endl;

[[nodiscard]] int Minimum(int, int); // fn. prototype

int main()
{
    int x = 5, y = 89;
    // function call with actual parameters
    cout << Minimum(x, y) << endl;
    return 0;
}

[[nodiscard]] int Minimum(int a, int b) // fn. definition
// with formal parameters
{
    return (a < b)? a : b;
}
```

The use of [[nodiscard]] preceding the return type of the function.
This indicates that the programmer should store the return value or otherwise utilize the return value (such as in an expression). The compiler will issue a warning if the return value of this function is ignored.

### Naming arguments in function prototypes

Function prototypes may optionally contain names that may differ from those in either the formal or actual parameter lists.

```cpp 
#include <iostream>
using std::cout; // preferred to: using namespace std;
using std::endl;
// function prototype with optional argument names
[[nodiscard]] int Minimum(int arg1, int arg2);
int main()
{
    int x = 5, y = 89;
    cout << Minimum(x, y) << endl; // function call
    return 0;
}
[[nodiscard]] int Minimum(int a, int b) // fn. definition
{
    return (a < b)? a : b;
}

```

### Adding default values to function prototypes

Default values may be specified in function prototypes. These values will be used in the absence of actual parameters in the function call and will serve as the actual parameters themselves. Default values adhere to the following criteria:
- Default values must be specified from right to left in the function prototype, without omitting
any values.
- Actual parameters are substituted from left to right in the function call; hence the right to left order for default value specification in the prototype is significant.

```cpp
#include <iostream>
using std::cout; // preferred to: using namespaces std;
using std::endl;
// fn. prototype with one default value
[[nodiscard]] int Minimum(int arg1, int arg2 = 100000);
int main()
{
    int x = 5, y = 89;
    cout << Minimum(x) << endl; // function call with only one argument (uses default)
    cout << Minimum(x, y) << endl; // no default vals used
    return 0;
}

[[nodiscard]] int Minimum(int a, int b) // fn. definition
{
    return (a < b)? a : b;
}
```

### Prototyping with different default values in different scopes

Functions may be prototyped in different scopes with different default values.
This allows functions to be built generically and customized through prototypes within multiple applications or for use in multiple sections of code.

```cpp
#include <iostream>
using std::cout; // preferred to: using namespace std;
using std::endl;

// standard function prototype
[[nodiscard]] int Minimum(int, int);
void Function1(int x)
{
    // local prototype with default value
    [[nodiscard]] int Minimum(int arg1, int arg2 = 500);
    cout << Minimum(x) << endl;
}
void Function2(int x)
{
    // local prototype with default value
    [[nodiscard]] int Minimum(int arg1, int arg2 = 90);
    cout << Minimum(x) << endl;
}

[[nodiscard]] int Minimum(int a, int b) // fn. definition
{
    return (a < b)? a : b;
}
int main()
{
    Function1(30);
    Function2(450);
    return 0;
}

```

## Understanding function overloading

C++ allows for two or more functions that share a similar purpose, yet differ in the types or number of
arguments they take, to co-exist with the same function name. This is known as function overloading.
This allows more generic function calls to be made, leaving the compiler to choose the correct version
of the function based on the type of the variable (object) using the function. In this section, we will add
default values to the basics of function overloading to provide flexibility and customization. We will
also learn how standard type conversions may impact function overloading, and potential ambiguities
that may arise (as well as how to resolve those types of uncertainties).

### Learning the basics of function overloading

When two or more functions with the same name exist, the differentiating factor between these
similar functions will be their signature. By varying a function’s signature, two or more functions
with otherwise identical names may exist in the same namespace. Function overloading depends on
the signature of a function as follows:
- The signature of a function refers to a function’s name, plus its type and number of arguments.
- A function’s return type is not included as part of its signature.
- Two or more functions with the same purpose may share the same name, provided that their
signatures differ.
A function’s signature helps provide an internal, “mangled” name for each function. This encoding
scheme guarantees that each function is uniquely represented internally to the compiler.

```cpp
#include <iostream>
#include <cmath>
using std::cout; // preferred to: using namespace std;
using std::endl;
constexpr float PI = 3.14159;
class Circle // simple user defined type declarations
{
public:
    float radius;
    float area;
};

class Rectangle
{
public:
    float length;
    float width;
    float area;
};
void Display(Circle); // 'overloaded' fn. prototypes
void Display(Rectangle); // since they differ in signature

int main()
{
    Circle myCircle;
    Rectangle myRect;
    Rectangle mySquare;
    myCircle.radius = 5.0;
    myCircle.area = PI * pow(myCircle.radius, 2.0);
    myRect.length = 2.0;
    myRect.width = 4.0;
    myRect.area = myRect.length * myRect.width;
    mySquare.length = 4.0;
    mySquare.width = 4.0;
    mySquare.area = mySquare.length * mySquare.width;
    Display(myCircle); // invoke: void display(Circle)
    Display(myRect); // invoke: void display(Rectangle)
    Display(mySquare);
    return 0;
}

void Display (Circle c)
{
    cout << "Circle with radius " << c.radius;
    cout << " has an area of " << c.area << endl;
}

void Display (Rectangle r)
{
    cout << "Rectangle with length " << r.length;
    cout << " and width " << r.width;
    cout << " has an area of " << r.area << endl;
}

```

### Eliminating excessive overloading with standard type conversion

Basic language types can be converted from one type to another automatically by the compiler. This
allows the language to supply a smaller set of operators to manipulate standard types than would
otherwise be necessary. Standard type conversion can also eliminate the need for function overloading
when preserving the exact data type of the function parameters is not crucial. Promotion and demotion
between standard types are often handled transparently, without explicit casting, in expressions
including assignments and operations.

```cpp
#include <iostream>

using std::cout; // preferred to: using namespace std;
using std::endl;
int Maximum(double, double); // function prototype
int main()
{
    int result = 0;
    int m = 6, n = 10;
    float x = 5.7, y = 9.89;
    result = Maximum(x, y);
    cout << "Result is: " << result << endl;
    cout << "The maximum is: " << Maximum(m, n) << endl;
    return 0;
}
int Maximum(double a, double b) // function definition
{
    return (a > b)? a : b;
}

```

### Ambiguities arising from function overloading and type conversion

When a function is invoked and the formal and actual parameters match exactly in type, no ambiguities
arise with respect to which of a selection of overloaded functions should be called – the function with
the exact match is the obvious choice. However, when a function is called and the formal and actual
parameters differ in type, standard type conversion may be performed on the actual parameters, as
necessary. There are situations, however, when the formal and actual parameter types do not match,
and overloaded functions exist. In these cases, it may be difficult for the compiler to select which
function should be selected as the best match. In these cases, a compiler error is generated indicating
that the available choices paired with the function call itself are ambiguous. Explicit type casting or
re-prototyping the desired choice in a more local scope can help correct these otherwise ambiguous
situations.

```cpp
#include <iostream>
using std::cout; // preferred to: using namespace std;
using std::endl;
int Maximum (int, int); // overloaded function prototypes
float Maximum (float, float);
int main()
{
    char a = 'A', b = 'B';
    float x = 5.7, y = 9.89;
    int m = 6, n = 10;
    cout << "The max is: " << Maximum(a, b) << endl;
    cout << "The max is: " << Maximum(x, y) << endl;
    cout << "The max is: " << Maximum(m, n) << endl;
    // The following (ambiguous) line generates a compiler
    // error - there are two equally good fn. candidates
    // cout << "The maximum is: " << Maximum(a, y) << endl;
    // We can force a choice by using an explicit typecast
    cout << "The max is: " <<
    Maximum(static_cast<float>(a), y) << endl;
    return 0;
}

int Maximum (int arg1, int arg2) // function definition
{
    return (arg1 > arg2)? arg1 : arg2;
}
float Maximum (float arg1, float arg2) // overloaded fn.
{
    return (arg1 > arg2)? arg1 : arg2;
}
```

## Summary

In this chapter, we learned about additional non-OO C++ features that are essential building blocks needed to base C++’s object-oriented features. These language necessities include using the const qualifier, understanding function prototypes, using default values in prototypes, function overloading, how standard type conversion affects overloaded function choices, and how possible ambiguities may arise (and be resolved).
Very importantly, you are now ready to move forward to the next chapter in which we will explore indirect addressing using pointers in reasonable detail. The matter-of-fact skills that you have accumulated in this chapter will help you more easily navigate each progressively more detailed chapter to ensure you are ready to easily tackle the OO concepts starting in Chapter 5, Exploring Classes in Detail.
Remember, C++ is a language filled with more gray areas than most other languages. The subtle nuances you are accumulating with your skill set will enhance your value as a C++ developer – one who can not only navigate and understand existing nuanced code but one who can create easily maintainable code.