## Avoid using ELSE, write a better code

At the beginning of our career, every developer learns the ```if else``` conditionals and immediately start using it everywhere.  I would say it is normal because the use case is quite simple to understand, "**if** the condition is true, then do this, otherwise, do something **else**".

You can even easily identify a code written by a junior developer just reading all the ```if else``` used in their code. This is normal and part of the learning process. But as we gain more experience we learn better ways to do the same things.

The problem with the ```else``` is that it doesn't have the proper context as it's own, and you always must back to ```if``` to remember what it does. And, in many cases, it is completely useless, as I will show you.

## The not necessary case
Consider the following code:
```
func multiply(ifExists value: Int?, by: Int = 2) -> Int {
	if let value = value {
		return value * by
	} else {
		return 0
	}
}
```
It's a simple Swift function with an optional parameter (the **Int?** describes it if you are new to Swift).
If the parameter is not **nil** (mainly known as **null**) multiply it by the second parameter (which is not optional but have a default value **2**). But if the parameter is *nil* then return **zero** as a result.

Wait a minute... is the ```else``` statement necessary in this code? It is not. We can easily refactor this to return right after the ```if``` statement:
```
func multiply(ifExists value: Int?, by: Int = 2) -> Int {
	if let value = value {
		return value * by
	}
	return 0
}
```
Much cleaner, right?

In Swift, you can also use **guard**, if you prefer (I like this one):
```
func multiply(ifExists value: Int?, by: Int = 2) -> Int {
	guard let value = value else { return 0 }
	return value * by
}
```

## The return earlier case
Every time you collect user input you will need to validate it before using it. It is very common to see codes like that:
```
func submit(email: String, phone: String) -> Void {
	if isValidEmail(email) {
		if isValidPhone(phone) {
			doSomething(email: email, phone: phone)
		} else {
			displayPhoneError()
		}
	} else {
		displayEmailError()
	}
}
```
Two properties and two ```else``` cases. It works, but we have to follow two indentation levels to read every line and understand what is going on.

I think it is better to read if we refactor the function to:
```
func submit(email: String, phone: String) -> Void {
	if !isValidEmail(email) {
		displayEmailError()
		return
	}
	if !isValidPhone(phone) {
		displayPhoneError()
		return
	}
	
	doSomething(email: email, phone: phone)
}
```
Now we are using ```return``` inside ```if``` to stop the execution of the function when the input is invalid. This way we maintain our code with just one indentation level, which is easy to read top to bottom.

## Conclusion
The ```else``` statement is a tool we have to write code but most of the time it is useless and you should avoid using it. We have other (and better) options.

Every time I write ```else``` in my code I immediately ask myself: is it really necessary? And then most of the time I just refactor it to remove it.