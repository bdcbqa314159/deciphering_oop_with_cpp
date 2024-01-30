# Chapter 3 - Indirect Addressing – Pointers

## Understanding pointer basics and memory allocation

Using pointer variables allows our applications to have greater flexibility. At runtime, we can determine
the quantity of a certain data type we may need (such as in a dynamically allocated array), organize data
in data structures that facilitate sorting (such as in a linked list), or gain speed by passing an address
of a large piece of data to a function (rather than passing a copy of the entire piece of data itself).

### Revisiting pointer basics

First and foremost, let us review the meaning of a pointer variable. A pointer variable is one that may
contain an address, and memory at that address may contain relevant data. It is typical to say that the
pointer variable points to an address containing the relevant data. The value of the pointer variable
itself is an address, not the data we are after. When we then go to that address, we find the data of
interest. This is known as indirect addressing. To summarize, the content of a pointer variable is
an address; if you then go to that address, you find the data. This is for a single level of indirection.
A pointer variable may point to the existing memory of a non-pointer variable, or it may point to
memory that is dynamically allocated on the heap. The latter case is the most usual situation. Unless
a pointer variable is properly initialized or assigned a value, the content of the pointer variable is
meaningless and does not represent a usable address. A large mistake can be assuming that a pointer
variable has been properly initialized when it may not have been. Let us look at some basic operators
that are useful with pointers. We will start with the address-of & and the dereference operator *.

### Using the address-of and dereference operators

The address-of operator & can be applied to a variable to determine its location in memory. The
dereference operator * can be applied to a pointer variable to obtain the value of the data at the valid
address contained within the pointer variable.
Let’s see a simple example:

```cpp
int x = 10;
int *pointerToX = nullptr; // pointer variable which may someday point to an integer

pointerToX = &x; // assign memory loc. of x to pointerToX
cout << "x: " << x << " and *pointerToX: " << *pointerToX;
```

Important note : 
For efficiency, C++ does not neatly initialize all memory with zeros when an application starts,
nor does C++ ensure that memory is conveniently empty, without values, when paired with a
variable. The memory simply has in it what was previously stored there; C++ memory is not
considered clean. Because memory is not given to a programmer clean in C++, the contents
of a newly declared pointer variable, unless properly initialized or assigned a value, should not
be construed to contain a valid address.

### Using operators new() and delete()

Operator new() can be utilized to obtain dynamically allocated memory from the heap. A pointer
variable may choose to point to memory that is dynamically allocated at runtime, rather than to point
to another variable’s existing memory. This gives us flexibility as to when we want to allocate the
memory, and how many pieces of such memory we may choose to have. Operator delete() can
then be applied to a pointer variable to mark memory we no longer require, returning the memory
to the heap management facility for later reuse in the application. It is important to understand that
once we delete() a pointer variable, we should no longer use the address contained within that
variable as a valid address.
Let’s take a look at simple memory allocation and release using a basic data type:

```cpp
int *y = nullptr; // ptr y may someday point to an int
y = new int; // y pts to uninit. memory allocated on heap
*y = 17; // dereference y to load the newly allocated memory with a value of 17
cout << "*y is: " << *y << endl;

delete y; // relinquish the allocated memory alternative ptr declaration, mem alloc. initialization
int *z = new int(22);
cout << "*z is: " << *z << endl;
delete z; // relinquish heap memory
```

Important note:
It is the programmer’s responsibility to remember that once memory has been deallocated,
you should never again dereference that pointer variable; please understand that that address
may have been reissued to another variable through another call to new() elsewhere in the
program. A safeguard would be to reset a pointer to nullptr once its memory has been
deallocated with delete().

### Creating and using pointers to user defined types

Let us examine how to declare pointers to user defined types, and how to allocate their associated
memory on the heap. To dynamically allocate a user defined type, the pointer will first be declared of
that type. The pointer then must either be initialized or assigned a valid memory address – the memory
can either be that of an existing variable or newly allocated heap memory. Once the address for the
appropriate memory has been placed within the pointer variable, the -> operator may be utilized to
access struct or class members. Alternatively, the (*ptr).member notation may be used to access
struct or class members.
Let’s see a basic example:

```cpp
#include <iostream>
using std::cout;

using std::endl;
struct collection
{
    int x;
    float y;
};

int main()
{
    collection *item = nullptr; // pointer declaration
    item = new collection; // memory allocation
    item->x = 9; // use -> to access data member x
    (*item).y = 120.77; // alt. notation to access member y
    cout << (*item).x << " " << item->y << endl;
    delete item; // relinquish memory
    return 0;
}
```

