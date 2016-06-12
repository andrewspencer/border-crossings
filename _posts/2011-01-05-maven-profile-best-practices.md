---
layout: post
title:  "Maven profile best practices"
date:   2011-01-05
---

Maven profiles, like chainsaws, are a valuable tool, with whose power  you can easily get carried away, wielding them upon problems to which  they are unsuited.  Whilst you're unlikely to sever a leg misusing Maven  profiles, you can cause yourself some unnecessary pain.  These three best practices all sprung from first-hand, real-world suffering:
<ul>
	<li>The build must pass when no profile has been activated</li>
	<li>Never use &lt;activeByDefault&gt;</li>
	<li>Use profiles to manage build-time variables, not run-time variables  and not (with rare exceptions) alternative versions of your artifact</li>
</ul>
<!--more-->
I'll expand upon these recommendations in a moment.  First, though,  let's have a brief round-up of what Maven profiles are and do.

<h3>Maven Profiles 101</h3>
A Maven profile is a sub-set of POM declarations that you can  activate or disactivate according to some condition.  When activated,  they override the definitions in the corresponding standard tags of the  POM.  One way to activate a profile is to simply launch Maven with a -P  flag followed by the desired profile name(s), but they can also be  activated automatically according to a range of contextual conditions:  JDK version, OS name and version, presence or absence of a specific file  or property. The standard example is when you want certain declarations  to take effect automatically under Windows and others under Linux.  Almost all the tags that can be placed directly in a POM can also be  enclosed within a &lt;profile&gt; tag.

The easiest place to read up further about the basics is the <a href="http://www.sonatype.com/books/mvnref-book/reference/profiles.html"><em>Build Profiles</em> chapter of Sonatype's Maven book</a>.   It's freely available, readable, and explains the motivation behind  profiles: making the build portable across different environments.

<h3>The build must pass when no profile has been activated</h3>
(Thanks to <a href="http://java.dzone.com/users/nfrankel">Nicolas Frankel</a> for this observation.)

<h4>Why?</h4>
Good practice is to minimise the effort required to make a successful  build.  This isn't hard to achieve with Maven, and there's no excuse  for a simple mvn clean package not to work.  A maintainer coming to the  project will not immediately know that profile wibblewibble has to be  activated for the build to succeed.  Don't make her waste time finding  it out.

<h4>How to achieve it</h4>
It can be achieved simply by providing sensible defaults in the main  POM sections, which will be overridden if a profile is activated.

<h3>Never use &lt;activeByDefault&gt;</h3>

<h4>Why not?</h4>

This flag activates the profile if no other profile is activated.   Consequently, it will fail to activate the profile if any other profile  is activated.  This seems like a simple rule which would be hard to  misunderstand, but in fact it's surprisingly easy to be fooled by its  behaviour.  When you run a multimodule build, the activeByDefault flag  will fail to operate when any profile is requested, <em>even if the profile is not defined in the module where the activeByDefault flag occurs.</em>

