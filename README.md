`argo` is a JSON media-type for Hypermedia APIs.

MIME type: `application/vnd.argo+json`

# Example

``` json
{
    "uri": "https://api.example.com/users/64",
    "data": {
        "email": "roy@example.com",
        "emailVerified": true,
        "details": {
            "name": "Roy",
            "age": 49,
            "address": {
                "uri": "https://api.example.com/users/64/address"
            }
        },
        "avatars": {
            "uri": "https://api.example.com/avatars?userId=64",
            "offset": 0,
            "length": 2,
            "total": 5,
            "data": [{
                "uri": "https://api.example.com/images/123",
                "data": {
                    "file": "https://assets.example.com/123.jpg",
                    "mimeType": "image/jpeg"
                }
            }, {
                "uri": "https://api.example.com/images/456",
                "data": {
                    "file": "https://assets.example.com/456.jpg",
                    "mimeType": "image/jpeg"
                }
            }]
        }
    },
    "links": [
        {"rel": "root", "href": "https://api.example.com"},
        {"rel": "friends", "href": "https://api.example.com/users/64/friends"}
    ]
}
```

## `uri`

The URI of the resource.

It is optional for the [Response Entity](#response-entity) (i.e. the
top-level entity in the response, here the "user"), if it can often be
inferred by the URI the client queried to retrieve the resource.

It is mandatory for [Embedded Entities](#embedded-entity)
(i.e. entities nested within the Response Entity, here the "address"
and "avatars").

## `data`

The content of the resource.

The value of the `data` property can be of any type supported by JSON
(object, array, string, number, boolean, null).  If an object or an
array, it may recursively contain any type, as well as
[Embedded Entities](#embedded-entity).

If `data` is an array, the resource may be a Collection Resource, and
the Entity may represent a subset of that collection. In that case,
the `offset`, `length` and `total` properties are used to identify the
received subset.

It is mandary for the [Response Entity](#response-entity) (i.e. the
top-level entity in the response, here the "user").

It is optional for [Embedded Entities](#embedded-entity).  Omitting
the data allows for lighter responses, or referencing a resource that
doesn't currently exist, similarly to a [Link](#link) but as part of an
existing Entity.

In the example above, the `data` for the "address" is omitted, whereas
the `data` for the "avatars" is present.

## `links`

Optional list of [Links](#link) from the resource.


## `offset`

The start offset of the range of the
[Response Entity](#response-entity) array, within the full Collection
Resource.

Only for a Collection Resource if `data` is an array.

## `length`

The length of the range of the [Response Entity](#response-entity)
array, within the full Collection Resource.

This is equivalent to the length of the `data` array.

Only for a Collection Resource if `data` is an array.

## `total`

The total length of the Collection Resource.

Only for a Collection Resource if `data` is an array.


# Vocabulary

## Response Entity

The top-level Entity returned in the server Response.

## Embedded Entity

Any Entity found within the Response Entity.

Embedded Entities allow denormalising other resources into a parent
one, typically to reduce the number of requests.  The structure of the
enclosing resource often also describes the relationship between the
Response and the Embedded Entities, similarly to the `rel` of [Links](#link).

## Link

A Link is a reference from a resource to another resource.

A Link is identified uniquely by its `rel` property (its *relation* to
the current resource).  The target of a Link is its `href` property
(or *hypertext reference*), which must be the URL of another resource.


# Clients

- [Theseus](https://github.com/argo-rest/theseus): JavaScript client for `argo` APIs


# See also

Similar media-type specifications:

- [HAL](http://stateless.co/hal_specification.html)
- [Collection+JSON](http://amundsen.com/media-types/collection/)
- [Siren](https://github.com/kevinswiber/siren)
- [JSON API](http://jsonapi.org/)
- [JSON-LD](http://json-ld.org/)

Other related efforts:

- [Hydra](http://www.markus-lanthaler.com/hydra/)
- [ALPS](http://alps.io/spec/)
