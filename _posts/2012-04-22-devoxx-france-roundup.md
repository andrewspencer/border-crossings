---
layout: post
title:  "Devoxx France roundup"
date:   2012-04-22
---
The first French edition of Devoxx was, first of all, a resounding success for the organisers: sold out with 1200 participants, I think we can be confident that there will be a second edition. It was also a success for the participants, or at any rate for me, in that what I got out of going there was worth the entrance ticket (notwithstanding that I went on my own time and at my own expense).
<!--more-->
Hats off to the organising team for the incredible amount of work they must have put in (for free), for having the courage to make their dream come true, and also because the logistics were top notch. Which is no mean feat, when twelve hundred people have to be looked after.
<h2>What I got out of it</h2>
What I got out of the conference was (1) the content of the talks, and (2) feeling part of a community. Getting a huge shot of new shiny stuff in the company of like minded people is a fantastic motivation boost. By the way, I can't recommend the conference to non French speakers: a quarter of talks were in English, but in practice you really needed to understand French to get most of the benefit.

There were technical talks where I got introductions to some tools and techniques that I don't see in my daily work, or haven't yet tried. But what I really valued were the talks - mostly keynotes - that purvey strategic insights. There's so much going on in the software development world that it's hard to pick out trends, especially when you've got your head down in everyday work and are only listening with half an ear.

To my mind, that's the biggest benefit of a big independent conference: in two or three days, you can take the pulse of a whole community.
<h2>What I didn't get out of it</h2>
I had been expecting to get some networking benefits, even though I'm a hopeless networker. But the style of the conference doesn't favour striking up conversations with complete strangers, unlike the Agile conferences I've been to. You do meet friends of friends that you didn't already know; so I met a couple of people from Geneva that I hadn't known, or put a name to, before. But I didn't really need to go to Paris for that.
<h2>The high point</h2>
The high point was Neal Ford's keynote "Abstraction distractions". Hilarious and tremendously useful at the same time. I love a talk that makes you better at thinking.
<h2>Is it worth going next year?</h2>
If you didn't go last year and are wondering whether to go next year, I'd say: go if you want a big conference, vendor-sponsored but independently organised, with lots to learn, mostly technical but also strategic, <strong>and you understand French</strong>. Go if you want to feel good about being a programmer. Go there to network, unless you're hopeless at it (like me) and/or hardly know anyone else who's going (surely impossible). Don't go there if you don't understand French, unless you've got an invitation; you'll only get 25% of the benefit.
<h2>What should they keep next year? What should they change?</h2>
They should keep: almost everything, and in particular
<ul>
	<li>the format: 2-3 days is exhausting enough, and longer than that is too hard to justify to employers and families</li>
	<li>the rigourous selection of speakers</li>
	<li>the invited keynote speeches from big names (more of those, if possible)</li>
	<li>the very professional venue and catering and logistics (despite the expense)</li>
	<li>the corporate sponsorship (which I dislike, but without which there wouldn't be the money to pay for professional facilities)</li>
</ul>
They should change: only minor details, like
<ul>
	<li>the lobby which didn't have enough space for 1200 people to mingle and move between sessions all at once</li>
	<li>the corporate keynotes sold to sponsors, even if that reduces sponsorship revenues. Sponsors should get regular slots in parallel with other sessions, so that we have the choice of going to listen or not. They will still get an audience, but only insofar as they have something to offer beyond a sales pitch.</li>
	<li>drinking water availability - it was too hard to find any this year.</li>
</ul>
That's the end of my round-up. In case they're useful to you, I'm including below my more-or-less raw notes from several of the sessions I attended, namely
<ul>
	<li>Fier d'être développeur - Pierre Piezzardi</li>
	<li>Manipulation de bytecode pour les nuls</li>
	<li>invokedynamic them all - Rémi Forax</li>
	<li>Kanban pour les nuls</li>
	<li>Behind the scenes of day-to-day development at Google - Petra Cross</li>
	<li>Pour un développement durable</li>
	<li>IBM talk on mobile apps</li>
	<li>Portrait du développeur en "The Artist" - Patrick Chanezon</li>
	<li>Abstraction distractions - Neal Ford</li>
	<li>sizeOf in Java</li>
	<li>TestNG parce que vos tests le valent bien</li>
	<li>Overview of Guava</li>
	<li>.NET for Java developers</li>
</ul>
<h2>Fier d'être développeur - Pierre Piezzardi</h2>
This was a call to arms for Agile principles of short positive feedback loops and focus on business value. (In some parts of the world it might have seemed superfluous, but not in France, alas.)
<h2>Manipulation de bytecode pour les nuls</h2>
See also a slide tutorial by Charles Nutter: Bytecode manipulation for dummies.

This talk was a series of tasters of 4 different techniques for manipulating bytecode: ASM, AspectJ, JByteMan and JooFlux.

By the way, the first thing to do if you're going to look at bytecode is learn its notation for types.

I'll skip most of the ASM example, because ASM is very low level and it's unlikely you'd need to use it directly in enterprise software. It's worth noting that ASM provides a reverse engineering utility: given an existing compiled class, it will generate for you the Java code that would generate that bytecode using the ASM library.

I'll also skip AspectJ because I'm not up to summarising aspects in one or two sentences, and it's well-known and well-documented enough already.

JByteMan was the most interesting section of the talk for me. This is a tool that can perform manipulations rather like AspectJ, except that instead of doing them statically at compile or load time, it connects to a running JVM and injects them into the already-running code. It's controlled by a simple DSM that allows you to define conditions upon which injection should happen, and an (extensible) library of injection actions.

JByteMan is particularly interesting for certain types of tests, because it can be used to trigger failures that are hard to similate otherwise. For instance, you could simulate a disk full error by declaring that the Nth execution of a certain method should throw an IOException. The pattern for this kind of usage is: run a coverage check on your unit (and integration) test suites, identify the branches that are uncovered and are hard to get into with standard unit testing, and slap a JByteMan annotation on the test to set off the case you need.

The final tool they showed, JooFlux, is under development. It replaces invokeXyz bytecodes by invokeDynamic and allows you to plug in a method of your choice as target of the invokeDynamic. This makes it more JIT-friendly than JByteMan since it does not modify method code, but redirects the method call chain. Thus it doesn't invalidate inlinings compiled by the JIT compiler. Which is nice, but not particularly important in a test context, so I'm not sure what are the use cases for JooFlux. Since it works via invokeDynamic, it's only usable from Java 7 onwards, but that doesn't matter because frankly I can't imagine that anyone who was forbidden from moving to Java 7 would be allowed to use something like this.

The downside of all these bytecode manipulation techniques - and we already see it with tools that use them, like Hibernate or Spring - is that you no longer have a direct relationship between your Java source and what really happens on execution. This makes debugging difficult or impossible, which means logs are your only hope when problems come up. And in my experience, no-one takes logging seriously until after the production bugs have already happened. So I asked a question about the difficulty of visualising the real execution path, and how to overcome it. The speakers agreed that it is difficult, and there isn't a canned solution, though you could write your own java agent that could capture the bytecode actually executed. (Getting java agents running in production isn't aways on the cards, though.)
<h2>Invokedynamic them all - Rémi Forax</h2>
This was a very technical talk about JIT internals, and I humbly admit to having understood less than 10% of it.

