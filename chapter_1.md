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

### Variables and array basics

Arrays can be declared of any data type. The array name represents the starting address of the
contiguous memory associated with the array’s contents. Arrays are zero-based in C++, meaning they
are indexed starting with array element[0] rather than array element[1]. Most importantly,
range checking is not performed on arrays in C++; if you access an element outside the size of an array,
you are accessing memory belonging to another variable, and your code will likely fault very soon.

```cpp
char name[10] = "Dorothy"; // size is larger than needed
float grades[20]; // array is not initialized; caution!
grades[0] = 4.0; // assign a value to one element of array
float scores[] = {3.3, 4.3, 4.0, 3.7}; // initialized array
```

Arrays of characters are often conceptualized as strings. Many standard string functions exist in
libraries such as ```<cstring>```. Arrays of characters should be null-terminated if they are to be treated
as strings. When arrays of characters are initialized with a string of characters, the null character is
added automatically. However, if characters are added one by one to the array via assignment, it would
then be the programmer’s job to add the null character ('\0') as the final element in the array.
In addition to strings implemented using arrays of characters (or a pointer to characters), there is a
safer data type from the C++ Standard Library, std::string. We will understand the details of
this type once we master classes in Chapter 5, Exploring Classes in Detail; however, let us introduce
string now as an easier and less error-prone way to create strings of characters. You will need to
understand both representations; the array of char (and pointer to char) implementations will
inevitably appear in C++ library and other existing code. Yet you may prefer string in new code
for its ease and safety.

```cpp
// size of array can be calculated by initializer
char book1[] = "C++ Programming";
char book2[25]; // this string is uninitialized; caution!
// use caution as to not overflow destination (book2)
strcpy(book2, "OO Programming with C++");
strcmp(book1, book2);
length = strlen(book2);
string book3 = "Advanced C++ Programming"; // safer usage
string book4("OOP with C++"); // alt. way to init. string
string book5(book4); // create book5 using book4 as a basis
```

## Recapping basic I/O

### The iostream library

One of the easiest mechanisms for input and output in C++ is the use of the ```<iostream>``` library.
The header file ```<iostream>``` contains definitions of data types istream and ostream. Instances of
these data types, cin, cout, cerr, and clog, are incorporated by including the std namespace.
The ```<iostream>``` library facilitates simple I/O and can be used as follows:
- cin can be used in conjunction with the extraction operator >> for buffered input
- cout can be used in conjunction with the insertion operator << for buffered output
- cerr (unbuffered) and clog (buffered) can also be used in conjunction with the insertion
operator, but for errors

```cpp
#include <iostream>
using namespace std; // we'll limit the namespace shortly
int main()
{
    char name[20]; // caution, uninitialized array of char
    int age = 0;
    cout << "Please enter a name and an age: ";
    cin >> name >> age; // caution, may overflow name var.
    cout << "Hello " << name;
    cout << ". You are " << age << " years old." << endl;
    return 0;
}
```

### Basic iostream manipulators

Often, it is desirable to be able to manipulate the contents of the buffers associated with cin, cout,
and cerr. Manipulators allow the internal state of these objects to be modified, which affects how
their associated buffers are formatted and manipulated. Manipulators are defined in the <iomanip>
header file. Common manipulator examples include the following:
- endl: Places a newline character ('\n') in the buffer associated with cout then flushes the
buffer
- flush: Clears the contents of the output stream
- setprecision(int): Defines the precision (number of digits) used to output floating
point numbers
- setw(int): Sets the width for input and output
- ws: Removes whitespace characters from the buffer

Let's see an example:

```cpp
#include <iostream>
#include <iomanip>
using namespace std; // we'll limit the namespace shortly
int main()
{
    char name[20]; // caution; uninitialized array
    float gpa = 0.0; // grade point average
    cout << "Please enter a name and a gpa: ";
    cin >> setw(20) >> name >> gpa; // won't overflow name
    cout << "Hello " << name << flush;
    cout << ". GPA is: " << setprecision(3) << gpa << endl;
    return 0;
}
```

## Revisiting control structures, statements, and looping