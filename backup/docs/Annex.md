# Annex A 

## Discarded Modelling choices



### Appellation

Possible modelling of the appellation are:

1. **E21 Person → P1 is identified by → E41 Appellation → P3 has note → rdfs:Literal**
2. **E21 Person → rdfs:Label → rdfs:Literal**
3. **E21 Person → P1 is identified by → rdfs:Literal**
4. **E21 Person → P1 is identified by → E41 Appellation → rdfs:label → rdfs:Literal**

The main differences between the three is that example 1 treat the appellation not as a literal but as an entity which has a literal. Thus we can express statements about E41 Appellation, such as when it was used, how it come to be, where this form originate and others of such type.

> <mark>The form chosen by **I Tatti**</mark> is the number 3 (_see the code block_). The reason is very practical. There was no need to express statements about the name itself, and the use of rdfs:label is preferable as a system level (_metaphacts_) in respect to crm:P1_is_identified_by . 
>
> The possibility of adding rdfs:label to each class in CRM was a specification which was modelled as such:
>
> ```xml-dtd
> <rdf:Property rdf:about="http://www.w3.org/2000/01/rdf-schema#label">
> 		<rdfs:domain rdf:resource="E1_CRM_Entity"/>
> 		<rdfs:range rdf:resource="http://www.w3.org/2000/01/rdf-schema#Literal"/>
> 	</rdf:Property>
> ```
>
> and added to the schema. The result is that each rdfs:label would be a property of crm:E1_CRM_Entity, and subsequently to each of the subclasses of E1. The rationale behind is that crm:E1_CRM_Entity has as a property crm:P1_is_identified_by, which is completely equivalent to rdfs:label. 
> The consequence is the possibility to use rdfs:label instead of crm:P1_is_identified_by for each of the class in CRM

```xml-dtd
a crm:E21_Person ;
rdfs:label "Giovanni da Udine" .
```

> <mark>The form chosen by **SARI**</mark> is the number 1. But on top of property crm:P3_has_note also rdf:value is used.

```xml-dtd
crm:E21_Person crm:P1_is_identified_by crm:E82_Actor_Appellation.
crm:E82_Actor_Appellation crm:P3_has_note "Semper, Gottfried".
crm:E82_Actor_Appellation rdf:value "Semper, Gottfried".
```

## 

### Preferred Name

Possible modelling of the preferred name are:

1. **E21 Person → L4 has preferred label → rdfs:Literal**
2. **E21 Person → skos:prefLabel → rdfs:Literal**
3. **E21 Person → P3_has_note|rdf:value → rdfs:Literal**

The main difference is that the 1 statements is encoded using CIDOC-CRM and CRMDig, while the second use SKOS. 

> <mark>The form chosen by **I Tatti** </mark> is the number 2 (_see the code block_). The reason is mainly because it employs SKOS, which we use in other part of the modelling. Moreover SKOS is more used globally.

```xml-dtd
skos:prefLabel "Giovanni da Udine" .
```

> <mark>The form chosen by **SARI** </mark> is the number 3 (_see the code block_). On top of having all appellations separately modelled, we attach the preferred one directly to the actor with P3_has_note and rdf:value. 

```xml-dtd
crm:E21_Person crm:P3_has_note "Semper, Gottfried".
crm:E21_Person rdf:value "Semper, Gottfried".
```



## Identifier

Digital object can has multiple identifiers which we want to link to. An easy example would be Michelangelo, which has an internal identifier, a page on wikidata with his own identifier, as well as another one on ULAN. We can chose to define them and chose a preferred one. 

Possible modelling in CRM are:

1. **E21 Person → P48 has preferred identifier → rdfs:Literal** (_see the code block number 1_)
2. **E21 Person → P1 is identified by → rdfs:Literal** (_see the code block number 2_)
3. **E21 Person → P1 is identified by → E42 Identifier → P3 has note | rdf:value → rdfs:Literal** (_see the code block number 3_)

```xml-dtd
crm:P1_is_identified_by "B00000737" ;
```

```xml-dtd
crm:P48_has_preferred_identifier "A00000737" ;
```

> <mark>Both form are employed by **ITatti**</mark>

> <mark>The form chosen by **SARI**</mark> is the number 3.

```xml-dtd
crm:E21_Person crm:P1_is_identified_by crm:E42_Identifier.
crm:E42_Identifier crm:P3_has_note "d701b57c-41a8-4ded-b411-08bcfdf01682".
crm:E42_Identifier rdf:value "d701b57c-41a8-4ded-b411-08bcfdf01682".
```

## 

## Place

#### Note

A place is a section of space identified indipendently from its temporal status. They are usually referenced using absolute (WGS, EPSG) or relative ("top of the mountain") reference systems.

The class SpatialObject, superclass of everything featureor geometry that can have a spatial representation.

Place is another important feature we should harmonise. Diverse are the possibilities, both inside and outside CRM. The latter offers an extension called CRMGeo that harmonise CRM with GeoSparql and offer a nice representation of the geographical features with a rich semantics. However the adoption is limited and the learning curve is steep. Other option are offered in CRM, which allow the expression of simple statements about place. 

The identified mapping options are:

1. **E53 Place → P87 is identified by →  E47 Spatial Coordinates →  rdfs:Label →  rdfs:Literal**
2. **wgs:SpatialThing→ wgs:lat →  rdfs:Literal / wgs:SpatialThing→ wgs:long →  rdfs:Literal**
3. **E53 Place→ P87 is identified by → wgs:Point→ wgs:lat →  rdfs:Literal**

We will see them in more details

1. **E53 Place → P87 is identified by →  E47 Spatial Coordinates →  rdfs:Label →  rdfs:Literal**

   This mapping actually require the specification of the Type of the Spatial Coordinates we are  annotating a place with, such as "Latitude" or "Longitude". Other options could be "WGS84" or other reference system.

| Entity    | Relationship         | Entity                  | Relationship | Entity       |
| --------- | -------------------- | ----------------------- | ------------ | ------------ |
| E53 Place | P87 is identified by | E47 Spatial Coordinates | rdfs:Label   | rdfs:Literal |
|           |                      | ⮑                       | P2 has type  | rdfs:Literal |

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



1. **wgs:SpatialThing→ wgs:lat →  rdfs:Literal / wgs:SpatialThing→ wgs:long →  rdfs:Literal**

   ​

   Another solution is to map a wgs:SpatialThing, which is an entity of the Ontology Geo as a subclass of E53 Place and express the latitude and longitude relative to the place. It is a straightforward solution which, however, does confuse, as it happens in many case, what it is a place and what are the coordinate of that place. The two are very different and should not be mix together.

```xml-dtd
   a wgs:SpatialThing;
   wgs:lat “60.1701801" ;
   wgs:long “24.9419037” .
```

1. **E53 Place→ P87 is identified by → wgs:Point→ wgs:lat →  rdfs:Literal**

This mapping takes in account the declaration, at an ontology level, that wgs:Point, an entity in the ontology Geo, is a subclass of E47 Spatial Coordinates. 

```xml-dtd
https://collection.itatti.harvard.edu/resource/ex/place/ a crm:E53_Place ;
crm:P87_is_identified_by https://collection.itatti.harvard.edu/resource/ex/place/point .
a wgs:Point;
wgs:lat "60.1701801" ;
wgs:long “24.9419037" .
```

> This solution number 3 is <mark>the one chosen by **I Tatti**</mark>, because it clarify the properties and it is properly mapped to CRM. Moreover Geo is, by far, the most used geographical ontology in the Linked Data  cloud [^1].

#Annex B 

## Meeting minutes







[...]