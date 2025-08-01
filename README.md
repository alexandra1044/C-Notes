# C

# Design Patterns

Table 1. Patterns for tidy code

| **Pattern name** | **Summary** |
| --- | --- |
| “Function Split” | The function has several responsibilities, which makes the function hard to read and maintain. Therefore, split it up. Take a part of a function that seems useful on its own, create a new function with that, and call that function. |
| “Guard Clause” | The function is hard to read and maintain because it mixes pre-condition checks with the main program logic of the function. Therefore, check if you have mandatory pre-conditions, and immediately return from the function if these pre-conditions are not met. |
| “Samurai Principle” | When returning error information, you assume that the caller checks for this information. However, the caller can simply omit this check and the error might go unnoticed. Therefore, return from a function victorious or not at all. If there is a situation for which you know that an error cannot be handled, then abort the program. |
| “Goto Error Handling” | Code gets difficult to read and maintain if it acquires and cleans up multiple resources at different places within a function. Therefore, have all resource cleanup and error handling at the end of the function. If a resource cannot be acquired, use the goto statement to jump to the resource cleanup code. |
| “Cleanup Record” | It is difficult to make a piece of code easy to read and maintain if this code acquires and cleans up multiple resources, particularly if those resources depend on one another. Therefore, call resource acquisition functions as long as they succeed, and store which functions require cleanup. Call the cleanup functions depending on these stored values. |
| “Object-Based Error Handling” | Having multiple responsibilities in one function, such as resource acquisition, resource cleanup, and usage of that resource, makes that code difficult to implement, read, maintain, and test. Therefore, put initialization and cleanup into separate functions, similar to the concept of constructors and destructors in object-oriented programming. |

Table 2. Patterns for returning error information

| **Pattern name**	 | **Summary** |
| --- | --- |
| “Return Status Codes” | You want to have a mechanism to return status information to the caller, so that the caller can react to it. You want the mechanism to be simple to use, and the caller should be able to clearly distinguish between different error situations that could occur. Therefore, use the Return Value of a function to return status information. Return a value that represents a specific status. Both of you as the callee and the caller must have a mutual understanding of what the value means. |
| “Return Relevant Errors” | On the one hand, the caller should be able to react to errors; on the other hand, the more error information you return, the more your code and the code of your caller have to deal with error handling, which makes the code longer. Longer code is harder to read and maintain and brings in the risk of additional bugs. Therefore, only return error information to the caller if that information is relevant to the caller. Error information is only relevant to the caller if the caller can react to that information. |
| “Special Return Values” | You want to return error information, but it’s not an option to explicitly Return Status Codes because that implies that you cannot use the Return Value of the function to return other data. You’d have to return that data via Out-Parameters, which would make calling your function more difficult. Therefore, use the Return Value of your function to return the data computed by the function. Reserve one or more special values to be returned if an error occurs. |
| “Log Errors” | You want to make sure that in case of an error you can easily find out its cause. However, you don’t want your error-handling code to become complicated because of this. Therefore, use different channels to provide error information that is relevant for the calling code and error information that is relevant for the developer. For example, write debug error information into a log file and don’t return the detailed debug error information to the caller. |

Table 3. Patterns for memory allocation

| **Pattern name**	 | **Summary** |
| --- | --- |
| “Stack First” | Deciding the storage class and memory section (stack, heap, …) for variables is a decision every programmer has to make often. It gets exhausting if for each and every variable, the pros and cons of all possible alternatives have to be considered in detail. Therefore, simply put your variables on the stack by default to profit from automatic cleanup of stack variables. |
| “Eternal Memory” | Holding large amounts of data and transporting it between function calls is difficult because you have to make sure that the memory for the data is large enough and that the lifetime extends across your function calls. Therefore, put your data into memory that is available throughout the whole lifetime of your program. |
| “Lazy Cleanup” | Having dynamic memory is required if you need large amounts of memory and memory where you don’t know the required size beforehand. However, handling cleanup of dynamic memory is a hassle and is the source of many programming errors. Therefore, allocate dynamic memory and let the operating system cope with deallocation by the end of your program. |
| “Dedicated Ownership | The great power of using dynamic memory comes with the great responsibility of having to properly clean that memory up. In larger programs, it becomes difficult to make sure that all dynamic memory is cleaned up properly. Therefore, right at the time when you implement memory allocation, clearly define and document where it’s going to be cleaned up and who is going to do that. |
| “Allocation Wrapper” | Each allocation of dynamic memory might fail, so you should check allocations in your code to react accordingly. This is cumbersome because you have many places for such checks in your code. Therefore, wrap the allocation and deallocation calls, and implement error handling or additional memory management organization in these wrapper functions. |
| “Pointer Check” | Programming errors that lead to accessing an invalid pointer cause uncontrolled program behavior, and such errors are difficult to debug. However, because your code works with pointers frequently, there is a good chance that you have introduced such programming errors. Therefore, explicitly invalidate uninitialized or freed pointers and always check pointers for validity before accessing them. |
| “Memory Pool” | Frequently allocating and deallocating objects from the heap leads to memory fragmentation. Therefore, hold a large piece of memory throughout the whole lifetime of your program. At runtime, retrieve fixed-size chunks of that memory pool instead of directly allocating new memory from the heap. |

