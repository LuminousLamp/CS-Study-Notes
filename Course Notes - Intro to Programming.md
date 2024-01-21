# Summary of VG101 C



## Lecture 8

### Start with C

1) Library inclusions

​		Format: include <libraryname.h>

​		`<Stdio>`: std librarys in ANSI C

2. Main function

3. Two basic statement

​		`Printf`

​		`Scanf`: `&` is needed to accept the input value

### Data types

##### Char

ASCII (as-key)

##### Integer

length depends on platform

`int` can be 32 bits in modern computers

##### Floating-point data

1) (use double if not limited by storage)

2) Floating-point data and printf

| %f             | %e          | %g(general)              |
| -------------- | ----------- | ------------------------ |
| floating-point | exponential | Use whichever is shorter |

3) Floating-point data and scanf

| %f                                        | %e %g               |
| ----------------------------------------- | ------------------- |
| If the target variable is double, use %lf | Identi1111cal to %f |

### Expressions

Expression = terms + operators

##### Terms

1. Types: constant / variable / function call

2. Variables: Mustn’t be a keyword; Must be declared

##### Assignment statements

Variable = expression

A = b = 1 <--> a = ( b = 1);

##### Operators

1) Remainder : `%`

2) Shorthand assignment operators: `+=` `-=` `*=` `/=`

3) Increment and decrement: Difference between `x++` and `++x`

##### Type conversion

1. Implicit type conversion

2. Explicit type conversion

##### If and switch statements

1. Some operators

   Relational operators

   `!=` not equal to

   Logical operators

| operator    | !    | &&   | \|\| |
| ----------- | ---- | ---- | ---- |
| description | not  | and  | or   |

​		`?:` operator

![img](file:///C:\Users\LED\AppData\Local\Temp\ksohtml20588\wps1.jpg) 

2. Statements

   `If` statement

   No `elseif`, but can use `else if`

   `Switch` statement



## Lecture 10

### Array

##### Basis

1. Specify both type and name and size (square bracket)

2. Memory: 1 `char` - 1 byte; 1 `int` - 4 bytes
3. Size definition: `string[N]` N must be a constant

##### Array elements

1. Numbering: numbered form 0
2. Accessing: can be used as left-value and right-value eg. i = `value[1] + 2;`  `value[1]++;`

3. Indexing

##### Array as function argument

1. Use an array name itself when passing an array as an argument
2. Array passed as an **address**: accessing/changing array in the function actually accesses/changes values in main
3. Array in function declaration: ignore the size specified (so a better way to take its size together is to take its size as a separate parameter)

### String and Characters

##### Characters

1. 2 types: Printing characters; Special characters - denoted by backslash

2. Character arithmetic

   be treated as integer;

   determine if  character is digit: `ch >= ‘0’ and ch <= ‘9’`

   convert a digit character to integer: `ch - '0'`

##### C-style string

1. Reading strings:

   `scanf("%s", string)`: cannot handle **spaces**

   `gets(string)`: accept a string **without** "\n"

   `fgets(string)`: accept a string with "\n"

   `getchar(string)`: accept any kind of string

##### Relevant functions

1) Get the length: `size_t strlen`

2. Create

   `char * strcpy`: copy the string (including terminating null character)

   `char * strncpy`: syntax - `(char *destination, const char * source, size_t num)`; copy the first *num* characters. (If the end of source if found before *num* characters have been copied, destination is padded with `"\0"`)

   `char * strcat`: appends a copy of the source string to the destination. (terminating `\0` character in destination is overwritten by the 1st character of source)

3. Compare 2 strings: `int strcmp`

​		Principle: compare characters until characters differ or until a terminating `\0` is reached.

​		Returns value: **0** - equal; **1** - str1 > str2; **-1** - str1 < str2

4. Find

   `char * strchr`: return to a **pointer** to the first occurrence of character

   ```
   int main() {
       char str[] = "This is a sample string";
       char *pch;
       printf ("Looking for the 's' character in \"%s\"...\n", str);
       pch=strchr(str,'s');
       while (pch!=NULL) {
           printf ("found at %d\n",pch-str+1);
           pch=strchr(pch+1,'s');
       }
       return 0;
   }
   ```

   `char * strstr`: returns a pointer to the first occurrence of str2 in str1 (null if not found)

