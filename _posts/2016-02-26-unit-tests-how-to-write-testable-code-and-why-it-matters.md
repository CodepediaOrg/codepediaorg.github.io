---
id: 2650
title: Unit Tests, How to Write Testable Code and Why it Matters
date: 2016-02-26T16:24:36+00:00
author: Toptal Blog
layout: post
guid: http://www.codingpedia.org/?p=2650
permalink: /toptal/unit-tests-how-to-write-testable-code-and-why-it-matters/
fsb_show_social:
  - 0
dsq_thread_id:
  - 4613556530
fsb_social_facebook:
  - 0
fsb_social_google:
  - 3
fsb_social_linkedin:
  - 0
fsb_social_twitter:
  - 0
fsb_social_pinterest:
  - 0
categories:
  - qa
  - testing
tags:
  - '#BestPractices'
  - '#DesignPatterns'
  - '#OOP'
  - '#QA'
  - '#UnitTesting'
  - 'C#'
  - testing
---
<p style="text-align: justify;">
  Unit testing is an essential instrument in the toolbox of any serious <a href="http://www.toptal.com/software">software developer</a>. However, it can sometimes be quite difficult to write a good unit test for a particular piece of code. Having difficulty testing their own or someone else’s code, developers often think that their struggles are caused by a lack of some fundamental testing knowledge or secret unit testing techniques.
</p>

<p style="text-align: justify;">
  In this article, I would like to show that unit tests are quite easy; the real problems that complicate unit testing, and introduce expensive complexity, are a result of poorly-designed, <em>untestable</em> code. We will discuss what makes code hard to test, which anti-patterns and bad practices we should avoid to improve testability, and what other benefits we can achieve by writing testable code. We will see that writing testable code is not just about making testing less troublesome, but about making the code itself more robust, and easier to maintain.<!--more-->
</p>

<p style="text-align: justify;">
  <img src="http://assets.toptal.io/uploads/blog/image/91302/toptal-blog-image-1434578005589-4e6897ec04cc0b3c7075b9b011ee915c.gif" alt="Unit tests" />
</p>

<h2 id="what-is-unit-testing" style="text-align: justify;">
  What is Unit Testing?
</h2>

<p style="text-align: justify;">
  Essentially, a unit test is a method that instantiates a small portion of our application and verifies its behavior<strong>independently from other parts</strong>. A typical unit test contains 3 phases: First, it initializes a small piece of an application it wants to test (also known as the system under test, or SUT), then it applies some stimulus to the system under test (usually by calling a method on it), and finally, it observes the resulting behavior. If the observed behavior is consistent with the expectations, the unit test passes, otherwise, it fails, indicating that there is a problem somewhere in the system under test. These three unit test phases are also known as Arrange, Act and Assert, or simply AAA.
</p>

<p style="text-align: justify;">
  A unit test can verify different behavioral aspects of the system under test, but most likely it will fall into one of the following two categories: <em>state-based</em> or <em>interaction-based</em>. Verifying that the system under test produces correct results, or that its resulting state is correct, is called <strong>state-based</strong> unit testing, while verifying that it properly invokes certain methods is called <strong>interaction-based</strong> unit testing.
</p>

<p style="text-align: justify;">
  As a metaphor for a proper unit test, imagine a mad scientist who wants to build some supernatural <a href="https://en.wikipedia.org/wiki/Chimera_(mythology)" target="_blank">chimera</a>, with frog legs, octopus tentacles, bird wings, and a dog’s head. (This metaphor is pretty close to what programmers actually do at work). How would that scientist make sure that every part (or unit) he picked actually works? Well, he can take, let’s say, a single frog’s leg, apply an electrical stimulus to it, and check for proper muscle contraction. What he is doing is essentially the same Arrange-Act-Assert steps of the unit test; the only difference is that, in this case, <em>unit</em> refers to a physical object, not to an abstract object we build our programs from.
</p>

<p style="text-align: justify;">
  <img src="http://assets.toptal.io/uploads/blog/image/91299/toptal-blog-image-1434577722896-66635fe9efe5898fa701037c0da6c0f4.jpg" alt="unit testing" />
</p>

