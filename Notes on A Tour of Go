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

- Return values can be named. Named returned values are only defined *within* the function whence they are returned. If you name your return value(s), then you don't need to name the variable to be returned in your return statement, you can just say `return`. This is called a *naked return statement*, use them with care as they can make things more confusing in long functions.
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
    - If you provide initial values for a variable, you can omit the type in the declaration and Go wil determine the type automatically. For example, `var i, j = 1, 2`

- Variables can also be assigned values in a list fashion. For example, `a, b, c = 1, 2, 3`.

- Inside of functions, statements like `var k = 3` can be shortened to `k := 3` using the `:=` construct. This is called a *short assignment statement*. A short assignment statement always uses implicit typing.
    - Outside of functions, every statement must begin with a keyword, so the short assignment statement can't be used.

- Go's *basic types* are `bool`, `string`, `byte` (alias for `uint8`), `rune` (alias for `int32`, used for Unicode code points), `int`,  `int8`, `uint8`  `int16`,  `int32`,  `int64`, `uint`, `uint8`, `uint16`, `uint32`, `uint64`, `uintptr`, `float32`, `float64`, `complex64`, `complex128`
    - The size of `int`, `uint` and `uintptr` depend on the machine word size. Like C, `int` is a pretty good choice for when you need an integer, unless you have a reason to use something else.

- The `var` statement can also be factored like the import statement. For example,
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

- Each type can be used like a function for type conversion. The expression `T(v)` returns the value `v` converted to the type `v`. For example, `f := float64(i)` declares `f` as a `i` converted to a `float64`.
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
