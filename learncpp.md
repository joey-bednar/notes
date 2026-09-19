Learncpp.com Notes

Further reading:
- Compiler, linker, libraries, make, cmake
- Standard streams

# Section 0: Introduction/Getting Started

## 0.4 Hello World
```c
#include <iostream>

int main() {
    std::cout << "Hello world!.";
    return 0;
}
```

## 0.5 Compiler/Linker/Libraries

Compiler
- checks code follows rules
- translates to machine language instructions.
- stores instructions in intermediate file called "object file" that contains data needed by linker/debugger.

Linker
- combines all object files and produces executable
- ensures cross-file dependencies resolved properly
    - use something in one file and another file, linker connects both together
    - error if linker cannot connect reference to its definition
- links to library files (precompiled code for reuse)
- C++ stdlib usually autolinked

## 0.7 Compiling

```bash
g++ -o output helloworld.cpp
```

## 0.9/0.11 Build configurations/compiler flags

Flags
-O0: default, debugging, no optimizations
-O2: recommended, release builds
-O3: most optimized, may/may not work better

-ggdb: for debugging
-DNDEBUG: 

-Wall:
-Weffc++
-Wextra
-Wconversion
-Wsign-conversion
-Werror

-std=c++14: Pick version (11,14,17,20,23)

# Section 1: Basics

## 1.1 Comments
```c
// inline

/*
multiline
*/
```

## 1.3 Objects

```c
std::cout << 5;       // print the literal number `5`
std::cout << -6.7;    // print the literal number `-6.7`
std::cout << 'H';     // print the literal character `H`
std::cout << "Hello"; // print the literal text `Hello`
```

```c
int x; // defining variable
```

## 1.4 Assignment

```c
int x; // definition
x = 5; // assignment
std::cout << x;
```

```c
int a;         // default-initialization (no initializer)
/* sets to indeterminite value (garbage) */

// Traditional initialization forms:

int b = 5;     // copy-initialization
/* Copies value on right into variable. Fell out of favor in modern C++
as it is less efficient for more complex types. C++17 fixes most of these issues,
making a comback in modern C++ as it looks nicer. Seen in older codebases or those ported
from C.*/

int c ( 6 );   // direct-initialization
/* More efficient but replaced by direct-list-initialization. */

// Modern initialization forms (preferred):

int d { 7 };   // direct-list-initialization (preferred)
int f = { 8 }; // copy-list-initialization (rarely used)
int e {};      // value-initialization (empty braces)
```

Outcome: Use direct-list-initialization (also called "uniform initialization" or "brace initialization")

### Why?
1. Disallows narrowing conversions on initialization.
```c
int w1 { 4.5 }; // compile error
int w2 = 4.5; // copy-initialized to 4
int w3 (4.5); // direct-initialized to 4
```
2. Value initialization instead of default-initialization.
```c
int w1 {}; // value-initialization/zero-initialization to 0
int w2; // default-initialization to garbage value
```
3. Works in most cases.
4. Allows for initialization with a list of values.

Instantiation: Defined (created) and initialized (set).

```c
[[maybe_unused]] double pi { 3.14 }; // don't show warning if pi is unused (C++17)
```

## 1.5 iostream

### std::cout

```c
std::cout << "Hello" << " world" << std::endl;
std::cout << "Hello" << " world\n";
int x { 5 };
std::cout << "x is equal to: " << x << '\n';
```

std::cout is buffered. Occasionally, the buffer is flushed and transfered to console.
If the program crashes/aborts/paused before flushing, the output is not displayed.

'\n' outputs a newline.
std::endl outputs a newline **and flushes the buffer**.

Prefer using '\n' to allow C++ to flush buffer automatically.

### std::cin

```c
std::cout << "Enter a number: ";
int x{};
std::cin >> x;
std::cout << "You entered " << x << '\n';
```


```c
std::cout << "Enter two numbers separated by a space: ";
int x{};
int y{};
std::cin >> x >> y;
std::cout << "You entered " << x << "and" << y << '\n';
```

std::cin is buffered. The extraction operator "<<" will take a value
from the front of the input buffer and pass it into the variable.

Buffer: "4 5\n"

