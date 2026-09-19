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

# 5: Constants and Strings

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

## 5.8 & 5.9 Intro to std::string_view

```c
using namespace std::string_literals;      // access the s suffix
using namespace std::string_view_literals; // access the sv suffix

std::cout << "foo\n";   // no suffix is a C-style string literal
std::cout << "goo\n"s;  // s suffix is a std::string literal
std::cout << "moo\n"sv; // sv suffix is a std::string_view literal
```

Owners and viewers

Analogy: need to paint picture of bicycle
- Option 1: buy bike
    - Pros: get to modify it
    - Cons: Buy, manage, dispose of later
- Option 2: look out window, paint neighbors bike
    - Pros: No need to buy, manage, dispose of
    - Cons: Can't modify

`std::string`: Owner
- Makes copy of temporary initializer into memory
- Able to modify freely

`std::string_view`: Viewer
- If object being viewed is destroyed or modified, the viewer exhibits undefined behavior
- Often used as read-only function parameter

Prefer `std::string_view` over `const std::string&` (explained in later chapters)

Many ways in which a `std::string_view` points to a `std::string` that is destroyed leading
to undefined behavior.

View Modification Functions

Like a window, you can close the curtains to affect which parts of the bike you see.
```c
std::string_view s {"test"};
s.remove_prefix(1) // est
s.remove_suffix(2) // es
s = "test" // reset to test
```

Because substrings can be viewed, `std::string_view` may or may not be null terminated.
C-style strings and `std::string`s are always null-terminated.

# 6: Operators

## 6.1 Operator precedence

The following code is ambiguous. Operator precedence determines the order of operations. 
However, operands, function arguments, and subexpressions may be evaluated in any order.
`clang` is left->right, `gcc` is right->left.

```c
int getValue() {
    std::cout << "Enter an int: ";
    int x{};
    std::cin >> x;
    return x;
}
void printCalc(int x,int y,int z) {
    std::cout << x + (y*z);
}
int main() {
    printCalc(getValue(),getValue(),getValue())
    return 0;
}
```

## 6.3 Remainder and Exponentiation

Exponentiation done with:

```c
double x{std::pow(3.0,4.0)};
```
parameters and return type are `double`. Rounding errors in floating point numbers
will cause errors even if passing in integers or whole numbers.

When doing integer exponentiation, write your own function using the "exponentiation by squaring"
algorithm for efficiency.

```c
#include <cassert> // for assert
#include <cstdint> // for std::int64_t
#include <iostream>

// note: exp must be non-negative
// note: does not perform range/overflow checking, use with caution
constexpr std::int64_t powint(std::int64_t base, int exp)
{
	assert(exp >= 0 && "powint: exp parameter has negative value");

	// Handle 0 case
	if (base == 0)
		return (exp == 0) ? 1 : 0;

	std::int64_t result{ 1 };
	while (exp > 0)
	{
		if (exp & 1)  // if exp is odd
			result *= base;
		exp /= 2;
		base *= base;
	}

	return result;
}
```

To avoid integer overflow, use this safer but slower version:
```c
#include <cassert> // for assert
#include <cstdint> // for std::int64_t
#include <iostream>
#include <limits> // for std::numeric_limits

// A safer (but slower) version of powint() that checks for overflow
// note: exp must be non-negative
// Returns std::numeric_limits<std::int64_t>::max() if overflow occurs
constexpr std::int64_t powint_safe(std::int64_t base, int exp)
{
    assert(exp >= 0 && "powint_safe: exp parameter has negative value");

    // Handle 0 case
    if (base == 0)
        return (exp == 0) ? 1 : 0;

    std::int64_t result { 1 };

    // To make the range checks easier, we'll ensure base is positive
    // We'll flip the result at the end if needed
    bool negativeResult{ false };

    if (base < 0)
    {
        base = -base;
        negativeResult = (exp & 1);
    }

    while (exp > 0)
    {
        if (exp & 1) // if exp is odd
        {
            // Check if result will overflow when multiplied by base
            if (result > std::numeric_limits<std::int64_t>::max() / base)
            {
                std::cerr << "powint_safe(): result overflowed\n";
                return std::numeric_limits<std::int64_t>::max();
            }

            result *= base;
        }

        exp /= 2;

        // If we're done, get out here
        if (exp <= 0)
            break;

        // The following only needs to execute if we're going to iterate again

        // Check if base will overflow when multiplied by base
        if (base > std::numeric_limits<std::int64_t>::max() / base)
        {
            std::cerr << "powint_safe(): base overflowed\n";
            return std::numeric_limits<std::int64_t>::max();
        }

        base *= base;
    }

    if (negativeResult)
        return -result;

    return result;
}
```

## 6.4: Increment and decrement

The following output ambiguous:
```c
int x{5};
int value { add(x,++x) };
```