## Allocating and deallocating arrays at runtime

Arrays may be dynamically allocated so that their size may be determined at runtime. Dynamically
allocated arrays may be of any type, including user defined types. Determining the size of an array at
runtime can be a space-saving advantage and gives us programming flexibility. Rather than allocating
a fixed-sized array of the largest possible quantity needed (potentially wasting space), you can instead
allocate the necessary size determined by various factors at runtime. You have the additional flexibility
to delete and reallocate an array should the need arise to change an array’s size. Arrays of any number
of dimensions can be dynamically allocated.

### Dynamically allocating single dimension arrays

Single-dimension arrays may be dynamically allocated so that their size may be determined at runtime.
We will use a pointer to represent each array and will allocate the required memory with operator
new(). Once the array is allocated, standard array notation can be used to access each array element.

```cpp
#include <iostream>
using std::cout;
using std::cin;
using std:::endl;
using std::flush;
struct collection
{
    int x;
    float y;
};

int main()
{
    int numElements = 0;
    int *intArray = nullptr; // pointer declarations to
    collection *collectionArray = nullptr; // future arrays
    cout << "How many elements would you like? " << flush;
    cin >> numElements;
    intArray = new int[numElements]; // alloc. array bodies
    collectionArray = new collection[numElements];
    // load each array with values
    for (int i = 0; i < numElements; i++)
    {
    intArray[i] = i; // load each array w values using
    collectionArray[i].x = i; // array notation []
    collectionArray[i].y = i + .5;
    // alternatively use ptr notation to print values
    cout << *(intArray + i) << " ";
    cout << (*(collectionArray + i)).y << endl;
    }
    delete [] intArray; // mark memory for deletion
    delete [] collectionArray;
    return 0;
}
```

### Dynamically allocating 2-D arrays – an array of pointers

Two or more dimensioned arrays may also be dynamically allocated. For a 2-D array, the column
dimension may be dynamically allocated and the row dimension may remain fixed, or both dimensions
may be dynamically allocated. Allocating one or more dimensions dynamically allows the programmer
to account for run time decisions regarding the array size.

```cpp
#include <iostream>
using std::cout;
using std::cin;
using std::endl;
using std::flush;

constexpr int NUMROWS = 5; // convention to use uppercase
// since value is known at compile time
int main()
{
    float *TwoDimArray[NUMROWS] = { }; // init. to nullptrs
    int numColumns = 0;
    cout << "Enter number of columns: ";
    cin >> numColumns;
    for (int i = 0; i < NUMROWS; i++)
    {
        // allocate column quantity for each row
        TwoDimArray[i] = new float [numColumns];
        // load each column entry with data
        for (int j = 0; j < numColumns; j++)
        {
            TwoDimArray[i][j] = i + j + .05;
            cout << TwoDimArray[i][j] << " ";
        }
        cout << endl; // print newline between rows
    }
    for (int i = 0; i < NUMROWS; i++)
        delete [] TwoDimArray[i]; // del col. for each row
    return 0;
}
```

### Dynamically allocating 2-D arrays – pointers to pointers

Dynamically allocating both the row and column dimensions for an array can add necessary runtime
flexibility to a program. To achieve this ultimate flexibility, a 2-D array can be modeled using a pointer
to a pointer of the desired data type. Initially, the dimension representing the number of rows will be
allocated. Next, for each row, the number of elements in each row will be allocated. As with the last
example using an array of pointers, the number of elements in each row (the column entries) need
not be uniform in size across rows. However, to accurately model the concept of a 2-D array, it is
assumed that the column size will be allocated uniformly from row to row.

```cpp
#include <iostream>
using std::cout;
using std::cin;
using std::endl;
using std::flush;
int main()
{
    int numRows = 0, numColumns = 0;
    float **TwoDimArray = nullptr; // pointer to a pointer
    cout << "Enter number of rows: " << flush;
    cin >> numRows;
    TwoDimArray = new float * [numRows]; // alloc. row ptrs
    cout << "Enter number of Columns: ";
    cin >> numColumns;
    for (int i = 0; i < numRows; i++)
    {
        // allocate column quantity for each row
        TwoDimArray[i] = new float [numColumns];
        // load each column entry with data
        for (int j = 0; j < numColumns; j++)
        {
            TwoDimArray[i][j] = i + j + .05;
            cout << TwoDimArray[i][j] << " ";
        }
        cout << end; // print newline between rows
    }
    for (i = 0; i < numRows; i++)
        delete [] TwoDimArray[i]; // del col. for each row
    delete [] TwoDimArray; // delete allocated rows
    return 0;
}

```

