# Go

```
// add package main to run as executable instead of library
package main

// can import with the below syntax or individually with `import "fmt"`
import (
    "fmt"
    "math/rand"
)

// func can take >= 0 args
// type is after arg name, and can be derived from last arg type
// the int outside refers to return type
func add(x, y int) int {
    return x + y
}

// func can return any number of results
func swap(x, y string) (string, string) {
     return y, x
 }

// return values can be named, and will be treated as var defined at top of func
// a return statement w/o arg returns the named return values (ie: naked return)
func split (sum int) (x, y int) {
    x = sum * 4 / 9
    y = sum - x
    return
}

// var to declare variable (can be list), type is last word
var c, python, java bool // false, false, false

// can also declare var with defaults
// if an initializer is present, the type can be omitted as it will be inferred
var i, j int = 1, 2

// inside func, := can be used instead of var
func assign() {
    k := 3
    fmt.Println(k)
}

// basic types:
// bool
// string
// int int8 int16 int32 int64 uint uint8 uint16 uint32 uint64 uintptr
// btye // alias for uint8
// rune // alias for int32, represents Unicode code point
// float32 float64
// complex64 complex128



func main() {
    // exported vars are capitalized because Go exports any capitalized var
    fmt.Println("hello")
}
```
