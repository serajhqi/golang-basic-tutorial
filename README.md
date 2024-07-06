## Package
1. A package is a collection of source files located in the same directory. All source files in a directory must share the same package name. 
2. When a package is imported, only entities (functions, types, variables, constants) whose names start with a `capital letter` can be used / accessed.
3. The recommended style of naming in Go is that identifiers will be named using `camelCase`, except for those meant to be accessible across packages which should be `PascalCase`.

```go
package greeting

// Hello is a public function (callable from other packages).
func Hello(name string) string {
    return "Hello " + name
}

// hello is a private function (not callable from other packages).
func hello(name string) string {
    return "Hello " + name
}
```
## Comments
1. Documenting comments are used by `godoc` command, which extracts these comments to create documentation about Go packages.
2. Variable docmuemt form: `// VariableName ...`
3. Package document form: `// Package x ...`
4. Function document form: `// FunctionName ...`
5. End the document with a period `(.)`
```go
// TemperatureCelsius represents a certain temperature in degrees Celsius.
var TemperatureCelsius float64

// Package kelvin provides tools to convert
// temperatures to and from Kelvin.
package kelvin

// CelsiusFreezingTemp returns an integer value equal to the temperature at which water freezes in degrees Celsius.
func CelsiusFreezingTemp() int {
	return 0
}
```
## Numbers

1. `int`: A signed integer that is at least 32 bits in size.  Most modern computers are 64 bit, therefore int will be 64 bits in size.
2. `float34`: Contains the set of all 34-bit floating-point numbers.
3. `float64`: Contains the set of all 64-bit floating-point numbers.
4. `uint`: e.g. 0, 255. An unsigned integer that is the same size as int.
5. `Converting` example:

```go
var x int = 42
f := float64(x)
```
## About Arithmetic Operators

In many languages you can perform arithmetic operations on different types of variables, but in Go this gives an error. For example:

```go
var x int = 42

// this line produces an error
value := float32(2.0) * x // invalid operation: mismatched types float32 and int

// you must convert int type to float32 before performing arithmetic operation
value := float32(2.0) * float32(x)
```

> Also, only the postfix notation is allowed (ie. **no** ++a or --a).

## Strings

1. Strings can be concatenated via the `+`
2. The strings package contains many useful functions to work on strings 


## Conditions

1. If statements can also include a short initialization statement that can be used to initialize one or more variables for the if statement. For example:

```go
num := 7
if v := 2 * num; v > 10 {
    fmt.Println(v)
} else {
    fmt.Println(num)
}
// Output: 14
```

> Note: any variables created in the initialization statement go out of scope after the end of the if statement.

2. The comparison operators above can also be used to compare strings. In that case a lexicographical (dictionary) order is applied. For example:

```go
"apple" < "banana"  // true
"apple" > "banana"  // false
```

3. The most commonly used function is Sprintf, which uses verbs like %s to interpolate values into a string and returns that string.
> In Go floating point values are conveniently formatted with Sprintf's verbs: %g (compact representation), %e (exponent) or %f (non exponent). All three verbs allow the field's width and numeric position to be controlled.

## Slices

Slices in Go are similar to lists or arrays in other languages. They hold several elements of a specific type (or interface).

Slices in Go are based on arrays. Arrays have a fixed size. A slice, on the other hand, is a dynamically-sized, flexible view of the elements of an array.

1. Define
```go
var empty []int                 // an empty slice
withData := []int{0,1,2,3,4,5}  // a slice pre-filled with some data
```
2. Get or Set:

```go
withData[1] = 5
x := withData[1] // x is now 5
```
3. Create new slice from the another slice:

```go
newSlice := withData[2:4]
// => []int{2,3}
newSlice := withData[:2]
// => []int{0,1}
newSlice := withData[2:]
// => []int{2,3,4,5}
newSlice := withData[:]
// => []int{0,1,2,3,4,5}
```

4. Add to the slice:

```go
a := []int{1, 3}
a = append(a, 4, 2)
// => []int{1,3,4,2}
```

5. Merge:

```go
nextSlice := []int{100,101,102}
newSlice  := append(withData, nextSlice...)
// => []int{0,1,2,3,4,5,100,101,102}
```

6. Index 

    Working with indexes of slices should always be protected in some way by a check that makes sure the index actually exists. `Failing to do so will crash the entire application.`

7. Empty slices

    `nil`-slices are the default empty slice. They have no drawbacks towards a slice with no values in them. The `len` function works on `nil`-slices, items can be added without initializing it, and so on. If creating a new slice prefer `var s []int` (`nil`-slice) over `s := []int{}` (empty, non-`nil` slice).
 
8. Performance

    When creating slices to be filled iteratively, there is a low-hanging fruit to improve performance, if the final size of the slice is known. The key is to minimize the number of times memory has to be allocated, which is rather expensive and happens if the slice grows beyond its allocated memory space. The safest way to do this is to specify a capacity `cap` for the slice with `s := make([]int, 0, cap)` and then `append` to the slice as usual. This way the space for `cap` amount of items is allocated immediately while the slice length is zero. In practice, `cap` is often the length of another slice: `s := make([]int, 0, len(otherSlice))`.

9. Append is not a pure function

    The `append function of Go is optimized for performance` and therefore does not make a copy of the input slice. This means that the original slice (1st parameter in append) will be changed sometimes.

10. Variadic Functions

    Usually, functions in Go accept only a fixed number of arguments. However, it is also possible to write variadic functions in Go.

    **A variadic function is a function that accepts a variable number of arguments.**

    If the type of the last parameter in a function definition is prefixed by `ellipsis ...`, then the function can accept any number of arguments for that parameter.
    
    ```go
    func find(a int, b ...int) {
        // ...
    }
    ```

    In the above function, parameter b is variadic and we can pass 0 or more arguments to b.

    ```go
    find(5, 6)
    find(5, 6, 7)
    find(5)
    ```
    > The variadic parameter must be the last parameter of the function.

    The way variadic functions work is by converting the variable number of arguments to a slice of the type of the variadic parameter.

    ```go
    func find(num int, nums ...int) {
    fmt.Printf("type of nums is %T\n", nums)

    for i, v := range nums {
        if v == num {
            fmt.Println(num, "found at index", i, "in", nums)
            return
        }
    }

        fmt.Println(num, "not found in ", nums)
    }

    func main() {
        find(89, 90, 91, 95)
        // =>
        // type of nums is []int
        // 89 not found in  [90 91 95]

        find(45, 56, 67, 45, 90, 109)
        // =>
        // type of nums is []int
        // 45 found at index 2 in [56 67 45 90 109]

        find(87)
        // =>
        // type of nums is []int
        // 87 not found in  []
    }
    ```

    Sometimes you already have a slice and want to pass that to a variadic function. This can be achieved by passing the slice followed by .... That will tell the compiler to use the slice as is inside the variadic function. The step described above where a slice is created will simply be omitted in this case.

    ```go
    list := []int{1, 2, 3}
    find(1, list...) // "find" defined as shown above
    ```

## Switch case

```go
operatingSystem := "windows"

switch operatingSystem {
case "windows":
    // do something if the operating system is windows
case "linux":
    // do something if the operating system is linux
case "macos":
    // do something if the operating system is macos
default:
    // do something if the operating system is none of the above
}
```
One interesting thing about switch statements, is that the value after the switch keyword can be omitted, and we can have boolean conditions for each case:

```go
age := 21

switch {
case age > 20 && age < 30:
    // do something if age is between 20 and 30
case age == 10:
    // do something if age is equal to 10
default:
    // do something else for every other case
}
```


## Struct

```go
type Shape struct {
    name string
    size int
}
```

Field names in structs follow the `Go convention` - fields whose name starts with a lower case letter are only visible to code in the same package, whereas those whose name starts with an upper case letter are visible in other packages.