- Leading whitespace is discarded from input buffer.
- If the buffer is empty, >> will wait for the user to enter more data.
- ">>" extracts as many characters as possible for the variable data type.

For example: 
```c
std::cout << "Enter two numbers.\n";

std::cout << "Number one: ";
int x{};
std::cin >> x;

std::cout << "Number two: ";
int y{};
std::cin >> y;

std::cout << "You entered " << x << "and" << y << '\n';
```
if the user inputs
"Number one: 5 6"
the second std::cin will not wait to prompt for a second number
as the buffer contains "5 6\n" The extraction "<<" operator will
load the "6" off the buffer instead of waiting for the user.

## 1.6 Uninitialized variables and undefined behavior

Undefined behavior:
```c
int x;
std::cout << x;
```

Implementation-defined behavior:
```c
std::cout << sizeof(int) << '\n';
```

## 1.7 Keywords and naming identifiers

Variables
- cannot be a keyword
- case sensitive
- start with letter or underscore
- contains letters, numbers, or underscore

## 1.9 Literals and Operators

Operator "arity" is the number of inputs it operates on:
- unary (1) "-"
- binary (2) "+"
- ternary (3)
- unary (null) "throw"

# Section 2: Basics: Functions and Files

## 2.4 Function parameters and arguments

Argument: 5
Function parameter: x (in this case an unreferenced parameter)
Passed by value

Why use unnamed parameter:
1. Temporary while refactoring function in order to not break everything.
2. Operators ++,-- have prefix and postfix variants. An unreferenced function paramter is used to
differentiate whether an overload for such an operator is for the prefix or postfix case.
3. When we need to determine something from the type (rather than the value) of a type template parameter.
```c
void g(int) { // unnamed parameter will not generate warning

}

void f(int x) {

}

f(5)
```

## 2.5 Intro to local scope

Temporary object:
```c
std::cout << f() << '\n';
```
Return value of f() is in temporary scope. Scope ends at the end of the full expression.

## 2.7 Forward declarations and definitions

Compiler error:
```c
int main() {
add(1,2)
}

int add(int x, int y){ 
}
```

Functions called must be defined above where they are called. Alternatively,
you can use a "function declaration".


Valid code:
```c
int add(int x, int y) // declaration

int main() {
add(1,2)
}

int add(int x, int y) { // definition

}
```

Linker error:
```c
int add(int x, int y)

int main() {
add(1,2)
}
```

One definition rule (ODR):
1. Within a file, each function, variable, type, or template can only have 1 definition. Variables
in different functions or functions in different namespaces don't violate this rule.
2. Within a program, each function can only have 1 definition. Functions not visible to linker do not
violate this rule.
3. Types, templates, inline functions, and inline variables can have duplicate definitions in different
files as long as they are identical.

## 2.8 Programs with multiple files

cpp files are compiled individually. In order to reference functions or libraries from outside the
file, a forward declaration or include statement must be in the file. This is why every file that
used std::cout must have #include <iostream> in that exact file.

## 2.9 Naming collisions and introduction to namespaces

```c
include <iostream> // allowed, import declared in global scope

int x; // allowed but strongly discouraged (non-const global variable definition)
x=5; // compiler error: executable statements not allowed in namespaces

int y { 5 }; // allowed but strongly discouraged (non-const global variable definition with initializer)
```

## 2.10 Intro to the preprocessor

Preprocessor commands execute from top to bottom of file and ignore all C++ scope or rules.
Directives in one file do not interact with directives in another file unless included.

```c
#include <iostream>

#define NAME "Joey" // mostly seen in legacy code: avoid macros with substitution text
                    // unless no viable alternatives exist

#define PRINT_JOEY

int main () {

// if defined
#ifdef PRINT_JOEY
    std::cout << "Joey\n";
#endif

// if not defined
#ifndef PRINT_BOB
    std::cout << "Bob\n";
#endif


#if 0
    std::cout << "Never runs";
#endif

#if 1
    std::cout << "Always runs";
#endif

}
```

## 2.11 Header files

Don't include cpp files
1. non-conventional
2. hard to avoid ODR issues
3. causes naming collisions
4. changing the cpp file will require other files that include it to recompile causing slowdown

Includes:
<>: file we didn't write. searches in predefined "include directories" (often come with compiler/OS)
"": file we wrote. searches in project directory. if not found there will search "include directories"