> _I will use C# for all examples in this article, but the concepts described apply to all object-oriented programming languages._

<p style="text-align: justify;">
  A simple unit test could look like this:
</p>

<pre class="lang:default decode:true ">[TestMethod]
public void IsPalindrome_ForPalindromeString_ReturnsTrue()
{
    // In the Arrange phase, we create and set up a system under test.
    // A system under test could be a method, a single object, or a graph of connected objects.
    // It is OK to have an empty Arrange phase, for example if we are testing a static method -
    // in this case SUT already exists in a static form and we don't have to initialize anything explicitly.
    PalindromeDetector detector = new PalindromeDetector();

    // The Act phase is where we poke the system under test, usually by invoking a method.
    // If this method returns something back to us, we want to collect the result to ensure it was correct.
    // Or, if method doesn't return anything, we want to check whether it produced the expected side effects.
    bool isPalindrome = detector.IsPalindrome("kayak");

    // The Assert phase makes our unit test pass or fail.
    // Here we check that the method's behavior is consistent with expectations.
    Assert.IsTrue(isPalindrome);
}</pre>

&nbsp;

<h2 id="unit-tests-vs-integration-tests" style="text-align: justify;">
  Unit Tests vs. Integration Tests
</h2>

<p style="text-align: justify;">
  Another important thing to consider is the difference between unit and integration tests.
</p>

<p style="text-align: justify;">
  The purpose of a unit test is to verify the behavior of a relatively small piece of software, independently from other parts. Unit tests are narrow in scope, and allow us to cover all cases, ensuring that every single part works correctly.
</p>

<p style="text-align: justify;">
  On the other hand, integration tests demonstrate that different parts of a system <strong>work together in the real-life environment</strong>. They validate complex scenarios (we can think of integration tests as a user performing some high-level operation within our system), and usually require external resources, like databases or web servers, to be present.
</p>

<p style="text-align: justify;">
  Let’s go back to our mad scientist metaphor, and suppose that he has successfully combined all the parts of the chimera. He wants to perform an integration test of the resulting creature, making sure that it can, let’s say, walk on different types of terrain. First of all, the scientist must emulate an environment for the creature to walk on. Then, he throws the creature into that environment and pokes it with a stick, observing if it walks and moves as designed. After finishing a test, the mad scientist cleans up all the dirt, sand and rocks that are now scattered in his lovely laboratory.
</p>

<p style="text-align: justify;">
  <img src="http://assets.toptal.io/uploads/blog/image/91300/toptal-blog-image-1434577772250-e8e5ff3bbe312ea395dd84a90db53c71.jpg" alt="" />
</p>

<p style="text-align: justify;">
  Notice the significant difference between unit and integration tests: A unit test verifies the behavior of small part of the application, isolated from the environment and other parts, and is quite easy to implement, while an integration test covers interactions between different components, in the close-to-real-life environment, and requires more effort, including additional setup and teardown phases.
</p>

<p style="text-align: justify;">
  A reasonable combination of unit and integration tests ensures that every single unit works correctly, independently from others, and that all these units play nicely when integrated, giving us a high level of confidence that the whole system works as expected. However, we must remember to always identify what kind of test we are implementing: a unit or an integration test. The difference can sometimes be deceiving. If we think we are writing a unit test to verify some subtle edge case in a business logic class, and realize that it requires external resources like web services or databases to be present, something is not right — essentially, we are using a sledgehammer to crack a nut. And that means bad design.
</p>

<h2 id="what-makes-a-good-unit-test" style="text-align: justify;">
  What Makes a Good Unit Test?
</h2>

<p style="text-align: justify;">
  Before diving into the main part of this tutorial, let’s quickly discuss the properties of a good unit test. A good unit test should be:
</p>

