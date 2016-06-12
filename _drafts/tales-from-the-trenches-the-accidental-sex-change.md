---
layout: post
title: "Tales from the Trenches: the Accidental Sex Change"
---

&#147;The whole experience of being hit by a bullet is very interesting and I think it is worth describing in detail&#148; wrote George Orwell in <i>Homage to Catalonia</i> <a href="#orwell">[1]</a>.  That's exactly how I feel about the events I'm about to relate - which are all true, though as a homage to Eric Arthur Blair, and in order to spare the blushes of some of those involved, I've transcribed the business context to the Spanish Civil War.  (The transcription is not intended to be historically accurate - other than the fact that some women did bear arms in that conflict. <!-- TODO check -->

The POUM militia regiment for which I was working had an old, creaking application to register the soldiers assigned to it. This application was scheduled for rewriting - though the database was to be preserved, in all its legacy glory (dates as integers, zero used in place of null, that sort of thing).  At the time this story begins, a couple of programmers had already been working on the new application for three months, without the ghost of a specification or anything approaching it.  Development had then ceased since both had left, not entirely of their own volition.

I was brought in, not to resuscitate development of the replacement app, but for a new requirement.  Data on soldiers' periods of service had to be uploaded to POUM headquarters, for those soldiers who were POUM party members.  In preparation for this, our soldier IDs had already been matched up with POUM party member IDs - a new column POUM_ID had been added to the SOLDIER table to hold the information.  Service data was not, however, uploaded directly from the existing SOLDIER and PERIOD_OF_SERVICE tables.  Instead, a new table had been created to contain the changes (creation, modification and deletion of service status per-soldier) that were to be sent to POUM HQ.  Initially some ad-hoc scripts were used to fill this table with the complete historical data, and the legacy app was modified so that any changes made in the app would also write a line into the table.  

I didn't do that work.  My job was to write the batch job that uploaded data from the INTERFACE_POUM table to a webservice that POUM HQ had provided.  I got to use Spring Batch for it, which was fun, and CXF to auto-generate the client stub and data classes from the POUM's WSDL [2] <!-- TODO footnote: now that I've read [article referenced by Martin Fowler], I'd think twice before doing this because it automatically validates everything against the WSDL's schema, which is brittle since almost any change to the schema will break the client even when there's no business need to change the client. -->.  That's not the important part, though.  The important part is this: I reused the persistence layer that my former colleagues had written in their unfinished replacement to the legacy management app.

There was also a microscopic web-app to consult the contents of the INTERFACE_POUM, so that the end users could verify what was being sent to POUM HQ and whether the lines had been uploaded successfully.  They couldn't modify anything though - a detail that will be significant later.  This web-app also used the persistence layer from the part-developed replacement app.

<!-- TODO describe how we found the persistence layer was an auto-generated set of JPA entities and DAOs. Mention that the batch updated a couple of other tables, though not SOLDIER -->

I was rather proud of my little batch job, because not only did it have over 90% test coverage, but it was finished within schedule.  It was the first time I'd worked on a project that achieved either of those things, and I suspect that the first achievement was the reason for the second.  Before anyone thinks I'm being smug about the test coverage, I should admit that there was hardly any code to unit-test - Spring Batch and CXF did all the heavy lifting.  I did write a reasonable set of integration tests, but I'm not smug about that either, because ...well, you'll see why.

So, we reached the ready-for-acceptance-testing milestone on time, which was a relief since the client was in a big hurry.  Then there was a hiatus.  Three months later, the client found time to do the acceptance tests.  It all went fine - I think we had to correct one or two edge cases - and we ran the full upload on the production database shortly afterwards.  The client was happy. (I don't have any evidence for the last statement, but can deduce it from the fact that we heard nothing more about it for some time.)

<!-- TODO fill out following parts -->

In the meantime, the specs for the new application arrived and a team was formed to develop it. I was on that team. We kept all of the persistence layer we'd inherited from the previous effort, and got rid of everything else. To this day, I don't understand how our project manager thought we could make any use of it.

One day, I happened to be reading the entity class for the SOLDIER table and was mildly disturbed to notice that the column SEX was mapped to a boolean.  A search of the sources turned up just one screen where the value was displayed, and another where it could be modified, so I created an enum and a Hibernate custom type for it, and replaced the boolean in the mapping with that. I thought no more of it for some time.

- found the soldiers all changed to male in the legacy app / DB
- managed to rescue the data from a DB backup made before the first full upload
- replicated the issue in dev database
- thought of the issue where SOLDIER.SEX had been a boolean, produced a new version of batch job using the corrected persistence layer, problem was corrected
- never did manage to find where in the code the Soldier object had been persisted
- problem cropped up again
- tore hair out
- eventually realised it was the consultation webapp
- [full description of bug]

What lesson can be learned from such a sorry tale of ineptitude and woe?  In a retrospective, you're supposed to come up with a single action for improvement.  Here, like an aviation accident, the mishap resulted from a chain of events, none of which taken individually were serious, and stopping any one of them would have prevented the disaster from happening.  Here are the recommendations that an accident board of enquiry might have made:
<ul>
	<li>If we hadn't used Hibernate, it wouldn't have happened.</li>
	<li>If the Hibernate team hadn't written a non-commutative mapping, it wouldn't have happened.</li>
	<li>If the ORM entities hadn't been auto-generated, it wouldn't have happened.</li>
	<li>If someone had read through the auto-generated ORM entities properly, it wouldn't have happened.</li>
	<li>If the ORM entity auto-generation tool hadn't assumed that a single-digit number is a boolean, it wouldn't have happened.</li>
	<li>If we hadn't taken over the project in a half-finished state, it wouldn't have happened (because we would have realised that the auto-generated ORM hadn't been checked).</li>
	<li>If we had marked the transaction read-only in Spring, it wouldn't have happened.</li>
	<li>If we had saved the objects individually rather than using a cascade, it wouldn't have happened (since the SOLDIER object would never have been saved).</li>
	<li>If we had checked the full state of the database after integration testing, rather than just the tables that we <em>thought</em> should have changed, it wouldn't have happened.</li>
</ul>



<h3>References and resources</h3>
<ul>
<li><a name="orwell"/>George Orwell, <i>Homage to Catalonia</i></a> (chapter 12 in the first edition).  <a href="http://gutenberg.net.au/ebooks02/0201111.txt">Online text</a>; <a href="http://orwell.ru/library/novels/Homage_to_Catalonia/english/e_htc">another online text</a>; <a href="http://en.wikipedia.org/wiki/Homage_to_Catalonia">Wikipedia entry</a>.  <i>Homage to Catalonia</i> is one of the most important books of the 20th century, and infinitely more worth reading than anything in this blog.
</li>
</ul>
