# Chapter 4 - Indirect Addressing - References

## Understanding reference basics

Using reference variables allows us to use a more straightforward notation than the notation that
pointers use when using indirectly accessed memory. Many programmers appreciate the clarity in
the notation of a reference versus a pointer variable. But, behind the scenes, memory must always be
properly allocated and released; some portion of memory that is referenced may come from the heap.
The programmer will undoubtedly need to deal with pointers for some portion of their overall code.
We will discern when references and pointers are interchangeable, and when they are not. Let’s get
started with the basic notation for declaring and using reference variables.

### Declaring, initializing, and accessing references

A reference variable, behind the scenes, can be compared to a pointer variable in that it holds the
address of the variable that it is referencing. Unlike a pointer variable, any usage of the reference
variable automatically dereferences the variable to go to the address that it contains; the dereference
operator * is simply not needed with references. Dereferencing is automatic and implied with each
use of a reference variable.

```cpp
#include <iostream>
using std::cout;
using std::endl;
int main()
{
    int x = 10;
    int *p = new int; // allocate memory for ptr variable
    *p = 20; // dereference and assign value
    int &refInt1 = x; // reference to an integer
    int &refInt2 = *p; // also a reference to an integer
    cout << x << " " << *p << " ";
    cout << refInt1 << " " << refInt2 << endl;
    x++; // updates x and refInt1
    (*p)++; // updates *p and refInt2
    cout << x << " " << *p << " ";
    cout << refInt1 << " " << refInt2 << endl;
    refInt1++; // updates refInt1 and x
    refInt2++; // updates refInt2 and *p
    cout << x << " " << *p << " ";
    cout << refInt1 << " " << refInt2 << endl;
    delete p; // relinquish p's memory
    return 0;
}

```

Note: Remember, a reference variable must be initialized to the variable it will refer to. The reference
may never be assigned to another variable. More precisely, we cannot rebind the reference to
another entity. The reference and its initializer must be the same type.

### Referencing existing objects of user defined types

Should a reference to an object of a struct or class type be defined, the object being referenced is
simply accessed using the . (member selection) operator. Again, it is not necessary (such as it is with
pointers) to first use the dereference operator to go to the object being referenced before choosing
the desired member.

```cpp
#include <iostream>
using std::cout;
using std::endl;
using std::string;
class Student // very simple class – we will add to it
{                // in our next chapter
public:
string name;
float gpa;
};
int main()
{
    Student s1;
    Student &sRef = s1; // establish a reference to s1
    s1.name = "Katje Katz"; // fill in the data
    s1.gpa = 3.75;
    cout << s1.name << " has GPA: " << s1.gpa << endl;
    cout << sRef.name << " has GPA: " << sRef.gpa << endl;
    sRef.name = "George Katz"; // change the data
    sRef.gpa = 3.25;
    cout << s1.name << " has GPA: " << s1.gpa << endl;
    cout << sRef.name << " has GPA: " << sRef.gpa << endl;
    return 0;
}

```

## Using references with functions

### Passing references as arguments to functions

References may be used as arguments to functions to achieve call-by-reference, rather than call-by-
value, parameter passing. References can alleviate the need for pointer notation in the scope of the
function in question as well as in the call to that function. Object or . (member selection) notation
is used to access struct or class members for formal parameters that are references.
In order to modify the contents of a variable passed as an argument to a function, a reference (or
pointer) to that argument must be used as a function parameter. Just as with a pointer, when a reference
is passed to a function, a copy of the address representing the reference is passed to the function.
However, within the function, any usage of a formal parameter that is a reference will automatically
and implicitly be dereferenced, allowing the user to use object rather than pointer notation. As with
passing a pointer variable, passing a reference variable to a function will allow the memory referenced
by that parameter to be modified.

```cpp

#include <iostream>
using std::cout;
using std::flush;
void AddOne(int &); // function prototypes
void AddOne(int *);
void Display(int &);
int main()
{
    int x = 10, *y = nullptr;
    y = new int; // allocate y's memory
    *y = 15; // dereference y to assign a value
    Display(x);
    Display(*y);
    AddOne(x); // calls ref. version (with an object)
    AddOne(*y); // also calls reference version
    Display(x); // Based on prototype, we see we are
    Display(*y); // passing by ref. Without prototype,
    // we may have guessed it was by value.
    AddOne(&x); // calls pointer version
    AddOne(y); // also calls pointer version
    Display(x);
    Display(*y);
    delete y; // relinquish y's memory
    return 0;
}


void AddOne(int &arg) // These two fns. are overloaded
{
    arg++;
}
void AddOne(int *arg) // Overloaded function definition
{
    (*arg)++;
}
void Display(int &arg) // Function parameter establishes
// a reference to arg
{
    cout << arg << " " << flush;
}
```

### Using references as return values from functions

With operator overloading, returning a value from a function using a
pointer will not be an option to preserve the operator’s original syntax. We must return a reference
(or a reference qualified with const); this will also allow overloaded operators to enjoy cascaded
use.
When returning a reference via the return statement of a function, be sure that the memory that is
referred to will persist after the function call is completed. Do not return a reference to a local variable
defined on the stack within the function; this memory will be popped off the stack the moment the
function completes.
Since we cannot return a reference to a local variable within the function, and since returning a
reference to an external variable is pointless, you may ask where the data that we return a reference
to will reside. This data will inevitably be on the heap. Heap memory will exist past the extent of the
function call. In most circumstances, the heap memory will have been allocated elsewhere; however,
on rare occasions, the memory may have been allocated within this function. In this unusual situation,
you must remember to relinquish the allocated heap memory when it is no longer required.
Deleting heap memory through a reference (versus pointer) variable will require you to use the
address-of operator, &, to pass the required address to operator delete(). Even though reference
variables contain the address of the object they are referencing, the use of a reference identifier is
always in its dereferenced state. It is rare that the need may arise to delete memory using a reference
variable.