5. Convert string into numbers

   `int atoi`: string 2 integer

   `double atof` / `double strtod`: string 2 double

   `long atol`: string 2 long integer



## Lecture 11

### Scope of Variable

1. Scope of a variable: declaration -> the end of the block that contains the variable
2. Can declare a variable with the same name in non-nesting blocks
3. Global variables: not recommended 
4. Static local variables: retain the variable until the program end; use keyword `static`

### Algorithms

2 criteria for an algorithm: how much **memory** used & what **time** does it take

### Recursion

Function: reducing a large problem into smaller problems of the same form.



## Lecture 12

### Pointer

##### Introduction

1. Memory **addresses** treated as data
2. Allows **shared** access to data

<img src="C:\Users\LED\AppData\Roaming\Typora\typora-user-images\image-20211105133846840.png" alt="image-20211105133846840" style="zoom:33%;" />

##### Basic Usage

1. Declaration

   Syntax: `int *px`(**recommended**) or `int* px`

   Be careful: `int* px,py` here `py` is not a pointer

   Conventionally use "p" to show the variable is a pointer (not mandatory)

2. Assigning an address

   Syntax: `px = &x` 

   `&`: **"address of"** **operator** (the operand of `&` must be **left-value** )

   Cannot assign an address as a constant (we never know the address names, and no need to know)

   Can assign a different address later

3. Pointer types

   `px` and `&x` are pointer to T [T= `int` / `char` / `double` ...]

4. Accessing a location

   Syntax

   ```c
   int x = 2, *px, y;
   px = &x; //px is the address of x
   y = *px; //(y = the value pointed to by px), y and *px both have the type of int
   ```

   `*`: **"deference" operator** (different from the star in pointer declaration)

   ​	`*px = 3` is the same as `y = 3`

5. Address Operation

   ```c
   int m, *px, *iptr;
   px = &m;
   iptr = px;//the address of m is shared by 2 pointers.
   ```

6. Initialization

   An un-initialized pointer can point to anywhere

   `NULL` can be used to initialize

##### Pointer as Function Argument

### File I/O

##### File

Text file vs. binary file

##### C-style File Operations

1. File I/O

   Define a file pointer: `FILE *fp` (FILE is a **type** (structure type) define in <stdio.h>)

   ​	`fp` is called a **stream**

   Open: `fp = fopen( const char * filename, const char * mode );`

   ​	Return `NULL` if failed

   Close: `int fclose(FILE * stream)`

2. Functions

   `fprintf` / `fscanf` / `rewind`



## Lecture 13

### Dynamic Storage Allocation

Premise: memory is stored in a linear way

##### Static memory allocation

```c
#define N 2*1024*1024
int main() {
    char arr[N];
    arr[N-1] = 'a';
    printf("arr[N-1] = %c\n",arr[N-1]);
    return 0;
}
```

the Stack: where local variables are allocated

​	size is limited  -  stack overflow

##### Dynamic storage allocation

1. the Heap: used for dynamic allocation

2. Example:

```c
char *cp = malloc(10);
char *cp = (char*)malloc(10);//need explicit type conversion
int *arr = (int*)malloc(10*sizeof(int));
free(arr);
```

​		`malloc`: the parameter is the **number of bytes** you need; return a pointer pointing to allocated memory

​		`free`: the storage must be freed manually

### Structure and Database

##### Structure

1. Concept

| Field 1 | Field 2 | Field 3 | Field 4 |
| ------- | ------- | ------- | ------- |
| record1 | record1 | record1 | record1 |
| record2 | record2 | record2 | record2 |
| ...     | ...     | ...     | ...     |

​	Kinds of type: built-in type (`char`, `int`, `double` ...) vs. user-defined type

2. Definition

   Method 1

```C
struct StructureName{
	//field-declaration
	char name[100];
    int number;
    //...
}
```

​		Method 2

```C
typedef struct{//typedef is a keyword
	//field-declaration
	char name[100];
    int number;
    //...
} StructureName;
```

3. Declaration

​		Method 1: `Struct StructureName VariableName`

​		Method 2: `StructureName VariableName`

4. Field assignment: use **dot**

```c
typedef strcut{
	double x,y;
}point;
point pt;
pt.x = 1; pt.y = 2;//pt1:(1,2)
```

##### Database

1. Order: from the bottom up

