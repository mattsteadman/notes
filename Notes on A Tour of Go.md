# Notes on *A Tour of Go*

## Packages, variables, and functions.

- You don't need semicolons at the end of lines in Go.

### Packages
- Import statements can be *factored*. For example, instead of 
```
import "fmt"
import "math/rand"
```
you can write
```
import (
    "fmt"
    "math/cmplx"
)
```

- Main file starts with `package main`.

- Exported names are Capitalized.

### Functions

- Types come *after* names in declarations of variables.

- `x int, y int` can be shortened to `x, y int`.

- Return types comes after function name and arguments:
```
func add(x int, y int) int {
    return x + y
}
```

- Functions can return more than one result. This function returns two strings:
```
func swap(x, y string) (string, string) {
    return y, x
}
```

- Return values can be named. 
    - Named returned values are only defined *within* the function whence they are returned. 
    - If you name your return value(s), then you don't need to name the variable to be returned in your `return` statement, you can just say `return`. This is called a *naked return statement*, use them with care as they can make things more confusing in long functions. For example,
```
func split(sum int) (x, y int) {
    x = sum * 4 / 9
    y = sum - x
    return
}
```


### Variables

- To declare a variable, you must use the `var` statement. 
    - The `var` statement can be used to declare a list of variables, just like a function argument list.  For example, `var a, b, c int`.
    - If you provide initial values for a variable, you can omit the type in the declaration and Go will determine the type automatically. For example, `var i, j = 1, 2`

- Variables can also be assigned values in a list fashion. For example, `a, b, c = 1, 2, 3`.

- Inside of functions, statements like `var k = 3` can be shortened to `k := 3` using the `:=` construct. This is called a *short assignment statement*. A short assignment statement always uses implicit typing.
    - Outside of functions, every statement must begin with a keyword, so the short assignment statement can't be used.

- Go's *basic types* are `bool`, `string`, `byte` (alias for `uint8`), `rune` (alias for `int32`, used for Unicode code points), `int`,  `int8`, `uint8`  `int16`,  `int32`,  `int64`, `uint`, `uint8`, `uint16`, `uint32`, `uint64`, `uintptr`, `float32`, `float64`, `complex64`, `complex128`
    - The size of `int`, `uint` and `uintptr` depend on the machine word size. Like C, `int` is a pretty good choice for when you need an integer, unless you have a reason to use something else.

- The `var` statement can also be factored like the `import` statement. For example,
```
var (
    ToBe   bool       = false
    MaxInt uint64     = 1<<64 - 1
    z      complex128 = cmplx.Sqrt(-5 + 12i)
)
```

- Each type has an associated *zero value*. Variables declared without an initial value are given their respective zero value.
    - The zero value for numeric types is `0`.
    - The zero value for `bool` is `false`.
    - The zero value for strings is the empty string `""`.

- Each type can be used like a function for type conversion. The expression `T(v)` returns the value `v` converted to the type `T`. For example, `f := float64(i)` declares `f` as a `i` converted to a `float64`.
    - Unlike C, implicit type conversion is *not* allowed!

- When determining the type of a numeric constant, Go figures out the type based on the precision of the number. For example, `i := 42` declares an `int`, whereas `f := 3.14159` declares a `float64`.
- Constants can be declared using the `const` keyword. For example, `const ChineseWordForWorld = "世界"`.

## Flow control statements: for, if, else, switch and defer
### For
- Go only has **one** loop construct, which is `for`.

- `for` does *not* require parentheses `()` around the control statements, but it *does* require braces `{}` around the loop body.

- `for` takes three statements separated by semicolons:
    - The init statement, executed before entering the loop
    - The condition expression, evaluated before every iteration
    - The post statement, executed at the end of every iteration

- For example,
```
for i := 0; i < 10; i++ {
}
```

- Short assignment statements are valid init statements (e.g. `i := 1`).

- The init and post statements are *optional*. For example,
```
for ; sum < 1000; {
}
```

- You can even drop the semicolons, and just use it as you would a while statement:
```
for sum < 1000 {
}
```

- Actually, the conditional statement is optional too. An infinite loop looks like this:
```
for {
}
```

### If

- Syntax for Go's `if` statement is similar to the `for` statement: parentheses are not needed, but braces are required. For example,
```
if x < 0 {
}
```

- Unlike C, `if` statements in Go can include init statements just like a `for` statement. For example,
```
if v := math.Pow(x, n); v < lim {
    return v
}
```

- Note that the scope of `v` in the above example is the `if` statement, so `v` cannot be used against outside of the `if` statement. An attached `else` statement counts as within the `if` statement.

### Switch

- The `switch` statement in Go can also include an init condition. For example,
```
switch os := runtime.GOOS; os {
case "darwin":
    fmt.Println("OS X.")
case "linux":
    fmt.Println("Linux.")
default:
    // freebsd, openbsd,
    // plan9, windows...
    fmt.Printf("%s.", os)
}
```

- Unlike C, `break` statements are not needed. There is no such thing as fall through.

- You can `switch` on anything in Go, including strings.

- The values of the cases don't need to be constants, you can use variables or the return value of a function.

- `switch` evaluates *only* the cases up to and including the first one to succeed.

- `switch` without a condition is the same as `switch true`, that is it evaluates each condition in order until it finds the first one that is true. This is functionally equivalent to an `if`-`else` chain.

### Defer

- The `defer` statement evaluates the arguments of a function call, but defers the evaluation of the function itself until surrounding functions have been evaluated. For example, the following program prints `hello\nworld`
```
package main

import "fmt"

func main() {
    defer fmt.Println("world")

    fmt.Println("hello")
}
```