1. Creating instances of a struct
    ```go
    s := Shape {
        name: "Square",
        size: 25,
    }
    ```
    > Fields that don't have an initial value assigned, will have their zero value.

    You can create an instance of a struct without using the field names, as long as you define the fields in order:
    
    ```go
    s := Shape {
        "Oval",
        20,
    }
    ```
    > However, this syntax is considered `brittle code` since it can break when a field is added, especially when the new field is of a different type.
2. Read or modify
    ```go
    // Update the name and the size
    s.name = "Circle"
    s.size = 35
    fmt.Printf("name: %s size: %d\n", s.name, s.size)
    // Output: name: Circle size: 35
    ```
3. "New" functions
    Sometimes it can be nice to have functions that help us create struct instances. By `convention`, these functions are usually called New or have their names starting with New.

    ```go
    func NewShape(name string) Shape {
        return Shape{
            name: name,
            size: 100, //default-value for size is 100
        }
    }

    NewShape("Triangle")
    ```
    Advantages:
    1. Validation of the given values
    2. Handling of default-values
    3. Since New functions are often declared in the same package of the structs they initialize, they can initialize even private fields of the struct

    Another way of creating a new instance of a struct is by using the new `built-in`:

    ```go
    s := new(Shape) // s will be of type *Shape (pointer to shape)
    fmt.Printf("name: %s size: %d\n", s.name, s.size)
    // Output: name:  size: 0
    ```
    > More often than not, you will not see new instances of structs created using the new builtin. Always prefer using a struct literal, a custom New function or any other function provided by the package that can help you build a struct. Use the new builtin to create new instances of structs as a `last resort`.

## Non-struct types

It's also possible to define non-struct types which you can use as an alias for a built in type declarations.

```go
type Name string
func SayHello(n Name) {
  fmt.Printf("Hello %s\n", n)
}
n := Name("Fred")
SayHello(n)
// Output: Hello Fred
```

## Randomness

Package math/rand

```go
import "math/rand"

n := rand.Intn(100)

f := rand.Float64() // f is a random float64, 0.0 <= f < 1.0


x := []string{"a", "b", "c", "d", "e"}
// shuffling the slice put its elements into a random order
rand.Shuffle(len(x), func(i, j int) {
	x[i], x[j] = x[j], x[i]
})
```

### Seeds

The number sequences generated by package math/rand are not truly random.
They are completely determined by an initial seed value. By default, the package uses the integer 1 as a seed.

In order to get a different sequence of random numbers, we need to specify a different seed value. A simple approach is to use the "current time":

```go
rand.Seed(time.Now().UnixNano())
```

Setting the seed must be done before calling any of the functions that generate random numbers. We usually only need to set the seed once in our program. From one seed we can generate many random numbers.

> If you don't call rand.Seed, then the package will produce the same sequence of random numbers each time your program runs!

## For loops

```go
for init; condition; post {
  // loop body - code that is executed repeatedly as long as the condition is true
}
```
>  Unlike other languages, there are no parentheses `()` surrounding the three components of the header. In fact, inserting such parenthesis is a compilation `error`. However, the braces `{ }` surrounding the loop body are `always` required.

## Functions

1. All parameters must be explicitly typed; there is no type inference for parameters. `There are no default values` for parameters so all function parameters are required.

2. Parameters of the same type can be declared together, followed by a single type declaration.

```go
func PrintGreetingName(greeting, name string) {
  fmt.Println(greeting + " " + name)
}
```

**Parameters vs. Arguments**

Let's quickly cover two terms that are often confused together: parameters and arguments. Function parameters are the names defined in the function's signature, such as `greeting` and `name` in the function PrintGreetingName above. Function arguments are the `concrete values` passed to the function parameters when we invoke the function. For instance, in the example below, `"Hello"` and `"Katrina"` are the arguments passed to the greeting and name parameters:

```go
PrintGreetingName("Hello", "Katrina")
```

3. There can be multiple return statements in a function. 

```go
func HelloAndGoodbye(name string) (string, string) {
  return "Hello " + name, "Goodbye " + name
}
```

**Named Return Values and Naked Return**

As well as parameters, return values can optionally be named. If named return values are used, a return statement without arguments will return those values. This is known as a 'naked' return.