2. Construction - example:

   <img src="A:\JI 2021 FA\VG101\Other Materials\Picture\Lecture13.jpg" alt="Lecture13" style="zoom:50%;" />

​	Can store pointer to objects



## Lecture 14

### More about Array

##### 2D Array

1. Initialization: use 2 for-loops
2. C is row-major (while MATLAB is column-major)

##### Array and Pointer

1. Concept

​	The name of an array == the address of the 1st element in the array

​	Array name is a reference to the list of variables

​	**Pointer constant**, cannot be changed

```c
char array[10];
array <==> &array[0];
int y = 1;
array = &y;//this is NOT VALID!
```

2. Accessing an array

   `array[k]` <==> `*(array + k)` (k is a constant)

   `&array[k]` <==> `array+k`

​	The compiler **multiplies** the integer by **the size of the base type** of the pointer, and added to the address that the pointer is pointing to. The value of `(array+n)` is `array+n*(sizeof(char))`.

### Searching and Sorting

1. Searching: Linear searching vs. Binary searching (based on sorting)

2. Sorting: Selection sort



## Lecture 15

### Preprocessor Directives

1. Transform the program before actual compilation (can simply be regarded as text substitution)

2. Important directives

   | #include                 | #define #undef                | #ifdef #ifndef #endif                | #if #else #elif #endif |
   | ------------------------ | ----------------------------- | ------------------------------------ | ---------------------- |
   | to include a source file | to define or undefine a macro | to judge if a macro has been defined | condition statement    |

### Library and Interface

1. Terminologies: library - contain **constants** & **functions**

   interface: boundary between the library and the programs that use that library

2. Example the interface

   Main file (**client**, use a certain fucntion)

   ```c
   #include "arithmetic.h"
   int main(){
   	isprime(number);
   }
   ```

   Header file (interface)

   ```c
   //arithmetic.h
   #ifndef_ARITHMETIC_H_
   #define_ARITHMETIC_H_
   int isprime(int number)
   #endif
   ```

   Implementation file

   ```c
   //arithmetic.c
   #include "arithmetic.h"
   int isprime(int number){
       ...//write the function
   }
   ```

### Random Numbers

Computers are deterministic - can only generate pseudo-random numbers

##### Generation

1. Seed

   acquire seed:`void srand(unsigned int seed)` (usually `srand(time(NULL))`)

   ​	`time_t time(time_t* timer)` get the current time

   ​		time_t == long long int

2. Range control

   Generating a random integer between [a,b]: `rand() % (b-a) + a`

   Generating a random double between [a,b]: `( rand() / (double)RAND_MAX ) * ( b - a ) + a`

##### Example

1. toss coin

```C
int i, n;
srand(time(NULL));
for (i = n = 0; i < N; ++i){
    if (rand() < RAND_MAX/2) n++;
}
printf("The probability to get HEAD is %f\n", (double)n / N);
```

2. calculate PI

```c
double x, y;
int i, M;

srand(time(NULL));
// 1: rand experiment
for (i = M = 0; i < N; ++i){
    // 1.1: generate a random point (x, y) in the square (0,0)-(1,1)
    x = rand() / (double)RAND_MAX;
    y = rand() / (double)RAND_MAX;
    // 1.2: if (x, y) is in the fan, M++
    if (x*x + y*y < 1) M++;
}
// 3: calculate PI
printf("PI = %f\n", 4.0 * M / N);
```



## Lecture 16

### Some Projects for Practice

##### Projects Related to Integer Calculation

1. Armstrong Number

   ```c
   int n, a, b, c;
   for (n = 100; n < 1000; ++n){
       a = n%10;
       b = (n/10)%10;
       c = n/100;
       if (n == a*a*a + b*b*b + c*c*c)
       printf("%d\n", n);
   }
   ```

   **More**: if a integer number T has N digit, the Mth digit can be obtained using the formula

   `t(M) = (T / pow(10,N-M)) % 10`

2. Dividing a number into cash

   ```c
   int n = 256, a, b, c, d, e;
   
   a = n/100;// number of 100 bill
   n -= a*100; // update n
   b = n/50;// number of 50 bill
   n -= b*50; // update n
   c = n/20;// number of 20 bill
   n -= c*20; // update n
   d = n/10;// number of 10 bill
   n -= d*10; // update n
   e = n/5;// number of 5 bill
   n -= e*5; // update n
   printf("n = %d * 100 + %d * 50 + %d * 20 + %d * 10 + %d * 5 + %d * 1\n", a, b, c, d, e, n);
   ```

