# Art history Data

*CRM modelling & examples*

The document outlines modelling choices in relation to diverse entities described in the Consortium's data. The modelling is provided, whenever possible, using CIDOC-CRM. However, in certain cases other ontologies are used. For each of the described entities diverse encoding choices were outlined, and decision about them were made. Scope of this document is not to provide a track of this process, but just to present its results. However, even if not present here, the documentation of the possible modelling and encoding of an entity in RDF can be found in the [Annex A](#).

For each entity, as an example of its use, an RDF encoding of a real-world statements is provided. The syntax of the encoding is Turtle, and all the prefix used can be found on http://prefix.cc/ .

## Appellation 

#### Note

An appelation is a sign, usually in the form of a string of characters, used for referencing an entity (person object, place, an event etc.). More appellations can refer, in the same language, to the same entity, as in the case of:

> Monna Lisa or La Gioconda. which both refers to the same painting

Appellations can also be tied to a specific time interval, such as in the case of:

> Prince, Alexander Nevermind, The Artist, The Artist Formerly Known as Prince (TAFKAP), Camille, Christopher, Tracy Jamie Starr, Joey Coco, Tora Tora, The Kid — which refers to Prince Rogers Nelson in diverse moments of his career

However, If more appellations can cohexist, a chosen one should be defined using a different property which identifies it with our <u>preferred</u> appellation.

### Appellation

A simple appellation can be modelled in CRM as

**E21 Person → P1 is identified by → E41 Appellation**

The encoding of the string of characters of the appellation can be done in multiple ways. The form chosen by the consortium is:

**crm:E21 Person → P1 is identified by → E41 Appellation → rdfs:Label → rdfs:Literal**

```xml-dtd
<http://vocab.getty.edu/ulan/500020981> 
a  crm:E21_Person ;
crm:P1_is_identified_by <https://collection.itatti.harvard.edu/resource/appellation/6c5a29fd81f9> .
<https://collection.itatti.harvard.edu/resource/appellation/6c5a29fd81f9>
a  crm:E41_Appellation ;
rdfs:label "Gian Francesco de Maineri" .
```

*<small>Codebox 1  - Appellation</small>*

**Modelling Note**

> *The use of crm:P3_has_note for encoding the value of the appellation is discouraged because of its confusing nature. Appellations can have both notes and values, which are part of two different information domains, and should not overlap. Addittionaly, the property rdf:value, which can be also used to encode the appellation, is to be set aside because of its lack of explicit semantics.*

### Preferred Appellation

