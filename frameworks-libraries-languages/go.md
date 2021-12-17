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

// zero value for var if not initialized, eg: var i int
// 0 for numeric
// false for boolean
// "" for string

// T(v) converts v to type T
i := 42
f := float64(i)
u := uint(f)

// numeric type is derived from the precision
i := 42           // int
f := 3.142        // float64
g := 0.867 + 0.5i // complex128

// const for constants
// cannot be declared with :=
const World = "世界"

// Numeric constants are high-precision values.
// An untyped constant takes the type needed by its context.

// Go only has one looping construct: for
// structure of for loop: init, condition, post
// Note: Unlike other languages like C, Java, or JavaScript
// there are no parentheses surrounding the three components 
// of the for statement and the braces { } are always required.
func loop() {
	sum := 0
	for i := 0; i < 10; i++ {
		sum += i
	}
	fmt.Println(sum)
}

// init and post in for loop are optional
func loop2() {
	sum := 1
	for ; sum < 1000; {
		sum += sum
	}
	fmt.Println(sum)
}

// therefore, for is while in Go
func whileLoop() {
	sum := 1
	for sum < 1000 {
		sum += sum
	}
	fmt.Println(sum)
}

// this runs forever
func infiniteLoop() {
	sum := 1
	for {
		sum += sum
	}
	fmt.Println(sum)
}


func main() {
    // exported vars are capitalized because Go exports any capitalized var
    fmt.Println("hello")
}
```
