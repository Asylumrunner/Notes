# GONOTES

* Go code is contained in what are called packages
  * When you import, you can do so with individual import statements or a block import
  * By convention, the last thing in the import path is the name of the package you're importing
* Uppercase variables and functions are from external packages, lowercase from within the package
* Functions take zero or more arguments, return zero or more values
* Go functions look like this:
``` go
func function-name(argument argument-type)(<return-value-name> return value type)
```
* You can perform what's called a "naked" return by naming your return valus, using those names in the function code, then just saying "return"
  * This can harm readability in longer functions
* You can initialize a list of variables with values, and those values don't have to be the same type, as in `var c, python, java = true, false, "AAH!"`
  * Inside a function only, you can cheat a bit by using := to assign a variable to a value (Constants can't, though!)
* Go only has a for-loop, and no other looping construct (no while, no do-while)
  * A while can be emulated by simply including a for with only a conditional, and no setup or post
* Variables declared in an if statement are in scope until the end of the if (that includes else blocks)
* A defer statement evaluates its arguments immediately, but doesn't execute until the surrounding function completes
``` go
func main() {
	defer fmt.Println("world")
	fmt.Println("hello")
}
```

## What The Hell Are Slices
* Arrays are of immutable length, but a lot of usecases use slices, which are sort of like windows to an array
* Slices contain references to a certain span of array elements
* A slice has a length, which is the number of elements it contains, and a capacity, which is the distance from the first value the slice references to the end of the array
* On a related note, you create dynamically-sized arrays that start out zeroed out
``` go
a := make([]int, 5) //produces a slice to a zeroed, len = 5 array
```
* You can append to slices. If doing so would overflow the array, a new, properly-sized array is created and the slice is pointed to that

* Range is a form of for loop that iterates over a slice or map, each iteration obtaining an index, and the element at the index (HANDY)
  * Using an underscore, you can get just the index or just the value if you so choose

## I'm The Map, I'm The Map, I'm The Map
* A map is a set of key, value parings. The syntax for the make is as follows
``` go
m = make(map[key-type]value-type)
```
* Here's some crazy shit: you can check if a key is present in a map by just expecting two return types, like this:
``` go
elem, ok = m['key']
```
* If elem is there, elem contains that value and OK is true. If not, elem is the zero value of the type, and ok is false. That seems v good

## Functions Are First-Class Citizens
* Go supports first-class functions and closures (closures are basically functions that you can build on the spot, by providing some of the extra values then and there)
  * For instance, say I wanted to run a variety of mathematical functions against the same value, for multiple values. I could use a closure that looks something like this:
``` go
func math_doer(value int) func(func(int)) int{
	return func(fx func(int)) int{
		return fx(value)
	}
}
```
* This is, to the best of my knowledge, dark sorcery

## A Method To The Madness
* You can create a method, which has a special receiver given between "func" and the method name
  * Basically, you're stapling a function onto a type
``` go
func (v Vertex) Abs() float64
```
* You now can call v.Abs(), and it'll be valid
* The reciever type must be defined in the same package as the method
  * This means you can't make methods for built-in types
* Methods can have pointer recievers
  * Methods with pointer recievers can modify the value it points to
  * This is because, otherwise, arguments are passed by value in Go, and the reciever is an argument
  * General note: While functions with a pointer argument must take a pointer, methods with pointer values can take either a pointer or a value as the reciever
``` go
func (v *Vertex) Scale(f float64) { //some garbage }
var v Vertex
v.Scale(5) //all good
p := &v
p.Scale(1) //also good
```
  * This also works in reverse, and methods with value recievers can take either a value or a pointer
  * All methods on a given type should be either pointer or value, but not a mix

## Interfacing With Interfaces
* An interface is a set of method signatures
* A value of interface type can hold any value that implements those methods
* A type implements an interface by implementing its methods.
  * You don't need to write "implements" or anything
* An interface is sort of a tuple, connecting a value and a concrete type. That is, when you call a method on an interface value, it secretly calls the correct version of that method for the underlying type
  * This seems like it basically handles overloading functions/inheritance without actually doing it
  * Interfaces can be called with a nil reciever, which doesn't necessarily cause an exception. You can catch it
  * Interface with no methods is the empty interface. 
* You can assert the type of an interface value
  * If the interface doesn't hold that type, assertion of that type triggers that panic
  * You can use the type assertion syntax to construct a switch statement based on the interface's type
  * There is a ubiquitous interface called Stringer, which is a type that can describe itself as a string. Fmt looks for a stringer when it prints values. Basically, implementing a method called String() which returns a string lets you print your custom types using regular print statements

## Something Went Wrong
* Go expresses error state with error values
* error is an interface, which expects an Error() function that returns a string
* functions might return an error value, so errors should be handled by checking whether the error equals nil

## Reading Rainbow
* Reader is anothe interface, which is basically just a stream reader

## Anything but Routine
* A goroutine creates a thread. Literally all you have to do is put go before a function call
  * Heh. Get it?
* A channel is a typed pipe through which you can send and recieve values
  * You do through the channel operator, <-
* Channels can be buffered in initialization, and if you try to send to a filled channel, it blocks
* A sender can close a channel to indicate no more valus will be sent.
  * Recievers can test to see if a channel is closed
* select is sort of a switch statement, and it blocks until one of its cases can run
  * It chooses one at random if multiple are ready
  * select, like any case, can have a default
* We have access to mutexes as well