<ul style="text-align: justify;">
  <li>
    <strong>Easy to write.</strong> Developers typically write lots of unit tests to cover different cases and aspects of the application’s behavior, so it should be easy to code all of those test routines without enormous effort.
  </li>
  <li>
    <strong>Readable.</strong> The intent of a unit test should be clear. A good unit test tells a story about some behavioral aspect of our application, so it should be easy to understand which scenario is being tested and — if the test fails — easy to detect how to address the problem. With a good unit test, we can fix a bug without actually debugging the code!
  </li>
  <li>
    <strong>Reliable.</strong> Unit tests should fail only if there’s a bug in the system under test. That seems pretty obvious, but programmers often run into an issue when their tests fail even when no bugs were introduced. For example, tests may pass when running one-by-one, but fail when running the whole test suite, or pass on our development machine and fail on the continuous integration server. These situations are indicative of a design flaw. Good unit tests should be reproducible and independent from external factors such as the environment or running order.
  </li>
  <li>
    <strong>Fast.</strong> Developers write unit tests so they can repeatedly run them and check that no bugs have been introduced. If unit tests are slow, developers are more likely to skip running them on their own machines. One slow test won’t make a significant difference; add one thousand more and we’re surely stuck waiting for a while. Slow unit tests may also indicate that either the system under test, or the test itself, interacts with external systems, making it environment-dependent.
  </li>
  <li>
    <strong>Truly unit, not integration.</strong> As we already discussed, unit and integration tests have different purposes. Both the unit test and the system under test should not access the network resources, databases, file system, etc., to eliminate the influence of external factors.
  </li>
</ul>

<p style="text-align: justify;">
  That’s it — there are no secrets to writing <em>unit tests</em>. However, there are some techniques that allow us to write <em>testable code</em>.
</p>

<h2 id="testable-and-untestable-code" style="text-align: justify;">
  Testable and Untestable Code
</h2>

<p style="text-align: justify;">
  Some code is written in such a way that it is hard, or even impossible, to write a good unit test for it. So, what makes code hard to test? Let’s review some anti-patterns, code smells, and bad practices that we should avoid when writing testable code.
</p>

<h3 id="poisoning-the-codebase-with-non-deterministic-factors" style="text-align: justify;">
  Poisoning the Codebase with Non-Deterministic Factors
</h3>

<p style="text-align: justify;">
  Let’s start with a simple example. Imagine that we are writing a program for a smart home microcontroller, and one of the requirements is to automatically turn on the light in the backyard if some motion is detected there during the evening or at night. We have started from the bottom up by implementing a method that returns a string representation of the approximate time of day (“Night”, “Morning”, “Afternoon” or “Evening”):
</p>

<pre class="lang:default decode:true">public static string GetTimeOfDay()
{
    DateTime time = DateTime.Now;
    if (time.Hour &gt;= 0 && time.Hour &lt; 6)
    {
        return "Night";
    }
    if (time.Hour &gt;= 6 && time.Hour &lt; 12)
    {
        return "Morning";
    }
    if (time.Hour &gt;= 12 && time.Hour &lt; 18)
    {
        return "Afternoon";
    }
    return "Evening";
}</pre>

<p style="text-align: justify;">
  Essentially, this method reads the current system time and returns a result based on that value. So, what’s wrong with this code?
</p>

<p style="text-align: justify;">
  If we think about it from the unit testing perspective, we’ll see that it is not possible to write a proper state-based unit test for this method. <code>DateTime.Now</code> is, essentially, a hidden input, that will probably change during program execution or between test runs. Thus, subsequent calls to it will produce different results.
</p>

<p style="text-align: justify;">
  Such <strong>non-deterministic</strong> behavior makes it impossible to test the internal logic of the <code>GetTimeOfDay()</code> method without actually changing the system date and time. Let’s have a look at how such test would need to be implemented:
</p>

<pre class="lang:default decode:true">[TestMethod]
public void GetTimeOfDay_At6AM_ReturnsMorning()
{
    try
    {
        // Setup: change system time to 6 AM
        ...

        // Arrange phase is empty: testing static method, nothing to initialize

        // Act
        string timeOfDay = GetTimeOfDay();

        // Assert
        Assert.AreEqual("Morning", timeOfDay);
    }
    finally
    {
        // Teardown: roll system time back
        ...
    }
}</pre>

<p style="text-align: justify;">
  Tests like this would violate a lot of the rules discussed earlier. It would be expensive to write (because of the non-trivial setup and teardown logic), unreliable (it may fail even if there are no bugs in the system under test, due to system permission issues, for example), and not guaranteed to run fast. And, finally, this test would not actually be a unit test — it would be something between a unit and integration test, because it pretends to test a simple edge case but requires an environment to be set up in a particular way. The result is not worth the effort, huh?