Table 4. Patterns for returning data from C functions

| **Pattern name**	 | **Summary** |
| --- | --- |
| “Return Value” | The function parts you want to split are not independent from one another. As usual in procedural programming, some part delivers a result that is then needed by some other part. The function parts that you want to split need to share some data. Therefore, simply use the one C mechanism intended to retrieve information about the result of a function call: the Return Value. The mechanism to return data in C copies the function result and provides the caller access to this copy. |
| “Out-Parameters” | C only supports returning a single type from a function call, and that makes it complicated to return multiple pieces of information. Therefore, return all the data with a single function call by emulating by-reference arguments with pointers. |
| “Aggregate Instance” | C only supports returning a single type from a function call, and that makes it complicated to return multiple pieces of information. Therefore, put all data that is related into a newly defined type. Define this Aggregate Instance to contain all the related data that you want to share. Define it in the interface of your component to let the caller directly access all the data stored in the instance. |
| “Immutable Instance” | You want to provide information held in large pieces of immutable data from your component to a caller. Therefore, have an instance (for example, a struct) containing the data to share in static memory. Provide this data to users who want to access it and make sure that they cannot modify it. |
| “Caller-Owned Buffer” | You want to provide complex or large data of known size to the caller, and that data is not immutable (it changes at runtime). Therefore, require the caller to provide a buffer and its size to the function that returns the large, complex data. In the function implementation, copy the required data into the buffer if the buffer size is large enough. |
| “Callee Allocates” | You want to provide complex or large data of unknown size to the caller, and that data is not immutable (it changes at runtime). Therefore, allocate a buffer with the required size inside the function that provides the large, complex data. Copy the required data into the buffer and return a pointer to that buffer. |

Table 5. Patterns for data lifetime and ownership

| **Pattern name**	 | **Summary** |
| --- | --- |
| “Stateless Software-Module” | You want to provide logically related functionality to your caller and make that functionality as easy as possible for the caller to use. Therefore, keep your functions simple and don’t build up state information in your implementation. Put all related functions into one header file and provide the caller this interface to your software-module. |
| “Software-Module with Global State” | You want to structure your logically related code that requires common state information and make that functionality as easy as possible for the caller to use. Therefore, have one global instance to let your related functions share common resources. Put all functions that operate on this instance into one header file, and provide the caller this interface to your software-module. |
| “Caller-Owned Instance” | You want to provide multiple callers or threads access to functionality with functions that depend on one another, and the interaction of the caller with your functions builds up state information. Therefore, require the caller to pass an instance, which is used to store resource and state information, along to your functions. Provide explicit functions to create and destroy these instances, so that the caller can determine their lifetime. |
| “Shared Instance” | You want to provide multiple callers or threads access to functionality with functions that depend on one another, and the interaction of the caller with your functions builds up state information, which your callers want to share. Therefore, require the caller to pass an instance, which is used to store resource and state information, along to your functions. Use the same instance for multiple callers and keep the ownership of that instance in your software-module. |

Table 6. Patterns for flexible APIs

| Pattern name	 | Summary |
| --- | --- |
| “Header Files” | You want functionality that you implement to be accessible to code from other implementation files, but you want to hide your implementation details from the caller. Therefore, provide function declarations in your API for any functionality you want to provide to your user. Hide any internal functions, internal data, and your function definitions (the implementations) in your implementation file and don’t provide this implementation file to the user. |
| “Handle” | You have to share state information or operate on shared resources in your function implementations, but you don’t want your caller to see or even access all that state information and shared resources. Therefore, have a function to create the context on which the caller operates and return an abstract pointer to internal data for that context. Require the caller to pass that pointer to all your functions, which can then use the internal data to store state information and resources. |
| “Dynamic Interface” | It should be possible to call implementations with slightly deviating behaviors, but it should not be necessary to duplicate any code, not even the control logic implementation and interface declaration. Therefore, define a common interface for the deviating functionalities in your API and require the caller to provide a callback function for that functionality, which you then call in your function implementation. |
| “Function Control” | You want to call implementations with slightly deviating behaviors, but you don’t want to duplicate any code, not even the control logic implementation or the interface declaration. Therefore, add a parameter to your function that passes meta-information about the function call and that specifies the actual functionality to be performed. |