The first takehome message from this talk is that I have absolutely no idea what is really going on when I run some Java code in a JVM. I thought I had some vague idea about how bytecode works, but when the JIT swings into action, the things that happen don't have much anymore to do with bytecode. Tying this in to Neal Ford's advice to know one abstraction below the one you usually use, reminds me that I need to learn more about JVM internals.

The second takehome message is that if I want to have the slightest hope of understanding what invokedynamic does, I need to learn how _normal_ method invocations work at the bytecode level.

The third takehome message is that you can see the inlining and the generated code done by the JIT, if you really want to optimise performance.

A few specific points from the talk follow below. I may well have misunderstood any or all of them.

NB All this applies to JVM v7+ since that's the version that introduced invokedynamic.
NB (2) JRuby is the 1st language to use invokedynamic and give feedback (on performance, notably) to the JVM developers. Groovy is close behind.
NB (3) You can't write any Java code that will compile to an invokedynamic question.

Some useful JVM flags:

-XX:+PrintCompilation
Prints out what the JIT is compiling ...but you need to know how to read the output!

-XX:+PrintInlining
Inlining of a method called via an interface: TypeProfile indicates that the virtual call has been inlined.
inline(hot) indicates frequently called methods that have been inlined.
The JIT collects profiling information to help it optimise. If it notices that a virtual call always goes in fact to the same concrete class, it will inline a direct call to the concrete implementation.