</p>

<p style="text-align: justify;">
  Turns out that all these testability problems are caused by the low-quality <code>GetTimeOfDay()</code> API. In its current form, this method suffers from several issues:
</p>

<ul style="text-align: justify;">
  <li>
    <strong>It is tightly coupled to the concrete data source.</strong> It is not possible to reuse this method for processing date and time retrieved from other sources, or passed as an argument; the method works only with the date and time of the particular machine that executes the code. Tight coupling is the primary root of most testability problems.
  </li>
  <li>
    <strong>It violates the <a href="https://en.wikipedia.org/wiki/Single_responsibility_principle" target="_blank">Single Responsibility Principle</a> (SRP).</strong> The method has multiple responsibilities; it consumes the information and also processes it. Another indicator of SRP violation is when a single class or method has more than one <em>reason to change</em>. From this perspective, the <code>GetTimeOfDay()</code> method could be changed either because of internal logic adjustments, or because the date and time source should be changed.
  </li>
  <li>
    <strong>It lies about the information required to get its job done.</strong> Developers must read every line of the actual source code to understand what hidden inputs are used and where they come from. The method signature alone is not enough to understand the method’s behavior.
  </li>
  <li>
    <strong>It is hard to predict and maintain.</strong> The behavior of a method that depends on a mutable global state cannot be predicted by merely reading the source code; it is necessary to take into account its current value, along with the whole sequence of events that could have changed it earlier. In a real-world application, trying to unravel all that stuff becomes a real headache.
  </li>
</ul>

<p style="text-align: justify;">
  After reviewing the API, let’s finally fix it! Fortunately, this is much easier than discussing all of its flaws — we just need to break the tightly coupled concerns.
</p>

<h4 id="fixing-the-api-introducing-a-method-argument" style="text-align: justify;">
  Fixing the API: Introducing a Method Argument
</h4>

<p style="text-align: justify;">
  The most obvious and easy way of fixing the API is by introducing a method argument:
</p>

<pre class="lang:default decode:true">public static string GetTimeOfDay(DateTime dateTime)
{    
    if (dateTime.Hour &gt;= 0 && dateTime.Hour &lt; 6)
    {
        return "Night";
    }
    if (dateTime.Hour &gt;= 6 && dateTime.Hour &lt; 12)
    {
        return "Morning";
    }
    if (dateTime.Hour &gt;= 12 && dateTime.Hour &lt; 18)
    {
        return "Noon";
    }
    return "Evening";
}</pre>

<p style="text-align: justify;">
  Now the method requires the caller to provide a <code>DateTime</code> argument, instead of secretly looking for this information by itself. From the unit testing perspective, this is great; the method is now deterministic (i.e., its return value fully depends on the input), so state-based testing is as easy as passing some <code>DateTime</code> value and checking the result:
</p>

<pre class="lang:default decode:true">[TestMethod]
public void GetTimeOfDay_For6AM_ReturnsMorning()
{
    // Arrange phase is empty: testing static method, nothing to initialize

    // Act
    string timeOfDay = GetTimeOfDay(new DateTime(2015, 12, 31, 06, 00, 00));

    // Assert
    Assert.AreEqual("Morning", timeOfDay);
}</pre>

<p style="text-align: justify;">
  Notice that this simple refactor also solved all the API issues discussed earlier (tight coupling, SRP violation, unclear and hard to understand API) by introducing a clear seam between <em>what</em> data should be processed and <em>how</em> it should be done.
</p>

<p style="text-align: justify;">
  Excellent — the method is testable, but how about its <em>clients</em>? Now it is the <em>caller’s</em> responsibility to provide date and time to the <code>GetTimeOfDay(DateTime dateTime)</code> method, meaning that <em>they</em> could become untestable if we don’t pay enough attention. Let’s have a look how we can deal with that.
</p>

<h4 id="fixing-the-client-api-dependency-injection" style="text-align: justify;">
  Fixing the Client API: Dependency Injection
