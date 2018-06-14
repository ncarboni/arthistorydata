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

The IIIF URI resolves to a JSON-LD document which contains information about the image, not the image itself. It may nevertheless, for practical purposes, be treated as an image – this is the route chosen by the [linked.art](http://linked.art/model/object/digital/) model, which types it as a E36 Visual Item.

There can be reasons why one wants to use a different URI for the image. For example, if the IIIF endpoint might change in the future, but the URI should remain the same. Or, if the image URI serves as the basis for other URIs (e.g. http://www.example.org/image/abcd1234/manifest) and one wants to avoid potential collision with the IIIF namespace.

The proposal here is to use a different URI for the image (for the above reasons), but to construct it so that the identifier matches the IIIF schema. The Image is the subject of the IIIF information.

e.g.

```ttl
<http://www.example.org/image/abcd1234> a crm:E38_Image .
<http://www.example.org/image-service/abcd1234> a crm:E73_Information_Object ;
  crm:P129_is_about <http://www.example.org/image/abcd1234> ;
  crm:P2_has_type <http://iiif.io/api/image>
```

If the IIIF URI is used to directly represent the image, one can model it as:

e.g.

```ttl
<http://www.example.org/image-service/abcd1234> a crm:E38_Image ;
  crm:P2_has_type <http://iiif.io/api/image>
```