Table 7. Patterns for flexible iterator interfaces

| Pattern name	 | Summary |
| --- | --- |
| “Index Access” | You want to make it possible for the user to iterate elements in your data structure in a convenient way, and it should be possible to change internals of the data structure without resulting in changes to the user’s code. Therefore, provide a function that takes an index to address the element in your underlying data structure and return the content of this element. The user calls this function in a loop to iterate over all elements. |
| “Cursor Iterator” | You want to provide an iteration interface to your user which is robust in case the elements change during the iteration and which enables you to change the underlying data structure at a later point without requiring any changes to the user’s code. Therefore, create an iterator instance that points to an element in the underlying data structure. An iteration function takes this iterator instance as argument, retrieves the element the iterator currently points to, and modifies the iteration instance to point to the next element. The user then iteratively calls this function to retrieve one element at a time. |
| “Callback Iterator” | You want to provide a robust iteration interface which does not require the user to implement a loop in the code for iterating over all elements and which enables you to change the underlying data structure at a later point without requiring any changes to the user’s code. Therefore, use your existing data structure—specific operations to iterate over all your elements within your implementation, and call some provided user-function on each element during this iteration. This user-function gets the element content as a parameter and can then perform its operations on this element. The user calls just one function to trigger the iteration, and the whole iteration takes place inside your implementation. |

Table 8. Patterns for organizing files in modular programs

| **Pattern name** | **Summary** |
| --- | --- |
| “Include Guard” | It’s easy to include a header file multiple times, but including the same header file leads to compile errors if types or certain macros are part of it, because during compilation they get redefined. Therefore, protect the content of your header files against multiple inclusion so that the developer using the header files does not have to care whether it is included multiple times. Use an interlocked #ifdef statement or a #pragma once statement to achieve this. WARNING: PRAGMA IS NON STANDARD |
| “Software-Module Directories” | Splitting code into different files increases the number of files in your codebase. Having all files in one directory makes it difficult to keep an overview of all the files, particularly for large codebases. Therefore, put header files and implementation files that belong to a tightly coupled functionality into one directory. Name that directory after the functionality that is provided via the header files. |
| “Global Include Directory” | To include files from other software-modules, you have to use relative paths like ../othersoftwaremodule/file.h. You have to know the exact location of the other header file. Therefore, have one global directory in your codebase that contains all software-module APIs. Add this directory to the global include paths in your toolchain. |
| “Self-Contained Component” | From the directory structure it is not possible to see the dependencies in the code. Any software-module can simply include the header files from any other software-module, so it’s impossible to check dependencies in the code via the compiler. Therefore, identify software-modules that contain similar functionality and that should be deployed together. Put these software-modules into a common directory and have a designated subdirectory for their header files that are relevant for the caller. |
| “API Copy” | You want to develop, version, and deploy the parts of your codebase independently from one another. However, to do that, you need clearly defined interfaces between the code parts and the ability to separate that code into different repositories. Therefore, to use the functionality of another component, copy its API. Build that other component separately and copy the build artifacts and its public header files. Put these files into a directory inside your component and configure that directory as a global include path. |

Table 9. Patterns for escaping #ifdef hell

