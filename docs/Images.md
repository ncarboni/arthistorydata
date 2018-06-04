# Images

**This is a draft for the modelling of image data in CIDOC-CRM with reference to IIIF**

## Images and IIIF

The [IIIF](http://iiif.io/) [Image API](http://iiif.io/api/image/2.1/) defines a standard URI format for accessing images on the web. The format is:

```
{scheme}://{server}{/prefix}/{identifier}/{region}/{size}/{rotation}/{quality}.{format}
```

For example:
```
http://www.example.org/image-service/abcd1234/full/full/0/default.jpg
```

In addition, it defines a standard format for information about an image as follows:

```
{scheme}://{server}{/prefix}/{identifier}/info.json
```

For example:
```
http://www.example.org/image-service/abcd1234/info.json
```

The information is returned in [JSON-LD](https://www.w3.org/TR/json-ld/) format.

The JSON-LD contains an @id which specifies the URI of the profile. This can (but does not need to?) follow the same schema:

```
{scheme}://{server}{/prefix}/{identifier}
```

Hence the URI resolves to the information about the image, not the image itself.

## IIIF and CIDOC-CRM

The above URI resolves to a JSON-LD document which contains information about the image. It can therefore be represented in CIDOC-CRM as a _E31 Document_ which _P70 documents_ the image.

The image itself is represented as a _E38 Image_. The URI of the image can be chosen freely. In practice it is however likely, that the image URI will share the identifier part with the IIIF URI. E.g.

```ttl
  <http://www.example.org/image/abcd1234> a crm:E38_Image .
  <http://www.example.org/image-service/abcd1234> a crm:E31_Document ;
    crm:P70_documents <http://www.example.org/image/abcd1234>
```

For ease of querying, the IIIF Document could be typed:

```ttl
  <http://www.example.org/image-service/abcd1234> crm:P2_has_type <http://iiif.io/api/image>
```