</h4>

<p style="text-align: justify;">
  Say we continue working on the smart home system, and implement the following client of the <code>GetTimeOfDay(DateTime dateTime)</code> method — the aforementioned smart home microcontroller code responsible for turning the light on or off, based on the time of day and the detection of motion:
</p>

<pre class="lang:default decode:true">public class SmartHomeController
{
    public DateTime LastMotionTime { get; private set; }

    public void ActuateLights(bool motionDetected)
    {
        DateTime time = DateTime.Now; // Ouch!

        // Update the time of last motion.
        if (motionDetected)
        {
            LastMotionTime = time;
        }

        // If motion was detected in the evening or at night, turn the light on.
        string timeOfDay = GetTimeOfDay(time);
        if (motionDetected && (timeOfDay == "Evening" || timeOfDay == "Night"))
        {
            BackyardLightSwitcher.Instance.TurnOn();
        }
        // If no motion is detected for one minute, or if it is morning or day, turn the light off.
        else if (time.Subtract(LastMotionTime) &gt; TimeSpan.FromMinutes(1) || (timeOfDay == "Morning" || timeOfDay == "Noon"))
        {
            BackyardLightSwitcher.Instance.TurnOff();
        }
    }
}</pre>

<p style="text-align: justify;">
  Ouch! We have the same kind of hidden <code>DateTime.Now</code> input problem — the only difference is that it is located on a little bit higher of an abstraction level. To solve this issue, we can introduce another argument, again delegating the responsibility of providing a <code>DateTime</code> value to the caller of a new method with signature <code>ActuateLights(bool motionDetected, DateTime dateTime)</code>. But, instead of moving the problem a level higher in the call stack once more, let’s employ another technique that will allow us to keep both <code>ActuateLights(bool motionDetected)</code> method and its clients testable: <a href="https://en.wikipedia.org/wiki/Inversion_of_control" target="_blank">Inversion of Control</a>, or IoC.
</p>

<p style="text-align: justify;">
  Inversion of Control is a simple, but extremely useful, technique for decoupling code, and for unit testing in particular. (After all, keeping things loosely coupled is essential for being able to analyze them independently from each other.) The key point of IoC is to separate decision-making code (<em>when</em> to do something) from action code (<em>what</em> to do when something happens). This technique increases flexibility, makes our code more modular, and reduces coupling between components.
</p>

<p style="text-align: justify;">
  Inversion of Control can be implemented in a number of ways; let’s have a look at one particular example —<a href="https://en.wikipedia.org/wiki/Dependency_injection" target="_blank">Dependency Injection</a> using a constructor — and how it can help in building a testable <code>SmartHomeController</code>API.
</p>

<p style="text-align: justify;">
  First, let’s create an <code>IDateTimeProvider</code> interface, containing a method signature for obtaining some date and time:
</p>

<pre class="lang:default decode:true">public interface IDateTimeProvider
{
    DateTime GetDateTime();
}</pre>

<p style="text-align: justify;">
  Then, make <code>SmartHomeController</code> reference an <code>IDateTimeProvider</code> implementation, and delegate it the responsibility of obtaining date and time:
</p>

<pre class="lang:default decode:true">public class SmartHomeController
{
    private readonly IDateTimeProvider _dateTimeProvider; // Dependency

    public SmartHomeController(IDateTimeProvider dateTimeProvider)
    {
        // Inject required dependency in the constructor.
        _dateTimeProvider = dateTimeProvider;
    }

    public void ActuateLights(bool motionDetected)
    {
        DateTime time = _dateTimeProvider.GetDateTime(); // Delegating the responsibility

        // Remaining light control logic goes here...
    }
}</pre>

<p style="text-align: justify;">
  Now we can see why Inversion of Control is so called: the <em>control</em> of what mechanism to use for reading date and time was <em>inverted</em>, and now belongs to the <em>client</em> of <code>SmartHomeController</code>, not <code>SmartHomeController</code>itself. Thereby, the execution of the <code>ActuateLights(bool motionDetected)</code> method fully depends on two things that can be easily managed from the outside: the <code>motionDetected</code> argument, and a concrete implementation of <code>IDateTimeProvider</code>, passed into a <code>SmartHomeController</code> constructor.