| Pattern name	 | Summary |
| --- | --- |
| “Avoid Variants” | Using different functions for each platform makes the code harder to read and write. The programmer is required to initially understand, correctly use, and test these multiple functions in order to achieve a single functionality across multiple platforms. Therefore, use standardized functions that are available on all platforms. If there are no standardized functions, consider not implementing the functionality. |
| “Isolated Primitives” | Having code variants organized with #ifdef statements makes the code unreadable. It is very difficult to follow the program flow, because it is implemented multiple times for multiple platforms. Therefore, isolate your code variants. In your implementation file, put the code handling the variants into separate functions and call these functions from your main program logic, which then contains only platform-independent code. |
| “Atomic Primitives” | The function that contains the variants and is called by the main program is still hard to comprehend because all the complex #ifdef code was only put into this function in order to get rid of it in the main program. Therefore, make your primitives atomic. Only handle exactly one kind of variant per function. If you handle multiple kinds of variants, for example, operating system variants and hardware variants, then have separate functions for that. |
| “Abstraction Layer” | You want to use the functionality which handles platform variants at several places in your codebase, but you do not want to duplicate the code of that functionality. Therefore, provide an API for each functionality that requires platform-specific code. Define only platform-independent functions in the header file and put all platform-specific #ifdef code into the implementation file. The caller of your functions includes only your header file and does not have to include any platform-specific files. |
| “Split Variant Implementations” | The platform-specific implementations still contain #ifdef statements to distinguish between code variants. That makes it difficult to see and select which part of the code should be built for which platform. Therefore, put each variant implementation into a separate implementation file and select per file what you want to compile for which platform.
Programming errors that lead to accessing an invalid pointer cause uncontrolled program behavior, and such errors are difficult to debug. However, because your code works with pointers frequently, there is a good chance that you have introduced such programming errors. Therefore, explicitly invalidate uninitialized or freed pointers and always check pointers for validity before accessing them. |
| “Out-Parameters” | You want to provide information held in large pieces of immutable data from your component to a caller. Therefore, have an instance (for example, a struct) containing the data to share in static memory. Provide this data to users who want to access it and make sure that they cannot modify it. |

# General Points of Confusion

## Pass by Pointer vs Reference

- Pass by Pointer - Here, the memory location (address) of the variables is passed to the 
parameters in the function, and then the operations are performed. It is also called the **call by pointer** method.

```cpp
#include <iostream>
using namespace std;
 
void swap(int *x, int *y)
{
    int z = *x;
    *x = *y;
    *y = z;
}
 
// Driver Code
int main()
{
    int a = 45, b = 35;
    cout << "Before Swap\n";
    cout << "a = " << a << " b = " << b << "\n";
 
    swap(&a, &b);
 
    cout << "After Swap with pass by pointer\n";
    cout << "a = " << a << " b = " << b << "\n";
}
```

- Pass by Reference - It allows a function to modify a variable without having to create a copy of it. We have to declare reference variables. The memory location of the passed variable and parameter is the same and therefore, any change to the parameter reflects in the variable as well.

```cpp
#include <iostream>
using namespace std;
void swap(int& x, int& y)
{
    int z = x;
    x = y;
    y = z;
}
 
int main()
{
    int a = 45, b = 35;
    cout << "Before Swap\n";
    cout << "a = " << a << " b = " << b << "\n";
 
    swap(a, b);
 
    cout << "After Swap with pass by reference\n";
    cout << "a = " << a << " b = " << b << "\n";
}
```

| **Parameters** | 
**Pass by Pointer**
 | 
**Pass by Reference**
 |
| --- | --- | --- |
| 
**Passing Arguments**
 | We pass the address of arguments in the function call. | We pass the arguments in the function call. |
| 
**Accessing Values**
 | The value of the arguments is accessed via the dereferencing operator * | The reference name can be used to implicitly reference a value. |
| 
**Reassignment**
 | Passed parameters can be moved/reassigned to a different memory location. | Parameters can’t be moved/reassigned to another memory address. |
| 
**Allowed Values**
 | Pointers can contain a NULL value, so a passed argument may point to a NULL or even a garbage value. | References cannot contain a NULL value, so it is guaranteed to have some value. |

# Preprocessor Directives

- Preprocessing allows the engineer to modify source code before sending it to the compiler.
- Directives begin with ‘#’ and are only meaningful to the preprocessor and never the compiler.

## Macros

Macros have a number of applications and you can see some of them as follows:

- Defining a constant
- Using as a function instead of writing a C function
- Loop unrolling
- Header guards
- Code generation
- Conditional compilation

Define and undefine a macro:

```c
#define ABC 5

int main(int argc, char** argv) {
int x = 2;
int y = ABC;
int z = x + y;

#undef ABC 5
```

You can also use macros to define a function-like macro which accepts arguments:

```c
#define ADD(a, b) a + b
int main(int argc, char** argv) {
int x = 2;
int y = 3;
int z = ADD(x, y);
return 0;

}
```

After preprocessing it looks like:

```c
int main(int argc, char** argv) {
int x = 2;
int y = 3
int z = x + y;
return 0;

}
```

If you use GCC or Clang you can use -E to dump code after preprocessing.

A *translation unit* (or a *compilation unit*) is the preprocessed C code that is ready to be passed to the compiler.

Macros can be used to define a new **domain specific language** (**DSL**) and write code using it.