-XX:+PrintAssembly
Not available with standard VM, the VM has to be compiled in fastdebug mode! And a few other tricks...

The output is erm... Well, it's assembly language, with some comments added.

By the way, did you know that the JIT profiles the frequency of execution of alternative if() branches and reorders them so that the most frequently executed test comes first? I didn't.

Hotspot makes C++ copies of all the Java objects it uses. When things get moved between heap generations upon GC, the JVM has to walk through all the objects and update addresses. When there's a NPE in JIT-compiled code, the machine code actually explodes and the JVM is able to catch the problem, identify the point where the null pointer access occurred, and reconstitute the stacktrace for the NPE. Whew.

In the case mentioned above where the JIT inlines the fact that a virtual method is always called on the same concrete class, it similarly causes a fault in the machine code which the JVM catches and de-optimises the code (it returns to interpreted mode). If I understood it rightly.

A word more about how it works. The JIT is going to inline cases where there are multiple possibilities in the code, but at execution only one case comes up. It will write a check that ensures we are in that case, and will inline (JIT-compile) only the code for the case that's really executed.

Second part of talk: invokedynamic itself

See java.lang.invoke for the classes that deal with invokedynamic by reflection.
CallSite, Lookup, MethodType
If I understood rightly, you can use this API to make invokedynamic calls from Java.

We got a runthrough of how to build invokedynamic with this API and a bit about how the JIT optimises the resulting bytecode. I understood almost none of it. You can tell that Rémi is a university professor rather than a teacher: instead of spoon-feeding nice digestible morsels of learning, he delivers great chunks of it and leaves his students to figure out how to digest them. This is certainly good training for the students, but it's kind of sadistic at a conference.
<h2>Kanban pour les nuls</h2>
Just a few random notes for this one.

Kanban is:
- kanban cards
- a kanban system: a production process, a set of rules, a "flux tiré" of kanban cards
- the Kanband method (with a capital K)

5 fundamental practices
- visualise (kanban board - visible without having to open JIRA, read a dashboard...)
- limit Work In Progress
- policy: explicit and visible rules, adopted (and owned?) by the team. e.g. Definition of done pinned up on the top of the kanban board.
- measure
- improvement. Getting stuck triggers dialogue. Collaborative. Using pragmatic and/or scientific methods.

Starting point is the existing process. You don't start, as in Scrum, but erasing your existing process and installing a new one from scratch. Nonetheless, although the speakers didn't really say it, Freddy Mallet told me afterwards that following Kanban does imply some radical organisational changes.

The kanban board: card colour used to manage granularity or categorisation.
Divided into columns by step. Max no of items per column.

The limit on WIP per col means that you work in "flux tiré" (pulled flow?). If all the columns are full, when you move something to "done" it creates a free slot on the penultimate col. You can then move something from the preceding column to that one, and so on backwards through the process. Items are pulled through the stages by other items that pass into Done.

The focus is on finished work, which goes hand-in-hand with working in pull mode. Each step in the chain requests work from the preceding step.

A key issue is how long an item takes from when it is first requested to when it is terminated (delivered in production).

The objective is for the team to settle into a working rhythm. There's a daily meeting to do a roundup of ongoing work. It's the most important among a number of ceremonies whose purpose is to manage the work rhythm.

Measuring: in Kanban a number of things are measured - number of ongoing items, time taken for an item to be done, production rate. The measurements help flag up opportunities for improvement.

Unlike scrum, where everything fits into the imposed rhythm of the sprints, in Kanban different activities (e.g. delivery, retrospectives) can take place at different rhythms.

By doing stats on the data on items, you can predict how long a given item will take, e.g. 80% chance of delivering within 5 days, ~100% chance of delivering within 7 days.

Kanban is not so much a method of production, as a method of accompanying change (to the production process).
<h2>Behind the scenes of day-to-day development at Google (Petra Cross)</h2>
This was a run-through of Google's development process, which is a hybrid Agile method although apparently not everyone at Google likes to use the A-word. I noted a few points that struck me.

50% of Google's code base changes every month!

When a feature is developed, the set of code changes is always sent for code review by another Googler.
They eat their own dog food (the GMail team uses the pre-release GMail version for their corporate email).
New features are typically activated for a small fraction of users (canary) and *results analysed* before activating globally. Analysed means that performance figures are checked...
They start from user stories (with a BDD-like expression) but developers don't work on user stories; the stories are broken down into tasks before development work starts. For both tasks and user stories, an acceptance test is part of its definition. And defines the complete scope of the work to be done - i.e. only work that's needed to pass the acceptance test will be performed.