### Dynamically allocating N-D arrays – pointers to pointers to pointers

In C++, you can model any dimensionality of a dynamically allocated array. You need only to be
able to imagine it, declare the appropriate levels of pointers, and make the required levels of memory
allocation (and eventual deallocation).

```cpp
#include <iostream>
using std::cout;
using std::cin;
using std::endl;

using std::flush;
int main()
{
    int dim1 = 0, dim2 = 0, dim3 = 0;
    int ***ThreeDimArray = nullptr; // 3D dyn. alloc. array
    cout << "Enter dim 1, dim 2, dim 3: ";
    cin >> dim1 >> dim2 >> dim3;
    ThreeDimArray = new int ** [dim1]; // allocate dim 1
    for (int i = 0; i < dim1; i++)
    {
        ThreeDimArray[i] = new int * [dim2]; // alloc dim 2
        for (int j = 0; j < dim2; j++)
        {
            // allocate dim 3
            ThreeDimArray[i][j] = new int [dim3];
            for (int k = 0; k < dim3; k++)
            {
                ThreeDimArray[i][j][k] = i + j + k;
                cout << ThreeDimArray[i][j][k] << " ";
            }
            cout << endl; // print '\n' between dimensions
        }
        cout << end; // print '\n' between dimensions
    }
    for (int i = 0; i < dim1; i++)
    {
        for (int j = 0; j < dim2; j++)
            delete [] ThreeDimArray[i][j]; // release dim 3
        delete [] ThreeDimArray[i]; // release dim 2
    }
    delete [] ThreeDimArray; // release dim 1
    return 0;
}
```

## Using pointers with functions

Functions in C++ will undoubtedly take arguments. We have seen many examples in the previous
chapters illustrating function prototypes and function definitions. Now, let’s augment our understanding
of functions by passing pointers as arguments to functions, and using pointers as return values from
a function.

### Passing pointers as arguments to functions

Arguments passed from actual to formal parameters in a function call are by default copied on the
stack. In order to modify the contents of a variable as an argument to a function, a pointer to that
argument must instead be used as a function parameter.

```cpp
#include <iostream>
using std::cout;
using std::endl;
void TryToAddOne(int); // function prototypes
void AddOne(int *);

int main()
{
    int x = 10, *y = nullptr;
    y = new int; // allocate y's memory
    *y = 15; // dereference y to assign a value
    cout << "x: " << x << " and *y: " << *y << endl;
    TryToAddOne(x); // unsuccessful, call by value
    TryToAddOne(*y); // still unsuccessful
    cout << "x: " << x << " and *y: " << *y << endl;
    AddOne(&x); // successful, passing an address
    AddOne(y); // also successful
    cout << "x: " << x << " and *y: " << *y << endl;
    delete y; // relinquish heap memory
    return 0;
}
void TryToAddOne(int arg)
{
    arg++;
}

void AddOne(int *arg)
{
    (*arg)++;
}
```

### Using pointers as return values from functions

Functions may return pointers to data via their return statements. When returning a pointer via the
return statement of a function, be sure that the memory that is pointed to will persist after the function
call is completed. Do not return a pointer to stack memory that is local to the function. That is, do
not return a pointer to local variables defined on the stack within the function. However, returning a
pointer to memory allocated using new() within the function is acceptable. As the allocated memory
will be on the heap, it will exist past the function call.

```cpp
#include <iostream>
#include <iomanip>
using std::cin;
using std::cout;
using std::endl;
using std::flush;
using std::setw;
constexpr int MAX = 20;
[[nodiscard]] char *createName(); // function prototype

int main()
{
    char *name = nullptr; // pointer declaration and init.
    name = createName(); // function will allocate memory
    cout << "Name: " << name << endl;
    delete [] name; // del alloc. memory (in a diff. scope
    return 0; // than allocated); this can be error prone!
}

[[nodiscard]] char *createName()
{
    char *temp = new char[MAX];
    cout << "Enter name: " << flush;
    cin >> setw(MAX) >> temp; // ensure no overflow of temp
    return temp;
}
```

