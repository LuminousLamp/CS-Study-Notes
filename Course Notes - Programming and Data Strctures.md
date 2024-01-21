# VE280 Lecture Notes

[TOC]

## 2 - Linux Command Lines

#### Directory Tree Navigation

- cd: `cd pathname`

| special characters | /              | ~                     | .                 | ..               |
| ------------------ | -------------- | --------------------- | ----------------- | ---------------- |
| symbol             | root directory | user's home directory | current directory | parent directory |

- ls: `ls directory`
  - List files with specific characters: for example `ls *.pdf`

| options | -l             | -a                         |
| ------- | -------------- | -------------------------- |
| meaning | in long format | all files including hidden |

#### File Manipulation

- Create directory `mkdir`, delete **empty** directory `rmdir`, create an empty file `touch`

- Copy directory/files: cp
  - `cp dir1 dir2` copy dir1 into dir2, but contents in dir1 are not copied
  - `cp file dir`: copy a file into directory
  - `cp file1 file2`: copy the content of file1 into file2 (**cover**)
  - `cp -r dir1 dir2` : If dir2 does not exist, copy dir1 as dir2. If dir2 exists, copy dir1 <u>inside</u> dir2

- **Rename**/move files: mv
  - `mv file1 file2`: rename file1 as file2
  - `mv file dir`: move file to dir
  - `mv dir1 dir2`: If dir2 does not exist, <u>rename</u> dir1 as dir2. Otherwise, move dir1 <u>inside</u> dir2
- Edit/show files: nano, gedit, cat, less

- Remove files: rm
  - `rm -r` recursively delete all contents

- compare two files: diff

#### Others

- I/O redirection ">"
- Program management:
  - Installation: `sudo apt-get install program_name`
  - Uninstallation: `sudo apt-get autoremove program_name`

- Access Rights `dxxxxxxxxx`

  - '-' regular file, 'd' directory

  - Read, write, execution permission of owner/group/everyone else

## 3 - Developing Programs

#### Compilation

- Compilation process: C++ source code --- compiler --- object code (*.o) --- linker --- a binary file
  - Object code: portion of machine code for one particular library or module

- Compile the program: `g++ -o program source.cpp` (program: the name of the output file)
  - `-o` tells what the name of output file is
  - `-Wall` turn on all warnings
  
- Run program: `./program`

#### Several source files management

- Two types of files:
  - Header files: class definitions & function declarations
  - C++ source files: function definitions & member functions of classes

- Files splitting: example

| file name | add.h                                     | add.cpp                                            | run_add.cpp       |
| --------- | ----------------------------------------- | -------------------------------------------------- | ----------------- |
| content   | class definitions & function declarations | function definitions & member functions of classes | call the function |

```c++
// add.cpp: definition of functions
int add(int a, int b){
    return a+b;
}
// add.h: declaration of functions
#ifndef ADD_H // header gaurd
#define ADD_H
int add(int a, int b);
#endif
// run_add.cpp
#include "add.h"
int main(){
    add(2,3);
    return 0;
}
```

`#ifdef`: header guard, to prevent multiple definitions of the same functions in a file

#### Compile multiple files

- Way 1: `g++ -o programname [all .cpp files]`

  - No need to list .h files

  - Disadvantages: inconvenient, should retype each time

