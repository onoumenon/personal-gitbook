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

// skip index of range by assigning to _ or omit
// for i, _ := range pow
// for _, value := range pow
// for i := range pow



// Map maps keys to values
// zero value of map is nil
// make function returns map of a given type
func makeMap() {
	m = make(map[string]Vertex)
	m["Bell Labs"] = Vertex{40, -99.2}	
}

// map literals are like struct literals, but keys are required
var m = map[string]Vertex{
	"Bell Labs": Vertex{1.0, 2.0},
	"Google": Vertex{37.42, -122.08}
	}
}

// if the top-level type is just a type name, you can omit it from the elements of literal
var m = map[string]Vertex{
	"Bell Labs": {1.0, 2.0},
	"Google": {37.42, -122.08}
	}
}

// Insert or update an element in map m:
m[key] = elem

// Retrieve an element:
elem = m[key]

// Delete an element:
delete(m, key)

// Test that a key is present with a two-value assignment:

elem, ok = m[key]
// If key is in m, ok is true. If not, ok is false.

// If key is not in the map, then elem is the zero value for the map's element type.

// Note: If elem or ok have not yet been declared you could use a short declaration form:
elem, ok := m[key]

// Function are values too, can be passed around
// Function values can be used as function arguments and return values

func compute(fn func(float64, float64) float64) float64 {
	return fn(3, 4)
}

// Go functions may be closures, that references var outside its body
// for eg, the adder func returns a closure
func adder() func(int) int {
	sum := 0
	return func(x int) int {
		sum += x
		return sum
	}
}

// the above returns
// 0 0
// 1 -2
// 3 -6
// 6 -12
// 10 -20
// 15 -30
// 21 -42
// 28 -56
// 36 -72
// 45 -90

// Go have no class, but you can define method on types
// A method is a function with a special receiver arg
// The receiver appears in its own arg list between the function keyword and the method name

type Vertex struct {
	X, Y float64
}

// Abs method has receiver of type Vertex named v
func (v Vertex) Abs() float64 {
	return math.Sqrt(v.X * v.X + v.Y * v.Y)
}
// invoke via 
// v:= Vertex{3, 4}
// v.Abs()

// You can declare a method on a non-struct type too
// You can only declare a method with a receiver whose type is defined in the same package
// therefore you can't declare a method of built-in types

type MyFloat float64

func (f MyFloat) Abs() float64 {
	if f < 0 {
		return float^4(-f)
	}
	return float64(f)
}

// you can declare methods with pointer receivers
// *T literal syntax for type T, where T cannot itself be a pointer
// methods with pointer receivers can modify the value that's pointed. 

func (v *Vertex) Scale(f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}

// v := Vertex{3, 4}
// v.Scale(10)

// Go interpretes v.Scale(5) as (&v).Scale(5) if a pointer is expected
// Why pointer?
// so the method can modify the value its receiver points to
// so as to avoid copying the value on each method call
// all methods on a given type should have either value or pointer receivers, not mixed

// An interface type is defined as a set of method signatures
// A value of interface type can hold any value that implements those methods

type Abser interface {
	Abs() float64
}

func main() {
	var a Abser
	f := MyFloat(-math.Sqrt2)
	v := Vertex{3, 4}

	a = f  // a MyFloat implements Abser
	a = &v // a *Vertex implements Abser



	fmt.Println(a.Abs())
}

type MyFloat float64

func (f MyFloat) Abs() float64 {
	if f < 0 {
		return float64(-f)
	}
	return float64(f)
}

type Vertex struct {
	X, Y float64
}