- Deferred functions are pushed onto a stack. When deferred calls are executed, they are popped off with the first function to be deferred evaluated last.


## More types: structs, slices, and maps.
### Pointers
- Go has pointers. Pointers hold the memory address of a value.

- The zero value of a pointer is `nil`.

- Unlike C, you declare the pointer itself with a special type. `*T` is a pointers to a value of type `T`. For example, `var p *int` declares a pointer `p` to an `int`.

- Dereferencing a pointer is like like C, you can write `*p = 21` to assign a value to the object pointed to by `p`.

- The `&` operator returns a pointer to it's operand, for example `p = &i` assigns the address of `i` to `p`.

- Unlike C, Go does not allow pointer arithmetic.

### Structs
- A `struct` is a collection of fields.

- Fields are accessed using a dot.

- You can have a pointer to a `struct`.

- To access a field of the `struct` behind a `struct` pointer, no need to use `->` like in C. Just use the regular dot (`p.x`), dereferencing is implicit. You can also say `(*p).x`, but it's not necessary.

- Go allows you to declare a new struct and initialize it's fields like in C, but the syntax is difference. 
    - The construct is called a *struct literal*. 
    - To specify the values of `X` and `Y`, you can either list them like `Vertex{1, 2}` or name them like `Vertex{X: 1}`. 
    - The `&` operator can be used just like on variables to generate a pointer to the operand. 
    - All of the following are valid declarations:
```go
type Vertex struct {
    X, Y int
}

var (
    v1 = Vertex{1, 2}  // has type Vertex
    v2 = Vertex{X: 1}  // Y:0 is implicit
    v3 = Vertex{}      // X:0 and Y:0
    p  = &Vertex{1, 2} // has type *Vertex
)
```

### Arrays
- The syntax for declaring an array is doubly "backwards". Declare an array like this: `var a [10]int`.
    - Note that this declaration is written in the order that it is read aloud in English: "`a` is (an array of) `[10]` `int`s".

- An array's size is part of it's type, so it cannot be resized.

- You can declare an *array literal* like this: `a := [6]int{2, 3, 5, 7, 11, 13}`.

- Need an array with dynamic size? That's called a *slice* in Go (as in, a "slice" of an array), and they are actually more common than arrays.

### Slices
- To declare a slice of type `T`, use the syntax `[]T` like this: `var s []int`.

- You can initialize a slice to be a slice of an array like this: 
```go
primes := [6]int{2, 3, 5, 7, 11, 13} //array

var s []int = primes[1:4] //slice of an array
```

- The syntax `a[low : high]` *includes* the `low` index, but *excludes* the `high` index. So `primes[1:4]` includes `2`, `3`, and `5`.

- Declaring a slice does **not** allocate any space for the members of the slice!
    - A slice is a *reference* to an array, so slices *require* an underlying array. 
    - Modifying the elements of a slice will modify the elements of the underlying array and vice versa.

- Just like there are array literals, there are also *slice literals*: `q := []int{2, 3, 5, 7, 11, 13}`.
    - A slice literal is like a reference to an array literal.

- When declaring a slice with the `a[low : high]` syntax, both `low` and `high` are *optional*. The default value for `low` is `0` and the default value of `high` is the upper bound of the array.
    - To declare a slice containing *all* of the array `var a [10]int`, the follow are equivalent: `a[0:10]`, `a[:10]`, `a[0:]`, `a[:]`.

- A slice has two important properties, a *length* and a *capacity*, which can be acessed with the built-in functions `len` and `cap`. [Also, Go has built-in functions apparently.]
    - The length of a slice is the number of elements it contains.
    - The capacity of a slice is the number of elements between the first element of the slice and the last element of the underlying array.

- A slice can be extended by *re-slicing* it, provided that the new length is less than or equal to it's capacity. For example,
```go
a := [6]int{2, 3, 5, 7, 11, 13}

s :=  a[2:] //cap(s) = 4

s = s[:0] // Slice the slice to give it zero length

s = s[:4] // Extend its length to something no more than cap(s)
```


- The zero value of a slice is `nil`. A nil slice has a length and capacity of `0`.

- The built-in function `make` can be used to create dynamically sized array, i.e. an array whose size is determined at runtime.
    - `make` allocates an array and returns a slice that refers to that array.
    - Use it like this: `b := make([]int, l, c)` to declare a slice with length `l` and capacity `c`.
    - Or leave off the capacity: `b := make([]int, l)`

- Slices can contain other slices as elements, like multi-dimensional arrays. For example, this program prints a tic-tac-toe board:
```go
package main

import (
    "fmt"
    "strings"
)

func main() {
    // Create a tic-tac-toe board.
    board := [][]string{
        []string{"_", "_", "_"},
        []string{"_", "_", "_"},
        []string{"_", "_", "_"},
    }

    // The players take turns.
    board[0][0] = "X"
    board[2][2] = "O"
    board[1][2] = "X"
    board[1][0] = "O"
    board[0][2] = "X"

    for i := 0; i < len(board); i++ {
        fmt.Printf("%s\n", strings.Join(board[i], " "))
    }
}
```

- The built-in function `append` allows you to append elements to a slice. It takes as arguments a single slice and an arbitrary number of new elements to append. For example:
```go
var s []int
s = append(s, 0)
s = append(s, 1, 2)
```


- You can loop over a slice with the `for` statement using the `range` form.
    - `range s` returns two values, the first is the index we are currently on, and the second is a copy of the element of `s` at that index. For example:
```go
var s = []int{10, 20, 30}

func main() {
    for i, v := range s {
        fmt.Printf("The value of s at index %d is %d\n", i, v)
    }
```

