## Simple but powerful Coding Practices that you can adopt immediately: Indentation & whitespace

Continuing the powerful coding practices series, today I will talk with two simple concepts that can greatly improve the readability of our code.

## Maintaining proper indentation
> A source file is a hierarchy rather like an outline. Each level of this hierarchy is a scope into which names can be declared and in which declarations and executable statements are interpreted. - *quote from Clean Code book - Robert C. Martin*

We use indentation to visually identify these hierarchies in our code. A correctly indented code is easy to read from top to bottom and we must keep it consistent throughout the whole project. 

Consider the following code:

```
class Payment {
let paymentPerHour: Double

init(paymentPerHour: Double) {self.paymentPerHour = paymentPerHour}

    private func normalHours(from workingHours: Int) -> Double {
return workingHours > 8 ? 8 : Double(workingHours)}

private func overtimeHours(from workingHours: Int) -> Double {
        return workingHours > 8 ? Double(workingHours - 8) : 0}}
```
Pretty bad, right? Our brain cannot find a pattern to identify where every method starts and finishes. It is very hard to read this code. And hard to read means that it's very hard to maintain it because we're always reading code to write new code.

Let's fix it with proper identation:

```
class Payment {
    let paymentPerHour: Double

    init(paymentPerHour: Double) {
        self.paymentPerHour = paymentPerHour
    }

    private func normalHours(from workingHours: Int) -> Double {
        return workingHours > 8 ? 8 : Double(workingHours)
    }

    private func overtimeHours(from workingHours: Int) -> Double {
        return workingHours > 8 ? Double(workingHours - 8) : 0
    }
}
```
Much better! Compare the two examples. The code is the same in both but the second one is correctly indented. 

Nowadays almost every code editor in the market can automatically apply the indentation to our code. Also, you can use tools to automate it for you, like Prettier.

## Using whitespace to create vertical separation
Nearly all code is read left to right and top to bottom. Every line represents a statement or expression and every group of lines must represent a complete thought.

Add whitespace (or blank line if you prefer) to separate these thoughts. The code became more readable.

What if our previous code was written without any whitespace?

```
class Payment {
    let paymentPerHour: Double
    init(paymentPerHour: Double) {
        self.paymentPerHour = paymentPerHour
    }
    private func normalHours(from workingHours: Int) -> Double {
        return workingHours > 8 ? 8 : Double(workingHours)
    }
    private func overtimeHours(from workingHours: Int) -> Double {
        return workingHours > 8 ? Double(workingHours - 8) : 0
    }
}
```

Even if it is well indented it is difficult to read because we have no clues as to where each thought (methods in this code) begins or ends. Ok, the colors in the code editors help a lot to identify these things (`init`, `private func`, etc), but correct use of whitespace can greatly improve readability.

## Conclusion
In this article, I wrote about the usage of indentation and whitespace to improve code readability. I hope you got some insights from it.

---

This article is the **third** in the series where I am writing about best practices on coding.

---

If you have any questions or want to give any feedback feel free to comment below. I will be happy to discuss this with you.

If you enjoy my work you can subscribe to be notified when I publish new articles.

Thank you! Have a great day!