Side effect: "Has some observable effect beyond producing a return value.
Common examples include changing the value of objects, doing input/output,
updating GUI, etc."

Unspecified behavior (compiler dependent):
```c
x + ++x
```

Using multiple side-effects is the source of many errors. Avoid using multiple
side effects in one given statement.

## 6.5: Comma Operator

Evaluates x, evaluates y, returns value of y
```c
int x{1};
int y{2};
std::cout << (++x,++y); // 3
```

```c
z = (a,b); // evaluate (a,b), get result of b, assign value to z
z = a,b; // evaluates as "(z=a),b" so z is assigned value of a, b is evaluated and discarded
```

Comma has lowest precedence and gets evaluated last.

Avoid using comma operator.

## 6.6: Conditional operator

```c
condition ? true : false;
```

To avoid operation precedence errors:
- parenthesize the entire conditional operation when used in a compound expression
- parenthesize the condition if it contains other operators

## 6.7: Relational operator

Non-literal floating point equality should be avoided. Use approximate comparisons instead.

```c
// C++14/17/20 version
#include <algorithm> // for std::max
#include <iostream>

// Our own constexpr implementation of std::abs (for use in C++14/17/20)
// In C++23, use std::abs
// constAbs() can be called like a normal function, but can handle different types of values (e.g. int, double, etc...)
template <typename T>
constexpr T constAbs(T x)
{
    return (x < 0 ? -x : x);
}

// Return true if the difference between a and b is within epsilon percent of the larger of a and b
constexpr bool approximatelyEqualRel(double a, double b, double relEpsilon)
{
    return (constAbs(a - b) <= (std::max(constAbs(a), constAbs(b)) * relEpsilon));
}

// Return true if the difference between a and b is less than or equal to absEpsilon, or within relEpsilon percent of the larger of a and b
constexpr bool approximatelyEqualAbsRel(double a, double b, double absEpsilon, double relEpsilon)
{
    // Check if the numbers are really close -- needed when comparing numbers near zero.
    if (constAbs(a - b) <= absEpsilon)
        return true;

    // Otherwise fall back to Knuth's algorithm
    return approximatelyEqualRel(a, b, relEpsilon);
}

int main()
{
    // a is really close to 1.0, but has rounding errors
    constexpr double a{ 0.1 + 0.1 + 0.1 + 0.1 + 0.1 + 0.1 + 0.1 + 0.1 + 0.1 + 0.1 };

    constexpr double relEps { 1e-8 };
    constexpr double absEps { 1e-12 };

    std::cout << std::boolalpha; // print true or false instead of 1 or 0

    constexpr bool same { approximatelyEqualAbsRel(a, 1.0, absEps, relEps) };
    std::cout << same << '\n';

    return 0;
}
```

## 6.8: Logical Operators

Short circuit evaluation

# O: Bit manipulation

## O.1: Bit flags and bit manipulation

```c
#include <bitset>
// bit position 76543210
// bits         00000101
std::bitset<8> b { 0b0000'0101 };

b.set(3); //   0000 1101
b.flip(4); //  0001 1101
b.reset(4); // 0000 1101
std::cout << bits; // prints 00001101
std::cout << bits.test(3); // prints 1
```

## O.2: Bitwise operators

```c
x << n; // shift left by n positions, new bits are 0
```

`operator~` and `operator<<` are width sensitive. Bitwise operators will promote operands with narrower integral types to `int` or `unsigned int`.
Avoid bit shifting integral types smaller than `int`.

```c
std::uint8_t c { 0b00001111 };

std::cout << std::bitset<32>(~c) << '\n';     // incorrect: prints 11111111111111111111111111110000
std::cout << std::bitset<32>(c << 6) << '\n'; // incorrect: prints 0000000000000000001111000000
std::uint8_t cneg { ~c };                     // error: narrowing conversion from unsigned int to std::uint8_t
c = ~c;                                       // possible warning: narrowing conversion from unsigned int to std::uint8_t

//////////////////////////////////////////

std::uint8_t c { 0b00001111 };

std::cout << std::bitset<32>(static_cast<std::uint8_t>(~c)) << '\n';     // correct: prints 00000000000000000000000011110000
std::cout << std::bitset<32>(static_cast<std::uint8_t>(c << 6)) << '\n'; // correct: prints 0000000000000000000011000000
std::uint8_t cneg { static_cast<std::uint8_t>(~c) };                     // compiles
c = static_cast<std::uint8_t>(~c);                                       // no warning
```

## O.4: Converting integers between binary and decimal

Decimal to binary:
148:
128/2=74r0
74/2=37r0
37/2=18r1
18/2=9r0
9/2=4r1
4/2=2r0
2/2=1r0
1/2=0r1
-> 1001 0100