```go
func SumAndMultiplyThenMinus(a, b, c int) (sum, mult int) {
    sum, mult = a+b, a*b
    sum -= c
    mult -= c
    return
}
```

**Pass by Value vs. Pass by Reference**

All arguments are passed by value in Go, i.e. a copy is made of the value or data provided to the function. But if you don't want to make a copy of the data that is passed to a function and want to change the data in the function, then you should use pointers as arguments, a.k.a. pass by reference.

```go
val := 2
func MultiplyByTwo(v int) int {
    v = v * 2
    return v
}
newVal := MultiplyByTwo(val)
// newval is 4, val is still 2 because only a copy of its value was passed into the function
```

```go
func HandlePointers(x, y *int) {
    // Some logic to handle integer values referenced by pointers x and y
}
```

**Exceptions**

Note that `slices` and `maps` are exceptions to the above-mentioned rule. When we pass a slice or a map as arguments into a function, they are treated as pointer types even though there is no explicit * in the type. `This means that if we pass a slice or map into a function and modify its underlying data, the changes will be reflected on the original slice or map`.

## Floating-point numbers

- `float32`: 32 bits (~6-9 digits precision).
- `float64`: 64 bits (~15-17 digits precision). This is the default floating-point type.

By default, Go will use `float64` for floating-point numbers, unless the floating-point number is:

1. assigned to a variable with type float32, or
2. returned from a function with return type float32, or
3. passed as an argument to the float32() function.

## Time

1. time.Now()
2. time.Parse()
3. Go has a special way of how you define the layout you expect for the parsing. You need to write an example of the layout using the values from this special timestamp: `Mon Jan 2 15:04:05 -0700 MST 2006`

For example:

```go
import "time"

func parseTime() time.Time {
    date := "Tue, 09/22/1995, 13:00"
    layout := "Mon, 01/02/2006, 15:04"

    t, err := time.Parse(layout,date) // time.Time, error
}
```

4. Time.Format() returns a string representation of time.

```go
import (
    "fmt"
    "time"
)

func main() {
    t := time.Date(1995,time.September,22,13,0,0,0,time.UTC)
    formatedTime := t.Format("Mon, 01/02/2006, 15:04") // string
    fmt.Println(formatedTime)
}

// => Fri, 09/22/1995, 13:00
```
> **`Important`** You need to write an example of the layout using the values from this special timestamp: Mon Jan 2 15:04:05 -0700 MST 2006.

-|-
-|-
Year |	2006 ; 06
Month |	Jan ; January ; 01 ; 1
Day |	02 ; 2 ; _2 (For preceding 0)
Weekday| 	Mon ; Monday
Hour |	15 ( 24 hour time format ) ; 3 ; 03 (AM or PM)
Minute| 	04 ; 4
Second |	05 ; 5
AM/PM |Mark 	PM
Day of Year |	002 ; __2

5. Hour: `Time.Hour()` , Month : `
`

6. The time includes another type, Duration, representing elapsed time, plus support for locations/time zones, timers, and other related functionality that will be covered in another concept.

## Map
To create a map, you can do:

```go
map[KeyType]ElementType

// With map literal
foo := map[string]int{}

// or with make function
foo := make(map[string]int)
```
```go
// Add a value in a map with the `=` operator:
foo["bar"] = 42

// Here we update the element of `bar`
foo["bar"] = 73

// To retrieve a map value, you can use
baz := foo["bar"]

// To delete an item from a map, you can use
delete(foo, "bar")
```
- If you try to retrieve the value for a key which does not exist in the map, it will return the `zero value` of the value type. This can confuse you, especially if the `default value` of your ElementType (for example, 0 for an int), is a valid value. **To check whether a key exists in your map, you can use:**

```go
 value, exists := foo["baz"]
  // If the key "baz" does not exist,
  // value: 0; exists: false
```

## Range

In Go, you can iterate over a slice using for and an index, or you can use range. range also allows you to iterate over a map or a channel.

