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

// if also doesn't need ()
func sqrt(x float64) string {
	if x < 0 {
		return sqrt(-x) + "i"
	}
	return fmt.Sprint(math.Sqrt(x))
}

// like for, if can start w a short statement
func pow(x, n, lim float64) float64 {
	if v := math.Pow(x, n); v < lim {
		return v
	}
	return lim
}

func pow(x, n, lim float64) float64 {
	if v := math.Pow(x, n); v < lim {
		return v
	} else {
		fmt.Printf("%g >= %g\n", v, lim)
	}
	// can't use v here, though
	return lim
}

// Go only runs the selected case, not all the cases that follow. 
// In effect, the break statement that is needed at the end of each case in those languages is provided automatically in Go. 
// Another important difference is that Go's switch cases need not be constants, and the values involved need not be integers.

func switchCases() {
	fmt.Print("Go runs on")
	switch os := runtime.GOOS; os {
		case "darwin":
			fmt.Println("OS X.")
		case "linux":
			fmt.Println("Linux")
		default:
			// freebsd, openbsd, plan9, windows...
			fmt.Printf("%s. \n", os)
	}
}

func saturday() {
	fmt.Println("When's Saturday?")
	today := time.Now().Weekday()
	switch time.Saturday {
		case today + 0: 
			fmt.Println("Today")
		case today + 1: 
			fmt.Println("Tomorrow")
		case today + 2: 
			fmt.Println("In two days")
		default:
			fmt.Println("Too far away")
	}
}

// Switch without a condition is the same as switch true
func switchTrue() {
	t := time.Now()
	switch {
	case t.Hour() < 12:
		fmt.Println("Good morning!")
	case t.Hour() < 17:
		fmt.Println("Good afternoon.")
	default:
		fmt.Println("Good evening.")
	}
}


// A defer statement defers the execution of a function until the surrounding function returns.
func deferFunc() {
	// Deferred function calls are pushed onto a stack. When a function returns, its deferred calls are executed in last-in-first-out order.
	for i := 0; i < 10; i++ {
		defer fmt.Println(i)
	}
	
	defer fmt.Println("world")

	fmt.Println("hello")
}

// The type *T is a pointer to a T value. Its zero value is nil.
// The & operator generates a pointer to its operand.
// The * operator denotes the pointer's underlying value.
func pointers() {
	i, j := 42, 2701

	p := &i         // point to i
	fmt.Println(*p) // read i through the pointer
	*p = 21         // set i through the pointer
	fmt.Println(i)  // see the new value of i

	p = &j         // point to j
	*p = *p / 37   // divide j through the pointer
	fmt.Println(j) // see the new value of j
}

// A struct is a collection of fields.
type Vertex struct {
	X int
	Y int
}

// Struct fields are accessed using a dot
func printStruct() {
	v := Vertex{1, 2}
	v.X = 4
	// you can write p.X instead of (*p).X
	p := &v
	fmt.Println(p.X)
}

// struct literal denotes a newly allocated struct value by listing field values
// order of named fields is irrelevant, eg: Vertex{y: 1, x:2}
var (
	v1 = Vertex{1, 2}  // has type Vertex
	v2 = Vertex{X: 1}  // Y:0 is implicit
	v3 = Vertex{}      // X:0 and Y:0
	p  = &Vertex{1, 2} // has type *Vertex
)

func arrays() {
	// declare a as array of 2 strings
	var a [2]string
	a[0] = "Hello"
	a[1] = "World"
	fmt.Println(a[0], a[1])
	fmt.Println(a)

	// arrays cannot be resized
	primes := [6]int{2, 3, 5, 7, 11, 13}
	fmt.Println(primes)
}

func slice() {
	primes := [6]int{2, 3, 5, 7, 11, 13}
	// slice can be resized, is declared as [low:high], which excludes the last one
	var s []int = primes[1:4]
	fmt.Println(s) // [3 5 7]
}

// slice does not store data, just describe a slice of underlying array.
// changing the slice element changes the corresponding element in array
// other slices sharing the same element will see that change
func sliceChange() {
	names := [4]string { "Alice", "Ben", "Candy", "Darren" }
	fmt.Println(names)
	
	a := names[0:2] // [Alice Ben]
	b := names[1:3] [Ben Candy]
	
	b[0] = "XXX"
	fmt.Println(a) // [Alice XXX]
	fmt.Println(names) [Alice XXX Candy Darren]
}

// A slice literal is like an array without length
// Array literal: [3]bool{true, false, false}
// this creates the array then builds the slice: []bool{true, false, false}
func sliceLiteral() {
	q := []int{2, 3, 5, 7, 11, 13}
	fmt.Println(q)

	r := []bool{true, false, true, true, false, true}
	fmt.Println(r)

	s := []struct {
		i int
		b bool
	}{
		{2, true},
		{3, false},
		{5, true},
		{7, true},
		{11, false},
		{13, true},
	}
	fmt.Println(s)
}

// you can omit the low and high bounds and use defaults of [0: length]
var a [10]int
// these are equivalent
a[0:10]
a[:10]
a[0:]
a[:]

// a slice has both length and capacity, capacity is the no of elements in underlying array
// len and capacity can be obtained via len(s) and cap(s)
// slice can be extended by re-slicing
func reslice() {
	s := []int{2, 3, 5, 7, 11, 13}
	printSlice(s)

	// Slice the slice to give it zero length.
	s = s[:0]
	printSlice(s)

	// Extend its length.
	s = s[:4]
	printSlice(s)

	// Drop its first two values.
	s = s[2:]
	printSlice(s)
}

// zero value of slice is nil
// slice can be create wit make
// make allocates a zeroed array and returns a slice that refers to that array
a := make([]int, 5) // len(a) = 5, [0 0 0 0 0]
// To specify capacity, pass a third arg
b := make([]int, 0, 5) // len(b)=0, cap(b)=5
b = b[:cap(b)] // len(b)=5, cap(b)=5
b = b[1:] // len(b)=4, cap(b)=4

// slices can contain any types, including slices
func tictactoe() {
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

// append: func append(s []T, vs ...T) []T
// The first parameter s of append is a slice of type T, and the rest are T values to append to the slice

func appendSlice() {
	var s []int
	printSlice(s) // len=0 cap=0 []

	// append works on nil slices.
	s = append(s, 0)
	printSlice(s) // len=1 cap=1 [0]

	// The slice grows as needed.
	s = append(s, 1)
	printSlice(s) // len=2 cap=2 [0 1]

	// We can add more than one element at a time.
	s = append(s, 2, 3, 4)
	printSlice(s) // len=5 cap=6 [0 1 2 3 4]
}

// range iterates over a slice or map
// range over slice returns index and element
var pow = []int{1, 2, 4, 8, 16, 32, 64, 128}
func rangeOverSlice() {
	// i is index, v is element
	for i, v := range pow {
		fmt.Printf("2**%d = %d\n", i, v)
	}
}

// skip index of rnage by assigning to _ or omit
// for i, _ := range pow
// for _, value := range pow
// for i := range pow

func main() {
    // exported vars are capitalized because Go exports any capitalized var
    fmt.Println("hello")
}
```