### Macro Parameters

- # operator turns the parameter into its string form surrounded by a pair of quotation marks
- ## just concatenates the parameters to other elements in the macro definition and usually forms variable names
- \ in a macro indicates that the following line is the continuation of the same macro definition

### Variadic Macros

- Can accept a variable number of input arguments
- Good when you don’t know the number of arguments in different usages of the same macro

```c
#define LOG_ERROR(format, ...) \

fprintf(stderr, format, __VA_ARGS__)
```

`__VA_ARGS__`. It is an indicator that tells the preprocessor to replace it with all the remaining input arguments that are not assigned to any parameter yet

### Problems with Macros

- Can be difficult for debugging since the information is lost after preprocessing, most debug information is denoted by the compiler so macro errors will not show up on logs
- Can be bad for design if you want modularity since macros make things linear and flat rather than in modules or packages

If a macro can be written as a C function, then you should write a C function instead!

### Loop Unrolling

- Technique is to remove loops and make them linear to increase performance
- Used for embedded systems/systems with limited processing power

```c
#define LOOP_3(X, ...) \

printf("%s\n", #X);
```

### Conditional Compliation

The preprocessor will pass different code to the compiler based on specific conditions

```c
#ifdef
#ifndef
#else
#elif
#endif
```

Macros can be defined using `-D` options passed to the compilation

```c
$ gcc -DCONDITION -E main.c
```

This allows you to have macros defined outside of source code files. Good for compiling things for different architectures. i.e. Mac or Linux.

### Header Guard

Common usage of `#ifndef` is to serve as a *header guard* statement. This statement protects a header file from being included twice in the preprocessing phase. Good for preventing errors.

```c
#ifndef EXAMPLE_1_8_H
#define EXAMPLE_1_8_H
void say_hello();
int read_age();
#endif
```

Could also achieve this with `#pragma once` but this is non-standard so if portability is important don’t use it and header guard instead.

```c
#pragma once
void say_hello();
int read_age();
```

# Pointers

### **Difference Between Reference Variable and Pointer Variable**

A reference is the same object, just with a different name and a reference must refer to an object. Since references can’t be NULL, they are safer to use.

- A pointer can be re-assigned while a reference cannot, and must be assigned at initialization only.
- The pointer can be assigned NULL directly, whereas the reference cannot.
- Pointers can iterate over an array, we can use increment/decrement operators to go to the next/previous item that a pointer is pointing to.
- A pointer is a variable that holds a memory address. A reference has the same memory address as the item it references.
- A pointer to a class/struct uses ‘->’ (arrow operator) to access its members whereas a reference uses a ‘.’ (dot operator).
- A pointer needs to be dereferenced with * to access the memory location it points to, whereas a reference can be used directly.

### Pointer Best Practice

Pointers *must* be initialized upon declaration. If you don't want to store any valid memory address while declaring them, don't leave them uninitialized. Make it null by assigning `0` or `NULL`

### Pointer Arithmetic

Think of memory like one long number-line, with a pointer to that memory. Incrementing the pointer makes the pointer go forward and decrementing it makes the pointer go backward.

*arithmetic step size - the number of bytes that the pointer will move if it is incremented or decremented by 1, determined by the data type of the pointer*

Data types that have the same number of bytes e.g. int and char have different step sizes, (int: 4 bytes) (char: 1)

You can use pointer arithmetic to iterate over memory. The elements should all be the same size so incrementing a pointer points to the next item in the array.

### Generic Pointer

Can point to any address like a normal pointer but forgets the data type. Cannot be de-referenced and cannot do arithmetic. Can be used to define generic pointer which can accept a wide range of different pointers as input arguments. Void pointer does not need a specific cast.

### Dangling Pointers

Reading or modifying an address where there is no variable registered is a big mistake.

If you malloc the variable to create it and then don’t free it inside the function, you can point to from outside the function because the variable is now on the heap.

# Stack Management

- The Stack segment is the default memory location where all local variables, arrays, and structures are allocated from.
- When you declare a local variable in a function, it is being allocated from the Stack segment, it always happens on top of the stack segment.
- First in, first out like a stack.
- Stack is also used for function calls

## The Stack and Function Calls

FUNCTION IS CALLED: When a function is called a Stack Segment is created with the return address of the function and all of the passing arguments is put on top of the Stack segment, and only then is the function logic executed.

FUNCTION RETURNS: When you return from a function the stack frame is popped out, and the instruction addressed by the return address gets executed which usually continues in the caller function.

