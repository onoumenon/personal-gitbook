# go crash course

![folder structure](<../../.gitbook/assets/Screenshot 2022-01-13 at 12.12.48 AM.png>)





{% embed url="https://go.dev/doc/code" %}

if using vsc, can install Go extension and other recommended



```
$ go get github.com/aws/aws-sdk-go/aws
```

will install aws folder in github/aws



main.go is the index

{% code title="01_hello/main.go" %}
```
package main

import "fmt"

func main() {
    fmt.Println("Hello World")
}
```
{% endcode %}

```
cd 01_hello
go install
// outputs binary in bin folder
```

```
cd 01_hello
go run main.go
// hello world is outputted
```

{% embed url="https://pkg.go.dev/fmt" %}

{% code title="02_var/main.go" %}
```
...

func main() {
    // Main types
    
    // string
    // bool
    // int
    // int int8 int16 int32 int64
    // uint uint8 ... (positive numbers)
    // byte - alias for uint8
    // rune - alias for int32
    // float32 float64(more common)
    // complex64 complex128
    
    var name string = "Brad" // string is inferred
    // so the below is valid
    var job = "Nurse"
    var age int32 = 30 // can state explicitly the int type
    
    const isCool = true // cannot be reassigned
    
    // shorthand
    name := "Brad" // needs to be inside func body
    size := 1.3 //float64
    
    // can combine
    name, email := "Tom", "tom@gmail.com"
    
            
    fmt.Println(name)
    fmt.Printf(%\n, age) // int
}
```
{% endcode %}

```
package main

import (
    "fmt"
    "math"
    "github.com/.../strutil"
)

func main() {
    fmt.Println(math.Floor(2.7))
    fmt.Println(math.Ceil(2.7))
    fmt.Println(math.Sqrt(16))
    fmt.Println(strutil.Reverse("hello"))
}
```

{% code title="strutil/reverse.go" %}
```
package stutil
// you can create utils in your own folder
func Reverse(s string) string {
    runes := []rune(s)
    for i, j := 0, len(runes)-1; i < j; i, j = i+1, j-1 {
        runes[i], runes[j] = runes[j], runes[i]
    }
    return string(runes)
}
```
{% endcode %}

```
package main

import "fmt"

func greeting(name string) string {
    return 
}

func main() {
    fmt.Println("Hello World")
}
```
