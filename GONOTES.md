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