Every iteration returns two values: the index/key and a copy of the element at that index/key.

```go
xi := []int{10, 20, 30}
for i, x := range xi {
  fmt.Println(i, x)
}
// outputs:
// 0, 10
// 1, 20
// 2, 30

hash := map[int]int{9: 10, 99: 20, 999: 30}
for k, v := range hash {
  fmt.Println(k, v)
}
// outputs, for example:
// 99 20
// 999 30
// 9 10
```

> It may seem the above output is incorrect, as one would expect the first key/value pair on the declaration of the map 9 10 to be the first one printed and not the last. However, maps are unordered by nature - there isn't a first or last key/value pair. Because of that, when iterating over the entries of a map, the order by which entries will be visited will be random and not follow any specific pattern. This means the above output is possible but might differ from what you get if you try to run this yourself. 

Last but not least, if you are required to perform some action but you are not interested in values nor keys of the slice or map, you can omit both index and value:

```go
xi := []int{10, 20, 30}
count := 0
for range xi {
  count++
}
// count value:
// 3
```

## Pointers

```go
var p *int // 'p' contains the memory address of an integer
```

- Here we declare a variable p of type "pointer to int" (*int). This means that p will hold the memory address of an integer. 
- The zero value of pointers is `nil` because a `nil pointer` holds no memory address.
- To find the memory address of the value of a variable, we can use the `&` operator.

```go
var a int
a = 2

var p *int
p = &a // the variable 'p' contains the memory address of 'a'
```

### Dereferencing

```go
var a int
a = 2

var p *int
p = &a // the variable 'p' contains the memory address of 'a'

var b int
b = *p // b == 2
```
The operation *p fetches the value stored at the memory address stored in p. This operation is often called "dereferencing".

We can also use the dereference operator to assign a new value to the memory address referenced by the pointer:

```go
var a int        // declare int variable 'a'
a = 2            // assign 'a' the value of 2

var pa *int
pa = &a          // 'pa' now contains to the memory address of 'a'
*pa = *pa + 2    // increment by 2 the value at memory address 'pa'

fmt.Println(a)   // Output: 4
                 // 'a' will have the new value that was changed via the pointer!
```

Assigning to *pa will change the value stored at the memory address pa holds. Since pa holds the memory address of a, by assigning to *pa we are effectively changing the value of a!

> A note of caution however: always check if a pointer is not `nil` before dereferencing. Dereferencing a nil pointer will make the program crash at runtime!

```go
var p *int // p is nil initially
fmt.Println(*p)
// panic: runtime error: invalid memory address or nil pointer dereference
```

### Pointers to structs

```go
type Person struct {
    Name string
    Age  int
}

var peter Person
peter = Person{Name: "Peter", Age: 22}

var p *Person
p = &peter


// or

var p *Person
p = &Person{Name: "Peter", Age: 22}
```
Slices and maps are special types because they already have pointers in their implementation. 

> However, actions that return a new slice like `append` are a special case and might not modify the slice outside of the function.

Some Examples: 

```go
package electionday
import ("fmt")
// NewVoteCounter returns a new vote counter with
// a given number of initial votes.
func NewVoteCounter(initialVotes int) *int {
	return &initialVotes
}

// VoteCount extracts the number of votes from a counter.
func VoteCount(counter *int) int {
    if counter == nil {
        return 0
    }
	return *counter
}

// IncrementVoteCount increments the value in a vote counter.
func IncrementVoteCount(counter *int, increment int) {
	*counter = *counter + increment
}

// NewElectionResult creates a new election result.
func NewElectionResult(candidateName string, votes int) *ElectionResult {
    result := ElectionResult {
        Name: candidateName,
        Votes: votes,
    }
    return &result
}

// DisplayResult creates a message with the result to be displayed.
func DisplayResult(result *ElectionResult) string {
    genStr := *result
	return genStr.Name + " (" + fmt.Sprint(genStr.Votes) + ")"
}

// DecrementVotesOfCandidate decrements by one the vote count of a candidate in a map.
func DecrementVotesOfCandidate(results map[string]int, candidate string) {
	value, exists := results[candidate] 
    if (exists){
        results[candidate] = value -1
    }
}
```