- Way 2: Makefile [A Simple Makefile Tutorial (colby.edu)](https://cs.colby.edu/maxwell/courses/tutorials/maketutor/)

  ```
  //Makefile
  run: [all dependency files]
  	g++ -o programname [all .cpp files]
  ```

  - Run `make` in command line
  - A command is only issued only when dependency is more recent than target

## 4 - Review of C++ Basics

#### lvalue and rvalue

| type of expressions | proposition                                       | remark                  |
| ------------------- | ------------------------------------------------- | ----------------------- |
| lvalue              | appear as either the left-hand or right-hand side | any non-const is lvalue |
| rvalue              | appear only right-hand side                       | any const is rvalue     |

#### Functions

- Declaration (prototype): must appear in the code before the function can be called
- Definition: can appear anywhere
  - function header + function body
- Function call mechanisms
  - Call-by-value: does not affect the variable outside
  - Call-by-reference: do effect on the inputting variable
  - An array is directly passed by reference

#### Pointers and Reference

- Pointer

  ```C++
  int *bar = &foo; // &, addressing operation
  *bar = 2 // *, dereference operation
  ```

  - Pointers and arrays: the array name is the <u>address of the first index of the array</u>

    The pointer to an array can be incremented/decremented. But <u>the array name itself CANNOT bee incremented/decremented</u>, because an array itself is rvalue
    
    ```C++
    int array[10];
    array == &array[0]; // true
    int *p = array;
    p++; //OK
    ```

- Reference: an alias for an object
  - <u>Must be initialized</u>
  - Cannot rebind a reference to a different object
  
- Comparison: pointer is more flexible

#### Struct

- Continuous in the memory

- Can be initialized in this way:

  ```C++
  struct St{
      string a;
      int b;
  }
  St object = {"abcdef", 100};
  ```

- If struct is directly passed to function, the whole values will be copied -- slow -- better to use a pointer

## 5 - Const Qualifiers

[C/C++ 基础中的基础： const 修饰符用法总结！ - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/90720012)

#### Const

- constant variable

  - <u>Usually defined as a global variable</u>
  - Must be initialized
  - Use const and avoid using `#define`

- const reference

  - `const int &ref = iVal`

  - <u>Can be initialized to an rvalue</u>, but non-const reference cannot

    ```C++
    const int &ref = iVal + 10; //OK
    int &ref = iVal + 10; //ERROR
    ```

  - Often used to pass struct/class into functions

  - Legality of different ways of bounding

    ```C++
    int i = 1;
    const int consti = 2;
    int &ref1 = i;//OK, bound nonconst reference to a nonconst variable
    const int &ref2 = i;//OK, bound const reference to a nonconst variable
    int &ref3 = consti; //ERROR, cannot bound nonconst reference to a const variable
    const int &ref4 = consti; //OK, bound const reference to a const variable
    ```

- const pointers

  | Type             | Usage                                                        | Expressions   |
  | ---------------- | ------------------------------------------------------------ | ------------- |
  | const pointer    | make the <u>value of the pointer</u> unchangeable<br />Initialization mandatory when declared | `T *const p;` |
  | pointer to const | make the <u>value of the object to which the pointer points</u> unchangeable <u>(cannot modify that object through this pointer, but still able to modify it in other ways as long as that object is not const)<br /></u>Initialization not mandatory when declared | `const T *p;` |

  - Legality of using pointer to const

    ```C++
    int i = 1;
    const int consti = 2;
    const int *p1 = &i; //OK, point a const pointer to nonconst variable
    int *p2 = &consti; //ERROR, cannot point a nonconst pointer to a const variable
    const int *
    ```

- const as function arguments/parameters

  - OK to pass a non-const as a const parameters
  -  ERROR to pass a const as a non-const parameters

  ```C++
  int func1(const int *p){}
  int func2(){int *p}
  int main(){
      int a = 0;
      const int b = 0;
      func1(&a); //OK
      func2(&b); //ERROR
  }
  ```

#### `typedef`

- Syntax: `typedef existing_type alias_name`

- Use const for things passed by reference, but won't be changed

#### `constexpr`

- Constant expression, a value known at the compilation time

  C++11中，constexpr是一个关键字，用于指定一个变量或函数在编译时就能得到计算结果。constexpr变量必须是一个常量表达式，而constexpr函数必须满足一定的条件才能被编译器视为常量表达式。constexpr变量和函数的使用可以提高程序的效率，因为它们在编译时就能得到计算结果，避免了运行时的计算。

  constexpr变量和函数的使用可以提高程序的效率，因为它们在编译时就能得到计算结果，避免了运行时的计算。

- Only literal types are allowed

- Caution: `constexpr int *p` is a <u>const pointer to int</u>

## 6 - Procedural Abstraction

- Abstraction: data abstraction / procedural abstraction

#### Procedural Abstraction

- (abstraction + abstraction implementation)
- Implementation property1: **Local** - does not depend on other abstraction implementation
- Implementation property2: **Substitutable** - the change of implementation does not affect its callers

#### Function as an abstraction

- Info1: type signature
- Info2: specification (describe in more detail, above function declaration)
  - REQUIRES: pre-condition to use the function (if any)
    - Functions without REQUIRES are <u>complete</u>, otherwise partial
  - MODIFIES: how <u>inputs</u> are modified (if any, usually when call-by-reference)
  - EFFECTS: what the the function computes

## 7 - Recursion, Function Pointers & Function Call

#### Recursion

- Recursion and loops can ALWAYS be converted each other
- Recursive helper function

#### Function Pointers

- A function has an address. The name of the function is a pointer to its address (like array name) -> manipulate functions like normal values

- Declare and assign a function pointer: `return_types (*pointer_name)(data_type1 arg1, data_type2 arg2, ...)`

  ```C++
  int func(int a, int b){}
  int main(){
      int (*fp)(int, int); // declare
      fp = func; // assign; the object that the pointer points to must have the same format of the return type and arguments as required by that function pointer
      fp(1,2); //call it
  }
  ```

  - Actually playing with pointer, but allowed to <u>ignore the "*" and "&" operators</u>
  - Function pointer CANNOT be incremented/decremented (like array name)  (but a normal pointer can)

- Using `typedef` on function pointers:

  ```C++
  typedef int (* funp_t)(int, int); // funp_t is the alias name
  ```

- A function can return a function pointer

  ```C++
  // Way 1
  int (*choose(int i))(int, int){
      if (i==0) return min; 
      else return max;
  }
  // Way 2 (after typedef)
  funp_t choose(int i){
      if (i==0) return min; 
      else return max;
  }
  ```

- Usage: pass a function pointer as arguments to functions - write a piece of code that depends on a function, and then change the function as needed

#### Type Inference `auto` (no exams)

- Deduce the type automatically

- Must be initialized when using `auto`

- <u>Ignores top-level const/ref, but not low-level const/ref</u>

  ```C++
  int i = 0;
  const int ci = i, &cref = i; // [ci] const int, [cref] const int &
  auto a = ci; // [a] int (ignore top-level const)
  const auto a2 = ci; // [a2] const int
  auto &a3 = ci; // [c] const int & (does not ignore low-level const)
  auto b = cref; // [b] int (ignore top-level reference and const)
  const auto b2 = cref; // [b2] const int
  auto &b3 = cref; // [b3] const int &
  auto *p1 = &ci; // [p1] const int * (i.e. pointer to costant, does not ignore low-level const)
  auto p2 = &ci; // [p2] (also) const int *
  const auto cp = &ci; // [cp] const int * const (the const we specify manually turns into "const pointer")
  ```

- Deduced type should be consistent, otherwise error

- `decltype`: declare exact type of expression 

  - Syntax: `decltype(c) a = b`
  - Define a variable call "a" of type the same as the variable c, and has value b (so b and c should be in the same type)

#### Function Call Mechanism (important)

- Function call process

  Evaluate the arguments - create activation record - copy the arguments to formal's storage space - evaluate the functions - replace the function call with result - destroy activation record

- Activation records maintained in call stacks
  - Activation record = <u>formal parameters + local variables</u>
  - Call stacks with pointers
  - Call stacks with recursion

- Sidenote: argument vs parameter:

  ```C++
  // a and b are the PARAMETERS
  int Mult(int a, int b) {return a * b;}
  int main(){
      int num1 = 10, num2 = 20, res;
      res = Mult(num1, num2); // num1 & num2 as ARGUMENTS.
      return 0;
  }
  ```

  - An argument is referred to the values that are passed within a function when the function is called. 

  - The parameter is referred to as the variables that are defined during a function declaration or definition.

## 8 - Enum

- Type definition: `enum type_name {A,B,C,...};`
- Variable declaration: `enum type_name variable_name;` or `type_name variable_name;`
- Variable assignment: `variable_name = A;`

- `enum` type can be converted to int, but int type cannot be converted to `enum` type

  ```C++
  enum Suit_t {A,B,C,D}
  int p = 2*D;//OK
  array[2*D]... //OK
  enum Suit_t q = 2*D; //ERROR
  ```


## 9 - Passing Arguments to Program

- Format:
  - Main function header: `int main(int argc, char *argv[])` (fixed format)
  - Run the program: `./program_name argument1 argument2 ... argumentn`

- `argc` (argument count): number of inputted arguments (**including the program name**)
- `argv` (argument vector): list of arguments - array of C-string
  - Must be converted to other data types
- <u>Check `argc` before accessing `argv`! Check type of arguments!</u>

## 10 - I/O Streams

- Streams: unidirectional; characters / binary data

#### Output Stream `cout`

- Format: insertion operator, `output_stream << something`, automatic conversion

- `setw()`: set <u>minimum width</u> of the following variables && <u>right-aligns</u> that variable
  - `#include <iomanip>`

- Buffering:
  - First saved in the buffer instead of directly written to the stream
  - Advantages: reduce number of times of interaction with hard drive, which is slow
  - Flushed when:
    - Newline inserted (`endl` or `'\n'`)
    - Buffer is full
    - Explicitly flushed: `cout << ... << flush;`
    - Program decides to read from cin
    - Program exits

- Alternative output streams: comparison between `cout` `cerr` `clog` 

  | Stream | Name                  | Usually used to...       | With buffers? |
  | ------ | --------------------- | ------------------------ | ------------- |
  | cout   | standard output       | print data to the screen | Yes           |
  | cerr   | standard error output | print the error messages | No            |
  | clog   | standard log output   | similar to cerr          | Yes           |

- Output redirection: Windows as an example

  | 流#  | 说明        | 已引入的版本   | 写入 Cmdlet     |
  | :--- | :---------- | :------------- | :-------------- |
  | 1    | **成功** 流 | PowerShell 2.0 | `Write-Output`  |
  | 2    | **错误** 流 | PowerShell 2.0 | `Write-Error`   |
  | 3    | **警告** 流 | PowerShell 3.0 | `Write-Warning` |

#### Input Stream `cin`

- Format: extraction operator, `cin >> a >> b;`, and the following:
  - `getline(the_stream, desinated_string)` read the whole line from `the_stream`, and assign it to `desinated_string` (of type string)
  - `cin.get(ch)`: get one character from `cin`, and assign it to `ch` (of type char)

- Buffering: flushed when enter key pressed
- Failed input stream

#### File Stream

- `#include <fstream>`: two data types `ifstream` and `ofstream`

- Input file stream: **[input from file (to the stream)]** file -> file stream -> do other things

- Output file stream: **[output (from the stream) to file]** other things -> file stream -> file

- Steps to use

  - Declare stream variable
  - Link to a file: `stream_name.open("file_name")`
  - Use the stream: `stream_name >> ...` or `getlint()` for input stream and `<<` for output stream
  - Close the stream: `stream_name.close()`

- Failed file streams: cannot be opened OR read the end of the file; <u>good habit to check the state</u>

  ```C++
  if (! stream_name){ // an example of checking the stream state
  	cerr << "cannot open \n";
  }
  ```

- Better way to avoid printing empty line:

  ```C++
  while (getline(iFile, line))
  	cout << line << endl;
  ```

#### String Stream

- `#include <sstream>`: two data types `istringstream` and `ostringstream`

- Input string stream: help extract variables from a string

  ```C++
  int variable1;
  string variable2;
  string line = "42 abcde";
  istringstream iStream;
  iStream.str(line);
  iStream >> variable1 >> variable2; // now variable1 == 42, variable2 == "abcde"
  ```

- Output string stream: help collate messy things into a string

  ```C++
  int variable1 = 100;
  char *variable2 = "opps";
  string result;
  ostringstream oStream;
  oStream << variable1 << " " << variable2;
  result = oStream.str(); // now result = "100 opps"
  ```

## 11 - Testing and Debugging

#### Testing

- Incremental testing
- Five steps
  - Understand the specification
  - Identify the required behaviors
  - Write specific tests: **Simple inputs, Boundary inputs, Nonsense inputs**
  - Know the answers in advance
  - Include stress tests (large/long running test cases)

- Automation

#### Debugging

- `assert(bool)` function (`#include <cassert>`)
  - test the condition that should hold
  - If argument false, stop the program immediately and print a error message
- **Disable** assert by `NDEBUG` (a preprocessor): all the assert functions will turn off
  
  - Way 1: `#define NDEBUG` in the code
  - Way 2: when compiling the program `g++ -DNDEBUG ...` (equivalent, just making #define outside the code)
  
- Take advantage of NDEBUG to do other things

  ```C++
  // in the middle of the code
  #ifndef NDEBUG
  	// do some debugging things more complicated than assert
  #endif
  ```

## 12 - Exception

#### Concept of Exception Handling

- Existing strategies for dealing with exception

|           | Runtime checking                                             | No runtime checking                                          |
| --------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Principle | Design in the function how to deal with illegal inputs       | Not doing anything to potential illegal inputs               |
| How to do | ①First fix the illegal input <br />②Just terminate by an error (like assert) (but not good) <br />③Encode failure in return values (but cumbersome and not always possible) | Write a REQUIRES specification (give the task of checking the input to the caller) |

- Better way: exception handling mechanism
  - Conceptually, normal part and error handling part are separate, but in C++, error handling part could appear in the same function as normal part.
  - **Automatic exception propagation**: prevent from propagating error (most commonly by encoding error in the return value) by myself

- Actually every exception handling can be written in if statements; exception handling mechanism is just a better way to make the code more legible

#### Exception in C++

```C++
int a_function(){
	// somethings before
	try {
		// somethings before
		throw n;
		// somethings after
	}
	catch (int v){
		// respond to the exception
	}
	// somethings after
}
```

- Activities:

  | Activities                   | How to                | Where to write                                  |
  | ---------------------------- | --------------------- | ----------------------------------------------- |
  | Throw an exception (raise)   | using `throw` keyword | <u>Everywhere, either in a try block or not</u> |
  | Catch an exception (respond) | In a `catch` block    | <u>Only after a try block</u>                   |

- Procedure:
  - Throw the exception together with an object, so an exception has a type
  - Find the catch type that match the thrown exception type
  - Exception will be propagated along the calling function (until main); the first catch block **with the same type** (if any) handle it
  - If the exception successfully handled, execution continues normally after the catch block (that handle it)
- Can have multiple catch block after a try block:
  - The last one is called the default handler

## 13 - Abstract Data Types

#### Introduction

- Role of a data type = set of values + set of operations performed on objects of this type
- Motivation to have an abstract data type:
  - Drawback of struct: <u>concrete implementation</u> exposed to all users of the type
  - Drawback of struct: need to remake all the program if the definition of the type is changed
- Abstract data types: **information hiding + encapsulation**
  - Only operations of the object have access to the representation
  - <u>Everyone else should access it through operation</u>

#### Class

- Terminology: class, attributes, methods, instances

- Syntax:

  ```C++
  class ClassName{
      // OVERVIEW: describe the whole class
      // define attributes
      public:
      // define methods; each have a specification
      // (but not all the methods have to be public)
  };
  ```

  - `this` : a **pointer** pointing to this object itself

  - Everything after public is `visible` to others

  - Methods have direct access to the object

    ```C++
    void a_method(int newvalue){
        an_attribute = newValue; // accessing "an_attribute" just like this, no prefix or suffix or other syntax
    }
    ```

- Also passed by value

- About declaring and defining methods in the class

  |                    | Where to write                                         |
  | ------------------ | ------------------------------------------------------ |
  | Method declaration | In the class definition, which is usually in a .h file |
  | Method definition  | Better separately in a .cpp file                       |

  ```C++
  // .h file
  class Class_Name {
  	//...
  	public:
  	void a_function(int value); // declare a function here, but no definition
  };
  
  // .cpp file (that write definition of methods)
  #include <xxx.h> // should have this include
  Class_Name::a_function(int value){
      // write implementation here
  }
  
  // .cpp file that use the type
  #include <xxx.h> // should ALSO have this include
  
  int main(){
      Class_Name an_object;
  }
  ```

  - Can also write the definition in the class definition, but that will exposes that implementation
  - In this way, enough to #include .h file when want to use the class

#### Constructors

- Name same as the class name; no return type

- Syntax

  ```C++
  class IntSet{
      int a,b,c;
      public:
      IntSet(); //declare a constructor with no arguments
  };
  
  IntSet::IntSet(): a(0), b(1), c(2) {} //initialize attribute "a" to be 0, ...
  ```

  <u>Can also write as the following, but not recommended</u>

  ```C++
  IntSet::IntSet(){ 
      a=0;b=1;c=2;
  }
  ```

- The order in which the elements are initialized is **the order they appear in the definition**, not the order in the initialization list

#### Const methods

- Syntax:

  ```C++
  class Class_Name{
  	//..some other implementations
  	public:
  	void a_function(int a) const;
  };
  ```

- Const make the pointer `this` become const: Const methods cannot modify `this`
- A const method can only call other const methods
- A const instance can only call its const methods

## 15 - Subclass, Inheritance, Virtual Functions

#### Subtypes

- Benefits: code reuse

- Principle: **Code written to correctly use type `T` is still correct if uses type `sub_T`**

- Attention: neither int nor long is subtype of the other (if int is used in where long is expected, int will be converted to long)
- Three things can do in a subtype
  - Add more operations
  - Strengthen the postcondition of operations
  - Weaken the precondition of operations (because `Sub_A::function` can handle arguments in a broader range compared to `A::function`, so where `A::function` is expected, can be replaced by `Sub_A::function`)

#### Subtypes in C++: Inheritance (Subclass)

- Syntax:

  ```C++
  class IntSet{ // call BASE class
  	// some attributes
  public:
  	// some methods
  };
  
  class Sub_IntSet: public IntSet{ // called DERIVED class
  
  	// define some new things here
  };
  ```

- Public/private/protected inheritance

  | Public inheritance                                           | Private inheritance                           |
  | ------------------------------------------------------------ | --------------------------------------------- |
  | public in base -> public in derived <br />private in base -> private in derived | all members in base become private in derived |

- In "public" mode, <u>all private members of BASE class are inaccessible to **additional** members of DERIVED class!</u>

  -  Solution: `protected` storage class - can be seen by all members of THIS class and ANY DERIVED classes
  - Fragile

- An ADT (concept and principle) can be implemented as a class (tool in C++), but a class is not always an ADT

- A subtype (concept and principle) can be implemented as subclass (tool in C++), but a subclass is not always an ADT subtype

#### Virtual Functions

- Allow subclass to be used whenever superclass is expected - bring problems

  ```C++
  Sub_IntSet a;
  IntSet* p_a = &a; // p_a is of the type "pointer to IntSet"(apparent type), but actually want "pointer to Sub_IntSet"(actual type)
  IntSet& r_a = a; // r_a is of the type "reference to IntSet"(apparent type), but actually want "reference to Sub_IntSet"(actual type)
  ```

  - By default C++ choose the method to run based on its apparent type
  - So we can modify a through p_a or r_a in a way that is allowed in `IntSet` but not allowed in `Sub_IntSet`

- Force C++ to choose the actual type by using <u>virtual functions</u>
  - Syntax: write `virtual` before the header of the method in the supertype
  - Mechanism:

## 16 - Interfaces and Invariants

#### Interfaces

- Motivation: in a class, data members are still exposed in the class definition

- Solution: base class (abstract base class/virtual base class) (as an interface) and derived class (to do implementation)

- Construction of interfaces

  ```C++
  // The IntSet definition, in a .h file, exposed to users
  class IntSet{
      // no data members
  public:
      virtual void insert(int v) = 0;
      virtual void remove(int v) = 0;
      // ... more...
  };
  
  // The declaration of an access function, in the same *h file, exposed to users
  IntSet *getIntSet(); // return type: pointer to IntSet
  
  // The IntSet_Impl definition, in a .cpp file, not exposed, only compile to an object code (.o) for the user
  class IntSetImpl : public IntSet{
      int elts[100];
      int numElts;
  public:
      IntSetImpl(); // must have a constructor (but this is invisible to user)
      void insert(int v);
      void remove(int v);
      // ... more...
  };
  
  // The definition of an access function, in the same .cpp file, ont exposed
  Int *getIntSet(){
      static IntSetImpl impl;
      return &impl;  
  } // call of getIntSet actually return a subtype of IntSet (i.e. IntSetImpl)
  ```

- Use the interfaces

  ```C++
  IntSet *s = getIntSet();
  // apparent type of s: pointer to IntSet
  // actual type of s: pointer to IntSetImpl
  s->insert(1);
  // originally want to call the method in the apparent type: IntSet::insert()
  // but IntSet::insert() is virtual, so finally turn to call: IntSetImpl::insert()
  ```

#### Invariant of an ADT

- Invariant: a set of conditions that must be true at well-defined points 

- Representation invariants should hold immediately before exiting each method of the implementation
- Write functions (can be a private method) to check for invariants

## 17/19 - Dynamic Memory Allocation

#### Dynamic Memory Allocation

- Three types of variables:

  | Global variables                 | Local variables                                        | Dynamic variables                         |
  | -------------------------------- | ------------------------------------------------------ | ----------------------------------------- |
  | Space reserved at compile time   | Space set aside at runtime (when the block is entered) | Space set aside when programmer specifies |
  | Size known to compiler           | Size known to compiler                                 | Size specified by programmer              |
  | Destroyed when program completes | Destroyed when leaving the block                       | Destroyed when the programmer instructs   |

- Create and destroy dynamic objects through dynamic storage management

  - Allocate space by `new`
    - `int *ip = new int`
    - `int *ip = new int(5)` to initialize
    - Will call the constructor if it is a class
  - Deallocate space by `delete`
    - `delete ip`
    - <u>Need destructor if it is a class where its methods (including the constructor) allocate dynamic storage</u>

- `new` and `delete` always appear in pairs

- Be aware of losing the access to the variables - <u>memory leak</u>

  - Memory works like this: TODO 图片

- Create and destroy dynamic arrays

  - Allocate space by `int *ia = new int[size]`; the size can be a variable
  - Deallocate space by `delete[] ia`

  ```C++
  class IntSet(){
      int *elements; // the array, but dynamically created;
      int sizeElts; // must have such a member to record the capacity
      int numElts; 
  public:
      IntSet(int size = MAXELTS); // the default argument only needs to appear in the declaration, no need in the definition signature
      // others...
  }
  
  IntSet::IntSet(int size): elements(new int[size]), sizeElts(size), numElts(0) {}
  ```

#### Destructors

- Syntax:

  ```C++
  IntSet::~IntSet(){
  	delete[] elts; // only need to deal with data members that are dynamically allocated
  }
  ```

- The destructor is called automatically when the block ends

#### Dynamic Resizing

- Goal: enlarge the capacity of the array in `IntSet` during the program
- Realization: define a `grow()` method
  - Allocate a bigger array - Copy the smaller array to the bigger one - Destroy the smaller array - Modify `numElts` & `sizeElts`
- Function complexity

## 18- Deep Copy

#### Shallow Copy vs. Deep Copy

- In shallow copy, if an array A is (shallow) copied to B, only the address is copied. Still share the same contents in the memory
  - When talking about array itself we say "it is by default called by reference when it is passed to an argument of a function", but this is actually call-by-value, but only due to the result of shallow copy
  - When an array is in a class and a class instance is passed (by value), problem occurs
  
- Problems of shallow copy:
  - Not "absolutely" copied; the original "source object" may still be modified
  
  - Pass an argument to a function by shallow copy -> the dynamically allocated array in that argument is the same array of the source object -> when the function completes, the dynamic array is automatically destroyed (<u>i.e. the array in the source object is destroyed!</u>)
  
    ```C++
    void foo(IntSet x){
    }
    int main(){
    	IntSet s;
    	s.insert(5);
    	foo(s); // after executing this function, s is destroyed
    	s.query(5); // error, because the data array in the IntSet s does not exist
    }
    ```
  
- Deep copy: full copy of everything

#### Implementing Deep Copy

- Implement deep copy by redefining the **copy constructor** & redefining the **assignment operator**

  - Copy constructor: create a new object
    - There is a default (shallow copy) copy constructor in every class we define - we want to rewrite it explicitly
    - Called when passing arguments by value to a function
  - Assignment operator: copy to an existing object

- Tasks of copy constructor and assignment operator

  - Step 1: Allocate space of the same size (only copy constructor)
  - Step 2: copy each element from the source to the destination (<u>both copy constructor & assignment operator - so define a private function for this step for better code reuse</u>)

- Abstract the copying process

  ```C++
  void IntSet::copyFrom(const IntSet &is){
  	// if size different, destroy and reallocate
      if (is.sizeElts != sizeElts){
          delete[] elts;
          sizeElts = is.sizeElts;
          elts = new int[sizeElts];
      }
  	// copy array
      for (int i=0; i< is.sizeElts; i++)
          elts[i] = is.elts[i];
      numElts = is.numElts;
  }
  ```

- Redefine the copy constructor

  ```C++
  IntSet(const IntSet &is); // must call by reference
  IntSet::IntSet(const IntSet &is): elts(nullptr), sizeElts(0), numElts(0){
      copyFrom(is); // copyFrom is already a private member function
  }
  ```

  - Versus the default (implemented implicitly) shallow copy constructor

    ```C++
    IntSet::IntSet(const IntSet &is): elts(is.elts), sizeElts(is.sizeElts), numElts(is.numElts) {}
    ```

- Redefine the assignment operator

  ```C++
  IntSet &IntSet::operator= (const IntSet &is){
  	copyFrom(is);
  	return *this;
      // "this" is a pointer pointing to the instance itself (who call this method)
  }
  ```

  - More about assignment operator `=`:
    - Return a value that is the **reference** to the left-hand-side object
    - Binds right-to-left

#### Rule of Big Three

- If there is any dynamically allocated storage in a class, must provide:
  - A destructor
  - A copy constructor
  - An assignment operator
- What to consider in the Big Three
  - The invariants hold
  - All dynamic resources are accounted for

## 20 - Linked List

#### Implementation of Linked List

- Class definition

  ```C++
  struct node{
    node *next;
    int value;
  }
  class IntList{
    node *first;
  public:
    int getSize();
    bool isEmpty();
  	void insert(int v); // insert into the front of the list
  	int remove(); // remove and return the firs telement
  }
  ```

- Method implementation

  - `getSize()` must traverse the list

    ```C++
    int IntList::getSize(){
    	int count = 0;
    	node *current = first;
    	while(current){
    		count++; // first increment for the node ew are at
    		current = current->next; // then jump
    	}
    }
    ```

  - Insert(): the declaration of new node **must be dynamic**, <u>otherwise it will disappear when the function exits</u>

    ```c++
    vodi IntList::insert(int v){
      node *np = new node; // must be dynamic
      np->value = v;
      np-> next = first;
      first = np;
    }
    ```

  - Remove()

    ```c++
    int IntLlist::remove(){
    	node *victim = first;
    	int result;
    	if (isEMpty()){
    		listIsEmpty e;
    		throw e;
    	}
    	first = victim->next;
    	result = victim->value; // store the result to return, before deleting the node
    	delete victim; // must manually call delete since all the nodes are dynamically allocated
    	return result;
    }
    ```

- Maintenance methods:

  - Default constructor

    ```C++
    IntList::IntList(): first(nullptr){}
    ```

  - Destructor

    ```C++
    IntList::~IntList(){
    	while (!isEmpty()) // remove all the nodes
    		remove();
    }
    ```

  - Copy constructor: cope with "walking backward"

    ```C++
    void IntList::copyList(node *list){
    	// this recursion realizes "copying backward"
    	if (!list) return;
    	copyList(list->next);
    	insert(list->value);
    }
    
    IntList::IntList(const IntList &l): first(nullptr){
    	copyList(l.first);
    }
    IntLIst &IntList::operator=(const INtLIst &l){
      if (this != &l){ // ensure that there is no self-assignment (otherwise do nothing)
        removeAll();
        copyList(l.first);
      }
      return *this;
    }
    ```

#### Double-Ended Linked Lists

- New class definition

  ```c++
  class IntList{
  	node *first;
  	node *last;
  public:
  	// ...
  }
  
  IntList::insertLast(int v){
    node *np = new node;
    np->value = v;
    np->next = nullptr;
    if (isEmpty()){ // discuss the two cases
      first = last = np;
    } else{
      last->next = np;
      last = np;
    }
  }
  ```

## 21 - Template & Container

#### Templates

- Motivation: polymorphism / polymorphic code: reusing code for different types

- Templates: make the type as a compile-time parameter

  ```C++
  template <class T> // T: name of the type
  class List{
  public:
  	// some methods
  private:
  	struct node{ // define the node here, since the definition of node also relies on T; here node type only available to class methods
  		node *next;
  		T v;
  	};
  }
  ```

- Writing method functions headers

  ```C++
  template <class T>
  void List<T>::isEmpty(); // a member function
  List<T>::List(); // default constructor
  List<T>::List(const List<T> &l); // copy constructor
  List<T>::~List(); // destructor
  List<T> &List<T>::operator=(const List<T> &l); // assignment op
  ```

- Function definition must be in the .h file (exposed to the user)

- Use templates: `List<int> li` (specify the type when creating the object)

#### Container of Pointers

- Motivation: avoid multiple-times copying in inserting and removing - container only stores the pointer to the data

  ```C++
  struct node{
  	node *next;
  	BigThing *value;
  }
  void insert(BigThing *v);
  BigThing *remove();
  ```

- In practice, define a template on pointer and define `List<BigThing> ls`

  ```C++
  template <class T> // When using, T is just a class, not pointer to class
  class A{
  private:
  	T *object;
  public:
  	void insert(T * v);
  }
  ```

- At-most-once invariant: any object can be linked to at most one container through pointer at any time

- Three rules of use:

  - **Existence**: an object must be <u>dynamically allocated</u> before a pointer to it is <u>inserted</u>
  - **Ownership**: once a pointer to an object is <u>inserted</u>, no one else may use or modify it in any way
  - **Conservation**: when a pointer is <u>removed</u> from a container, either the pointer must be inserted into some other container, or its referent must be deleted
    - Implication: when a container is destroyed, the objects contained in the container should also be deleted
      - Be careful in [the destructor] & [the assignment operator], both of whom can destroy a container

- Not full deep copy (does copy all the nodes, but the nodes are also containing pointers)

  <img src="/Users/led/Library/Application Support/typora-user-images/截屏2023-07-16 22.11.53.png" alt="截屏2023-07-16 22.11.53" style="zoom:50%;" />

  - Solution

    ```C++
    template <class T>
    void List<T>::copyList(node *list){
    	if (!list) return;
    	copyList(list->next);
    	T *o = new T(*list->value); // CRITICAL STATEMENT: create a new object, as opposed to using the original one
    	insert(o);
    }
    ```

#### Polymorphic Container

- Motivation: make the container to be able to hold more than one type within one object

- How to do

  ```C++
  class Object{
  public:
  	virtual ~Object() {}
  }
  
  class BigThing: public Object{
  // ...
  }
  
  class List{
  //...
  public:
  	void insert(Object *o);
  	Object *remove();
  }
  
  int main(){
  	// ...
  	BigThing *bp = new BigThing;
  	list.insert(bp); // BigThing is also of an Object type
  
  }
  ```

- dynamic_cast<>

## 22 - Operator Overloading

#### Rules of Operator Overloading

- We are able to redefine operators when (and <u>ONLY WHEN</u>) applied to objects of <u>class type</u>

  ```C++
  char operator+(char a, char b){
  } // ERROR
  ```

  非成员运算符要求类类型或枚举类型的参数C/C++(898)

  overloaded 'operator+' must have at least one parameter of class or enumeration type

- Can de defined as nonmember function or member function

  - Overloaded operator that is a member of a class appears to have one fewer parameter (`this` is automatically the first operand)

- Should return a reference of a type

#### Examples of Operator Overloading

- `operator+`: binary
- `operator-`: binary

- `operator[]`:

  - Binary (<u>1st operand is the object, 2nd operand is index</u>)

  - const version vs non-const version

    ```C++
    const int &IntSet::operator[](int i) const;
    
    int &IntSet::operator[](int i);
    ```

    - Both are needed
    - Call non-const version when both versions are OK

- `operator<<`

  - Parameters: (ostream **&**, **const &** of the object), return: ostream**&** of the original parameter

  ```C++
  ostream &operator<<(ostream &os, const IntSet & is){
  	for (int i = 0; i < is.size(); i++)
      	os << is[i] << " ";
  	return os;
  }
  ```

  - Here non-const version of `operator[]` is called

- `operator>>`

  - Parameters: (istream **&**, **&** of the object (**NON-const**)), return: stream**&** of the original parameter

  ```C++
  istream &operator>>(istream &is, IntSet & is){
  	//...
  	return is;
  }
  ```

#### Friendship

- Motivation: give access of private members of a class to <u>a non-member function / another class</u>

- Syntax:

  ```C++
  class A{
    friend void outsideFunction(); // expose to a function
    friend class B; // expose to a class: objects of class B can access private members of A
    private: //...
    public: //...
  };
  void outsideFunction();
  class B{
  };
  ```

## 23/24 - Linear List

- Linear list: a collection of objects, duplicates possible, insertion by position

#### Stack

- Principle of stack: **LIFO** - last in first out

- Implementation:

  ```C++
  int size();
  bool isEmpty();
  void push(Object o);
  void pop();
  Object &top(); // return a reference to the top element
  ```

- Comparison between array & linked list implementation

  |      | Array                               | Linked list (singly-linked)             |
  | ---- | ----------------------------------- | --------------------------------------- |
  | Pros | Time-efficient (for size operation) | Memory-efficient                        |
  | Cons | Not memory-efficient                | Not time-efficient (for size operation) |

- Application

#### Queue

- Principle of queue: **FIFO** - first in first out

- Implementation:

  ```C++
  int size();
  bool isEmpty();
  void enqueue(Object o);
  void dequeue();
  Object &front();
  Object &rear();
  ```

  - Using linked list (**doubly-linked**)
  - Using arrays - **circular arrays** to avoid memory waste
    - `front = (front+1)%maxsize`, `rear = (rear+1)%maxsize`
    - Need a flag indicating empty or full to distinguish the two status

- Application: lee's algorithm

- **Deque**: double-ended queue

- Up to now: three types of linear list (stack, queue, deque) <u>(array & linked-list are only ways of implementing them)</u>

## 25/26 - Standard Template Library

#### STL

- Standard template library (**STL**)

  - Containers

    - Sequential containers (**vector, deque, list**)

    - Associative containers: store elements based on the values (**map, set**)
    - Container adapters (**stack, queue, priority_queue**)
  - Iterators
  - Algorithms

#### Sequential Containers

|                | vector                      | deque                   | list                                |
| -------------- | --------------------------- | ----------------------- | ----------------------------------- |
| Implementation | Implemented with arrays     | Implemented with arrays | Implemented with doubly-linked list |
| Access         | Random access (fast)        | Random access (fast)    | **Only bidirectional**              |
| Insert/remove  | Any point (slow if not end) | **Only front & back**   | Any point (fast)                    |

##### Vector

- `#include <vector>`

- Initialization

  ```C++
  vector<int> v; // create an empty vector
  vector<int> v(v2); // copy constructor
  vector<int> v(n,t); // n elements, each with value t
  
  vector<int> v(it1, it2); // copy from the range [it1, it2)
  // including it1, not including it2
  
  int c = {1,2,3,4,5,6};
  vector<int> v(c+1,c+5); // copy from an array
  
  v1 = v2; // deep copy
  ```

- Important methods

  - `v.size()`: num of elem

    - Return `vector<T>::size_type` (a companion type), <u>can be converted into `unsigned int`, don't converted to `int`</u>

    - Better to call .size() everytime need a size

      ```C++
      vector<int>::size_type ix;
      for (ix = 0; ix != ivec.size(); i++){
      	//...
      }
      ```

  - `v.empty()`

  - `v.push_back(T t)`: push at the end

    - Container elements are copies

  - `v.pop_back()` (no return)

  - `v[i]`: subscription allowed

  - `v.clear()`
  
  - `v.front()`, `v.end()`: return a <u>reference</u> to the first/last element
  
  - `v.insert(iterator it, T t)`: insert elem with value t **before** the element pointed by it
  
  - `v.erase(iterator it)`: remove element pointed by it
  
    - Return an iterator pointing to the element **after the one deleted**

##### Iterator

- <u>Each container type has a companion iterator type</u>

  - But not all have subscripting

- Declaration: `vector<int>::iterator it;`

- Essentially **pointers**

- Linking an iterator to a container

  - `v.begin()` returns an iterator pointing to the 1st element
  - `v.end()` returns an iterator pointing to **1 + the last position**
  - `v.begin()==v.end()` if empty

- Operations of iterators (<u>except iterator returned by `end()`</u>)

  - `*it`: dereference to access the element
  - `it++, it--`: go to the next item/previous item

- Looping based on iterators

  ```C++
  vector<int> ivec;
  vector<int>::iterator it;
  for (it = ivec.begin(); it != ivec.end(); it++){
  	//..
  }
  for (int v : ivec){
  	//..
  }
  ```

- const_iterator: cannot be used to change values
  - Declaration: `vector<int>::const_iterator it;`

- Some containers supports iterator arithmetic & relational operation (e.g. vector, NOT list)
  - iter+n, iter-n
  - iter1 < iter2 (iterators must refer to elements in the same container)

##### Deque and List

|                               | Deque                                                        | List                                                         |
| ----------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| To use it                     | `#include<deque>`                                            | `#includ<list>`                                              |
| Subscripting                  | Y                                                            | **N**                                                        |
| `insert()`, `remove()`        | Y                                                            | Y                                                            |
| Operations on Iterators       | Y                                                            | Y                                                            |
| Iterator Arithmetic           | Y                                                            | **N**                                                        |
| Iterator relational operation | Y                                                            | **N**                                                        |
| Additive funtionalities       | Insert & remove at beginning<br />`d.push_front(T t)`<br />`d.pop_front()` | Insert & remove at beginning<br />`l.push_front(T t)`<br />`l.pop_front()` |

##### Selecting Container

- Use `vector`
- Random access: `vector` or `deque`
- Insertion/deletion in the middle: `list`
- Insertion/ deletion never in the middle: `deque`

#### Container Adapters

- Container adapter: takes an existing container type and makes it act like a different type

- Initialize a container

  - Stack and queue are both implemented using deque (the default container the initialize)

    `queue<int> deq(c); //c must be a deque `

  - Or specify the underlying container

    `stack<int,vector<int>> stk(ivec); // here ivec is vector type`

- Operations
  - Stack: empty(), size(), pop(), push(), top()
  - Queue: empty(), size(), pop(), push(), front(), back()

#### Associative Containers

- Map: (key, value) pairs, an implementation of dictionary'

- Set: only key, a collection of distinct values

##### `map`

- Dictionary: a collection of (key, value) pairs
- `#include <map>`
- Initialization: `map<key_type, value_type> m;`

- Pair type

  - p.first, p.second, return the <u>reference</u> to the first/second member
  - Construct a pair: `pair<type_1, type_2> p = make_pair(value1, value2)`

- Key in an element of map are not changeable

- Adding elements to a map

  - Using subscript operator

    - Add if not in
    - 0 by default

  - Using the insert member

    ```C++
    map<string, int> word_count;
    while (cin >> word){
      pair<map<string,int>::iterator, bool> result = word_count.insert(make_pair(word, 1));
      if (!ret.second) // word already in word_count
        ++ret.first->second;
    
    }
    ```