They have an "icebox" which looks a bit like a product backlog (many of the things on it will never get done) and a backlog (things that are planned for doing within the iteration). Backlog items are prioritised.

They don't do standups.

Some teams at least use real whiteboards with real physical post-its that travel across the swim lanes. They own the post-its by sticking their photo on top.

They don't add tasks to the ongoing iteration.

They do use planning poker! I.e. relative estimation (points), with velocity. However, to synchronise diverging estimates, they aren't allowed to say anything involving a value judgement (like "it's really difficult"), but only to detail the work that is involved in the task. If they don't know enough to estimate, they play the question-mark card. Then the highest and lowest estimators list the work involved, and the questioner can also ask yes-no questions if that's not enough. All of this removes any possibility of getting into open-ended discussion, and avoids emotionalising the debate. Consequently the backlog estimation sessions are rapid (20 mins weekly).

The objective of the estimation sessions is only secondarily to get visibility on how much will be done by the end of the iteration. Most of all, its purpose is to get agreement about what needs doing.

Developers aren't allowed to cherry-pick the backlog: they have to take the highest priority remaining task, even if there's another below that they could do more easily. The point of this is to encourage knowledge spread among the team by having everyone work on everything (avoids "knowledge silos").

They have retrospectives monthly, with a simple 3-coloured post-it system: good - bad - suggestions for improvement. I got the impression that they don't vary their retrospective format, which they probably should.
<h2>Pour un développement durable</h2>
A good talk with plenty of good advice in it, but nothing new for me.

Given that most of the cost of a project is in the maintenance phase, an investment in quality during the initial development phase can easily be recouped if it reduces subsequent maintenance costs.

The cost of quality assurance is visible, but the cost of low quality is rarely considered.

The speaker has been developing a model contract for Agile software services companies in France: http://contrat-agile.org/
<h2>IBM talk on mobile apps</h2>
Round-up of the different approaches for mobile dev: native, web, hybrid (I think this means running in the web browser but with components that give access to the native APIs), and cross-platform toolkits.

The talk's very enterprise oriented of course. The challenges for businesses include the fact that mobile platforms are evolving faster than business development cycles. (Think of how often new versions of Android come out, and how often new Android devices are released.) The challenge for mobile development in general is platform fragmentation, which is why native development, though it gives the best result, is so expensive.

The second half of the talk was a plug for IBM's mobile development sleep, and my thoughts wandered. When I said that we shouldn't be forced to sit through sponsors' talks, this was the kind of thing I meant.
<h2>Portrait du développeur en "The Artist"</h2>
A funny, fast-moving and close to the bone talk about the woes of corporate software developers in France. With a ray of light at the end.

According to the speaker, the balance between client and server swings every fifteen years or so and we're in one of those swings. After many years of everything happening on the server and the client (browser) being a relatively dumb renderer, we're now going back to a client-server model where there are real components on the browser and the server is just providing the data.

The end of the talk transitioned rather neatly into a plug for cloud computing in general and Cloud Foundry in particular. The speaker is from Cloud Foundry "The Open Paas" - open source, Apache license. This means that you can install your own private cloud, and that 3rd parties have added support for languages as divergent as PHP, Erlang...
<h2>Abstraction distractions - Neal Ford</h2>
Easily the best talk of the conference.

Note: by abstractions, we mean "mental models".

The talk is about the dangers of forgetting that the abstractions we work with are abstractions only and not the real thing.

How do you store something so that it'll be readable in 100 years? What format do you use?

Some really powerful examples to illustrate the following lessons.

Lesson 1: Don't mistake the abstraction for the real thing
Lesson 2: Always know the abstraction 1 level below your usual level.
Lesson 3: Once internalised, abstractions are really hard to shake off.
Lesson 4: Abstractions are both walls and prisons.
Lesson 5: Don't name things that expose underlying details. (E.g. the floppy disk icon on a "save" shortcut. E.g. the lpstr prefix on Windows API, meaning "long pointer to null terminated string"; when they moved to 32 bit the "long" part became obsolete.)
Lesson 6: Good APIs are not merely high-level or low-level, they're both at the same time.
Lesson 7: Generalise the 80%, get the other 20% out of the way [check]

