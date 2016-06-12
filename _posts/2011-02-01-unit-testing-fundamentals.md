---
layout: post
title: "Unit testing fundamentals"
date: 2011-02-01
---

This article describes the principles of unit testing, rather than the technical details.  It's aimed at beginners.  Experienced unit-testers will get little from it, except perhaps the pleasure of pointing out mistakes.
<!--more-->

<h3>What is a unit test?</h3>
A unit test is a test that tests a single unit of functionality, in isolation from all others, for a single set of conditions, and has a binary pass/fail result.  For object-oriented languages, that means testing a single method on a single class with a single set of parameters.  It is written by the developer, at approximately the same time as the code, for its purpose is satisfy the developer that said code behaves as she believes it should.  (I believe there are huge advantages to <a href="http://en.wikipedia.org/wiki/Test-driven_development">writing the unit test immediately <i>before</i> the code it tests</a>, but that isn't the subject of this article.)

In practice, it is impossible to test the code of a class in total isolation from all other classes.  Even the simplest method uses classes from the Java standard library, not to mention classes from other libraries and classes from the application under development.  With each of these dependencies, we have only two options.  We can use it as is, or we can isolate the dependency by replacing it with a placeholder object, known as a stub, or mock, or fake, or double, or spy (the differences between these terms are subtle, and often ignored.  I'll elaborate shortly).  

<h3>Placeholders</h3>

If a placeholder is used, it needs to be created for the purpose, and often told how to behave.  This needs to be done within the test code, before calling the method under test.  After calling the method under test, you may also need to interrogate your placeholder to find out whether it was called as you expected.  

<h4>Is it a mock or a stub?</h4>
A short terminological digression: Whether your placeholder is strictly speaking a mock, stub, or spy depends on whether you tell the placeholder how to behave, or ask it how it was treated.  
<ul>
<li>If you simply tell it how to behave in certain circumstances (e.g. "always return 3 from countFoos()"), it's a stub.</li>
<li>If you don't much mind how it behaves, but interrogate it afterwards to find out whether certain methods were called on it, and with what parameters, it's a spy.</li>
<li>If you do both, and especially if you construct the object such that it will explode if it is not called according to your expectations, then it's a mock.</li>
<li>"Double" is a catch-all term for placeholder objects.</li>
</ul>
(Here's a <a href="http://martinfowler.com/bliki/TestDouble.html">brief article from Martin Fowler outlining the taxonomy</a>, though I'm not convinced anybody really uses the terms "fake" and "dummy" with as much precision as those definitions provide.)

<h4>Off-the-shelf packages</h4>
Occasionally you find off-the-shelf packages of mock objects or stubs provided to assist testing of commonly-used classes (such as network or UI packages).  Be wary of them: such a static mock framework is rather a heavyweight dependency to add to your project, and nothing guarantees that it will be maintained as assiduously as the product it mocks.  You might easily find yourself trapped in an obsolete version of the product, because the mock framework is no longer maintained and you can't afford to rewrite all the test code that uses it.  Furthermore, modern on-the-fly mock generators have reduced the amount of work that static mock frameworks can save you.

<h4>Mock object generators</h4>
I recommend, then, that you to generate mocks using a mock-object generator, and for Java the generator I recommend is <a href="http://mockito.org/">Mockito</a>.  That is the only recommendation for a specific technology you'll find in this article, and I'm making it because (as described in <a href="http://www.testingtv.com/2010/12/20/mocks-suck-and-what-to-do-about-it/">this presentation</a>), Mockito's approach where you specify only those behaviours you need, and afterwards verify only those interactions you need, encourages less brittle and easier-to-read tests, as compared to an older mock generator like <a href="http://easymock.org/">EasyMock</a> which requires you to specify all behaviours and expected interactions up-front.

<h4>Masochism</h4>
Of course, you can also write your placeholders entirely by hand.  But the tedium of it will just drive you to write your own mock-object framework.  Don't do this (unless, that is, the framework you write is so good that you release it and it supplants all the existing ones).

<h3>Dependency isolation requires dependency injection</h3>

Since we want to keep test and production code separate, we can only replace a dependency by a mock or stub during testing if that dependency is <a href="http://martinfowler.com/articles/injection.html">injected</a> into the object under test.  We cannot replace those that the tested object instantiates or retrieves explicitly by itself.  

Thus, isolating dependencies involves extra effort: design effort in the tested class, to ensure that its dependencies are injectable, and coding effort in the test code, to prepare the mocked dependencies.  When, then, should we do it, and when should we content ourselves with the real object?  We can find the answer by looking at what we aim to achieve with a unit test. 

<h3>The utility of unit tests</h3>
Each unit test tells us whether a particular method call with particular parameters behaved as expected, or not.  One use of this is to detect bugs.  But integration and acceptance tests will also detect bugs.  The particular value of a unit test is that:

<ol><li>It helps us find the cause of a bug, since whenever a test fails, the incorrect code must necessarily be in the tested method - insofar as we have taken the trouble to isolate the tested method.</li>
<li>It finds bugs as soon as they are written, insofar as we launch them very frequently during coding (to encourage which, they need to be quick of execution).</li>
</ol>

<h3>Which dependencies to isolate?</h3>

To optimise our effort, we should therefore aim to eliminate those dependencies that might: 
<ul>
  <li>make the test fail when the tested class is correct (or make it pass when the tested class is faulty), thus reducing the value of the information that a test failure gives us
    <ul>
      <li>Uncertain behaviour: classes that we don't trust to be bug-free (typically the ones we've written ourselves), or whose behaviour we don't fully understand (this sometimes happens when getting to grips with complex UI or persistence frameworks)</li>
      <li>Indeterminate behaviour: most often, classes whose behaviour is time-sensitive, such that a particular behaviour cannot predictably be reproduced with the original dependency.</li>
      <li>States that are difficult to trigger: for example, we might want to test how our class behaves when a write to the filesystem throws an IOException.</li>
    </ul>
  </li>
  <li>slow down the test, thus reducing the incentive to launch tests often
    <ul>
      <li>Accesses to underlying resources: database, remote webservices, the filesystem...</li>
    </ul>
  </li>
</ul>

Armed with these criteria, we can decide, for each dependency in a class, whether to mock.  In practice, we don't even need to think about it in many cases:

<ul>
  <li>Should not be mocked: objects from most library classes, especially those in java.lang and java.util, apart from the exceptions below; any objects with no real behaviour, such as parameter objects, data transfer objects and other bean-type objects.</li>
  <li>Should be mocked: persistence classes; objects that provide user input; objects backed by network resources; timers; anything not trusted to be totally bug free (i.e. outside of a well-used library class); anything whose behaviour may change, such as a service returning real-world data whose content varies from one day to another</li>
  <li>Grey area: anything that is merely more difficult to obtain or set up than the equivalent mock/stub, but not less reliable or significantly slower to run.  The maintenance burden of creating mocks for a test needs to be balanced against the difficulty of using the real object.  Further, using the real object makes it more likely that the dependency will behave under test as it does in production.
</ul>

<h3>Integration tests: when it's impossible to isolate dependencies</h3>
There is one further limitation to isolating dependencies.  In some cases it is technically impossible either to dependency-inject or to mock: 
<ul>
  <li>you can only use DI with an object whose instantiation you control.  Any framework that creates objects itself, even if the class of the created object is one you've written, does not give you an opportunity to inject dependencies. </li>
  <li>you can only create mocks of non-final classes (which is another reason no-one mocks java.lang.String, aside from the pointlessness of such an act).
</ul>

In these cases, your only option is to do an integration test.  Integration tests, by my definition, are any tests written by developers, as unit tests are, that produce an pass/fail result, as unit tests do, but that cover more than one significant class in the system or its libraries.  If you test a data access object with a real underlying database, that is an integration test, since you are simultaneously testing your DAO code (sometimes minimal or auto-generated), your ORM mapping and your database schema.  And - not the least important - you're testing their coherence with respect to each other.

Good practice for integration tests is to have a the minimal set necessary to make sure that different elements like ORM mapping and database schema are coherent.  You need at least this much, because integration tests will catch things that unit tests never can: incompatibility between elements (a property added in a persistence entity bean and the ORM mapping, but missing from the database...) and configuration errors.  You don't want any more than that, because integration tests tend to require more work to set up and maintain -- and take more time to run.