</p>

<p style="text-align: justify;">
  Why is this significant for testing? It means that different <code>IDateTimeProvider</code> implementations can be used in production code and unit test code. In the production environment, some real-life implementation will be injected (e.g., one that reads actual system time). In the unit test, however, we can inject a “fake” implementation that returns a constant or predefined <code>DateTime</code> value suitable for testing the particular scenario.
</p>

<p style="text-align: justify;">
  A fake implementation of <code>IDateTimeProvider</code> could look like this:
</p>

<pre class="lang:default decode:true">public class FakeDateTimeProvider : IDateTimeProvider
{
    public DateTime ReturnValue { get; set; }

    public DateTime GetDateTime() { return ReturnValue; }

    public FakeDateTimeProvider(DateTime returnValue) { ReturnValue = returnValue; }
}</pre>

<p style="text-align: justify;">
  With the help of this class, it is possible to isolate <code>SmartHomeController</code> from non-deterministic factors and perform a state-based unit test. Let’s verify that, if motion was detected, the time of that motion is recorded in the <code>LastMotionTime</code> property:
</p>

<pre class="lang:default decode:true">[TestMethod]
void ActuateLights_MotionDetected_SavesTimeOfMotion()
{
    // Arrange
    var controller = new SmartHomeController(new FakeDateTimeProvider(new DateTime(2015, 12, 31, 23, 59, 59)));

    // Act
    controller.ActuateLights(true);

    // Assert
    Assert.AreEqual(new DateTime(2015, 12, 31, 23, 59, 59), controller.LastMotionTime);
}</pre>

<p style="text-align: justify;">
  Great! A test like this was not possible before refactoring. Now that we’ve eliminated non-deterministic factors and verified the state-based scenario, do you think <code>SmartHomeController</code> is fully testable?
</p>

<h3 id="poisoning-the-codebase-with-side-effects" style="text-align: justify;">
  Poisoning the Codebase with Side Effects
</h3>

<p style="text-align: justify;">
  Despite the fact that we solved the problems caused by the non-deterministic hidden input, and we were able to test certain functionality, the code (or, at least, some of it) is still untestable!
</p>

<p style="text-align: justify;">
  Let’s review the following part of the <code>ActuateLights(bool motionDetected)</code> method responsible for turning the light on or off:
</p>

<pre class="lang:default decode:true ">// If motion was detected in the evening or at night, turn the light on.
if (motionDetected && (timeOfDay == "Evening" || timeOfDay == "Night"))
{
    BackyardLightSwitcher.Instance.TurnOn();
}
// If no motion was detected for one minute, or if it is morning or day, turn the light off.
else if (time.Subtract(LastMotionTime) &gt; TimeSpan.FromMinutes(1) || (timeOfDay == "Morning" || timeOfDay == "Noon"))
{
    BackyardLightSwitcher.Instance.TurnOff();
}</pre>

<p style="text-align: justify;">
  As we can see, <code>SmartHomeController</code> delegates the responsibility of turning the light on or off to a <code>BackyardLightSwitcher</code> object, which implements a <a href="https://en.wikipedia.org/?title=Singleton_pattern" target="_blank">Singleton pattern</a>. What’s wrong with this design?
</p>

<p style="text-align: justify;">
  To fully unit test the <code>ActuateLights(bool motionDetected)</code> method, we should perform interaction-based testing in addition to the state-based testing; that is, we should ensure that methods for turning the light on or off are called if, and only if, appropriate conditions are met. Unfortunately, the current design does not allow us to do that: the <code>TurnOn()</code> and <code>TurnOff()</code> methods of <code>BackyardLightSwitcher</code> trigger some state changes in the system, or, in other words, produce <a href="http://en.wikipedia.org/wiki/Side_effect_(computer_science)" target="_blank"><em>side effects</em></a>. The only way to verify that these methods were called is to check whether their corresponding side effects actually happened or not, which could be painful.
</p>

