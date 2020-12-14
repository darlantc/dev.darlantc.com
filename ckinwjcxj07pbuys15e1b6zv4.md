## Never rely on Strings for conditionals

Almost all the time is a bad decision to use Strings in conditionals because you will have no help from the compiler to alert you when you typed them wrong.

Throw the first stone the developer who never lost hours of work debugging because the code doesn't work until he finds out that he typed an incorrect String...
```swift
// a utility function somewhere
func isTheMain(route: String) -> Bool {
	return route == "Main"
}

// used somewhere else...
let route = "main"
if isTheMain(route: route) {
	...
}
```
Can you spot the problem in the above code? Only the case of the string is different, enough to hide hard-to-find bugs. We can do better. We must write better code!

Sure, we can use constants to write the String just once and be safe using it. But I prefer to use enums because they add great flexibility to my code. Let's see it.

## Enum to the rescue
This kind of special data structure is very useful for many needs, especially in Swift where it is very powerful when used with Switch conditionals, for example. More on that later.

First, let's fix the previous code using enum:
```swift
enum Routes: String {
	case Main
	case AnotherRoute
}
func isTheMain(route: Routes) -> Bool {
	return route == .Main
}

// used somewhere else...
let route: Routes = .Main
if isTheMain(route: route) {
	...
}
```
We now have a source of truth for anything related to routing in the app and we can use this with confidence because we are no longer writing texts manually. Note that in Swift we can omit the Enum's name when using it, just type dot and the value. The compiler infers the type for us.

Also, if the String comes from an API or database, for example, we can easily create our route just by using the Enum initializer:
```swift
// we believe it comes from a reliable source
let routeStringFromDatabase = "Main"

let route = Routes.init(rawValue: routeStringFromDatabase)
```

## In Swift, Enum works great with Switch conditionals
Another benefit of using Enum can be perceived when used in Switch conditionals because the compiler obligates us to define all the actions for every Enum option if we don't use the default case. Let's see:
```swift
enum Routes: String {
	case About
	case Home
}

func doSomethingWith(route: Routes) -> Void {
	switch route {
		case .About:
			print("Let's see the about page")
		case .Home:
			print("The boring home page")
	}
}
```
Note in this code above that we have two cases in the Routes enum and in the function we have a switch conditional that defines actions for every case (just a print in this example). We call it an exhaustive switch. 

And the great thing about it is when you need to add another route and the compiler immediately throws errors at you because the switch is not exhaustive anymore:

![not exhaustive switch.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1607909044068/B3VCm1YTA.jpeg)

There we have a compiler working for us to write more reliable code.

## Look at your prepareForSegue methods
I bet you will find something to refactor in them because it is very common to open Swift projects and find code like this:
```swift
override func prepareForSegue(segue: UIStoryboardSegue!, sender: AnyObject!) {
	guard let identifier = segue.identifier else { return }
	
	if identifier == "About" {
		...
	} else if identifier == "Home" {
		...
	} else if identifier == "Login" {
		...
	}
}
```

I know, the `identifier` String comes from the UIKit and we have to work with it. But you can easily change the `if else` conditionals to be more safe using our Routes enum:
```swift
override func prepareForSegue(segue: UIStoryboardSegue!, sender: AnyObject!) {
	guard let identifier = segue.identifier else { return }
	
	if identifier == Routes.About.rawValue {
		...
	} else if identifier == Routes.Home.rawValue {
		...
	} else if identifier == Routes.Login.rawValue {
		...
	}
}

```
The `rawValue` property returns the String value of the enum and we eliminate the change of typing it wrong. Awesome!

## Conclusion
It is a very good practice to never rely on Strings to do conditionals because **hey** we're human and we fail all the time. Let's just help the compiler help us by preventing our own bugs.

That's it! I hope you liked the article and if you have anything to discuss or wanna give some feedback feel free to comment below, I will be happy to chat with you!