## Using the const qualifier with pointers

The const qualifier can be used to qualify pointers in several different ways. The keyword const
can be applied to the data pointed to, to the pointer itself, or both. By using the const qualifier in
these ways, C++ offers means to protect values in a program that may be meant to be initialized but
never again modified. Let’s examine each of these various scenarios. We will also be combining const
qualified pointers with return values from functions to understand which of these various scenarios
are reasonable to implement.

### Using pointers to constant objects

A pointer to a constant object may be specified so that the object that is pointed to may not be directly
modified. A dereferenced pointer to this object may not be used as an l-value in any assignment. An
l-value means a value that can be modified, and that occurs on the left-hand side of an assignment.

```cpp
// const qualified str; the data pointed to will be const
const char *constData = "constant";
const char *moreConstData = nullptr;
// regular strings, defined. One is loaded using strcpy()
char *regularString = nullptr;
char *anotherRegularString = new char[8]; // sized to fit
// this string
strcpy(anotherRegularString, "regular");
// Trying to modify data marked as const will not work
// strcpy(constData, "Can I do this? "); // NO!
// Trying to circumvent by having a char * point to
// a const char * also will not work
// regularString = constData; // NO!
// But we can treat a char * more strictly by assigning to
// const char *. It will be const from that viewpoint only
moreConstData = anotherRegularString; // Yes - can do this!

```

### Using constant pointers to objects

A constant pointer to an object is a pointer that is initialized to point to a specific object. This pointer
may never be assigned to point to another object. This pointer itself may not be used as an l-value in
an assignment.

```cpp
// Define, allocate, load simple strings using strcpy()
char *regularString = new char[36]; // sized for str below
strcpy(regularString, "I am a modifiable string");
char *anotherRegularString = new char[21]; // sized for
// string below
strcpy(anotherRegularString, "I am also modifiable");
// Define a const pointer to a string; must be initialized
char *const constPtrString = regularString; // Ok

// You may not modify a const pointer to point elsewhere
// constPtrString = anotherRegularString; // No!
// But you may change the data which you point to
strcpy(constPtrString, "I can change the value"); // Yes
```

### Using constant pointers to constant objects

A constant pointer to a constant object is a pointer that is established to point to a specific object
and to unmodifiable data. The pointer itself must be initialized to a given object, which is (hopefully)
initialized with appropriate values. Neither the object nor the pointer may be modified or used as
l-values in assignments.

```cpp
// Define two regular strings and load using strcpy()
char *regularString = new char[36]; // sized for str below
strcpy(regularString, "I am a modifiable string");
char *anotherRegularString = new char[21]; // sized for
// string below
strcpy(anotherRegularString, "I am also modifiable");
// Define const ptr to a const object; must be initialized
const char *const constStringandPtr = regularString; // Ok

// Trying to change the pointer or the data is illegal
constStringandPtr = anotherRegularString; // No! Can't
// modify address
strcpy(constStringandPtr, "Nope"); // No! Can't modify data
```

### Using pointers to constant objects as function arguments and as return types from functions

Copying arguments on the stack that are user defined types can be time-consuming. Passing a pointer
as a function argument is speedier, yet permits the dereferenced object to possibly be modified in the
scope of the function. Passing a pointer to a constant object as a function argument provides both
speed and safety for the argument in question. The dereferenced pointer simply may not be an l-value
in the scope of the function in question. The same principle holds true for the return value from a
function. Constant qualifying the data pointed to insists that the caller of the function must also
store the return value in a pointer to a constant object, ensuring the object’s long-term immutability.

```cpp
#include <iostream>
#include <iomanip>
#include <cstring> // we'll generally prefer std::string,
// however, let's understand ptr concept shown here
using std::cout;
using std::endl;
char suffix = 'A';
const char *GenId(const char *); // function prototype
int main()
{
    const char *newId1, *newId2; // pointer declarations
    newId1 = GenId("Group"); // func. will allocate memory
    newId2 = GenId("Group");
    cout << "New ids: " << newId1 << " " << newId2 << endl;
    delete [] newId1; // delete allocated memory
    delete [] newId2; // caution: deleting in different
    // scope than allocation can
    // lead to potential errors
    return 0;
}
const char *GenId(const char *base)
{
    char *temp = new char[strlen(base) + 2];
    strcpy(temp, base); // use base to initialize string
    temp[strlen(base)] = suffix++; // Append suffix to base
    temp[strlen(base) + 1] = '\0'; // Add null character
    return temp; // temp will be upcast to a const char *
    // to be treated more restrictively than
    // it was defined
}

```