148:
148>=128? yes, 148-128=20
20>=64? no
20>=32? no
20>=16? yes, 20-16=4
4>=8? no
4>=4? yes, 4-4=0, (rest of bits must be zero)
0>=2? no
0>=1? no
-> 1001 0100

Twos complement:
- first digit is signed bit
- *-1: flip bits and add one

twos complement to decimal:
- if negative number, flip bits and add one
- convert to decimal normally

Floating point: https://tfinley.net/csarch-notes/2000/floating

# 7: Scope, duration, and linkage

## 7.2: User defined namespaces

```c
namespace Foo {
    int f() {
        return 0;
    }
}

Foo::f();
```

```c
::f() // call f() in global namespace
```

```c
namespace A {
    namespace B {

    }
}

namespace C::D {

}

namespace Active = C::D; // Active refers to C::D
```

## 7.3: Local variables

Nested scope ends after {}
```c
int x {5};
int y {1};
{
int x {4}; // this x refers to a different object than the previous x (name shadowing)
int z=x+y;
}
```

## 7.4: Global variables

Globals must be initialized.

## 7.6-7: Internal/External Linkage

Internal linkage
```c
static int x{};
const int y{1};
constexpr int z{2};

static int add(int x, int y) {}
```

External Linkage
```c
int x{};
extern const int y{};
extern constexpr z{};
int add(int x,int y) {}
```

## 7.9: Inline functions

Don't use inline anymore. Compiler optimizes for you and ignores this.
In modern C++ inline has evolved to mean "function can be duplicated".

Useful for header-only libraries

## 7.10: TODO
## 7.11: TODO
## 7.12: TODO
## 7.13: TODO
## 7.14: TODO

# 8: Control Flow

## 8.4: Constexpr if

Use constexpr if the conditional is constexpr (C++17):
```c
constexpr double g {9.8};
if constexpr (g == 9.8) {
    std::cout << "Earth";
}
```

## 8.5: Switch statement

```c
switch (x) {
case 1:
    std::cout << "one";
    break;
case 2:
    std::cout << "two";
    break;
case 3:
    [[fallthrough]]
case 4:
    [[fallthrough]]
case 5:
    std::cout << "three to five";
    break;
default:
    std::cout << "idk";
    break;
}
```

## 8.7: Goto

```c
int main() {
    double x{};
tryAgain:
    std::cout << "Enter positive number";
    std::cin >> x;
    if(x < 0.0) {
        goto tryAgain;
    }
}
```

## 8.9: Do while

```c
do {
std::cout << "hi";
} while(x > 5);
```

## 8.12 Halts

`std::exit()`:      Normal exit of the program (happens at end of main())
`std::atexit()`:    Runs when program exits.
`std::abort()`:     Abnormal termination due to runtime error. Exits with no cleanup.
                    Failing `static_assert` calls `std::abort` implicitly.
`std::terminate()`: Used for exceptions. Often called implicitly. Calls `std::abort()`.

```c
void cleanup() {

}
int main() {
    std::atexit(cleanup)
}
```

## 8.13: Intro to RNG

Basic example PRNG (pseudo-rng):
```c
unsigned int LCG16() {
    static unsigned int state {0};
    s_state = 8253729 * s_state + 2396403;
    return s_state % 32768;
}
```

Use more state variables (seeding).

Theoretical max number of unique sequences = number of bits in PRNG state.
In practice = number of unique seeds the program using the PRNG can provide.

Providing not enough bits of quality seed data = "underseeding"

Ideal seed:
- seed should contain >= bits as the state of the PRNG
- each bit in the seed should be independently randomized
- mix of 0s and 1s across all bits
- every bit changes, "stuck bits" provide no value
- low correlation with previously generated seeds

Ideal PRNG:
- method by which next number is generated is not predictable
- good dimensional distribution of numbers
- high period for all seeds
- efficient

## 8.14: Mersenne Twister

`mt19937`: 32-bit unsigned int
`mt19937_64`: 64-bit unsigned int

Not always secure: Predictable results after seeing 624 numbers.

```c
#include <iostream>
#include <random>
std::mt19937 mt{};
int main() {
std::cout << mt();
}
```

```c
#include <iostream>
#include <random>

int main() {

    std::mt19937 mt{}; // don't seed
    std::mt19937 mt{ static_cast<std::mt19937::result_type>(std::chrono::steady_clock::now().time_since_epoch().count()) }; // seed with system clock
    std::mt19937 mt{ std::random_device{}() }; // seed with /dev/random

    std::random_device rd{}; // or (correctly) seed with all 32 bits
    std::seed_seq ss{ rd(), rd(), rd(), rd(), rd(), rd(), rd(), rd() };
    std::mt19937 mt{ ss };

    std::uniform_int_distribution die6{1,6};
    std::cout << "Random die roll: " << die6(mt);
}
```




