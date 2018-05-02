# CIDOC-CRM & Time



Ontologically speaking, the Time dimension is quite a complicated affair. Time, in fact, has different expressions, different calendars, and different degrees of certainty, making it very difficult to calculate it precisly over diverse datasets. 

The problem was faced in calculus by Allen in 1983, which, in order to overcome the calculation problem, defined seven relationships useful for reasoning about time intervals (X, Y):

1. X before Y P120
2. X meets Y P119
3. X overlaps Y P118
4. X during Y P117
5. X starts Y P116
6. X finishes Y P115
7. X equals Y P114



CIDOC-CRM was heavily influenced by this approach, and the properties associated with the Temporal entity in CRM refer exactly to these relationships.

1. P120 occurs before (occurs after)
2. P119 meets in time with (is met in time by)
3. P118 overlaps in time with (is overlapped in time by)
4. P117 occurs during (includes)
5. P116 starts (is started by)
6. P115 finishes (is finished by)
7. P114 is equal in time to

Each temporal entity has to be seen as part of a time dimension where we can calculate diversity, closeness or equality in time between described event using these expression. However, for such objective to be achieved, it is paramount to express time intervals using a common standard. As mentioned above, the time expression can varies, and could includes different date values such as: 

> 1 September 1890, 01/09/1890, 1-09-1890, 09/01/1890, 1st Sept 1980

While for a human it is easy to understand that they all refers to the same date, computers need precision and normalisation. In order to reason, make inferences and integrate other time-related data, it is important to use a common standardise format such as the one expressed by ISO 8601:2004, which prescribe the composition of the date using the format 

> YYYY-MM-DD

Using the same time format helps greatly in calculating time-intervals, but sometimes it is not enough. Specifically, when talking about past event, the observation about time are intertwined with imprecision and incompleteness. 

In order to overcome the problem, CRM introduced a fuzzy models where the time intervals are modelled as fuzzy volumes with inner and outer sets. The interior set is the set of all determinate (knowledgeable) time intervals, while the exterior set is the set of all the indeterminate time intervals (uncertainty).







This methodology deal with the differences in certainty levels both in reference to a time statements, as well as in reference to a collection of time statements. This type of differentiation is quite important when querying diverse dataset and help us reasoning over events. 





CIDOC-CRM 