## Using pointers to objects of unspecified types

Sometimes, programmers ask why they cannot simply have a generic pointer. That is, why must we
always declare the type of data to which the pointer will eventually point, such as int *ptr;?
C++ certainly does allow us to create pointers without associated types, but C++ then requires the
programmer to keep track of things that would normally be done on their behalf. Nonetheless, we
will see why void pointers are useful and what the programmer must undertake when using more
generic void pointers in this section.

However, the need still exists to have a more generic pointer. For example, you may want a pointer that
might point to an integer in one situation, yet to a set of user defined types in another situation. Using
a void * allows just this to happen. But what about type? What happens when you dereference a
void pointer? If C++ does not know how many bytes to go from one element in a set to another, how
can it index into a dynamically allocated array of void pointers? How will it interpret the bytes once
at an address? What is the type?

The answer is that you, the programmer, must personally remember what you are pointing to at all
times. Without the type associated with the pointer, the compiler cannot do this for you. And when
it is time to dereference the void pointer, you will be in charge of correctly remembering the ultimate
type involved and performing the appropriate type cast on that pointer.

### Creating void pointers

Pointers to objects of unspecified types may be specified by using void *. The void pointer may then
point to an object of any type. Explicit casting must be used in order to dereference actual memory
pointed to by the void *. Explicit casting must also be used in C++ to assign memory pointed to by
a void * to a pointer variable of a known type. It is the programmer’s responsibility to ensure that
the dereferenced data types are the same before making the assignment. Should the programmer be
incorrect, there will be an elusive pointer mistake to find elsewhere in the code.

```cpp
#include <iostream>
using std::cout;
using std::endl;
int main()
{
    void *unspecified = nullptr; // may point to any
    // data type
    int *x = nullptr;
    unspecified = new int; // void ptr now points to an int
    // void * must be cast to int * before dereferencing
    *(static_cast<int *>(unspecified)) = 89;
    // let x point to the memory that unspecified points to
    x = static_cast<int *>(unspecified);
    cout << *x << " " << *(static_cast<int *>(unspecified))
    << endl;
    delete static_cast<int *>(unspecified);

    return 0;
}
```

### Looking ahead to smart pointers for safety

Is there another way to have the benefits of dynamically allocated memory, and
yet have a safety net to govern its use? Fortunately, the answer is yes. The concept is a smart pointer,
and there are several types of smart pointers in C++, including unique_ptr, shared_ptr,
and weak_ptr. The premise of a smart pointer is that it is a class to safely wrap the usage of a raw
pointer, minimally handling the proper deallocation of heap memory when the outer smart pointer
goes out of scope.

## Summary

In this chapter, we have learned many aspects surrounding pointers in C++. We have seen how to allocate
memory from the heap using new() and how to relinquish that memory to the heap management
facility using delete(). We have seen examples using both standard and user defined types. We
have also understood why we may want to dynamically allocate arrays and have seen how to do so for
1, 2, and N dimensions. We have seen how to release the corresponding memory using delete[].
We have reviewed functions by adding pointers as parameters to functions and as return values from
functions. We have also learned how to const qualify pointers as well as the data to which they point
(or both) and why you may want to do so. We have seen one way to genericize pointers by introducing
void pointers. Lastly, we have looked ahead to the concept of smart pointers.

All of the skills using pointers from this chapter will be used freely in the upcoming chapters. C++
expects programmers to have great facility using pointers. Pointers allow the language great freedom
and efficiency to utilize a vast number of data structures and to employ creative programming solutions.
However, pointers can provide a massive way to introduce errors into a program with memory leakage,
returning pointers to memory that no longer exists, dereferencing pointers that have been deleted,
and so on. Not to worry; we will utilize many examples going forward using pointers so that you
will be able to manipulate pointers with great facility. Additionally, we will later add specific types of
smart pointers to our upcoming programming repertoire to allow us to use add pointer safety when
constructing code from scratch.
Most importantly, you are now ready to move forward to Chapter 4, Indirect Addressing – References,
in which we will explore indirect addressing using references. Once you have understood both types
of indirect addressing – pointers and references – and can manipulate either with ease, we will take
on the core object-oriented concepts in this book, starting in Chapter 5, Exploring Classes in Detail.