##### Projects Related to Algorithm

1. Palindrome

   ```c
   int is_palindrome_helper(char* s, int begin, int end){
       if (begin>=end) return 1;
       if (s[begin] == s[end]) return is_palindrome_helper(char* s, begin+1, end-1);
       else return 0; 
   }
   int is_palindrome (char* s){ return is_palindrome_helper(s, 0, strlen(s)-1);}
   ```

### Some More

The first character of a variable can be a hyphen.



# Summary of VG101 C++

[TOC]

## Lecture 17

### C++ Style

##### Header File Including

1. without "`.h`"

2. widely used standard libraries: `cstdio`, `iostream`, `string`, `vector`

##### Namespace

1. Purpose: solve the problem of **name confliction** - to allow two or more functions / class / variables to have the same name (but must have different implementation)

2. Usage

   ```c++
   namespace namespace_name1 {
   	void func(){cout << "Inside first_space" << endl;}
   }
   namespace second_space{
   	void func(){cout <<"Inside second_space" <<endl;}
   }
   using namespace second_space;
   int main() {
   	first_space::func();//调用第一个命名空间中的函数
   	func();//调用第一个命名空间中的函数
       ...
   ```

   1. Operator `::` - **scope operator**

      to indicate which **namespace / class** does the variable / function belongs to

   2. Keyword `using namespace`: allow all members in a certain namespace to be used directly without adding the name of the namespace in front of it.

      > Attention: on the example above, cannot `using` both namespace at the same time, because that will make the name `func` conflict again.


##### Overloading

1. Function overloading

   Concept: to declare functions with the same name and having similar effects (but the formal parameters must be different)

   Implementation: when using, the compiler will compare the parameters used with the parameters of all those functions and select the most suitable overloading function (called **overloading decision**)

   ```c++
   class printData{
   public:
   	void print(int i) {cout << "整数为: " << i << endl;}
   	void print(double  f) {cout << "浮点数为: " << f << endl;}
       void print(char c[]) {cout << "字符串为: " << c << endl;}
   };
   int main(void){
      printData pd;
      pd.print(5);// 输出整数
      pd.print(500.263);// 输出浮点数
      char c[] = "Hello C++";// 输出字符串
      pd.print(c);
      return 0;
   }
   ```

2. Operator overloading

   Concept: **redefine** most of the built-in operators (not all)

   Usage:

   ```c++
   class box{
   public:
       Box operator+(Box b){//重载+运算符，定义为下面的操作
           //...write implementation
       }
   }
   ```

##### simple I/O

1. `cin` and `cout`

   1. Usage: `cin >> variable_name;`  `cout << variable_name/string_in_quotes << ... << endl;`

   2. Concept: `cin` and `cout` are **objects** in the **class** `ostream` in **library** `<iostream>`

2. `<<` and `>>` : output / input operators 

   Bit operations; cannot be nested

   `cin >>` and `cout <<` can be simply regarded as functions which return a "cout that has been added something" / a "cin with something input"

### Class & Object

##### Class

```c++
class Box{
public：//access specifiers访问修饰符
    double length, breadth, height;//member variables
    void func(int var1, string var2 ...){//member functions(methods)
        //implementation
    }
};//semicolon
```

1. Concept: similar to "structure" in C, can be treated as a specific datatype

2. Declaration: begin with keyword `class` then the name of the class; end with a semicolon

3. Members

   1. Member variables

   2. Member functions: two ways to declare

      ①declare inside the declaration of class

      ②declare outside, with scope operator `::`

      ```c++
      class Box{
      public:
      	double getVolume();//just a prototype
      };
      double Box::getVolume() {return length * breadth * height;}
      ```

4. Access specifiers: private (default) / public / protected

##### Using Class: Object

```c++
int main(){
    Box onebox;//declare a variable "onebox" with the type "Box"
    onebox = {.length = 1.0, .breadth = 2.0};//assignment
    onebox.height = 3.0;//assignment
    onebox.func(var1,var2);//using a member function in the class
}
```

1. Usage: declare the variable as normal; assigning the value similarly as "structure"
2. Access the member of the variable with  `.`  (member accessing operator)

### The String & Vector class

##### `String`