func (v *Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

// A type implements an interface by implementing its methods
// no "implements" keyword

type I interface {
	M()
}

type T struct {
	S string
}

// this method means type T implements the interface I,
// but we don't need explicit declaration
func (t T) M() {
	fmt.Println(t.S)
}

var i I = T{"hello"}
i.M()

// under the hood, interface values can be thought of as tuples with value and type:
// (value, type)
// an interface value holds a value of a specific underlying concrete type
// calling a method on a interface value executes the method on its type

type I interface {
	M()
}

type T struct {
	s String
}

func (t *T) M() {
	fmt.Println(t.S)
}

type F float64

func (f F) M() {
	fmt.Println(f)
}

func describe(i I) {
	fmt.Printf("(%v, %T)\m", i, i)
}

var i I
i = &T{"Hello"}
describe(i) // (&{Hello}, *main.T)
i.M() // Hello

i = F(math.Pi)
describe(i) // (3.14159, main.F)
i.M() // 3.14159

// Interface with underlying nil value
// if the concrete value inside the interface is nil, the method will be called with a nil receiver
// common to write methods that gracefully handles nil receiver
// interface value that holds a nil concrete value is non-nil

func (t *T) M() {
	if t == nil {
		fmt.Println("<nil>")
		return
	}
	fmt.Println(t.S)
}

// Nil interface values
// nil interface value holds neither value nor concrete type
// calling a method on a nil interface is a run-time error, because there is no type

// Empty interface
// interface type that specifies zero methods:
// interface{}
// empty interface may hold values of any type, every type implements at least 0 methods
// empty interfaces are used to handle values of unknown type. eg: fmt.Print takes any no of args of type interface{}

var i interface{}
describe(i) // (<nil>, <nil>)
i = 42
describe(i) // (42, int)
i = "hello"
describe(i) // (hello, string)

func describe(i interface{}) {
	fmt.Printf("(%v, %T)\n", i, i)
}

// Type assertions
// a type assertion provides access to an interface's value underlying type
// t := i.(T)
// above statement asserts that the interface value i hold concrete type T and assigns the underlying value to t
// if i does not hold a T, the statement will trigger a panic
// to test if an interface value holds a specific type, type assertion returns two value:
// the underlying value and a boolean that reports if assertion suceeds
// t, ok := i.(T)

func main() {
	var i interface{} = "hello"

	s := i.(string)
	fmt.Println(s)

	s, ok := i.(string)
	fmt.Println(s, ok)

	f, ok := i.(float64)
	fmt.Println(f, ok)

	f = i.(float64) // panic
	fmt.Println(f)
}

// Type switch
// a construct that permits several type assertions in series
// like regular switch statement, but the cases are types
// declaration in switch statement has same syntax as type assertion, 
// but specific type T is replaced with the keyword type
	
func do(i interface{}) {
	switch v := i.(type) {
	case int:
		fmt.Printf("Twice %v is %v\n", v, v*2)
	case string:
		fmt.Printf("%q is %v bytes long\n", v, len(v))
	default:
		fmt.Printf("I don't know about type %T!\n", v)
	}
}

// Stringer, a common interface defined by fmt
type Stringer interface {
    String() string
}
// is a type that can describes itself as a string

type Person struct {
	Name string
	Age  int
}

func (p Person) String() string {
	return fmt.Sprintf("%v (%v years)", p.Name, p.Age)
}

func main() {
	a := Person{"Arthur Dent", 42}
	z := Person{"Zaphod Beeblebrox", 9001}
	fmt.Println(a, z)
}

// Stringers exercise
type IPAddr [4]byte

func (ip IPAddr) String() string {
	var s string
	for _, v := range ip {
		s += fmt.Sprintf("%d.", v)
	}
	return s[:len(s)-1]
}

func main() {
	hosts := map[string]IPAddr{
		"loopback":  {127, 0, 0, 1},
		"googleDNS": {8, 8, 8, 8},
	}
	for name, ip := range hosts {
		fmt.Printf("%v: %v\n", name, ip)
	}
}

// Go programs express error state with error values.

// The error type is a built-in interface similar to fmt.Stringer:

type error interface {
    Error() string
}

// (As with fmt.Stringer, the fmt package looks for the error interface when printing values.)

// Functions often return an error value, and calling code should handle errors by testing whether the error equals nil.

type MyError struct {
	When time.Time
	What string
}

func (e *MyError) Error() string {
	return fmt.Sprintf("at %v, %s", e.When, e.What)
}

func run() error {
	return &MyError{
		time.Now(),
		"it didn't work"
	}
}

if err := run(); err != nil {
	fmt.Println(err)
}

// io package has io.Reader interface, which represents the read end of a stream of data

// Go standard library contains many implementation of this interface, including files, network connection, compressors, ciphers, etc
// io.Reader has a Read method:
func (T) Read(b []byte) (n int, err error)

// Read populates the given byte slice with data and returns the no of bytes populated and an error value. io.EOF error is returned when stream ends.

func main() {
	r := strings.NewReader("Hello Reader")
	b := make([]byte, 8)
	for {
		n, err := r.Read(b)
		fmt.Printf("n = %v err = %v b = %v\n", n, err, b)
		fmt.Printf("b[:n] = %q\n", b[:n])
		if err == io.EOF {
			break
		}
	}
}

// Package image defines the Image interface:

package image

type Image interface {
    ColorModel() color.Model
    Bounds() Rectangle
    At(x, y int) color.Color
}
// Note: the Rectangle return value of the Bounds method is actually an image.Rectangle, as the declaration is inside package image.

// The color.Color and color.Model types are also interfaces, but we'll ignore that by using the predefined implementations color.RGBA and color.RGBAModel. These interfaces and types are specified by the image/color package

func main() {
	m := image.NewRGBA(image.Rect(0, 0, 100, 100))
	fmt.Println(m.Bounds())
	fmt.Println(m.At(0,0).RGBA())
}


func main() {
    // exported vars are capitalized because Go exports any capitalized var
    fmt.Println("hello")
}
```