## Methods

A method is a function with a special receiver argument. The receiver appears in its own argument list between func keyword and the name of the method.

```go
func (receiver type) MethodName(parameters) (returnTypes) {

}
```
Example:

```go
type Person struct {
    Name string
}

func (p Person) Greetings() string {
	return fmt.Sprintf("Welcome %s!", p.Name)
}

s := Person{Name: "Bronson"}
fmt.Println(s.Greetings())
// Output: Welcome Bronson!
```

> There are two types of receivers, value receivers, and pointer receivers.

```go
type rect struct {
	width, height int
}
func (r *rect) squareIt() {
	r.height = r.width
}

r := rect{width: 10, height: 20}
fmt.Printf("Width: %d, Height: %d\n", r.width, r.height)
// Output: Width: 10, Height: 20

r.squareIt()
fmt.Printf("Width: %d, Height: %d\n", r.width, r.height)
// Output: Width: 10, Height: 10
```

## Runes

The rune type in Go is an alias for int32. Given this underlying int32 type, the rune type holds a signed 32-bit integer value. However, unlike an int32 type, the integer value stored in a rune type represents a single Unicode character.

### UTF-8

UTF-8 is a variable-width character encoding that is used to encode every Unicode code point as 1, 2, 3, or 4 bytes. Since a Unicode code point can be encoded as a maximum of 4 bytes, the rune type needs to be able to hold up to 4 bytes of data. That is why the rune type is an alias for int32 as an int32 type is capable of holding up to 4 bytes of data.

> Go source code files are encoded using UTF-8.


- Since the rune type represents a Unicode character, a string in Go is often referred to as a sequence of runes.

- Since runes can be stored as 1, 2, 3, or 4 bytes, the length of a string may not always equal the number of characters in the string. 
- A character within single quotes is a rune in Go.

```go
myString := "❗hello"
for index, char := range myString {
  fmt.Printf("Index: %d\tCharacter: %c\t\tCode Point: %U\n", index, char, char)
}
// Output:
// Index: 0	Character: ❗		Code Point: U+2757
// Index: 3	Character: h		Code Point: U+0068
// Index: 4	Character: e		Code Point: U+0065
// Index: 5	Character: l		Code Point: U+006C
// Index: 6	Character: l		Code Point: U+006C
// Index: 7	Character: o		Code Point: U+006F
```

```go
import "unicode/utf8"

myString := "❗hello"
stringLength := len(myString)
numberOfRunes := utf8.RuneCountInString(myString)

fmt.Printf("myString - Length: %d - Runes: %d\n", stringLength, numberOfRunes)
// Output: myString - Length: 8 - Runes: 6
```

## Interface

In its simplest form, an interface type is a set of method signatures. Here is an example of an interface definition that includes two methods `Add` and `Value`:

```go
type Counter interface {
    Add(increment int)
    Value() int
}
```
- Interface names in Go do not contain the word Interface or I. Instead, they often end with er, e.g. Reader, Stringer.

### Implementing an interface

Any type that defines the methods of the interface automatically implicitly "implements" the interface. There is no implements keyword in Go.

The following type implements the `Counter` interface we saw above.

```go
type Stats struct {
    value int
    // ...
}

func (s Stats) Add(v int) {
    s.value += v
}

func (s Stats) Value() int {
    return s.value
}

func (s Stats) SomeOtherMethod() {
    // The type can have additional methods not mentioned in the interface.
}
```

> The following type implements the Counter interface we saw above.

## Zero Values

Variables declared without an explicit initial value default to the zero value for their respective type.

Type |	Zero Value
-|-
boolean |	false
numeric |	0
string 	|""
pointer |	nil
function |	nil
interface |	nil
slice 	|nil
channel |	nil
map 	|nil

You may have noticed struct types are absent from the above table. That is because the zero value for a struct type depends on its fields. Structs are set to their zero value when all of its fields are set to their respective zero value.