1. Included in the library `<string>`

2. Treated as a variable

   ```c++
   string str;
   str = "hello";
   ```

##### `Vector`

1. Concept: a **template**, can be applied to different types; included in `<vector>`

2. Usage

   Declaration: `vector<datatype> vector_name;`

   Add a new element on the end: function `push_back(element)`

   Access the element: `vector_name[i]`



## Lecture 18

### Constant and Inline Function

1. Constant

   Can still use `#define NAME value`, but **not suggested**

   use `const type_name var_name = value;`

2. Macro

   implemented with the preprocessor, not the compiler processing

   Simply **replaces** all macro calls **directly** with the macro code - has problems

   ```c++
   #define F(x) x*x + x 
   int main() {
   	cout << F(2) << endl;
       return 0;//output:2*2+2*2 = 8, not 12
   }
   //if wanting to get 12,
   #define F(x) (x*x + x)
   int main() {
   	cout << F(2) << endl;
       return 0;//output:12
   }
   ```

3. Inline functions

   Only different from normal functions: act like a preprocessor macro

### Text Files

##### Read and Write Files

`#include <fstream>`

1. Method 1

   Open file for reading: `ifstream` object (`ifstream` is a datatype), behaves like `cin`

   Create file for writing: `ofstream` object (`ofstream` is a datatype), behaves like `cout`

   > Something more: the datatype of objects `cin` and `cout` is `ifstream` and  `ostream` respectively (but we're not allowed to create an object with datatype `ifstream` and `ostream`)

   Example:

   ```c++
   int main() {
       int a;
       double b;
       ifstream in("data.txt");
       ofstream out("output.txt");
       in >> a >> b;
       out << b << a << endl;
   ```

2. Method 2

   ```c++
   ifstream in;
   in.open("data.txt");
   ```

##### `getline` function

1. `getline()` function in `<string>` , as a normal function (**suggested**)

   Format:

   ```c++
   istream& getline (istream&  is, string& str, char delim);
   istream& getline (istream&& is, string& str, char delim);
   istream& getline (istream&  is, string& str);
   istream& getline (istream&& is, string& str);
   ```

   `is`: represents an input stream (such as `cin` / an `ifstream` object)

   `str`: a string object, to store the information

   `delim`: a variable of datatype char, a **cut-off** character; if not declared, **terminated by '`\n`'**(and discard '`\n`')

2. `getline()` function in `<istream>`, as a **member function** of a input_stream object

   Terminated if (reach the maximum number of characters `n` || encounter the `delim` character)

   Format:

   ```C++
   istream& getline (char* s, streamsize n );
   istream& getline (char* s, streamsize n, char delim );
   //example:
   char line[10];
   cout << "shit" << endl;
   cin.getline(line, 10, 'i');
   cout << line << endl; //output "sh"
   ```

### Example of Using Class



## Lecture 19

### Constructor and Destructor

##### Constructor

1. Function: to initialize a variable / object

2. Syntax:

   Written similar as a normal function, but ①the name is same as the class name ②**without any return type**

   The class declaration part:

```C++
class test{
    double x,y;
public:
    test(double xx,double yy);//constructor 1
    test(std::string str);//constructor 2
    test();//constructor 3
    void display(){ std::cout << x << y << std::endl;}
};

```

​		The constructor definition part:

```c++
test::test(double xx, double yy) {//construct with 2 double varibles
    x = xx;
    y = yy;
}
test::test(std::string str) {//constructor with a string
//......write implementation
}
test::test(){//without any parameters
    x = y = 0;
}
```

3. Rules

- Either do not define constructor at all, or once having defined a constructor (or more), use constructor for **all variable declarations**
- The compiler chooses which to use

##### Destructor

1. Function: clean up, release memory

2. Syntax:

```C++
class test{
	double x,y;
public:
    ~test(){
        //write implementation
    }
}
```

### Dynamic Memory Allocation

`new` & `delete`: have same effects as `malloc()` and `free()` in C

##### `new`

1. Syntax:

   ①For a variable: `<pointer_declaration> = new <corresponding_type>`

   ②For an array: `<pointer_declaration> = new <corresponding_type>[array_size]`

   ​	The array size can be a variable

   ③For a class: **the constructor will be automatically called**

   ```C++
   int *pint = new int;
   int N=10;
   double *pdouble = new double[N];
   myclass *pc = new myclass(1,2);//the constructor will be automatically called
   ```

2. Can only access this variable / array through that pointer (i.e. have no variable name)

##### `delete`

1. Syntax: variable: `delete pint;` array: `delete[]pdouble;`
2. Must delete after finish using

### Pointer, Reference, and `const`

**Pointer and Constant**

1. Pointer to a constant (`const` is applied to the variable the constant is pointing to)

​		Syntax: `const int *pu`

​		the variable doesn't need to be a constant and **can be changed elsewhere**, but cannot be changed **through this pointer**

2. Constant Pointer (`const` is applied to the pointer itself )

   Cannot change the address that the pointer is pointing to

   Syntax: `int *const pu = &a;` (must be initialized)

##### Pointer and Reference

1. Essence: an alias for another variable

2. Rules:

   Must be initialized (cannot be `NULL`)

   cannot be changed later

3. Constant reference (usually used in passing paramters)

   ```C++
   void func(const int& a){
       //in the implementation, variable "a" cannot be changed
   }
   ```

### Operator Overloading

##### Operator Overloading

1. Essence: an simpler way to make a **function call**

​		The name of this operator overloading function is `operator@`

2. Number of arguments needed

   Two factors: ①property of the operator (unary / binary) ②position of the function (global / member function)

   |                 | unary | binary |
   | --------------- | ----- | ------ |
   | global function | 1     | 2      |
   | member function | 0     | 1      |

3. Syntax:

   ```C++
   class Coordinate{
       double x,y;
   public:
       //Write some sonstructors here to help the operator below
       operator+(const Coordinate& b){
           return Coordinate(x+b.x,y+b.y);
       }
   }
   ```

##### Operator `=`

1. Default: a member-wise copy from once object to another

   (i.e. can apply `=` between any two variables with the same data type / class)

2. The same as the default **copy constructor**



## Lecture 20

### Default Arguments

Rule 1: In a parametric list of a function, can never exist a case where a parameter without default argument is placed after another parameter with default argument.

```c++
int times2(int x = 0);//correct
int times2(int x,int y = 1, int z = 2);//correct
int times2(int x,int y = 1, int z);//error
```

Rule 2: Can not be vague when having overloading

### Templates

Generic programming: Write the code in a way that is independent of any particular type

##### Two types of templates

1. Class Templates: `template<class 形参名, class 形参名，...> class 类名{ //data structure };`

   ```c++
   template<class T> class Stack{
       T data;
   public:
       void push(const T& t);
       //...
   }
   //Defining a member function outside the class definition:
   template<class T1,class T2> void A<T1,T2>::func_name(){//...}
   ```

​		The declaration / definition of a template can be only in global or in a class.

2. Function Templates: `template <class 形参名, class 形参名，......> 返回类型 函数名(参数列表)`

   ```C++
   template <class T> void swap(T& a, T& b){
   	//...
   }
   ```

### STL

1. STL: Standard template Library
2. Components: Containers, Iterators, Algorithms, Allocators

##### Containers: `vector`

Constructors: 

​	①`vector<T> V` ②`vector<T> V(n, value)` ③`vecotor<T> V(n)`: vector with n copies of default (for `int` , is 0)

##### Iterators

1. Generalization of pointers

2. Common member functions: 

   - `begin()` ; `end() ` (an iterator that refers to the **next position** after the last element)

     ​	Distinguish with `front()` ; `back()` (return a **reference**)

   - Insertion: ①`push_back(value)` ②`insert(iterator position, value)`
   - Deletion: ①`pop_back(value)` ②`erase(iterator position)` (or delete a range of values : `erase(iter1, iter2)`)

##### Algorithms

| function name | syntax                                                | effect                                                       |
| ------------- | ----------------------------------------------------- | ------------------------------------------------------------ |
| find          | `find(iter first, InputIterator last, const T& val)`  | return an iterator to **the first** element that is equal to val |
| count         | `count(iter first, InputIterator last, const T& val)` | return the number of elements that is equal to val           |
| max / min     | `max_element(iter first, iter last)`                  |                                                              |
| swap          | `swap(type x)`                                        | swap all the elements between this container and the container x |
| reverse       | `reverse(iter first, iter last)`                      | reverse the order in **[first, last)**                       |
| sort          | `sort (iter first, iter last)`                        | **ascending**                                                |