<p style="text-align: justify;">
  Indeed, let’s suppose that the motion sensor, backyard lantern, and smart home microcontroller are connected into an Internet of Things network and communicate using some wireless protocol. In this case, a unit test can make an attempt to receive and analyze that network traffic. Or, if the hardware components are connected with a wire, the unit test can check whether the voltage was applied to the appropriate electrical circuit. Or, after all, it can check that the light actually turned on or off using an additional light sensor.
</p>

<p style="text-align: justify;">
  As we can see, unit testing side-effecting methods could be as hard as unit testing non-deterministic ones, and may even be impossible. Any attempt will lead to problems similar to those we’ve already seen. The resulting test will be hard to implement, unreliable, potentially slow, and not-really-unit. And, after all that, the flashing of the light every time we run the test suite will eventually drive us crazy!
</p>

<p style="text-align: justify;">
  Again, all these testability problems are caused by the bad API, not the developer’s ability to write unit tests. No matter how exactly light control is implemented, the <code>SmartHomeController</code> API suffers from these already-familiar issues:
</p>

<ul style="text-align: justify;">
  <li>
    <strong>It is tightly coupled to the concrete implementation.</strong> The API relies on the hard-coded, concrete instance of <code>BackyardLightSwitcher</code>. It is not possible to reuse the <code>ActuateLights(bool motionDetected)</code> method to switch any light other than the one in the backyard.
  </li>
  <li>
    <strong>It violates the Single Responsibility Principle.</strong> The API has two reasons to change: First, changes to the internal logic (such as choosing to make the light turn on only at night, but not in the evening) and second, if the light-switching mechanism is replaced with another one.
  </li>
  <li>
    <strong>It lies about its dependencies.</strong> There is no way for developers to know that <code>SmartHomeController</code>depends on the hard-coded <code>BackyardLightSwitcher</code> component, other than digging into the source code.
  </li>
  <li>
    <strong>It is hard to understand and maintain.</strong> What if the light refuses to turn on when the conditions are right? We could spend a lot of time trying to fix the <code>SmartHomeController</code> to no avail, only to realize that the problem was caused by a bug in the <code>BackyardLightSwitcher</code> (or, even funnier, a burned out lightbulb!).
  </li>
</ul>

<p style="text-align: justify;">
  The solution of both testability and low-quality API issues is, not surprisingly, to break tightly coupled components from each other. As with the previous example, employing Dependency Injection would solve these issues; just add an <code>ILightSwitcher</code> dependency to the <code>SmartHomeController</code>, delegate it the responsibility of flipping the light switch, and pass a fake, test-only <code>ILightSwitcher</code> implementation that will record whether the appropriate methods were called under the right conditions. However, instead of using Dependency Injection again, let’s review an interesting alternative approach for decoupling the responsibilities.
</p>

<p style="text-align: justify;" class="note_normal">
  <span>Published at Codingpedia.org with the permission of Toptal – source </span><a title="Are you getting worked up over code duplication?" href="http://www.toptal.com/qa/how-to-write-testable-code-and-why-it-matters" target="_blank">Unit Tests, How to Write Testable Code and Why it Matters</a><span> from </span><a title="http://www.toptal.com/blog" href="http://www.toptal.com/blog" target="_blank">http://www.toptal.com/blog</a>
</p>

<div id="about_author" style="background-color: #e6e6e6; padding: 10px;">
  <img id="author_portrait" style="float: left; margin-right: 20px;" src="{{site.url}}/images/authors/toptal-logo.png" alt="Toptal-logo" />

  <p id="about_author_header">
    <strong>Toptal Blog</strong>
  </p>

  <div id="social_logos_up">
    <a class="icon-earth" href="http://www.toptal.com/blog" target="_blank"> </a> <a class="icon-twitter" href="https://twitter.com/toptalllc" target="_blank"> </a> <a class="icon-facebook" href="https://www.facebook.com/Toptal-141928212544793/" target="_blank"> </a> <a class="icon-gplus" href="https://plus.google.com/+Toptalllc/posts" target="_blank"> </a>
  </div>

  <div id="author_details" style="text-align: justify;">
    The Toptal Engineering Blog is a hub for in-depth development tutorials and new technology announcements created by professional freelance software engineers in the Toptal network.
  </div>

  <div class="clear">
  </div>
</div>
