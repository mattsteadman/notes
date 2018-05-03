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
