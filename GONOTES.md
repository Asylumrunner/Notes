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
* A map is a set of key, value parings. The syntax for the make is as follows
``` go
m = make(map[key-type]value-type)
```
* Here's some crazy shit: you can check if a key is present in a map by just expecting two return types, like this:
``` go
elem, ok = m['key']
```
* If elem is there, elem contains that value and OK is true. If not, elem is the zero value of the type, and ok is false. That seems v good
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
* You can create a method, which has a special receiver given between "func" and the method name
  * Basically, you're stapling a function onto a type