The Stack is a limited portion of memory, and you could fill it up and potentially receive a *stack overflow* error.

All Stack variables become freed when leaving the function.

This usually happens when we have too many function calls consuming up all the Stack segment by their stack frames - this can happen commonly with recursive functions

# **Pass-by-value versus pass-by-reference**

Since C technically doesn’t have references you can’t ‘pass by reference’. Everything is copied into the function's local variable.

Accessing the pointer with & allows you to chance the contents.

Use pointers as function arguments rather than passing in big chunks of data as it is much more efficient.

# Function Pointers

```c
#include <stdio.h>

int sum(int a, int b) {
	
return a + b;

}

int subtract(int a, int b) {

return a - b;

}

int main() {

	int (*func_ptr)(int, int);

	func_ptr = NULL;

	func_ptr = &sum;

	int result = func_ptr(5, 4);

	printf("Sum: %d\n", result);

	func_ptr = &subtract;

	result = func_ptr(5, 4);

	printf("Subtract: %d\n", result);

return 0;

}
```

- Like a variable pointer addressing a variable, a function pointer addresses a function and allows you to call that function indirectly.
- This is the only way to support polymorphism in C.
- Aliases can help with this and it is standard to define a new one to use a function pointer, and can be defined with `typedef`, they usually end with `_t`

```c
#include <stdio.h>

typedef int bool_t;

typedef bool_t (*less_than_func_t)(int, int);

bool_t less_than(int a, int b) {

	return a < b ? 1 : 0;

}

bool_t less_than_modular(int a, int b) {

return (a % 5) < (b % 5) ? 1 : 0;

}

int main(int argc, char** argv) {
	
	less_than_func_t func_ptr = NULL;
	
	func_ptr = &less_than;
	
	bool_t result = func_ptr(3, 7);
	
	printf("%d\n", result);
	
	func_ptr = &less_than_modular;
	
	result = func_ptr(3, 7);
	
	printf("%d\n", result);

return 0;

}
```

# Structures

Structures encapsulate related values under a single unified type. As an early example, we can group `red`, `green`, and `blue` variables under a new single data type called `color_t`

```c
struct color_t {

	int red;
	
	int green;
	
	int blue;

};
```

To avoid typing struct every time you do a typedef:

```c
typedef struct {

	char first;
	
	char second;
	
	char third;
	
	short fourth;

} sample_t;
```

Then you can just declare the variable `sample_t var;`

# Growable buffer

```c
typedef struct my_type {
    uint64_t size;
    uint32_t buff[];
} my_type_t;

my_type_t* test2(uint64_t size) {
    malloc(sizeof (my_type_t) + sizeof (uint32_t)* size);

}
```

We can dynamically allocate a buffer.

Use case - passing data between two processes, but if we wanted to pass an array between them that would be quite difficult, instead we can pass a struct with a growable buffer which is in one data chunk which can be memcpy’d rather than passing the buffer around memory.

Whenever you need to store dynamic data inside of a struct.

# Memory Alignment

Memory alignment - when a data type is aligned, the CPU can fetch that data in the minimum number of cycles.

*Packed structures* are not aligned and using them may lead to binary incompatibilities and performance degradation. Packed structures are usually used in memory-constrained environments, but they can have a huge negative impact on the performance on most architecture

# Structures

## Nested Structures

- Otherwise called complex data types.
- The size of a complex structure is calculated exactly the same as a simple structure, by summing up the sizes of all its fields.
- We should be still careful about the alignment, of course, because it can affect the size of a complex structure.

It is common to call structure variables objects. They are exactly analogous to objects in 
object-oriented programming, and we will see that they can encapsulate both values and functions. So, it is not wrong at all to call them C objects.

## Structure Pointers

It is important to remember that a structure variable points to the address of the first field of the structure variable.

# Source Code to Program

A platform is a combination of an operating system running on specific hardware (or architecture), and its CPU's *instruction set* is the most important part of it.

Cross platform and portable are different:

- Cross-platform software usually has different binaries (final object files) and installers for each platform.
- Portable software uses the same produced binaries and installers on all platforms.

## Compilation Pipeline

### Building

Header files can include other header files, but never a source file. Source files can only 
include header files. It is bad practice to let a source file include another source file. If you do, then this usually means that you have a serious design problem in your project.

### Compiling

You don’t need to compile header files since they don’t contain actual C code (or at least, they shouldn’t).

Each file should be compiled separately 

### Step 1: Preprocessing

Preprocessor copies all file contents into a single body of code.
