## Simple but powerful Coding Practices that you can adopt immediately: Classes and Functions

**Functions** are one of the best and most essential tools we have to do our job as developers. Probably we all learned to use them to remove duplicated code, grouping instructions together to easy reuse.

**Classes** are generally used to group related properties and functions (usually called "methods" in this case). 

These two tools help us to create better and more organized code when used fairly following good coding principles.

Let's talk about some.

## Pure functions are better
A function is considered pure when it doesn't have external dependencies that can modify its result. Their output value must **always** by equal when called with the same input.

Consider the example:

```
func sum(_ a: Int, plus b: Int) -> Int {
	return a + b
}
func add(value: Int) -> Int {
	return totalValue + value
}

```
Can you identify if the functions above are pure?

The sum function is pure because it **always** produces the same result when called with the same inputs, no matter how many times you call it.
```
func sum(_ a: Int, plus b: Int) -> Int {
	return a + b
}
sum(15, plus: 20) // 35
sum(15, plus: 20) // 35
sum(100, plus: 20) // 120
sum(100, plus: 20) // 120
```

But the add function is not pure because it uses an external variable named "totalValue" which can be different in every call. The function cannot control what happens to the dependency and therefore cannot guarantee the result will be consistent.
```
var totalValue = 0

func add(value: Int) -> Int {
	return totalValue + value
}

add(value: 10) // 10
totalValue = 20
add(value: 10) // 30
totalValue = add(value: 20) // 40
add(value: 20) // 60
```

As you can see, impure functions can hide hard to track bugs because the dependencies that are totally out of their control.

It is much easy to test and maintain a pure function.

## Keep them as small as possible
When we start coding something it is quite common to create large and confusing functions that do everything. This is normal while we are trying to solve a problem and still don't know exactly how to do it. We need to think, implement, and test to make sure it works as it should.

However, when the code is working as expected the work is not yet complete. Now it is time to refactor it cleaning the large confusing functions by breaking them into smaller ones.

Consider the following fictional problem: the ACME company pays its employees for hours of work in the day. On weekdays the total payment must be $30 per hour. On Saturdays add a bonus of 20% and on Sundays, the payment must be doubled. Let's code it:

```
func calculateHourlyPay(for workingHours: Int, at dayOfWeek: String) -> Double {
	let paymentPerHour: Double = 30
	
	if dayOfWeek == "sun" {
		return Double(workingHours) * paymentPerHour * 2
	}
	if dayOfWeek == "sat" {
		return Double(workingHours) * paymentPerHour * 1.2
	}
	
	return Double(workingHours) * paymentPerHour
}

let sunPayment = calculateHourlyPay(for: 10, at: "sun") // 600
let monPayment = calculateHourlyPay(for: 10, at: "mon") // 300
let tuePayment = calculateHourlyPay(for: 10, at: "tue") // 300
let wedPayment = calculateHourlyPay(for: 10, at: "wed") // 300
let thuPayment = calculateHourlyPay(for: 10, at: "thu") // 300
let friPayment = calculateHourlyPay(for: 10, at: "fri") // 300
let satPayment = calculateHourlyPay(for: 10, at: "sat") // 360
```

Looking good. But now the customer has asked to calculate overtime hours that exceed the first 8 hours of work. Overtime hours pay 50% more.

Ok, should be easy:

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

let sunPayment = calculateHourlyPay(for: 10, at: "sun") // 660
let monPayment = calculateHourlyPay(for: 10, at: "mon") // 330
let tuePayment = calculateHourlyPay(for: 10, at: "tue") // 330
let wedPayment = calculateHourlyPay(for: 10, at: "wed") // 330
let thuPayment = calculateHourlyPay(for: 10, at: "thu") // 330
let friPayment = calculateHourlyPay(for: 10, at: "fri") // 330
let satPayment = calculateHourlyPay(for: 10, at: "sat") // 396
```

Awesome! Ready to ship, right? No... not really.

Now the function is large and full of calculations that can be difficult to read if you don't understand exactly what's going on. I guarantee you: if you need to do changes to this code in the future you will need a couple of minutes reading this to understand. Waste of time. It is better to refactor and write it better now that it is still fresh in our minds.

Which brings us to the next tip:

## Classes and Functions must have only one purpose 
The **S** in  [SOLID principles](https://en.wikipedia.org/wiki/SOLID) stands for Single-responsibility. That is, do just one thing.

Look at our last code. Our function does a lot of things right now:
* keep the payment per hour value as a constant (hard to update);
* calculate normal and overtime work hours;
* verify the day of the week;
* calculate normal hours payment;
* calculate overtime hours payment;
* sum normal and overtime payments to return.

Six responsibilities. We can do better! Let's refactor it:

```
class Payment {
	let paymentPerHour: Double

	init(paymentPerHour: Double) {
		self.paymentPerHour = paymentPerHour
	}

