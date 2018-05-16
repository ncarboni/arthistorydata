# CIDOC-CRM & Time



Ontologically speaking, the Time dimension is quite a complicated affair. Time, in fact, has different expressions, different calendars, and different degrees of certainty, making it very difficult to precisly calculate it, specifically in relation to diverse datasets. 

The problem was originally solved, at least for calculus, by Allen in 1983. In order to overcome the calculation problem, Allen defined seven relationships useful for reasoning about time intervals (X, Y):

1. X before Y (P120)
2. X meets Y (P119)
2. X overlaps Y (P118)
3. X during Y (P117)
4. X starts Y (P116)
5. X finishes Y (P115)
6. X equals Y (P114)


CIDOC-CRM was heavily influenced by this approach, and the properties associated with the Temporal entity in CRM refer exactly to these relationships.  


1. P120 occurs before (occurs after)
2. P119 meets in time with (is met in time by)
3. P118 overlaps in time with (is overlapped in time by)
4. P117 occurs during (includes)
5. P116 starts (is started by)
6. P115 finishes (is finished by)
7. P114 is equal in time to


Each temporal entity has to be seen as part of a time dimension where we can calculate diversity, closeness or equality in time between described event using these expression. However, for such objective to be achieved, it is paramount to express time intervals not only using topological relationships between temporal entities but fixed time point. This solution open up the problem of a common standard. Time expressions can, in fact, differ. The cause of this diversity is geographical, cultural, normative or just based on personal preferences. Different date values would be: 

> 1 September 1890, 01/09/1890, 1-09-1890, 09/01/1890, 1st Sept 1980

While for a human it is easy to understand that they all refers to the same date, computers need precision and normalisation. In order to reason, make inferences and integrate other time-related data, it is important to use a common standardise format such as the one expressed by [ISO 8601:2004](https://www.iso.org/standard/40874.html), which prescribe the composition of the date using the format 

> YYYY-MM-DD

Employing the same time format helps greatly in calculating time-intervals, but sometimes it is not enough. Specifically, when talking about past event, our statements in regard of time can be imprecise and incomplete. 

In order to overcome the problem, CRM made use of Fuzzy Logic and introduced a fuzzy model where the time intervals are modelled as fuzzy volumes with inner and outer sets. The interior set is the set of all determinate (knowledgeable) time intervals, while the exterior set is the set of all the indeterminate time intervals (uncertainty).



![fuzzy_time](https://s14.postimg.cc/42xwwks81/fuzzy_time.png)





The image above help us clarify this concept. Let's assume that the event *x* {A,B} is an historical event. Our knowledge of the time interval where the event took place has some level diverse degree of certainty. We will, therefore, model these statements as certain and uncertain, where the certain statements are going to be model using the property P81, while the uncertain statements using the property P82. What we will have at the end is a larger set of time intervals (A-C + D-B in the picture) and an internal set of time intervals (C-D), correspondig respectively to imprecise and precise statements.

The level of precision and imprecision is determined by the researcher carrying out the work. However, there are more clues to determine what should be considered imprecise. An event which is carried out during the course of a year is usually reported as been made in YYYY (e.g. 1894). However, such notation implies imprecision, because it literally signify a timespan ranging from 1894-01-01 till 1894-12-31. This type of notation is usually used for the creation date of a work of art, but in the majority of cases this is just an approximation, and should be recorded using the imprecise time intervals. It would be quite rare, in fact, that an artist started working the first of January and finished it off for the thirty one of December of the same year. A more precise time statement would become apparent as a result of the collection of a large pool of statements about the artist, its artwork and its activities.

Coming back to the modelling practice, as mentioned above, the the more "imprecise" statements are going to be model using the property P82.

crm:P82a _begin_of_the_begin identifty the beginning of the outer set of fuzzy time intervals (A)

crm:P82b_end_of_the_end identify the end of the fuzzy time intervals (B)

The collection of P82a/b statements about the event (as well as about precedent and following events) coming from multiple institution would allow us to detect more precisly the time interval about our historical event.

For defining the inner boundaries of our time intervals, and therefore the precise statements about an event, we will use P81.

crm:P81a_end_of_the_begin identify the end of the outer boundaries and the beginning of the internal one (C)

crm:P81b_begin_of_the_end identify the end of the internal boundary and the begin of the external one (C)



