# Bibliographic Data

We model bibliographic data using [FRBRoo](http://www.cidoc-crm.org/frbroo/), the CIDOC-CRM extension for bibliographic data. We demonstrate the modelling of a printed book, but the approach can be adapted to similar items of expressive matters, such as letters, manuscripts, recordings, etc.

Note: the information here is based on FRBRoo 2.4, which is the latest release at the time of writing. FRBRoo 3.0 is currently under development.

## References

The examples are based on the data of the [Sphaera](http://sphaera.mpiwg-berlin.mpg.de) project. The data model for this was developed following the guidelines in:

 Doerr, M., Gradmann, S., LeBoeuf, P., Aalberg, T., Bailly, R., & Olensky, M. (2013). _Final Report on EDM - FRBRoo Application Profile Task Force_. Europeana. Retrieved from <http://pro.europeana.eu/taskforce/edm-frbroo-application-profile>

A tutorial on FRBRoo is available [here](http://83.212.168.219/FRBR_Tutorial/)

## Book

In FRBRoo, a book is understood as consisting of several (CRM) entities:

- the physical book (F5 Item): an individual material copy of a book
- the manifestation (F3 Manifestation Product Type): the 'prototype' of a particular book, identifiable for example by an ISBN number
- the expression (F22 Self-Contained Expression): the text contained in a book
- the work (F1 Work): the work conveyed by a book

Using this terminology we can talk of my personal copy of Virginia Woolf's _Orlando_ (F5 Item), which is the Oxford World's Classics edition (F3 Manifestation Product Type), that contains the text edited by Michael H. Whitworth (F22 Self-Contained Expression) and conveys Vriginia Woolf's work _Orlando_ (F1 Work).

Note: In FRBRoo 2.4 we distinguish between the _F24 Publication Expression_ that includes the entire content of a book (including page numbers, TOC, etc.) and the _F22 Self-Contained Expression_ that includes the text as written by the author. This discrimination is likely to be abandoned in version 3.0.

The components are modelled as follows:

1. **F5 Item → P128 carries → F24 Publication Expression**
2. **F24 Publication Expression → P165 incorporates → F22 Self-Contained Expression**
3. **F5 Item → R7 is example of → F3 Manifestation Product Type**
4. **F1 Work → R3 is realised in → F22 Self-Contained Expression**

```ttl
@prefix frbroo: <http://iflastandards.info/ns/fr/frbr/frbroo/>.
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/>.

<http://example.org/book> a frbroo:F5_Item ;
	crm:P128_carries <http://example.org/book/publication> ;
	crm:R7_is_example_of <http://example.org/book/manifestation> .

<http://example.org/book/publication> a frbroo:F24_Publication_Expression ;
	crm:P165_incorporates <http://example.org/book/expression> . 

<http://example.org/book/expression> a frbroo:F22_Self-Contained_Expression .

<http://example.org/book/work> a frbroo:F1_Work ;
	frbroo:R3_is_realised_in <http://example.org/book/expression> .
```

### Author

As author we understand the actor(s) responsible for the creation of a F22 Self-Contained Expression. Therefore, the term does not convey a particular idea of authorship and can include different roles, such as translators, copyists, etc.

We model the creation of the Expression as follows:

1. **F22 Self-Contained Expression → R17i was created by → F28 Expression Creation**[^3]

The author is then represented as the actor carrying out the event

1. **F28 Expression Creation → P14 carried out by → E39 Actor**

```ttl
@prefix frbroo: <http://iflastandards.info/ns/fr/frbr/frbroo/>.
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/>.

<http://example.org/book/expression> a frbroo:F22_Self-Contained_Expression ;
	frbroo:R17i_was_created_by <http://example.org/book/expression/event> .
	
<http://example.org/book/expression/event> a frbroo:F28_Expression_Creation ;
	crm:P14_carried_out_by <http://example.org/actor> .
	
<http://example.org/actor> a crm:E39_Actor .
```

The typed property PC14 carried out by is used when we need to represent additional information:

1. **F28 Expression Creation → P01 is domain of → PC14 carried out by → P02 has range → E39 Actor**
2. **PC14 carried out by → P14.1 in the role of → E55 Type**

```ttl
@prefix frbroo: <http://iflastandards.info/ns/fr/frbr/frbroo/>.
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/>.

<http://example.org/book/expression> a frbroo:F22_Self-Contained_Expression ;
	frbroo:R17i_was_created_by <http://example.org/book/expression/event> .
	
<http://example.org/book/expression/event> a frbroo:F28_Expression_Creation ;
	crm:P01_is_domain_of <http://example.org/book/author/1> .
	
<http://example.org/book/author/1> a crm:PC14_carried_out_by ;
	crm:P02_has_range <http://example.org/actor> ;
	crm:P14.1_in_the_role_of <http://example.org/type/author>
	
<http://example.org/actor> a crm:E39_Actor .

<http://example.org/type/author> a crm:E55_Type .
```

### Publisher

As publisher we understand the actor(s) responsible for the creation of a F24 Publication Expression. A publisher may be the same as the printer, in which case they are also the actor responsible for the F32 Carrier Production Event.

We model the creation of the F24 Publication Expression as follows:

1. **F24 Publication Expression → R24i was created through → F30 Publication Event**

The publisher is then represented as the actor carrying out the event

1. **F30 Publication Event  → P14 carried out by → E39 Actor**


```ttl
@prefix frbroo: <http://iflastandards.info/ns/fr/frbr/frbroo/>.
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/>.

<http://example.org/book/publication> a frbroo:F24_Publication_Expression ;
    frbroo:R24i_was_created_through <http://example.org/book/publication/event> .

<http://example.org/book/publication/event> a frbroo:F30_Publication_Event ;
    crm:P14_carried_out_by <http://example.org/actor> .

<http://example.org/actor> a crm:E39_Actor .
```

### Date published

The publishing date is the date the F30 Publication has been carried out.

It is modelled as follows:

1. **F30 Publication Event → P4 has time-span  → E52 Time-Span**

See the section on Time for more detail on how to model time spans. In bibliographic data we typically have a year, which we model using the **P82a** and **P82b** properties. For readability, we also include the year as a rdfs:label to the E52 Time-Span.

See the following example:

```ttl
@prefix frbroo: <http://iflastandards.info/ns/fr/frbr/frbroo/>.
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/>.

<http://example.org/book/publication/event> a frbroo:F30_Publication_Event ;
    crm:P4_has_time-span <http://example.org/book/publication/date> .

<http://example.org/book/publication/date> a crm:E52_Time-Span ;
	rdfs:label "1980" ;
	crm:P82a_begin_of_the_beginning "1980-01-01"^^xsd:date;
 	crm:P82b_end_of_the_end  "1980-12-31"^^xsd:date .
```

#### Implementation in ResearchSpace

We require users to specify three dates: the earliest year, the latest year and a textual date. If the exact year is known, users enter the same year in all three fields. If the exact year is not known, users can enter boundaries in the earliest and latest field and specify a verbal description in the textfield (e.g. "ca. 1950"). This setup works in our case, where most years are specified, but might not be the best solution when dates are uncertain (see Time section for further discussion on this).

ResearchSpace offers a Date Input field. However, it requires a complete date whereas we want to allow users to just enter a year. This year then gets translated to 1 January for the earliest year and 31 December for the latest year. This step is necessary to enable searching and for the data that is stored in the dataset to be properly formated (xsd:date).

The insert pattern for the earliest publication date looks as follows:

```sparql
INSERT { 
  ?publication_event crm:P4_has_time-span ?time_span .
  ?time_span a ecrm:E52_Time-Span ;
  	crm:P82a_begin_of_the_beginning ?value_begin .
} WHERE {
  BIND(xsd:date(CONCAT(STR($value), "-01-01")) as ?value_begin)
  BIND(URI(CONCAT(STR($subject), "/publication/event")) as ?publication_event)
  BIND(URI(CONCAT(STR($subject), "/publication/date")) as ?time_span)
}
```

The bound ``$value`` is converted to a ``xsd:date`` and bound to ``?value_begin`` which is the value that is written into the database. The conversion to ``xsd:date`` occurs when writing to the database, which means the XSD Datatype of the field needs to be empty and a validation needs to occur via a ASK query.

For example, the following query validates the input as a four digit number:


```sparql
ASK { 
  BIND(STRLEN(STR(xsd:integer($value))) as $num_value_len)
  FILTER($num_value_len = 4)  
} 
```
