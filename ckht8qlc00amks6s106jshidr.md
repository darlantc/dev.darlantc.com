## Lessons I learned from watching the Essential Developer's Quiz App series

Recently I got a new job as an iOS developer and felt the need to refresh my knowledge in Swift because I was mainly coding in Javascript and Flutter for the past two years in my startup.

So I did some research and luckily found Caio and Mike [Essential Developer YouTube Channel](https://www.youtube.com/channel/UCjFr010oOpmlzZNw79f-1fA)  with an amazing playlist creating a Quiz App with advanced techniques like TDD & Clean Architecture. I learned a lot from them, so I will write about it here so I never forget the lessons learned.

I highly recommend that you watch their series too. I assure you will learn a lot too, even if you don't work with Swift because the concepts explained can be applied in any language. You can find a link to the first episode at the end of this article.

## TDD is awesome. You must learn how to use it
It had been about two years since I knew I needed to write unit tests on my code. I studied several techniques for this, but I couldn't find a way to put It into practice. My code was difficult to test and I didn't have the tools and the knowledge to write them.

But the way Caio and Mike write tests is just... easy. Easy to follow, easy to understand, and easy to maintain.

They used  [TDD](https://en.wikipedia.org/wiki/Test-driven_development) approach to develop the whole app, starting with the game's Engine. Even ViewControllers were covered by tests.

The process follows the following steps:

**1- Red**: you write a very simple and minimum test to validate something (usually a Class or Struct). It will be red because the system under test (known as SUT) still doesn't exist (compile error). **The test will crash**.

```
import XCTest
@testable import QuizEngine

class FlowTest: XCTestCase {
	func test_start_withNoQuestions_doesNotHaveQuestions() {
		let sut = Flow(questions: [])
		
		XCTAssertTrue(sut.questions.isEmpty)
	}
}
```

**2- Yellow**: you create the SUT (your Class our Struct you are testing) and the methods it needs just to remove compile errors to build. **The test must fail**.

```
class Flow {
	let questions: [String]
	
	init(questions: [String]) {}
}
```

![XCode tests failed](https://cdn.hashnode.com/res/hashnode/image/upload/v1606021923632/qcEpx_1xE.jpeg)

**3- Green**: you finish the implementation of the SUT with the minimum necessary code just to pass the test. Don't think ahead trying to implement everything upfront. Let the tests guide you.

```
class Flow {
	let questions: [String]
	
	init(questions: [String]) {
		self.questions = []
	}
}
```

![XCode tests succeeded](https://cdn.hashnode.com/res/hashnode/image/upload/v1606021906377/gITM2kLQI.jpeg)

**4- Yellow again**: add another test to continue advancing towards your goal:

```
import XCTest
@testable import QuizEngine

class FlowTest: XCTestCase {
	func test_start_withNoQuestions_doesNotHaveQuestions() {
		let sut = Flow(questions: [])
		
		XCTAssertTrue(sut.questions.isEmpty)
	}
		
	func test_start_withOneQuestion_doesHaveOneQuestion() {
		let sut = Flow(questions: ["Q1"])
		
		XCTAssertEqual(sut.questions, ["Q1"])
	}
}
```

**5- Green again**: fix the init to properly function as expected and the two tests must pass now.

```
class Flow {
	let questions: [String]
	
	init(questions: [String]) {
		self.questions = questions
	}
}
```

**6- Repeat**: continue repeating the process: add new tests that will fail, implement the code that will make them pass.

Sometimes the process seems silly and unnecessary, but you need to practice to make it a habit. It is a total change in the way of writing code.

But I guarantee you: the very first time a test saves you from yourself when changing 
a code you will be very grateful you have written it previously.

## Refactoring isn't scary if you have test coverage
In season two Caio and Mike refactored the entire game Engine, deprecating everything in the core to create an enhanced version of it, introducing the Quiz class.

We've all been there: we've decided that we need to rewrite an entire class because its design is very bad. Then we start changing everything and then we have multiple errors to handle, changing code everywhere to match the refactor we did. It is a mess. And it will produce multiple new bugs, you know that! So much so that many times we decided to simply give up refactoring and continue to maintain bad code. Right?

Well, Caio and Mike approach to refactor the game Engine is very awesome. They start deprecating class and functions to produce warning errors while maintaining backward compatibility. The first rule is: don't break the existing code for current users.

```
@available(*, deprecated, message: "use Quiz instead")
public class Game <R: Router> {
    ...
}

@available(*, deprecated, message: "use Quiz.start instead")
public func startGame() {
    ...
}
```

Then they create the new classes and functions with new behaviors, continuing step by step to replace the old behaviors. Here the tests play an important role: they guarantee the old code continues to work regardless of what changed.

When refactoring ends, clients who depend on the Engine still work in exactly the same way. Zero bugs produced. They now just got deprecated code warnings with instructions to update to the new features.

## Commit very often. Commit everything
During season two I also learned with them the importance of keeping commits small and to commit every change we made.

![commits.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1606025069458/Qk-5qwWWY.jpeg)

The reason is very simple: small commits are easy to read and understand. You can easily open it and discover the changes it made to the code. Also, I think you get a better understanding of your thought process in that code by just reading the small commits.

## Business rules code should be platform agnostic
We can have many benefits if we create our business rules code without any platform dependency, separating it from everything else:
* They are easy to write and maintain;
* You can easily re-use the code to publish other apps like macOS, watchOS, tvOS, etc;
* You can even re-use the code in a new project. For example, the QuizEngine that we build in this series can be used to create survey apps and multiple types of quiz games with the very same core code.

In the iOS / Swift world, we have another powerful benefit: we can run the tests of the core very very fast because we create it targeting the macOS, which runs much faster than an iOS target app (which depends on Device or iOS Simulator).

## Good code survives changes in the platform or programming language new versions
Caio and Mike started the series in 2017 when Swift 3 was the newest version. Today we have Swift 5.2 as the newest version.

It is not uncommon in this type of scenario that old code created in such different versions does not work correctly in the current version and are hard to upgrade. This kind of problem scales with every third party dependency the code has. But if we keep our code with minimum dependency (the business rules shouldn't have any) we can update with no pain.

And again, the tests help a lot in this case, working as a safety net to update without fear.

## Clean Architecture provides a SOLID foundation to follow
It is hard to adopt the Clean Architecture, it seems like so much work to be effective and productive. It is very fast to simply create classes and functions that are directly dependent on each other or simply put all the code in the same file/class. But as soon as that code grows, the costs to maintain it grows exponentially as well.

The SOLID principles provide a bunch of ways to create easier to maintain, extend, and test code that pays the cost in the long run because you can continue to work fast in it without regressions.

In my  [previous article](https://blog.darlantc.com/simple-but-powerful-coding-practices-that-you-can-adopt-immediately-naming-and-dry), I started a series with some tips towards this.

I highly recommend that you learn these concepts and try to implement them in your own code.

## Conclusion
Learning from highly skilled developers who know how to teach is very very helpful. Caio and Mike certainly know what they are doing and I am very happy to have found their content.

Currently, they started season three that I still need to watch. I'm sure I'll learn a few more things from them and maybe I'll write another article in the future.

Enjoy you too:

[How to Build iOS Apps with Swift, TDD & Clean Architecture](https://www.essentialdeveloper.com/professional-ios-engineering-series) 

<iframe width="780" height="440" src="https://www.youtube.com/embed/videoseries?list=PLyjgjmI1UzlSUlaQD0RvLwwW-LSlJn-F6" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

> Disclaimer: **this is not** a sponsored article. I talked to Caio and asked for permission to share it here simply because I really liked the content.