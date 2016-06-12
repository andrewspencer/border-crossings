---
layout: post
title: Living fossils, or, How do you preserve old development environments?
date: 2011-07-21
---

What's the average lifespan of a corporate software application?  I'm feeling too lazy to find a serious study, but a licked finger in the air says that a quarter or so live out a decade.

We all know that a decade is a long time in IT, but one tends to forget just how long, until one encounters a living fossil - an application that hasn't evolved in the last <del>hundred million</del> ten years.<!--more-->

<a href="http://www.flickr.com/photos/oddwick/875285129/"><img title="Coelacanth" src="http://farm2.static.flickr.com/1363/875285129_64013aff1a.jpg" alt="" width="500" height="333" /></a><br/>
<i>I recently came face to face with this - written back when Struts was a mere gleam in Craig McClanahan's eye</i>

Now, there are some applications that survive by evolving, through regular maintenance releases or new functionality, for years on end.  Like living organisms, these applications become a hodgepodge of <a href="http://en.wikipedia.org/wiki/Cerebral_cortex">recent innovations</a> and <a href="http://en.wikipedia.org/wiki/Pineal_gland">ancient parts</a>.

Those are not living fossils.  A living fossil is the kind of application that has run untouched in production for years, until suddenly someone somewhere (not a programmer, obviously) decides a label needs changing, or that the number of thingummies that can be added to a doobrey shall no longer exceed fifteen.  And then suddenly, BANG! this <em>thing</em> arrives on your plate, as a developer, and you have to figure out how to modify it.

(In passing, I'd like to mention how deeply I hate being asked to do maintenance on an application of which I understand neither the code nor the business logic.)

And it's at that moment that you realise the last development was done three years ago, the person who did it has left, and there is absolutely no documentation concerning how to run the application in a test server on the developer's machine (it's a web application).  You <em>can</em> find out what application server it's running on in production, but oops - the license for the IDE plugin to run that particular server locally expired years ago.  Maybe it ran under Tomcat too?  Ah no - it doesn't. Should you spend an unknown amount of time figuring out how to get it to? How long was this maintenance estimated to last? Two days? How long have you already spent getting a grasp on things?  Three hours?  Hmm...

Of course, you don't absolutely <em>have</em> to run it on your own machine.  You could just build the deployable artifact and get it deployed to the dev environment and test your changes there.  (Let's assume, for the sake of argument, that there's a working build script in the project that doesn't depend on some artifact on your local machine that no-one thought to check in.)  It won't exactly make for a lightning fast code-execute cycle, but you only have a very minor change to make.  (What's that you ask?  No, no unit tests, and not testable code.  Don't be silly.)

Oh, and wait a minute.  The application servers are in a DMZ so you won't be able to connect a debugger, and you'll have to email the systems team every time you want a copy of the logs.  Better cross your fingers that you guess right first time, eh?  (Did I mention that you didn't know anything at all about the code or its business logic until about two hours ago?)

<h3>The nub of the problem</h3>
I'm not only writing all of this to let off steam.  The dismal story above must happen every day in hundreds of organisations around the world.  The fundamental problem is that <strong>what's under source control isn't enough to continue developing an application; it only allows you to build it</strong>.  To develop, you need tools, and often specific versions of those tools.  But software tools change, and developers upgrade their machines and install new versions or different tools.  If the organisation doesn't take conscious countermeasures, it will find itself unable to make trivial modifications to its older and less-frequently-maintained applications, since the effort required to reconstitute a development environment is out of all proportion to the value of the enhancement.  Or, in the worst case, as I discovered, it can become simply impossible: your license can expire and the supplier no longer be in business.

What happens in your organisation?  Does this sort of problem occur?  Has the organisation looked for solutions?  Does it have a strategy?  And if you've found a magic bullet to solve the problem, do write and tell me.

<h4>Postscript</h4>
My own recent experience wasn't quite as bad as I made it sound, since the previous developer was still available and helped me out.  And it all ended happily: I did guess right, and my change worked first time. I *did* manage to -- whoops! -- cause a blocking bug in production, but that was fixed within the hour, not least because <em>I still had a working development environment on my machine</em>.
