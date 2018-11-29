# Engineering Values

* [Minimize Complexity](#minimize-complexity)
  * [Follow the Single Responsibility Principle](#single-responsibility)
  * [Maintain Separation of Concerns](#separation-of-concerns)
  * [Minimize Complexity Caused by State Handling](#state-handling)
  * [Use Functional Programming Where Possible](#functional-programming)
  * [Prefer Duplication Over the Wrong Abstraction](#duplication)
  * [You Aren't Going to Need It (Yagni)](#yagni)
  * [Use Short Methods](#short-methods)
  * [Pursue Ephemeralization](#ephemeralization)
  * [Recognize the Law of Leaky Abstractions](#leaky-abstractions)
* [Write Tests](#write-tests)
  * [Spend Your Time Writing the Right Tests](#the-right-tests)
  * [Delete Tests that Don’t Provide Value](#delete-tests)
  * [Write The Right Number of Unit Tests](#unit-tests)
  * [Write Fewer E2E/Acceptance Tests](#e2e-tests)
  * [Favor Integration Tests Over UI/Acceptance Tests](#integration-tests)
  * [Don’t Test Internal Implementation](#dont-test-implementation)
  * [Tests are not a substitute for good design](#tests-good-design)
  * [Don’t Be a TDD Zealot](#tdd-zealotry)
* [References](#references)

<a name="minimize-complexity"></a>
## Minimize Complexity

“The biggest problem in the development and maintenance of large-scale software systems is complexity — large systems are hard to understand. We believe that the major contributor to this complexity in many systems is the handling of state and the burden that this adds when trying to analyse and reason about the system. Other closely related contributors are code volume, and explicit concern with the flow of control through the system.” - [Moseley and Marks][1]

<a name="single-responsibility"></a>
### Follow the Single Responsibility Principle

In SRP, a responsibility is a “reason for change”. If you can think of more than one motive for changing a class, it has more than one responsibility.

“Changes to one responsibility may impair or inhibit the class’ ability to meet the others. This kind of coupling leads to fragile designs that break in unexpected ways when changed.” - [Uncle Bob][2]

"Gather together the things that change for the same reasons. Separate those things that change for different reasons."  - [Uncle Bob][2]

“[R]emember that the reasons for change are people. It is people who request changes. And you don't want to confuse those people, or yourself, by mixing together the code that many different people care about for different reasons.” - [Uncle Bob][2]

“Every module … is characterized by its knowledge of a design decision which it hides from all others. Its interface or definition was chosen to reveal as little as possible about its inner workings.” - [David L. Parnas][7]

<a name="separation-of-concerns"></a>
### Maintain Separation of Concerns

“intelligent thinking” is predicated on discovering aspects of a problem that can be “studied in isolation for the sake of their own consistency” - [Edsger Dijkstra][6]

Example: “The task of ‘making a thing satisfying our needs’ as a single responsibility is split into two parts ‘stating the properties of a thing, by virtue of which it would satisfy our needs’ and ‘making a thing guaranteed to have the stated properties’” - [Edsger Dijkstra][6]

<a name="state-handling"></a>
### Minimize Complexity Caused by State Handling

“try it again”, or “reload the document”, or “restart the program”, or “reboot your computer” or “re-install the program” - The reason that they are often successful in resolving the problem is that many systems have errors in their handling of state. The reason that many of these errors exist is that the presence of state makes programs hard to understand. It makes them complex. - [Moseley and Marks][1]

“testing for one set of inputs tells you nothing at all about the behaviour with a different set of inputs… even though the number of possible inputs may be very large, the number of possible states the system can be in is often even larger.“ - [Moseley and Marks][1]

<a name="functional-programming"></a>
### Use Functional Programming Where Possible

“Classes (and prototypes) have their place in JavaScript. But they’re an optimization. They don’t make your code simpler, they make it more complex. It’s better to narrow your focus on things that are not only simple to learn but simple to understand: functions and objects.” - [Kent C. Dodds][12]

“Functional programming is not about lambda calculus, monads, morphisms, and combinators. It’s about having many small well-defined composable functions without mutations of global state, their arguments, and IO.” - [Victor Nakoryakov][11]

“When you collaborate in a project developed with functional programming principles, you notice the consequence pretty quickly. Doing a review now requires a much lower cognitive load. If you look at the function, the function code is all you should think about. You no longer have to imagine what consequences of mutating this field for that components will be. You don’t think whether a shallow copy, deep copy, or just a reference is more appropriate here. You just don’t have to think broader than the ten lines of code you’re looking at right now.” - [Victor Nakoryakov][11]

Use pure functions (no side effects, output for any given input will always be the same)

<a href="duplication"></a>
### Prefer Duplication Over the Wrong Abstraction

"DRY [Don't Repeat Yourself] says that every piece of system knowledge should have one authoritative, unambiguous representation. ... if you have more than one way to express the same thing, at some point the two or three different representations will most likely fall out of step with each other. Even if they don't, you're guaranteeing yourself the headache of maintaining them in parallel whenever a change occurs. And change will occur. DRY is important if you want flexible and maintainable software." - [Dave Thomas][14]

“Readability matters. Don't try to be overly DRY. Duplication is okay, if it improves readability. Try to find a balance between DRY and DAMP [Descriptive And Meaningful Phrases] code” - [Ham Vocke][8]

"Don't get trapped by the sunk cost fallacy. If you find yourself passing parameters and adding conditional paths through shared code, the abstraction is incorrect. It may have been right to begin with, but that day has passed. Once an abstraction is proved wrong the best strategy is to re-introduce duplication and let it show you what's right." - [Sandy Metz][13]

<a name="yagni"></a>
### You Aren't Gonna Need It (Yagni)

"The first argument for yagni is that while we may now think we need this presumptive feature, it's likely that we will be wrong. After all the context of agile methods is an acceptance that we welcome changing requirements." - [Martin Fowler][15]

Building a presumptive feature can incur these types of costs (paraphrasing [Martin Fowler][15]):

* **Cost of build**: the cost in terms of time spent analyzing, building, and testing a useless feature
* **Cost of delay**: the cost of not releasing another revenue-producing feature first
* **Cost of carry**: code for [a] presumptive feature adds some complexity to the software, this complexity makes it harder to modify and debug that software, thus increasing the cost of other features.
* **Cost of repair**: you often realize that a feature coded six months ago wasn't done the way you now realize it should be done. In that case you have accumulated TechnicalDebt and have to consider the cost of repair for that feature or the on-going costs of working around its difficulties.

"Yagni only applies to capabilities built into the software to support a presumptive feature, it does not apply to effort to make the software easier to modify. Yagni is only a viable strategy if the code is easy to change, so expending effort on refactoring isn't a violation of yagni because refactoring makes the code more malleable." - [Martin Fowler][15]

<a name="short-methods"></a>
### Use Short Methods

"The more code you have to read before you can form units of meaning larger than line-level constructs in your programming LanguageOfChoice, the harder your task. Therefore, be lazy. Make it easy on yourself. Never write a method that would require hard thinking to analyze. Factor out units of semantic meaning, even if they are only ever used once. Define them reasonably close by each other until and unless you identify a clear opportunity for meaningful reuse. Use short methods. Lots of short methods." - [Ward Cunningham][19]

"Breaking up methods makes them self documenting code, without comments. Each time that you pull out a piece of the method, you have to create a name for it. Well-chosen names allow you to avoid comments." - [Ward Cunningham][19]

"Small chunks are more easily understood than larger chunks. It is easier to reuse things that you understand well. The smaller your Legos, the more flexibly you can build." - [Ward Cunningham][19]

<a name="ephemeralization"></a>
### Pursue Ephemeralization

"Do more and more with less and less until eventually you can do everything with nothing." - R. Buckminster Fuller

"[Ephemeralization] is accomplished by building systems that are more robust, more automatic, and less prone to problems because the tendency to grow in complexity that’s inherent to them has been understood, harnessed, and reversed." - [Brandur Leach][16]

"**Retire old technology.** Is something new being introduced? Look for opportunities to retire older technology that’s roughly equivalent. If you’re about to put Kafka in, maybe you can get away with retiring Rabbit or NSQ." - [Brandur Leach][16]

"**Build common service conventions.** Standardize on one database, one language/runtime, one job queue, one web server, one reverse proxy, etc. If not one, then standardize on as few as possible." - [Brandur Leach][16]

"**Favor simplicity and reduce moving parts.** Try to keep the total number of things in a system small so that it stays easy to understand and easy to operate. In some cases this will be a compromise because a technology that’s slightly less suited to a job may have to be re-used even if there’s a new one that would technically be a better fit." - [Brandur Leach][16]

"**Avoid custom technology.** Software that you write is software that you have to maintain. Forever. Don’t succumb to NIH [not invented here] when there’s a well supported public solution that fits just as well (or even almost as well)." - [Brandur Leach][16]

"**Use services.** Software that you install is software that you have to operate. From the moment it’s activated, someone will be taking regular time out of their schedule to perform maintenance, troubleshoot problems, and install upgrades. Don’t succumb to NHH (not hosted here) when there’s a public service available that will do the job better." - [Brandur Leach][16]

<a name="leaky-abstractions"></a>
## Recognize the Law of Leaky Abstractions

"All non-trivial abstractions, to some degree, are leaky." - [Joel Spolsky][17]

"The law of leaky abstractions means that whenever somebody comes up with a wizzy new code-generation tool that is supposed to make us all ever-so-efficient, you hear a lot of people saying “learn how to do it manually first, then use the wizzy tool to save time.” Code generation tools which pretend to abstract out something, like all abstractions, leak, and the only way to deal with the leaks competently is to learn about how the abstractions work and what they are abstracting. So the abstractions save us time working, but they don’t save us time learning." - [Joel Spolsky][17]

<a name="write-tests"></a>
## Write Tests

<a name="the-right-tests"></a>
### Spend Your Time Writing the Right Tests

First, aim for 100% coverage of “critical” code (code that breaks often, gets most of the new features, has a big impact on users)

Then, aim for 100% coverage of “core” code (code that breaks sometimes, gets few new features, has a medium impact on users)

Then, cover “other” code (code that rarely changes or breaks, has minimal impact on users) - [Kostis Kapelonis][4]

<a name="delete-tests"></a>
### Delete Tests that Don’t Provide Value

“Sometimes that's hard, especially if you know that coming up with a test was hard work. Beware of the sunk cost fallacy and hit the delete key. There's no reason to waste more precious time on a test that ceased to provide value.” - [Ham Vocke][8]

<a name="unit-tests"></a>
### Write The Right Number of Unit Tests

Unit tests provide the opportunity to test all (or most) code paths of your business logic modules. The total number of code paths is additive in unit tests for each module, but multiplicative in integration tests which use those same same modules sequentially chained together. - [Kostis Kapelonis][4]

Now that we have seen why we need both kinds of tests (unit and integration), we need to decide on how many tests we need from each category. … [Y]ou need to spend some time to understand what type of tests add the most value to your application. The test pyramid is only a suggestion on the amount of tests that you should create. It assumes that you are writing a commercial web application, but that is not always the case. - [Kostis Kapelonis][4]

"If you are painting a house, you want to start with a biggest brush at hand and spare the tiny brush for the end to deal with fine details. If you begin your QA work with unit tests, you are essentially trying to paint entire house using the finest chinese calligraphy brush." - [Martin Sústrik][18]

"If all you have are unit tests, you are pretty sure that all the individual gears inside the project work as expected. However, you have no idea whether the project as a whole works or not. It may well be that the user won't be able to performs a single task." - [Martin Sústrik][18]

<a name="e2e-tests"></a>
### Write Fewer E2E/Acceptance Tests

Integration tests are slow, and integration tests are harder to debug since the error could happen anywhere in the business logic - [Kostis Kapelonis][4]

“You don't test all the conditional logic and edge cases that your lower-level tests already cover in the higher-level test again. Make sure that the higher-level test focuses on the part that the lower-level tests couldn't cover.” - [Ham Vocke][8]

<a name="integration-tests"></a>
### Favor Integration Tests Over UI/Acceptance Tests

“Service-level [integration] testing is about testing the services of an application separately from its user interface. So instead of running a dozen or so multiplication test cases through the calculator’s user interface, we instead perform those tests at the service level.” - [Mike Cohn][9]

“Integration tests strike a great balance on the trade-offs between confidence and speed/expense. This is why it’s advisable to spend most (not all, mind you) of your effort there.” - [Kent C. Dodds][10]

<a name="dont-test-implementation"></a>
### Don’t Test Internal Implementation

“Tests that are too close to the production code quickly become annoying. As soon as you refactor your production code (quick recap: refactoring means changing the internal structure of your code without changing the externally visible behaviour) your unit tests will break.” - [Ham Vocke][8]

“if you find yourself continuously fixing existing tests as you add new features, it means that your tests are tightly coupled to internal implementation.” - Kostis Kapelonis [Kostis Kapelonis][4]

"I have seen a nice definition of "architecture" somewhere. It said that architecture is that what makes software able to change. If we embrace this point of view, unit tests can be considered to be a strictly an anti-architecture device." - [Martin Sústrik][18]

<a name="tests-good-design"></a>
### Tests are not a substitute for good design

“testing is hopelessly inadequate....(it) can be used very effectively to show the presence of bugs but never to show their absence.” - [Edsger Dijkstra][3]

<a name="tdd-zealotry"><a/>
### Don’t Be a TDD Zealot

“… it would be immature for a startup to follow blindly TDD. If you work in a startup company you might write code that will change so fast that TDD will not be a big help. You might even throw away code until you get it “right”. Writing tests after the implementation code, is a perfectly valid strategy in that case.” - [Kostis Kapelonis][4]

<a name="references"><a/>
## References

1. [Moseley and Marks - Out of the Tar Pit (2006)][1]
1. [Uncle Bob - The Single Responsibility Principle (2014)][2]
1. [Edsger Dijkstra - Notes on Structured Programming (1970)][3]
1. [Kostis Kapelonis - Software Testing Antipatterns (2018)][4]
1. [Martin Fowler - The Test Pyramid (2012)][5]
1. [Edsger Dijkstra - On the Role of Scientific Thought (1974)][6]
1. [David L. Parnas - On the Criteria To Be Used in Decomposing Systems into Modules (1971)][7]
1. [Ham Vocke - The Practical Test Pyramid (2018)][8]
1. [Mike Cohn - The Forgotten Layer of the Test Automation Pyramid (2009)][9]
1. [Kent C. Dodds - Write Tests. Not Too Many. Mostly Integration. (2017)][10]
1. [Victor Nakoryakov - Two Years of Functional Programming in JavaScript: Lessons Learned (2018)][11]
1. [Kent C. Dodds - Classes, Complexity, and Functional Programming (2017)][12]
1. [Sandy Metz - The Wrong Abstraction (2016)][13]
1. [Bill Venners, Dave Thomas, Andy Hunt - Orthogonality and the DRY Principle (2003)][14]
1. [Martin Fowler - Yagni (2015)][15]
1. [Brandur Leach - In Pursuit of Production Minimalism (2017)][16]
1. [Joel Spolsky - The Law of Leaky Abstractions (2002)][17]
1. [Martin Sústrik - Unit Test Fetish (2014)][18]
1. [Ward Cunningham - Short Methods (2008)][19]

[1]: http://www.cs.tufts.edu/comp/98/out-of-the-tar-pit.pdf
[2]: https://8thlight.com/blog/uncle-bob/2014/05/08/SingleReponsibilityPrinciple.html
[3]: http://www.cs.utexas.edu/users/EWD/ewd02xx/EWD249.PDF
[4]: http://blog.codepipes.com/testing/software-testing-antipatterns.html
[5]: https://martinfowler.com/bliki/TestPyramid.html
[6]: http://www.cs.utexas.edu/users/EWD/ewd04xx/EWD447.PDF
[7]: https://www.cs.umd.edu/class/spring2003/cmsc838p/Design/criteria.pdf
[8]: https://martinfowler.com/articles/practical-test-pyramid.html
[9]: https://www.mountaingoatsoftware.com/blog/the-forgotten-layer-of-the-test-automation-pyramid
[10]: https://blog.kentcdodds.com/write-tests-not-too-many-mostly-integration-5e8c7fff591c
[11]: https://hackernoon.com/two-years-of-functional-programming-in-javascript-lessons-learned-1851667c726
[12]: https://blog.kentcdodds.com/classes-complexity-and-functional-programming-a8dd86903747
[13]: https://www.sandimetz.com/blog/2016/1/20/the-wrong-abstraction
[14]: https://www.artima.com/intv/dry.html
[15]: https://martinfowler.com/bliki/Yagni.html
[16]: https://brandur.org/minimalism
[17]: https://www.joelonsoftware.com/2002/11/11/the-law-of-leaky-abstractions/
[18]: http://250bpm.com/blog:40
[19]: http://wiki.c2.com/?ShortMethods