Add ./source/includes to path
```bash
g++ -o main -I./source/includes main.cpp
```

History:
like many other std C++ libraries, iostream doesn't end in ".h":

The original versions of cout and cin were declared in iostream.h in the global 
namespace. When the language was 
standardized by the ANSI committee, they decided to move all of the names used 
in the standard library into the std namespace to help avoid naming conflicts 
with user-declared identifiers. However, this presented a problem: if they moved
all the names into the std namespace, none of the old programs (that included 
iostream.h) would work anymore! To work around this issue, C++ introduced new 
header files that lack the .h extension. These new header files declare all 
names inside the std namespace. This way, older programs that include #include 
<iostream.h> do not need to be rewritten, and newer programs can
#include <iostream>.

Best practice:
- inclusion order:
    - paired header file with code file (add.cpp,add.h)
    - headers in same project ("mymath.h")
    - 3rd party headers (<boost/tuple/tuple.h>)
    - std lib headers (<iostream>)
- Always include header guards
- Do not define variables and functions in header files (for now).
- Give a header file the same name as the source file it’s associated with 
  (e.g. grades.h is paired with grades.cpp).
- Each header file should have a specific job, and be as independent as possible. 
  For example, you might put all your declarations related to functionality A in 
  A.h and all your declarations related to functionality B in B.h. That way if 
  you only care about A later, you can just include A.h and not get any of the 
  stuff related to B.
- Be mindful of which headers you need to explicitly include for the 
  functionality that you are using in your code files, to avoid inadvertent 
  transitive includes.
- A header file should #include any other headers containing functionality it 
  needs. Such a header should compile successfully when #included into a .cpp 
  file by itself.
- Only #include what you need (don’t include everything just because you can).
- Do not #include .cpp files.
- Prefer putting documentation on what something does or how to use it in the 
  header. It’s more likely to be seen there. Documentation describing how 
  something works should remain in the source files.

## 2.12 Header guards

```c
#ifndef DEF_H
#define DEF_H

// declarations here

#endif
```

Use a complex name for the header guard to avoid a conflict.

In modern C++
```c
#pragma once

// code here
```
will do the same thing.
Pragma once will fail if multiple copies of the file are included.
Google does not use pragma in their coding guidelines.

# Section 3: Debugging C++ Programs

## 3.5 Debugging tactics

Conditionalize code
```c
#define ENABLE_DEBUG

#ifdef ENABLE_DEBUG
std::cerr << "something"
#endif
```

Logging using plog example:
```c
#include <plog/Log.h> // Step 1: include the logger headers
#include <plog/Initializers/RollingFileInitializer.h>
#include <iostream>

int getUserInput()
{
	PLOGD << "getUserInput() called"; // PLOGD is defined by the plog library

	std::cout << "Enter a number: ";
	int x{};
	std::cin >> x;
	return x;
}

int main()
{
	plog::init(plog::debug, "Logfile.txt"); // Step 2: initialize the logger

	PLOGD << "main() called"; // Step 3: Output to the log as if you were writing to the console

	int x{ getUserInput() };
	std::cout << "You entered: " << x << '\n';

	return 0;
}
```

Output:
```bash
2018-12-26 20:03:33.295 DEBUG [4752] [main@19] main() called
2018-12-26 20:03:33.296 DEBUG [4752] [getUserInput@7] getUserInput() called
```

Turn off logging display:
```c
plog::init(plog::none , "Logfile.txt"); // plog::none eliminates writing of most
                                        // messages, essentially turning logging off
```

## 3.6 Using an integrated debugger

- step into
- step over
- step out: execute remaining code in function

- run to cursor
- start
- continue
- breakpoints (set/delete)

```c
#ifdef DEBUG
std::cout << std::unitbuf; // enable automatic flushing
#endif
```

step over std::cout to avoid entering stdlib function

# Section 4: Fundamental Data Types

## 4.1 Intro to fundamental data types

Each memory address hold one byte (8 bits on modern systems)

Standard integer types: short, int, long, long long
Integral types: bool, char

Floating point:
- float
- double
- long double

Integral (Boolean)
- bool (true/false)

