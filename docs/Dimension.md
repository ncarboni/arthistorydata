
## Dimension/Quantity

#### Note

Dimension are measurable data which results from an observation. The outcome of an observation is always to be seen as a couple value/system where the meaning of the value is dependant on the system used.

Dimensions can be modelled in CRM as:
**E54 Dimension → P91 has unit →  E58 Measurement Unit**
**E54 Dimension → P90 has value →  E60 Number**

The modelling above is based on the differentiation between the value and the system which provide meaning to the value. 

### Modelling Dimension

When talking about Dimension it is paramount to use an external recognisable authority to define the system which provide meaning to the value.
In order to define the measurement units and the the quantity kind (length, weight) we rely on E55 Type and the vocabulary provided with the Ontology of units of measure (OM). 

```xml-dtd
<https://collection.itatti.harvard.edu/resource/dimension/example> a crm:E54_Dimension ;
crm:P90_has_value	"6"^^xsd:decimal ;
crm:P91_has_unit <http://www.ontology-of-units-of-measure.org/resource/om-2/centimetre> ;
crm:P2_has_type <http://www.ontology-of-units-of-measure.org/resource/om-2/Width> .
```

OM includes all the necessary dimensions required by our use cases. It is easy to browse, and well developed.

Other ontologies were considered, specifically QUDT, which is "Quantity, Unit, Dimension and Type" schema used for modelling physical measurable properties in a machine processable form. While QUDT was the initial candidate, it was not chosen because the browsing of the units was quite difficult and, moreover, it is still under development and not yet complete or stable.   

OM can be easily browsed here:
[http://www.ontology-of-units-of-measure.org/]([http://www.ontology-of-units-of-measure.org)


