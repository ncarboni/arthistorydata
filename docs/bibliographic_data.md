# Bibliographic Data

We model bibliographic data using [FRBRoo](http://www.cidoc-crm.org/frbroo/), the CIDOC-CRM extension for bibliographic data. We demonstrate the modelling of a printed book, but the approach can be adapted to similar items of expressive matters, such as letters, manuscripts, recordings, etc.

Note: the information here is based on FRBRoo 2.4, which is the latest release at the time of writing. FRBRoo 3.0 is currently under development.

## References

The examples are based on the data of the [Sphaera](http://sphaera.mpiwg-beriln.mpg.de) project. The data model for this was developed following the guidelines in:

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

[^]: 