Integral (Character)
- char
- wchar_t           // related to windows API
- char8_t (C++20)   // for unicode compatibility
- char16_t (C++11)  // for unicode compatibility
- char32_t (C++11)  // for unicode compatibility

Integral (Integer)
- short int
- int
- long int
- long long int (C++11)

Null Pointer:
std::nullptr_t (C++11)

Void:
void

## 4.2 Void

Will compile but is deprecated:
```c
int f(void) {

}
```

## 4.3 Object sizes and sizeof operator
name	         min bytes   typical bytes
bool	         1
char	         1
wchar_t	         1           2 or 4
char8_t	         1
char16_t         2
char32_t         4
short            2
int              2           4
long             4           4 or 8
long long        8
float            4
double           8
long double      8           8, 12, or 16
std::nullptr_t   4           4 or 8


`sizeof()` returns number of bytes of type
`sizeof(x)` and `sizeof(int)` both return 4

`sizeof(void)`: compilation error (gcc may return 4 bytes)

`std::setw(width)`: sets width size of subsequent output
```c
std::cout << std::setw(5) << "bool:" << sizeof(bool)
```

Types that use less memory are not always faster. CPUs are optimized for int.

## 4.4 Signed integers
Range: -(2^(n-1)-1) to 2^(n-1)-1 // -128 to 127 for chars

8bit: -128 to 127
16bit: -32,768 to 32,767
32bit: ~2.1*10^9 (2 billion)
64bit: ~9.2*10^18 (9 quintillion)

8bit overflow: 127+1 = -128
integer division: 8/5 = 1

## 4.5 Unsigned integers

`unsigned int x = 5;`

Range: 0 to (2^n)-1 // 0 to 255 for chars

8bit: 0 to 255
16bit: 0 to 65,535
32bit: ~4.2*10^9 (2 billion)
64bit: ~1.8*10^19 (18 quintillion)

8bit overflow: 255+1 = 0
integer division: 8/5 = 1

Unsigned integers are avoided because underflow is more common

Adding a signed an unsigned integer:
- signed int casted to unsigned int before addition
- signed -1 < unsigned 1: false (-1 casted to MAXINT-1)

When to use:
- bit manipulation
- wrap around behavior is required (encryption/rng)
- unavoidable in some cases (array indexing)
- memory constraints (embedded)

## 4.6 Fixed width integers

- `std::int8_t` // behaves like char
- `std::uint8_t` // behaves like char
- `std::int16_t`
- `std::uint16_t`
- `std::int32_t`
- `std::uint32_t`
- `std::int64_t`
- `std::uint64_t`

Fixed width sizes may be slower if not optimized to CPU architecture

best practice to avoid:
`std::int_least8_t` and other variants: smallest type with width of at least # bits
`std::int_fast8_t` and other variants: fastest signed integer type of ate least # bits
because:
- most people don't know them
- fast can use more memory than intended
- different behavior on different architectures

best practice:
- use `int` when doesn't matter
- `std::int#_t` for guaranteed range
- `std::uint_#t` for bit manipulation or wrap around behavior required

avoid
- `short` `long` (use fixed-width)
- fast, least (use fixed width)
- unsigned for quantities (use signed)
- 8 bit fixed (16 bit fixed)
- compiler specific types

`std::size_t` at least 16 bits, usually address width
```c
#include <cstddef>

std::size_t s { sizeof(x) };
```

## 4.8 Floating point numbers

```c
std::cout << std::boolalpha; // print bool as "true" or "false"
std::cout << std::noboolalpha; // turn off
```
also allows for reading strings "true" and "false" into bools

C++ defaults to double, use "f" to make a float
```c
int a { 5 };
double b { 5.0 };
float c { 5.0f };
```

```c
#include <iomanip>
std::setprecision(num) // show num digits of precision in std::cout (default for cout is 6)
```

float: 6-9 digits of precision
double: 15-18 digits of precision

rounding errors increase with more operations

## 4.9 Boolean values

```c
// !true == false

bool bFalse {0}; // initializes to false
bool bTrue {1}; // initializes to true
bool bNo {2}; //error, narrowing conversion not allowed

bool b1 = 4; // copy-initialization allowed with warnings. anything other than 0 is true
```

## 4.10 Intro to if statement

```c
if() {

} else if () {

} else
```

## 4.11 Chars

