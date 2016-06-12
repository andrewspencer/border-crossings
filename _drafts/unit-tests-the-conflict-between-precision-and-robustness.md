---
layout: post
title: "Unit tests: the conflict between precision and robustness"
---

When we think about testing, we think about the contract that a class offers to its clients. A well defined contract helps us to write tests, and writing tests helps us to define the contract clearly.

However, a class is not only the provider of a contract; it is also the consumer of the contracts of all of its dependencies.

A genuine unit test only tests a single class.  The utility of unit testing is that the test not only identifies a failure, but localises it.  In order to test only a single class, however, the class has to be isolated from its dependencies.

Isolated tests are also fragile tests. The use of stubs/mocks freezes the class into a particular use mode of its dependencies. The problem is that, while the class can define the contract that it provides, the contracts that it is consuming may be flexible, and offer several possible ways of achieving the class's aim.

Ideally we would like to write robust tests.  A robust test is expressed in terms of the expected behaviour. It is not sensitive to the details of how that behaviour is implemented.  Unfortunately, we can't write stubs or mocks that anticipate all the possible ways in which the class might use its dependencies.  That entail as much complexity as using the real dependencies themselves - and duplicate their logic for good measure.

So, is it better to write isolated tests or robust tests? And is there any way of reconciling the two?


One of the benefits of genuine unit tests - those that isolate the unit under test, by mocking/stubbing its dependencies - is that they help to locate failures.  If your test fails, you know where to look for the bug: in the tested class... or in the test itself.  

When you test a class without isolating from its dependencies, your test may fail due to an error in the class itself, or in the dependencies.  With this testing style, a single error in production code will typically lead to two or three tests failing in different layers of the software, which hinders rapid diagnosis.

Consequently, a number of authorities advise writing unit tests that isolate the unit under test.

There are also authorities who advise making tests as robust as possible, by testing the <i>what</i> rather than the <i>how</i>.  That is, testing the interface rather than the implementation.  An example: you have a method that finds persons, and currently it uses the criteria API of your ORM - let's say Hibernate.  You could change that method so that it instead used HQL or native SQL, but its behaviour remained exactly the same.  An ideal test would still pass, since an ideal test would express what it is that you want to achieve.  An isolated unit test would fail, because it would have mocked the ORM library call, and the mock setup required for a Hibernate Criteria API call would be quite different from that required for an HQL query.

This is the debate between black-box and white-box testing, but I think it helps to get a concrete grasp of the implications when it's framed in terms of its effects on the precision and the robustness of tests.

The preference for black- or white-box testing tends to take on quasi-religious dimensions, but I ...
<!-- should keep in mind the pros and cons and decide case-by-case, e.g. for DAO level I prefer to test with the real persistence layer.  One way to compromise is to test by layers, lowest first, and skip higher levels when a lower-level test fails. Then you can depend on lower-level classes in your tests, knowing that they have passed their own tests. -->
\