(So if you've got a default profile in your persistence module, and a <a href="http://docs.codehaus.org/display/MAVENUSER/Solving+the+Skinny+Wars+problem">skinny war</a> profile in your web module... when you build the whole project,  activating the skinny war profile because you don't want JARs duplicated  between WAR and EAR, you'll find your persistence layer is missing  something.)

activeByDefault automates profile activation, which is a good thing;  activates implicitly, which is less good; and has unexpected behaviour,  which is thoroughly bad.  By all means activate your profiles  automatically, but do it explicitly and automatically, with a clearly  defined rule.

<h4>How to avoid it</h4>
There's another, less documented way to achieve what  &lt;activeByDefault&gt; aims to achieve.  You can activate a profile in  the <em>absence</em> of some property:

[xml]
<profile id="nofoobar">
    <activation>
        <property>
            <name>!foo.bar</name>
        </property>
    </activation>
</profile>
[/xml]
This will activate the profile "nofoobar" whenever the property foo.bar is not defined.

Define that same property in some other profile: nofoobar will  automatically become active whenever the other is not.  This is  admittedly more verbose than &lt;activeByDefault&gt;, but it's more  powerful and, most importantly, surprise-free.

<h3>Use profiles to adapt to build-time context, not run-time context,  and not (with rare exceptions) to produce alternative versions of your  artifact</h3>

Profiles, in a nutshell, allow you to have multiple builds with a single POM.  You can use them in two ways:
<ul>
	<li>To adjust <em>how</em> you build: that is, to adapt the build to variable circumstances (developer's machine or CI  server; with or without integration tests) whilst still producing the  same final artifact, or</li>
	<li>To adjust <em>what</em> you build: that is, to produce variant artifacts.</li>
</ul>

We can further divide the second option into: structural variants,  where the executable code in the variants is different, and variants  which vary only in the value taken by some variable (such as a database  connection parameter).

If you need to vary the value of some variable at run-time, profiles  are typically not the best way to achieve this.  Producing structural  variants is a rarer requirement -- it can happen if you need to target  multiple platforms, such as JDK 1.4 and JDK 1.5 -- but it, too, is not  recommended by the Maven people, and profiles are not the best way of  achieving it.

A common case where profiles seem like a good solution is when  you need different database connection parameters for development, test  and production environments.  It is tempting to meet this requirement  by combining profiles with Maven's <a href="http://www.sonatype.com/books/mvnref-book/reference/resource-filtering-sect-description.html">resource filtering capability</a> to set variables in the deliverable artifact's configuration files (e.g. Spring context).  This is a bad idea.

<h4>Why?</h4>
<ul>
	<li>It's indirect: the point at which a variable's value is determined  is far upstream from the point at which it takes effect.  It makes work  for the software's maintainers, who will need to retrace the chain of  events in reverse</li>
</ul>
<ul>
	<li>It's error prone: when there are multiple variants of the same  artifact floating around, it's easy to generate or use the wrong one by  accident.</li>
</ul>
<ul>
	<li>You can only generate one of the variants per build, since the  profiles are mutually exclusive.  Therefore you will not be able to use  the Maven release plugin if you need release versions of each variant  (which you typically will).</li>
</ul>

<ul>
	<li>It's against Maven convention, which is to produce a single artifact  per project (plus secondary artifacts such as documentation).</li>
</ul>

<ul>
	<li>It slows down feedback: changing the variable's value requires a  rebuild.  If you configured at run-time you would only need to restart  the application (and perhaps not even that).  One should always aim for <a href="http://agile.dzone.com/articles/feedback-key">rapid feedback</a>.</li>
</ul>


Profiles are there to help you ensure your project will build in  a variety of environments: a Windows developer's machine and a CI  server, for instance.  They weren't intended to help you build variant  artifacts from the same project, nor to inject run-time configuration  into your project.

<h4>How to achieve it</h4>
If you need to get variable runtime configuration into your project, there are alternatives:
<ul>
	<li>Use JNDI for your database connections.  Your project only contains  the resource name of the datasource, which never changes.  You configure  the appropriate database parameters in the JNDI resource on the server.</li>
	<li>Use system properties: Spring, for example, will pick these up when attempting to resolve variables in its configuration.</li>
	<li>Define a standard mechanism for reading values from a configuration  file that resides outside the project.  For example, you could specify  the path to a properties file in a system property.</li>
</ul>
Structural variants are harder to achieve, and I confess I have no first-hand experience with them.  I recommend you read <a href="http://www.sonatype.com/people/2010/01/how-to-create-two-jars-from-one-project-and-why-you-shouldnt/">this explanation of how to do them and why they're a bad idea</a>,  and if you still want to do them, take the option of multiple JAR  plugin or assembly plugin executions, rather than profiles.  At least  that way, you'll be able to use the release plugin to generate all your  artifacts in one build, rather than only one of them.

<h4>Consider also Maven's per-user settings</h4>

Per-user settings are a bad idea in most cases, because the whole objective of the exercise is to have all artifacts under source control or in a Maven repository, such that the build can be replicated on any machine.  However, when you want persistence tests to run in a different database schema for every developer, Maven's per-user settings file (~/.m2/settings.xml) is a sensible alternative to profiles.  In this case, you really do want the project to build differently depending on who runs the build.  If you do this, make sure you still provide working default values in the POM itself (they will be over-ridden by user settings), such that builds will still work with an empty ~/.m2/settings.xml.

(Thanks to <a href="http://blog.tallan.com/author/efitchett/">Eric Fitchett</a> for this suggestion.)


<h3>Further reading</h3>
<ul>
	<li><a href="http://www.sonatype.com/books/mvnref-book/reference/profiles.html">Profiles chapter</a> from the Sonatype Maven book.</li>
	<li>Deploying to multiple environments (prod, test, dev):
<ul>
	<li><a href="http://stackoverflow.com/questions/2424118/maven-best-practice-for-generating-artifacts-for-multiple-environments-prod-tes"> Stackoverflow.com discussion</a>; see the first and top-rated answer.   Short of creating a specific project for the run-time configuration, you  could simply use run-time parameters such as system properties.</li>
</ul>
</li>
	<li>Creating multiple artifacts from one project:
<ul>
	<li><a href="http://www.sonatype.com/people/2010/01/how-to-create-two-jars-from-one-project-and-why-you-shouldnt/"> How to Create Two JARs from One Project (…and why you shouldn’t)</a> by Tim O'Brien of Sonatype (the Maven people)</li>
	<li><a href="http://blog.jayway.com/2010/01/21/one-artifact-with-multiple-configurations-in-maven/"> Blog post explaining the same technique</a></li>
</ul>
</li>
	<li>Maven best practices (not specifically about profiles):
<ul>
	<li><a href="http://mindthegab.com/2010/10/21/boost-your-maven-build-with-best-practices/"> http://mindthegab.com/2010/10/21/boost-your-maven-build-with-best-practices/</a></li>
	<li><a href="http://blog.tallan.com/2010/09/16/maven-best-practices/"> http://blog.tallan.com/2010/09/16/maven-best-practices/</a></li>
</ul>
</li>
</ul>

