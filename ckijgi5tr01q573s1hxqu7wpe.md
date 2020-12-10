## Simple but powerful Coding Practices that you can adopt immediately: Consistent naming conventions

I already wrote about naming conventions in a  [past article](https://blog.darlantc.com/simple-but-powerful-coding-practices-that-you-can-adopt-immediately-naming-and-dry) but I wanna explore a little more because I think this topic is very important for every developer.

## The "One word per concept" rule
Consider the following code:
```swift
class UserManager {
    func getData() { ... }
}
class CategoryController {
    func requestData() { ... }
}
class PostStore {
    func retrieveData() { ... }
}
```

Can you see any difference between `get`, `request`, and `retrieve` in such a scenario?
And how about the `Manager`, `Controller`, and `Store`? What is the essential difference between them?

Essentially they are all the same and you must stick with just one word for each concept in the whole project to create a consistent naming throughout the project. Let's refactor it:

```swift
class UserStore {
    func getData() { ... }
}
class CategoryStore {
    func getData() { ... }
}
class PostStore {
    func getData() { ... }
}
```

Using the same word for each concept will save you tons of time for thoughts like "should I call `get` or `request` function now?". Time is money, you know!

## Don't confuse the reader
You are a reader from your own code. In fact, as developers we are constantly reading old code to write new code. And if you're not working alone on the project, you have other developers who constantly read your code as well.

As authors, our goal is to write code that is easy to read and understand code all the time.

> **We want our code to be a quick skim, not an intense study**. - Robert C. Martin

Using the same term for two different purposes is essentially a pun and can lead the reader to confusion:

```swift
class UserStore {
    var users = [String]()

    func add(user: String) {
        users.append(user)
    }
}
class CountStore {
    var counter = 0

    func add(value: Int) {
        counter += value
    }
}
```

Two classes with an add method. Consistent name with different results. The first one is used to append a `String` to a list (Array) of Strings. The second one is used to sum the value with another existing property value. This is bad.

## Know your readers
The people who will read your code will be mainly developers. So go ahead and use computer science terms, algorithm names, math terms, and so forth to name things.

The `LoginViewModel` is easy to understand by any developer who is familiar with the view model pattern. The same is valid for `Service`, `Controller`, `Interface`, etc.

If you are just starting in this industry you can easily find [ lists of common words](https://www.wholewhale.com/tips/developer-terms-glossary/) in our universe.

## Conclusion

Naming things is quite difficult but it is a very necessary skill for any developer. Experience makes it easier over time but we always need to try to get out of the comfort zone and improve this essential skill.