Read character including whitespace:
```c
char ch{};
std::cin.get(ch);
```

```c
'a' // char
"a" // c-style string

'56' // don't use multiple chars, will compile with nonstandard results (depends on compiler)
```

## 4.12 Intro to type conversion and static_cast

Implicit type conversion: compiler does for us
```c
void f(double x) {

}

f(5)
```
If narrowing conversion happens, warning will show

Explicit type conversion:
```c
static_cast<int>(5.5)
```

`std::int8_t` acts like a char.

```c
int s {-1};
static_cast<unsigned int>(s) // 4294967295
unsigned int u {4294967295}; // largest 32-bit unsigned int
static_cast<int>(u) // implementation defined prior to C++20, -1 as of C++20
```

# Section 5: Constants and Strings

## 5.1 Constant variables

```c
const int x = 5;
```
Must be initialized and cannot change.

```c
void f(const int x) // unnecessary when passing by value
```

```c
const int f(int x) // unnecessary as a temp copy will be returned, 
                   // impedes compiler optimizations (move semantics)
```

Make everything possible const except by-value function parameters and by-value return types

Prefer const over macros because:
- macros don't follow C++ rules leading to strange compilation errors
- hard to debug macros

Type qualifiers (cv-qualifiers):
- `const`
- `volatile`

## 5.2 Literals

Suffix tables
```
integral	u or U	                                unsigned int
integral	l or L	                                long
integral	ul, uL, Ul, UL, lu, lU, Lu, LU	        unsigned long
integral	ll or LL	                        long long
integral	ull, uLL, Ull, ULL, llu, llU, LLu, LLU	unsigned long long
integral	z or Z	                                The signed version of std::size_t (C++23)
integral	uz, uZ, Uz, UZ, zu, zU, Zu, ZU	        std::size_t (C++23)
floating point	f or F	                                float
floating point	l or L	                                long double
string	        s	                                std::string
string	        sv	                                std::string_view
```

```c
5.0  // type double by default
5.0f // float
```

```c
std::cout << 5 << '\n';  // 5 (no suffix) is type int (by default)
std::cout << 5L << '\n'; // 5L is type long
std::cout << 5u << '\n'; // 5u is type unsigned int

int a { 5 };             // ok: types match
unsigned int b { 6 };    // ok: compiler will convert int value 6 to unsigned int value 6
long c { 7 };            // ok: compiler will convert int value 7 to long value 7

float f { 4.1 };         // error: use 4.1f for float

"Hello, world!" // c-style string, const char[14], null terminator \0
```

## 5.3 Numeral systems

```c
int x{012}; // 0 before number means octal
std::cout << x; // prints 10 (12oct->10dec)

int x{0xF}; // 0x before number means hexadecimal
std::cout << x; // prints 15

int x{0b1};         // 0000 0000 0000 0001 (0b before a number means binary, C++14)
int x{0b1001'0010}; // 0000 0000 1001 0010 (' separator is optional)
```

Use within std::cout to print different formats:
```
std::oct
std::hex
std::dec
```

To print binary, use std::bitset
```c
std::bitset<8> b1 {0b1100'0101};
std::cout << b1;
```

Better in modern C++:
```c
std::cout << std::format("{:b}\n", 0b1010);  // C++20, {:b} formats the argument as binary digits
std::cout << std::format("{:#b}\n", 0b1010); // C++20, {:#b} formats the argument as 0b-prefixed binary digits
std::println("{:b} {:#b}", 0b1010, 0b1010);  // C++23, format/print two arguments (same as above) and a newline

// 1010
// 0b1010
// 1010 0b1010
```

## 5.4 as-if rule and compile-time optimization

as-if rule: Compilers can optimize anything as long as it doesn't change observable behavior

Optimizations:
- Compile-time evaluation
- Constant folding
- Constant propagation
- Dead code elimination

Compile-time constants:
- Literals
- Constant objects whose initializers are compile-time constants

Runtime-constant:
- Constant function parameters
- Constant objects whose initializers are non-constants or runtime constants

Some runtime constants can be evaluated at compile-time.
Some compile-time constants cannot be used in compile-time features.

## 5.5 Constant expressions
```c
constexpr int x {expr}; // expr must be evaluated at compile time
```
