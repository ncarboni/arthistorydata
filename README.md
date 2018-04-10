#  Data for art history

*CRM modelling & examples*

---



[[TOC]]

---



## Name

A name is an identifier of a person which refers to an agent in a specific moment in time (it is important to remember that it can change over time, see the case of _Prince_).A person's name in CRM can be model differently accordingly to our intention, as well as on the granularity of the information we want to map.

More than one name can be linked to one person, and one person can be known with diverse name, both during its life and after. The very first thing we should ask ourself is if the name we are modelling is the preferred name of that person or not. We will see examples of both.

### Preferred Name

Possible modelling of the preferred name are:

1. E21 Person → L4 has preferred label → rdfs:Literal
2. E21 Person → skos:prefLabel → rdfs:Literal

The main difference is that the 1 statements is encoded using CIDOC-CRM and CRMDig, while the second use SKOS. The form preferred by I Tatti is the number 2 (_see the code block_). The reason is mainly because it employs SKOS, which we use in other part of the modelling. Moreover SKOS is more used globally.

```{caption="caption example" .xml-dtd}
skos:prefLabel "Giovanni da Udine" .
```


### Name

Possible modelling of the name are:

1. E21 Person → P1 is identified by → E41 Appellation → P3 has note → rdfs:Literal
2. E21 Person → rdfs:Label → rdfs:Literal
3. E21 Person → P1 is identified by → rdfs:Literal

The main differences between the three is that example 1 treat the appellation not as a literal but as an entity which has a literal. Thus we can express statements about E41 Appellation, such as when it was used, how it come to be, where this form originate and others of such type.

The form chosen by I Tatti is the number 3 (_see the code block_). The reason is very practical. There was no need to express statements about the name itself, and the use of rdfs:label is preferable as a system level (_metaphacts_) in respect to crm:P1_is_identified_by . 


```xml-dtd
a crm:E21_Person ;
rdfs:label "Giovanni da Udine" .
```



## Identifier

A object can has multiple identifiers which we want to link to. An easy example would be Michelangelo, which has an internal identifier, a page on wikidata with his own identifier, as well as another one on ULAN. We can chose to define them and chose a preferred one. 

Possible modelling in CRM are:

1. E21 Person → P48 has preferred identifier → rdfs:Literal (_see the code block number 1_)
2. E21 Person → P1 is identified by→ rdfs:Literal (_see the code block number 2_)

```xml-dtd
crm:P1_is_identified_by "B00000737" ;
```


```xml-dtd
crm:P48_has_preferred_identifier "A00000737" ;
```



## Time

Time is treated quite profoundly in CRM and diverse can be the time representations as well as time expression that the model offer. A possible summarisation of these approaches is outside the scope of the document but the one interested could consult the resource **list resources**

Such a diverse approach allow for a great diversity in the mapping:

1. E12 Production→ P4 has time-span → E52 Time-Span → P79 beginning is qualified by → xsd:gYear / E12 Production→ P4 has time-span → E52 Time-Span → P80 end is qualified by → xsd:gYear

```xml-dtd
 a crm:E225_Time-Span ;
crm:P79_beginning_is_qualified_by "1401"^^xsd:gYear ;
crm:P80_end_is_qualified_by  "1700"^^xsd:gYear .
```



## Place

Place is another important feature we should harmonise. Diverse are the possibilities, both inside and outside CRM. The latter offers an extension called CRMGeo that harmonise CRM with GeoSparql and offer a nice representation of the geographical features with a rich semantics. However the adoption is limited and the learning curve is steep. Other option are offered in CRM, which allow the expression of simple statements about place. 

The identified mapping options are:

1. E53 Place → P87 is identified by →  E47 Spatial Coordinates →  rdfs:Label →  rdfs:Literal
2. wgs:SpatialThing→ wgs:lat →  rdfs:Literal / wgs:SpatialThing→ wgs:long →  rdfs:Literal
3. E53 Place→ P87 is identified by → wgs:Point→ wgs:lat →  rdfs:Literal

We will see them in more details

1. E53 Place → P87 is identified by →  E47 Spatial Coordinates →  rdfs:Label →  rdfs:Literal

   This mapping actually require the specification of the Type of the Spatial Coordinates we are  annotating a place with, such as "Latitude" or "Longitude". Other options could be "WGS84" or other reference system.

| Entity    | Relationship         | Entity                  | Relationship | Entity       |
| --------- | -------------------- | ----------------------- | ------------ | ------------ |
| E53 Place | P87 is identified by | E47 Spatial Coordinates | rdfs:Label   | rdfs:Literal |
|           |                      |                         | P2 has type  | rdfs:Literal |


```xml-dtd
 a crm:E53_Place ;
crm:P87_is_identified_by  ,  .


a crm:E47_Spatial_Coordinates;
rdfs:label "11.8818051" ;
crm:P2_has_type “Longitude" .


a crm:E47_Spatial_Coordinates;
rdfs:label "60.1701801" ;
crm:P2_has_type “Latitude" .
```
The solution is complex and require the use of type for something which should be properly expressed. Moreover, another type would be use if the reference system should be specified.



2. wgs:SpatialThing→ wgs:lat →  rdfs:Literal / wgs:SpatialThing→ wgs:long →  rdfs:Literal

   ​

   Another solution is to map a wgs:SpatialThing, which is an entity of the Ontology Geo as a subclass of E53 Place and express the latitude and longitude relative to the place. It is a straightforward solution which, however, does confuse, as it happens in many case, what it is a place and what are the coordinate of that place. The two are very different and should not be mix together.

```xml-dtd
   a wgs:SpatialThing;
   wgs:lat “60.1701801" ;
   wgs:long “24.9419037” .
```


3. E53 Place→ P87 is identified by → wgs:Point→ wgs:lat →  rdfs:Literal

This mapping takes in account the declaration, at an ontology level, that wgs:Point, an entity in the ontology Geo, is a subclass of E47 Spatial Coordinates. 

```xml-dtd
https://collection.itatti.harvard.edu/resource/ex/place/ a crm:E53_Place ;
crm:P87_is_identified_by https://collection.itatti.harvard.edu/resource/ex/place/point .
a wgs:Point;
wgs:lat "60.1701801" ;
wgs:long “24.9419037" .
```

This solution is, for us, the most satisfying, because it clarify the properties and it is properly mapped to CRM. Moreover Geo is, by far, the most used geographical ontology in the Linked Data  cloud [^1].

## OWL sameAs

A specific paragraph should be dedicated to owl:sameas which is probably one of the most used element in owl, and a way to connected entities bewteen diverse graph.

```xml-dtd
https://collection.itatti.harvard.edu/resource/person/example
owl:sameas "http://vocab.getty.edu/ulan/500004700" .
```

Its popularity has made it particolarly misused, and it is better to remind everyone (author included) that for linking entities which are not exactly the same[^2] other properties are available, such as skos:exactMatch which should be considered always as possible alternative.





[^1]: Not recognised officialy by W3C, Geo becomes a *standard de facto* for geographical data. However it is not as complex or precise as GEOSparql or CRMGeo

[^2]: See the case of historical place vs actual places.
