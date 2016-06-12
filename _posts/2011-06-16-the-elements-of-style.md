---
layout: post
title: "The Elements of Style"
date: 2011-06-16
---

William Strunk Jr. would have written beautiful code.

<a href="http://en.wikipedia.org/wiki/The_Elements_of_Style"><i>The Elements of Style</i>, by Strunk &amp; White</a>, is a style guide for English prose, widely used in writing classes in the US but less known elsewhere.  I was introduced to it many years ago when I was attempting to become a biologist: my PhD supervisor, in despair at the inscrutability of typical scientific writing, would implore all his students to read it.  

Reading it again recently, I was struck by how applicable it is to programming.  Sure, writing clearly is a useful skill for programmers, since all of us find ourselves doing technical writing from time to time.  But I mean that it was striking how much of what makes good prose also makes good code.
<!--more-->
<h3>Caution</h3>
Notwithstanding the qualities of the book, a word of warning is needed.  Some of the book's prescriptions, especially the most specific, are idiosyncratic (or, depending whom you believe, <a href="http://chronicle.com/article/50-Years-of-Stupid-Grammar/25497">wrong</a>.)  They should be read, not as laws of grammar, but as opinionated style advice.  They do not dispense with the need to apply one's own judgement.

<h3>Clarity and concision</h3>

Every guide to good style preaches clarity and concision.  <a href="http://www.ourcivilisation.com/smartboard/shop/gowerse/complete/index.htm">Ernest Gower's <i>The Complete Plain Words</i></a> springs to mind as an example in British English.  But none puts its own advice into practice so completely as <i>The Elements of Style</i>.  

Like <a href="http://www.codinghorror.com/blog/2004/10/a-pragmatic-quick-reference.html"><i>The Pragmatic Programmer</i></a>'s  &#147;Don't repeat yourself&#148;, the book's maxim is just three words long: &#147;Omit needless words.&#148;

<blockquote>Vigorous writing is concise. A sentence should contain no unnecessary words, a paragraph no unnecessary sentences, for the same reason that a drawing should have no unnecessary lines and a machine no unnecessary parts. This requires not that the writer make all his sentences short, or that he avoid all detail and treat his subjects only in outline, but that every word tell. </blockquote>

One could add, &#147;a software program no unnecessary lines of code.&#148;  

<h3>Further parallels</h3>
Several other, more specific, items of advice have obvious parallels in programming style:

<h4>Abbreviations and acronyms</h4>

<blockquote>Do not take shortcuts at the cost of clarity. ...Write things out.  Not everyone knows that MADD means Mothers Against Drunk Driving, and even if everyone did, there are babies being born every minute who will someday encounter the name for the first time.  They deserve to see the words, not simply the initials... Many shortcuts are self-defeating: <strong>they waste the reader's time instead of conserving it</strong> <i>(p80, my emphasis)</i>.</blockquote>

Compare with Steve McConnell's advice from <a href="http://www.cc2e.com/">Code Complete</a>:

<blockquote>The most important consideration in naming a variable is that the name fully and accurately describe the entity the variable represents.  An effective technique for coming up with a good name is to state in words what the variable represents.  Often that statement itself is the best variable name.  <strong>It's easy to read because it doesn't contain cryptic abbreviations</strong>, and it's unambiguous. <i>(2nd edition, p260, my emphasis)</i></blockquote>

<h4>Negatives</h4>
Strunk & White:
<blockquote>Put statements in positive form.  Avoid ... the weakness inherent in the word <i>not</i>... [T]he reader is dissatisfied with being told only what is not; the reader wishes to be told what is... [I]t is better to express even a negative in positive form.  [For example:] &ldquo;forgot&rdquo; [is better than] &ldquo;did not remember.&rdquo; <i>(p20-21)</i></blockquote>

Programmers soon learn the risks of cumulating negatives in boolean statements.  Expressing negatives in positive form simplifies such situations, as recommended <a href="http://www.cis.upenn.edu/~matuszek/General/JavaSyntax/boolean-expressions.html">here</a>: 
<blockquote>Double negations (or worse) should be avoided.  To help avoid double negations, boolean methods should be given positive names such as <code>legalMove</code> or <code>gameOver</code>, not negative ones such as <code>illegalMove</code> or <code>gameNotOver</code>.</blockquote>

