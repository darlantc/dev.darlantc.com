## Simple but powerful Coding Practices that you can adopt immediately: Naming and DRY

Everyone can write code with the right focus to learn the basics. Good developers can write code that solves big problems in real life. But the bop developers don't just write awesome code, their code is easily read and understood by other developers.

Take some time to reflect: while coding you use more time writing or reading existing code? I bet it is the second option.

> “Indeed, the ratio of time spent reading versus writing is well over 10 to 1. We are constantly reading old code as part of the effort to write new code. ...[Therefore,] making it easy to read makes it easier to write.” *― Robert C. Martin, Clean Code: A Handbook of Agile Software Craftsmanship*

As software developers, we must always seek to improve our code every day. 

So in this series of articles, I will describe some good practices that you can (and probably should) adopt immediately.

## Master the "naming skill"
Names are everywhere: variables, functions, arguments, classes, packages, directories, source files, etc.

Once I understood this concept I really think it is the most important skill that a developer needs to master. I know, it is very hard to master it, but you must practice it!

Consider the following example:
```
func fn(a: Int, b: Int) -> Int {
	var c = 0
	for i in a...b {
		if i % 2 == 0 {
			c = c + 1
		}
	}
	return c
}
func fn2(a: Int, b: Int) -> Int {
	var c = 0
	for i in a...b {
		if i % 2 != 0 {
			c = c + 1
		}
	}
	return c
}

let r1 = fn(a: 0, b: 100)
print(r1) // 51

let r2 = fn2(a: 0, b: 100)
print(r2) // 50

```
Can you understand what these two functions do? Can you at least see the difference between the two? I bet you don't!

Well, let's try a new version:
```
func countEvenNumbers(from: Int, to: Int) -> Int {
	var count = 0
	for index in from...to {
		if index % 2 == 0 {
			count = count + 1
		}
	}
	return count
}
func countOddNumbers(from: Int, to: Int) -> Int {
	var count = 0
	for index in from...to {
		if index % 2 != 0 {
			count = count + 1
		}
	}
	return count
}

let totalEvenNumbers = countEvenNumbers(from: 0, to: 100)
print(totalEvenNumbers) // 51

let totalOddNumbers = countOddNumbers(from: 0, to: 100)
print(totalOddNumbers) // 50
```
And now? Can you understand the code? I bet that even if you don't know Swift you can understand him simply because all the names are clear, self-explanatory.

For me the main rule when naming things is: always prefer a big but self-explanatory name over a small name that is impossible to understand for itself.

If you ask me, the best source to learn this skill is Robert C. Martin's **Clean Code** book.

## Always avoid code duplication
The **DRY** rule is mandatory: don't repeat yourself!

Every time you find yourself copying and pasting your own code take a deep breath and ask yourself: why am I doing this?

If you are just testing new code, it is fine to duplicate it. However the moment you realize that the code is ready you must transform the duplicate code into a utility function (or even a class) to reuse it, removing the duplication.

Can you see duplicated code in our previous example? The two functions are practically the same, only the condition changes. Let's try to improve it?
```
func loopAndCount(_ from: Int, _ to: Int, when: (Int) -> Bool) -> Int {
	var count = 0
	for index in from...to {
		if when(index % 2) {
			count = count + 1
		}
	}
	return count
}
func countEvenNumbers(from: Int, to: Int) -> Int {
	func when(result: Int) -> Bool {
		return result == 0
	}
	return loopAndCount(from, to, when: when)
}
func countOddNumbers(from: Int, to: Int) -> Int {
	func when(result: Int) -> Bool {
		return result != 0
	}
	return loopAndCount(from, to, when: when)
}
```
Now we introduce the **loopAndCount** utility function and move all the duplicated logic to it, passing the condition using a **Closure** as an argument named **when**.

In Swift, we can make it even smaller (and still quite readable):
```
func loopAndCount(_ from: Int, _ to: Int, when: (Int) -> Bool) -> Int {
	var count = 0
	for index in from...to {
		if when(index % 2) {
			count = count + 1
		}
	}
	return count
}
func countEvenNumbers(from: Int, to: Int) -> Int {
	return loopAndCount(from, to) { $0 == 0 }
}
func countOddNumbers(from: Int, to: Int) -> Int {
	return loopAndCount(from, to) { $0 != 0 }
}
```

Now, if for any reason we need to change the code logic we just need to update it in one place. This is the magic behind DRY. So **don't repeat yourself**!