The preference for a specific sign or string for identifying an entity can be modelled using SKOS. In respect to the encoding of the [appellation](#appellation), the initial part of the modelling remain the same:

**E21 Person → P1 is identified by → E41 Appellation**

What change is the property used for encoding the Literal, which is not rdfs:Label as before but skos:prefLabel. PrefLabel identify a preferred lexical label of a resource:

**crm:E21 Person → P1 is identified by → E41 Appellation → skos:prefLabel → rdfs:Literal**

```xml-dtd
<http://vocab.getty.edu/ulan/500020981> 
a  crm:E21_Person ;
crm:P1_is_identified_by <https://collection.itatti.harvard.edu/resource/appellation/6c5a29fd8> .
<https://collection.itatti.harvard.edu/resource/appellation/6c5a29fd8>
a  crm:E41_Appellation ;
skos:prefLabel "Maineri, Gian Francesco de" .
```

*<small>Codebox 2  - Preferred Appellation</small>*

**Modelling Note**

> *CRM does provide a property for encoding a preffered identifier, P48 has preferred identifier, but the range is E42 Identifier, a subclass of E41 Appellation, therefore it cannot be used for encoding the preferred appellation.*

## Identifier

#### Note

An identifier is a string assigned to an entity that uniquely identify it in a system (usually, a digital system). Examples of identifiers comprises inventory numbers, databases keys or other strings used for retrieval and identification purpose. As for the appellation, an entity can has multiple identifiers from the same or different system, and a preferred one can be chosen.

### Identifier

A simple identifier can be modelled in CRM as

**E21 Person → P1 is identified by → E42 Identifier**

The encoding of the string of characters of the identifier can be done in multiple ways. The form chosen by the consortium is:

**E21 Person → P1 is identified by → E42 Identifier → rdfs:Label → rdfs:Literal**

```xml-dtd
<http://vocab.getty.edu/ulan/500020981> 
a  crm:E21_Person ;
crm:P1_is_identified_by <https://collection.itatti.harvard.edu/resource/identifier/6c5a29fd8> .
<https://collection.itatti.harvard.edu/resource/identifier/6c5a29fd8>
a  crm:E42_Identifier ;
rdfs:label "6c5a29fd8" .
```

*<small>Codebox 3  - Identifier</small>*

### Preferred identifier

The preference for a specific identifier can be modelled using CIDOC-CRM as

**E21 Person → P48 has preferred identifier → E42 Identifier**

The encoding of the string of characters of the identifier can be done in multiple ways. The form chosen by the consortium is:

**E21 Person → P1 is identified by → E42 Identifier → rdfs:Label → rdfs:Literal**

```xml-dtd
<http://vocab.getty.edu/ulan/500020981> 
a  crm:E21_Person ;
crm:P48_has_preferred_identifier <https://collection.itatti.harvard.edu/resource/identifier/6c5a88> .
<https://collection.itatti.harvard.edu/resource/identifier/6c5a88>
a  crm:E42_Identifier ;
rdfs:label "6c5a88" .
```

*<small>Codebox 4 - Preferred Identifier</small>*

## Time-span

#### Note

CIDOC-CRM assume the existence of a SpaceTime dimension, where each temporal entity can be mapped to. This system allow the computing of diversity, closeness or equality in time between events. In doing so, it treats temporal extents in their physical sense, characterising each time interval with a beginning, an end and a duration. Additionally, CIDOC-CRM assume that our knowledge of the time intervals of an historical event has diverse degree of certainty, and it allow us to record such certainty/uncertainty with specific properties. A useful summarisation of the treatment of time in CIDOC-CRM is available in the document [Time](time.html).  

Following CIDOC-CRM we will model time-related assertions as certain or uncertain, where the certain statements are going to be model using the property P81, while the uncertain statements using the property P82. The level of certainty have to be determined by the researcher carrying out the work. 

We can have two diverse modelling scenario:

1. Fuzzy time statements
2. Precise time statements

In both cases the date is formatted following the [ISO 8601:2004](https://www.iso.org/standard/40874.html) reccomandations that prescribe the composition of the date using the format:

> YYYY-MM-DD

### Fuzzy Time Statements

Fuzzy time statements are modelled using CIDOC-CRM as:

- **E52 Time-Span → P82a begin of the begin → xsd:dateTime**
- **E52 Time-Span → P82b end of the end → xsd:dateTime**



The date need to be expressed with its datatype (gYearMonth, gMonthDay, gDay, gMonth and gYear). See the [W3C reccomandations](https://www.w3.org/TR/xmlschema-2/#isoformats) for more details.

```xml-dtd
<https://collection.itatti.harvard.edu/resource/work/W00060289/production/timespan>
a  crm:E52_Time-Span ;
crm:P82a_begin_of_the_begin  "1401-01-01"^^xsd:date ;
crm:P82b_end_of_the_end      "1700-12-31"^^xsd:date .
```

*<small>Codebox 5 - Fuzzy Time Statements</small>*

**Modelling Note**

> *The production event recorded here has a time interval of almost 3 centuries. The choice of using a fuzzy time statement is, in this case, quite clear. *

### Precise time statements

Precise time statements are modelled using CIDOC-CRM as:

- **E52 Time-Span → P81a end of the begin → xsd:dateTime **
- **E52 Time-Span → P81b begin of the end → xsd:dateTime**

```xml-dtd
<https://collection.itatti.harvard.edu/resource/work/W00060289/production/timespan>
a  crm:E52_Time-Span ;
crm:P81a_begin_of_the_begin  "2001-10-21T21:32:52"^^xsd:dateTime;
crm:P81b_end_of_the_end      "2001-10-26T00:00:00"^^xsd:dateTime.
```

*<small>Codebox 6 - Precise Time Statements</small>*

**Modelling Note**

> *The precision of the date is a clear sign we are dealing with a more precise time statement.*



## Spatial Coordinate

#### Note

A place is a section of space identified indipendently from its temporal status. They are usually referenced using absolute (WGS, EPSG) or relative reference systems. Absolute reference system use point-based coordinates to identify representation of space.



### Longitude & Latitude

The spatial coordinates of a place can be modelled in CIDOC-CRM as 

**E53 Place → P87 is identified by →  E47 Spatial Coordinates**

The encoding of the latitude and longitude can be done in multiple ways. The form chosen by the consortium is to rely on a basic RDF vocabulary for representing latitude and longitude using WGS84 as a reference system. In order to do so, we will have to map the class crm:E47_Spatial_Coordinates with the corresponding class in [geo](https://www.w3.org/2003/01/geo/).

We can declare geo:Point as sub class of crm:E47_Spatial_Coordinates with the following

```xml-dtd
<rdfs:Class rdf:about="http://www.w3.org/2003/01/geo/wgs84_pos#Point">
	<rdfs:subClassOf rdf:resource="E47_Spatial_Coordinates"/>
</rdfs:Class>
```
*<small>Codebox 7</small>*

Having mapped crm:E47_Spatial_Coordinates to geo:Point we can use two new properties

 * geo:lat
 * geo:long

To describe, respectively, the latitude and longitude of a E53 Place.

```xml-dtd
https://collection.itatti.harvard.edu/resource/ex/place/ a crm:E53_Place ;
crm:P87_is_identified_by https://collection.itatti.harvard.edu/resource/ex/place/point .
a wgs:Point;
wgs:lat "60.1701801" ;
wgs:long “24.9419037" .
```

*<small>Codebox 8 - Latitude and Longitude of a Place</small>*



<!--

 [**To be revised**]

## Dimension/Quantity

#### Note

Dimension are measurable data which results from an observation. The outcome of an observation is always to be seen as a couple value/system where the meaning of the value is dependant on the system used.

Dimensions can be modelled in CRM as:

**E54 Dimension → P91 has unit →  E58 Measurement Unit**

**E54 Dimension → P90 has value →  E60 Number**





```xml-dtd
https://collection.itatti.harvard.edu/resource/dimension/example a crm:E54_Dimension ;
rdf:value	"6" ;
rdf:literal	"centimeters" .
```

The modelling above is based on the differentiation between the value and the system which provide meaning to the value. This type of statements works if there is no real need of a proper measurement analysis or comparison. rdf:value and rdfs:literal are, in fact, quite neutral:

* rdf:value can be used to describe structural value, bu carries no meaning of its own.  
* rdfs:literal is just a class of literal value, and has no connection to any type of structured measurement unit.

Depending on the needs, the solution to this problem could be resolved using rdfs:isDefinedby plus an external authority for the measurement unit.

```xml-dtd
https://collection.itatti.harvard.edu/resource/dimension/example a crm:E54_Dimension ;
rdf:value	"6" ;
rdfs:isDefinedBy "http://qudt.org/vocab/unit/CM" ;
crm:P2_has_type “width" .
```

In the above example we use QUDT as authority for the centimeters unit. QUDT is the "Quantity, Unit, Dimension and Type" schema used for modeling physical measurable properties in a machine processable form, with no ambiguities. QUDT does not only provide a basic vocabulary of units, but it is also a formal ontology used for dimensional modelling. A NASA-sponsored initiative provide a clearer way to fix quantities, value and dimension to meaning-provider system as well as compare and convert quanty information. 

QUDT seems to be the perfect candidate for representing dimensionla data, however it is not yet mapped with CRM. From an initial analysis appear that crm:E54_Dimension correspond to the qudt:Quantity class, and not on qudt:Dimension as the label suggest. The class qudt:Dimension is, in fact, a relationship between "*a quantity systems, a quantity kind of that system, and one or more dimension vectors. The dimension of a quantity can be expressed as a product of basic dimension vectors for each of the system's base quantiy kinds, such as mass, length and time*".

It is possible to map qudt:Quantity as a subclass of crm:E54_Dimension:

```xml-dtd
<rdfs:Class rdf:about="http://qudt.org/schema/qudt/Quantity">
		<rdfs:subClassOf rdf:resource="crm:E54_Dimension"/>
	</rdfs:Class>
```

And then state:

**qudt:Quantity → qudt:quantityValue  → qudt:QuantityValue → qudt:numericValue  → xsd:double**

**qudt:QuantityValue → qudt:unit → <http://qudt.org/vocab/unit/CM> **

**qudt:Quantity → qudt:hasQuantityKind → qudt:QuantityKind → qudt:description →  "Height"**

—>



[^]: 