<h4>Revise, rewrite, refactor</h4>
Two suggestions that call to mind refactoring:
<blockquote>Revise and rewrite. <i>(p72)</i><br/>
Clarity, clarity, clarity.  When you become hopelessly mired in a sentence, it is best to start fresh... Usually what is wrong is that the construction has become too involved at some point; the sentence needs to be broken apart and replaced by two or more shorter sentences. <i>(p79)</i></blockquote>

Exactly the same could be said about writing method bodies.  Cf <a href="http://martinfowler.com/books.html#refactoring">Martin Fowler's <i>Refactoring</i></a>, which is all about revising and rewriting code, why it should be done, and how to go about it.  

Here are some more of Strunk & White's recommendations that could equally well be taken as advice on composing and decomposing methods:

<blockquote>[R]emember that paragraphing calls for a good eye as well as a logical mind.  Enormous blocks of print look formidable to readers, who are often reluctant to tackle them.  Therefore, breaking long paragraphs in two, even if it is not necessary to do so for sense, meaning, or logical development, is often a visual help... Moderation and a sense of order should be the main considerations in paragraphing.</blockquote>

<blockquote>Keep related words together... The writer must... bring together the words and groups of words that are related in thought and keep apart those that are not so related. <i>(p28)</i></blockquote>

<blockquote>Express coordinate ideas in similar form.  This principal, that of parallel construction, requires that expressions similar in content and function be outwardly similar.  The likeness of form enables the reader to recognize more readily the likeness of content and function. <i>(p26)</i></blockquote>

Paragraph composition uses the same skills as breaking up long methods into understandable chunks (cf the Long Method smell in <a href="http://martinfowler.com/books.html#refactoring"><i>Refactoring</i></a>, p76) - though methods benefit from a name, as though one were to place a subheading on every paragraph.

<h4>Cohesion</h4>
Keeping related words together (above) is closely related to the idea of cohesion in routines (Code Complete, p168), and to the <a href="http://www.markhneedham.com/blog/2009/06/12/coding-single-level-of-abstraction-principle/">&#147;Single Level of Abstraction Principle&#148; (SLAP)</a>.  The first says that a routine should do one thing and one thing only.  The second states that the lines of code in a method should all be expressed at the same level of abstraction: you shouldn't mix, for example, lines expressing a business rule with lines having a purely technical significance like writing to a file.  Rather you should refactor the lower-level code into its own method and give the method a name that is at the appropriate level of abstraction for the code from which you are calling it.

&#147;Express coordinate ideas in similar form&#148; crops up in <a href="http://www.infoq.com/articles/implementations-patterns-br">Kent Beck's Implementation Patterns</a> (p15) <!-- TODO quote --> as "symmetry".  (If you'll excuse an off-topic hat-tip, it's also reminiscent of Christopher Alexander's principle of Alternating Repetition from <a href="http://www.natureoforder.com/"><i>The Nature of Order</i></a>; Alexander's work concerns architecture but has <a href="http://www.cs.pitt.edu/~chang/budha/coplien.htm">much influenced computer science</a>.)

<h3>The underlying principle</h3>
Of far less obvious relevance to programming are the chapters dealing with such matters as the placement of commas, or the difference of meaning between &#147;nauseous&#148; and &#147;nauseating.&#148;  And yet, and yet... What guides every single one of these recommendations -- even the most whimsical -- is an underlying principle: <b>to bring the form of what you write as close as possible to its meaning</b>.  In programming, this principle is not merely relevant; it is critical.  It is the foundation of comprehensibility.  For this reason, I think that the most valuable aspect of the book is not its variously-reliable prescriptions on points of grammar, nor its excellent style advice, nor even the model of crisp writing that it provides.  Its most valuable lesson is the adoption of clarity, precision and brevity as ideals to aim for.  Infusing oneself with this spirit can do only good to one's code, and it is for that reason that I commend the book to you, my fellow programmers.
