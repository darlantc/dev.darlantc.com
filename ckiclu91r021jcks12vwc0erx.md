## Quick tip: Improve your code blocks with a language tag

Today I learned something very useful about code blocks on Hashnode (and it probably works on other blogging platforms too) that I immediately wanted to share with the community.

To explain this let me first show the result without the language tag:
```
func calculateHourlyPay(for workingHours: Int, at dayOfWeek: String) -> Double {
    let paymentPerHour: Double = 30

    let normalHours: Double = workingHours > 8 ? 8 : Double(workingHours)
    let overtimeHours = workingHours > 8 ? Double(workingHours - 8) : 0

    var payment: Double = 0

    if dayOfWeek == "sun" {
        payment = normalHours * paymentPerHour * 2
        payment += overtimeHours * paymentPerHour * 2 * 1.5
    } else if dayOfWeek == "sat" {
        payment = normalHours * paymentPerHour * 1.2
        payment += overtimeHours * paymentPerHour * 1.2 * 1.5
    } else {
        payment = normalHours * paymentPerHour
        payment += overtimeHours * paymentPerHour * 1.5
    }

    return payment
}
```

Notice how the code was misinterpreted, with little color distinction. The code block did not understand that I wrote code in Swift. Let's help it understand it by adding the language tag:

```swift
func calculateHourlyPay(for workingHours: Int, at dayOfWeek: String) -> Double {
    let paymentPerHour: Double = 30

    let normalHours: Double = workingHours > 8 ? 8 : Double(workingHours)
    let overtimeHours = workingHours > 8 ? Double(workingHours - 8) : 0

    var payment: Double = 0

    if dayOfWeek == "sun" {
        payment = normalHours * paymentPerHour * 2
        payment += overtimeHours * paymentPerHour * 2 * 1.5
    } else if dayOfWeek == "sat" {
        payment = normalHours * paymentPerHour * 1.2
        payment += overtimeHours * paymentPerHour * 1.2 * 1.5
    } else {
        payment = normalHours * paymentPerHour
        payment += overtimeHours * paymentPerHour * 1.5
    }

    return payment
}
```

Wow, much better!

How to do this? Here's how I wrote the first code block without a language tag:
![without language tag.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1607226812670/A-CKOjZ3J.jpeg)

Now the enhanced version with a language tag:
![with language tag.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1607226823045/PvCJBidph.jpeg)

## Conclusion
Code blocks are great and very useful for writing sample code in technical articles. But sometimes it can't understand the code language and it is better to explicitly indicate it by adding a language tag. 