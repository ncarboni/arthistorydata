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

- rdf:value can be used to describe structural value, bu carries no meaning of its own.  
- rdfs:literal is just a class of literal value, and has no connection to any type of structured measurement unit.

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