Re Lesson 6, a good abstraction should leak in well-defined ways -&gt; onionskin abstractions. This means that when the abstraction fails you, it is easy and natural to go down to the level below and achieve what you need.

At this point he made a huge dig at Maven. He distinguished between composable tools vs contextual. I failed to pick up a clear definition of this dichotomy, and can't find words to express it myself, though I sense approximately what it means. As examples, composable tools are bash, rake, gant, emacs, vi; contextual are powershell, ant, maven, eclipse or visual studio. Languages are composable, frameworks are contextual.

Contextual gives you behaviour out of the box, contextual intelligence, but less flexibility.
Composable gives you implicit behaviour (not out of the box), but better building blocks, so more flexibility. I realise that the frameworks vs libraries debate is the same issue.

Another example of a contextual tool: MIcrosoft Access.
In these tools, 80% of what the user needs is really easy. A further 10% is really hard because you have to armtwist the tool to get what you want. (I'm reminded of Hibernate here too.) The final 10% is just impossible because you can't dig deep enough under the abstraction to get it.

When you're using these tools, you're reluctant to give them up when you have trouble, because you've invested in it. But every such tool has a tipping point, after which it is "never, ever wonderful again." Keep an eye out for that tipping point, and leave the tool behind when it arrives.

Things that are composable and that are onionskin APIs: git, DSLs.
<h2>sizeOf in Java</h2>
This talk was given by one of the Terracotta staff working on ehcache, and explained the efforts they've made to measure size of Java objects as a prerequisite to dimensioning the cache by object size. I'm not 100% sure I got all the numbers below right.

An object on the heap, on top of its fields, has an object header of 2 words (8 bytes on a 32bit JVM).

However, its fields don't consume exactly their own size: object size is aligned on word boundaries. So adding/removing a byte field (8 bits) might add/remove 32 bits to/from the object size - or have no effect at all.

The VM arranges object fields, first grouped by class in the hierarchy (parent first), then for each class starting with primitives in reverse order of size, and finishes with the OOPs (other object pointers). However if the first field in a subclass is a long, it has to be aligned and this can leave a "hole" between that long and the end of the parent class's fields. In this case the JVM will squeeze in any smaller fields that will fit.

You can find out programmatically how an object is laid out. There's some code to do it in the ehcache code repository.

Because of compression, objects don't take 2x more size in 64bit JVMs. Pointers are only 32bit if they can all be fit into 4GB address space.

Another approach for finding object size in memory: Sun's Unsafe class (Oracle/Sun JVM only).

Third approach is JVMTI -&gt; java.lang.instrument.Instrumentation.getObjectSize(Object). You need a Java agent to obtain an instance of the Instrumentation interface... which means launching the JVM with a -javaagent argument: not always easy to get in production.

Ehcache didn't want to use the java agent approach, so they used AttachAPI which was introduced in 1.6. It allows you to attach to a running Java process - including the same one you're already in - and add an agent to it. But it's not on the classpath under Windows or Linux - although it is there in the JDK (JRE?) install!

There's one gotcha in all the above, which is that the CMS GC algorithm also adds an overhead to objects, used to flag whether they're available for collection. So you also need to count that.

New version of Ehcache allows you to specify cache sizes (and total size limit for all caches) in terms of bytes rather than number of elements, using the parameter maxBytesLocalHeap. It has one (big) draweback: if an object appears in several cached object graphs, its size gets counted in every one. There is an annotation that you can use to tell it about shared objects.
<h2>TestNG parce que vos tests le valent bien (quickie)</h2>
Things I've tried: dependencies between tests (briefly), parameterized tests.

Things I haven't tried: listeners and factories. Factories allow you to generate TestNG tests on the fly.

The ability to introduce dependencies between tests allows you to do stuff like, have a test that deploys the built webapp to Tomcat and then make your Selenium tests depend upon it. If the deployment fails, you don't have 101 tests failing (1 for the deployment and 100 for Selenium). Only the genuinely failing test is marked failed, the others are skipped.
<h2>Guava</h2>
<h3>Base classes</h3>
Preconditions: checkNotNul(), checkArgument()... Essentially assertions for calling at entry into methods, except they throw NPE or IllegalArgumentException or IllegalStateException, instead of AssertionError.

Objects.toStringHelper() very similar API to Apache ToStringBuilder.

Stopwatch gives nanosecond elapsed (so you don't have to subtract start from end) times.

An improved String.split(): Splitter with its own mini-fluent API for specifying detailed behaviour.
Joiner concatenates Strings optionally skipping or defaulting nulls.

CharMatcher is a base class with a bunch of methods like removeFrom(), retainFrom()...

Optional&lt;T&gt; is (IIRC) an alternative to null with some API to replace the missing value by a default. Advantages/uses: makes it explicit that you're dereferencing a potentially non-existing thing; allows you to differentiate "It's really not there" from "I don't know" cases; is a useful wrapper to put nullable values into non-null-accepting collections.

Function&lt;F, T&gt; is a one-way transformation from F to T.
Predicate&lt;F&gt;... we know what that does, right? Common use: filtering collections.
<h3>Collection classes</h3>
FluentIterable with chainable methods: skip(), filter(), transform(),
and querying methods: contains(), toImmutable()
and extraction methods: first(), firstMatch()...

You can do FP with this, but it often ends up longer than imperative Java...

Some extra data structures: Multiset&lt;E&gt; (= bag) can have multiple instances of same element. Multimap&lt;K, V&gt; but values are collections (if no value for the key, it returns empty collection, never null). BiMap&lt;K1, K2&gt; maps both ways, both keys and values are unique. Table&lt;R, C, V&gt; is equivalent to Map&lt;R, Map&lt;C, V&gt;&gt;, with a choice of implementations (sparse or dense).

Immutable collections for all JDK and Guava collection types. (Unlike Collections.unmodifiableXyz(), it makes a totally immutable (shallow) copy rather than an unmodifiable view of a fundamentally modifiable collection.)

ComparisonChain allows chaining comparisons for Comparator, only executes the chain up to the first difference, and makes the comparison order explicit (you can see reverse comparisons easily).

Ordering is an alternative, more FP approach to comparing. You start with a basic Ordering and then use the fluent API to adjust it: reverse(), compound(Comparator), onResultOf(Function), nullsFirst() which sorts nulls at front, nullsLast()... At the end of the method chain a Comparator is returned. But you can also perform operations on Iterables, like asking if they are already ordered.
<h3>Hashing API</h3>
Object.hashCode() has some interesting limitations. When you compose hashcodes from multiple objects, the intermediate value is truncated to 32 bits at each stage. You really want to truncate just at the end. Also, it isn't pluggable, i.e. doesn't separate "what to hash" from "which hashing algorithm". These limitations make it OK for in-memory hash maps but not other uses.

In Google's hashing API you start by getting a hash algorithm, pushing the data into it and retrieving the hash at the end - the calling code looks very like ToStringHelper utilisation. The hashcode you get at the end is an object and you can retrieve int, long, etc from it.

There's also a goodFastHash() which gives you the best current algorithm - values should not be persisted as they might be invalidated if the algorithm changes.

There's a caching API.
<h2>.NET for Java developers</h2>
Not many people in the room. Looks like Java developers are as parochial as they're made out to be.

.NET was polyglot right from the start (even if the start of .NET was a good many years after the start of the JVM...): now there's C#, VB, F#.

C# has acquired a number of features that Java could reasonably be jealous of (as well as not having some things that Java does have). Lambdas, auto properties, anonymous objects...
The yield keyword is rather nice. In an iterable, it lets you only construct the values when the iterator really visits them, rather than building a whole list in memory when you don't know if you're going to iterate through to the end.

var keyword is a syntactic sugar that activates type inference - it replaces the type declaration. E.g. "var thing = [expression that returns an int]" instead of "int thing = ...". (Like Scala only not so good, basically)

C# lambdas make collection manipulation far more succinct. They run through some examples of FP-style collection manipulation. Rather reminiscent of Scala...

Default methods on interfaces - hey, that sounds awfully like Scala traits, too.

Class properties with implicit getters and setters - why, that's a bit like Scala or Groovy.

If the FP stuff using lambdas is too messy for you, there's a query language LINQ that operates on collections. And the compiler knows what it means and static-type-checks it. Wow. Compare that to HQL. Erm, or JPA query language. Or JPA criterion API.

There are also dynamics, and I'm not quite sure what they are. But they appear to be useful in C#'s XML integration which looks really rather nice indeed.

They finished off by observing that, since syntax and VM details are a relatively small part of software development, .NET and Java development have more in common than they have differences, especially as process is concerned.

&nbsp;