```cpp
#include <iostream>
using std::cout;
using std::endl;
int &CreateId(); // function prototype
int main()
{
    int &id1 = CreateId(); // reference established
    int &id2 = CreateId();
    cout << "Id1: " << id1 << " Id2: " << id2 << endl;
    delete &id1; // Here, '&' is address-of, not reference
    delete &id2; // to calculate address to pass delete()
    return 0; // It is unusual to delete in fashion shown,
} // using the addr. of a ref. Also, deleting in
// a diff. scope than alloc. can be error prone
int &CreateId() // Function returns a reference to an int
{
    static int count = 100; // initialize with first id
    int *memory = new int;
    *memory = count++; // use count as id, then increment
    return *memory;
}

```

## Using the const qualifier with references

The const qualifier can be used to qualify the data in which references are initialized or refer to. We
can also use const qualified references as arguments to functions and as return values from functions.
It is important to understand that a reference is implemented as a constant pointer in C++. That is,
the address contained within the reference variable is a fixed address. This explains why a reference
variable must be initialized to the object to which it will refer, and may not later be updated using
an assignment. This also explains why constant qualifying the reference itself (and not just the data
that it refers to) does not make sense. This variety of const qualification is already implied with its
underlying implementation.

### Using references to constant objects

The const qualifier can be used to indicate that the data to which references are initialized are
unmodifiable. In this fashion, the alias always refers to a fixed piece of memory, and the value of that
variable may not be changed using the alias itself. The reference, once specified as constant, implies
that neither the reference nor its value may be changed. Again, the reference itself may not be changed
due to its underlying implementation as a constant qualified pointer. A const qualified reference
may not be used as an l-value in any assignment.

Note: Recall, an l-value is a value that can be modified and that occurs on the left-hand side of an
assignment.

```cpp
#include <iostream>
using std::cout;
using std::endl;
int main()
{
    int x = 5;
    const int &refInt = x;
    cout << x << " " << refInt << endl;
    // refInt = 6; // Illegal -- refInt is const
    x = 7; // we can inadvertently change refInt
    cout << x << " " << refInt << endl;
    return 0;
}
```

### Using pointers to constant objects as function arguments and as return types from functions

A function that takes a reference to an object as a parameter often has less overhead than a comparable
version of the function that takes a copy of an object as a parameter. This most notably occurs when
the object type that would be otherwise copied on the stack is large. Passing a reference as a formal
parameter is speedier, yet permits the actual parameter to be potentially modified in the scope of the
function. Passing a reference to a constant object as a function argument provides both speed and
safety for the argument in question. The reference qualified as const in the parameter list simply
may not be an l-value in the scope of the function in question.

```cpp
#include <iostream>
using std::cout;
using std::cin;
using std::endl;
struct collection
{
int x;
float y;
};
void Update(collection &); // function prototypes
void Print(const collection &);
int main()
{
    collection collect1, *collect2 = nullptr;
    collect2 = new collection; // allocate mem. from heap
    Update(collect1); // a ref to the object is passed
    Update(*collect2); // same here: *collect2 is an object
    Print(collect1);
    Print(*collect2);
    delete collect2; // delete heap memory
    return 0;
}
void Update(collection &c)
{
    cout << "Enter <int> and <float> members: ";
    cin >> c.x >> c.y;
}

void Print(const collection &c)
{
    cout << "x member: " << c.x;
    cout << " y member: " << c.y << endl;
}

```

## Realizing underlying implementation and restrictions

To understand the implementation, let’s consider a sample reference declaration: int &intVar =
x;. From an implementation aspect, it is as though the former variable declaration is instead declared
as int *const intVar = ```&x```;. Note that the & symbol shown on the left-hand side of an
initialization takes on the meaning of reference, whereas the & symbol shown on the right-hand side
of an initialization or assignment implies address-of. These two declarations illustrate how a reference
is defined versus its underlying implementation.
Even though a reference is implemented as a constant pointer, the usage of the reference variable is
as if the underlying constant pointer has been dereferenced. For this reason, you cannot initialize a
reference with a nullptr – not only can a nullptr not be dereferenced but since references can
only be initialized and not reset, the opportunity would be lost to establish the reference variable to
refer to a meaningful object. This also holds true for references to pointers.

### Understanding when we must use pointers instead of references

Based on the underlying implementation of references (as const pointers), most of the restrictions
for reference usage make sense. For example, references to references are generally not allowed; each
level of indirection would need to be initialized upfront and that often takes multiple steps, such as
when using pointers.

```cpp
#include <iostream>
using std::cout;
using std::endl;
int main()
{
    int *ptr = new int;
    *ptr = 20;
    int *&refPtr = ptr; // establish a reference to a ptr
    cout << *ptr << " " << *refPtr << endl;
    delete ptr;
    return 0;
}

```

## Summary

In this chapter, we have learned numerous aspects of C++ references. We have taken the time to
understand reference basics, such as declaring and initializing reference variables to existing objects,
as well as how to access reference components for basic and user defined types.
We have seen how to utilize references in a meaningful fashion with functions, both as input parameters
and as a return value. We have also seen when it is reasonable to apply the const qualifier to references,
as well as seen how this concept can be combined with parameters and return values from functions.
Lastly, we have seen the underlying implementation of references. This has helped explain some of
the restrictions references encompass, as well as understand which cases of indirect addressing will
require the use of pointers instead of references.
As with pointers, all of the skills using references from this chapter will be used freely in the upcoming
chapters. C++ allows programmers to have a more convenient notation for indirect addressing using
references; however, programmers are expected to utilize either for indirect addressing with relative ease.