	func calculateHourlyPay(for workingHours: Int, at dayOfWeek: String) -> Double {
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
}
```

First, we refactored it to be a Class and moved the `paymentPerHour` constant to be a property. Now we can easily change this property value whenever necessary or even reuse the class with different `paymentPerHour` values. Let's continue:

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
	
	func calculateHourlyPay(for workingHours: Int, at dayOfWeek: String) -> Double {
		let normalHours = self.normalHours(from: workingHours)
		let overtimeHours = self.overtimeHours(from: workingHours)
		
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
}
```

Second, we created two private methods to calculate normal and overtime hours. Did you notice that the `calculateHourlyPay` method is losing its responsibilities? Let's move on:

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
	
	private func calculateNormalHoursPayment(for hours: Double, withBonus bonus: Double = 1) -> Double {
		return hours * self.paymentPerHour * bonus
	}
	
	private func calculateOvertimeHoursPayment(for hours: Double, withBonus bonus: Double = 1) -> Double {
		return hours * self.paymentPerHour * bonus * 1.5
	}
	
	func calculateHourlyPay(for workingHours: Int, at dayOfWeek: String) -> Double {
		let normalHours = self.normalHours(from: workingHours)
		let overtimeHours = self.overtimeHours(from: workingHours)
		
		var payment: Double = 0
		
		if dayOfWeek == "sun" {
			payment = calculateNormalHoursPayment(for: normalHours, withBonus: 2)
			payment += calculateOvertimeHoursPayment(for: overtimeHours, withBonus: 2)
		} else if dayOfWeek == "sat" {
			payment = calculateNormalHoursPayment(for: normalHours, withBonus: 1.2)
			payment += calculateOvertimeHoursPayment(for: overtimeHours, withBonus: 1.2)
		} else {
			payment = calculateNormalHoursPayment(for: normalHours)
			payment += calculateOvertimeHoursPayment(for: overtimeHours)
		}
		
		return payment
	}
}
```

Third, we created two more private methods to take care of calculating the payment for normal and overtime hours. Okay, our code is now bigger in the number of lines, but this is not the metric we should consider: we now have four small private methods (functions) that are easy to read and understand and our main method `calculateHourlyPay` has ** fewer responsibilities**:
* verify the day of the week;
* delegate the calculations for other methods;
* sum normal and overtime payments to return.

Just three responsibilities now. I really hope that you get to see the improvement we've made here. Let's finish it?

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
	
	private func calculateNormalHoursPayment(for hours: Double, withBonus bonus: Double = 1) -> Double {
		return hours * self.paymentPerHour * bonus
	}
	
	private func calculateOvertimeHoursPayment(for hours: Double, withBonus bonus: Double = 1) -> Double {
		return hours * self.paymentPerHour * bonus * 1.5
	}
	
	private func calculateHorlyPay(for workingHours: Int, withBonus bonus: Double = 1) -> Double {
		let normalHours = self.normalHours(from: workingHours)
		let overtimeHours = self.overtimeHours(from: workingHours)
		
		let normalPay = self.calculateNormalHoursPayment(for: normalHours, withBonus: bonus)
		let overtimePay = self.calculateOvertimeHoursPayment(for: overtimeHours, withBonus: bonus)
		
		return normalPay + overtimePay
	}
	
	func bonus(for dayOfWeek: String) -> Double {
		if dayOfWeek == "sun" {
			return 2
		}
		if dayOfWeek == "sat" {
			return 1.2
		}
		return 1
	}
	
	func calculateHourlyPay(for workingHours: Int, at dayOfWeek: String) -> Double {
		return calculateHorlyPay(
			for: workingHours,
			withBonus: self.bonus(for: dayOfWeek)
		)
	}
}

let payment = Payment(paymentPerHour: 30)

let sunPayment = payment.calculateHourlyPay(for: 10, at: "sun") // 660
let monPayment = payment.calculateHourlyPay(for: 10, at: "mon") // 330
let tuePayment = payment.calculateHourlyPay(for: 10, at: "tue") // 330
let wedPayment = payment.calculateHourlyPay(for: 10, at: "wed") // 330
let thuPayment = payment.calculateHourlyPay(for: 10, at: "thu") // 330
let friPayment = payment.calculateHourlyPay(for: 10, at: "fri") // 330
let satPayment = payment.calculateHourlyPay(for: 10, at: "sat") // 396
```

Yes! I'm now very happy with this code. Our main (and the unique not private) method is very small and does just one thing: it delegates the calculation for other specific methods and returns the result.

## Conclusion

Functions and Classes can be the source of nasty bugs if written without care. But they can be the best thing in our code if used following the principles discussed in this article.

I sincerely hope that you had some insights and learning from reading this article. So far it is the article that I dedicated myself the most to writing!

If you still don't already have it I strongly recommend that you read the [Clean Code](https://amzn.to/2V8WWEE) book (*affiliate link*). It is a must-read for developers and most of the contents of this article came from what I learned from it.

---

This article is the **second** in the series where I am writing about best practices on coding.

---

If you have any questions or want to give any feedback feel free to comment below. I will be happy to discuss this with you.

If you enjoy my work you can subscribe to be notified when I publish new articles.

Thank you! Have a great day!