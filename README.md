# STAC API - Collection Search

- **OpenAPI specification:** [openapi.yaml](openapi.yaml) (todo)
- **Conformance URIs:**
  - <https://api.stacspec.org/v1.0.0-rc.1/collection-search>
  - <https://api.stacspec.org/v1.0.0-rc.1/core>
- **[Maturity Classification](../README.md#maturity-classification):** Proposal
- **Dependencies**:
- [STAC API - Collections](https://github.com/radiantearth/stac-api-spec/tree/main/ogcapi-features)
- [STAC API - Core](https://github.com/radiantearth/stac-api-spec/blob/main/core)

A search endpoint provides the ability to query
STAC [Collections](https://github.com/radiantearth/stac-spec/blob/master/collection-spec/README.md)
objects across collections.
It retrieves a group of Collection objects that match the provided parameters and provides them as
the `GET /collections` endpoint does.

The Item Search endpoint intentionally defines only a limited group of operations. It is expected that
most behavior will be defined in [Extensions](#extensions). These extensions can be composed by an implementer to
cover only the set of functionality the implementer requires.

Implementing `GET /search/collections` is **required**, `POST /search/collections` is optional, but recommended.

## Link Relations

This conformance class also requires implementation of the link relations in the STAC API - Core conformance class.

The following Link relations must exist in the Landing Page (root).

| **rel**             | **href**              | **Description**             |
| ------------------- | --------------------- | --------------------------- |
| `collection-search` | `/search/collections` | URI for the Search endpoint |

This `collection-search` link relation must have a `type` of `application/json`. If no `method` attribute is
specified, it is assumed to represent a GET request. If the server supports both GET and POST requests, two links should be included, one with a `method` of `GET` one with a `method` of `POST`.

Other links with relation `collection-search` may be included that advertise other content types the server may respond
with, but these other types are not part of the STAC API requirements.

## Query Parameters and Fields

### Basics

A basic set of query parameters can be implemented based on
[OGC API - Commons - Part 2](https://portal.ogc.org/files/99149#rc-simple-query-section):
- `bbox`
- `datetime`
- `limit`

Conformance classes:
- `http://www.opengis.net/spec/ogcapi-common-2/1.0/req/simple-query`
- `https://api.stacspec.org/v1.0.0-rc.1/collection-search#simple`

These are basically the same as defined by OGC API - Features for Items.

### Query (STACQL)

The query extension for STACQL support can be implemented, too. It works as it does for Items.
See <https://github.com/stac-api-extensions/query> for details.

Conformance class: `https://api.stacspec.org/v1.0.0-rc.1/collection-search#query`

### Filter (CQL)

The filter extension for CQL support can be implemented, too. It works as it does for Items.
See <https://github.com/stac-api-extensions/filter> for details.

Conformance class: `https://api.stacspec.org/v1.0.0-rc.1/collection-search#filter`

### Pagination

Pagination works for Collections exactly as it works for Items.

Conformance class: None

### Sorting

The sort extension can be implemented, too. It works as it does for Items.
See <https://github.com/stac-api-extensions/sort> for details.

Conformance class: `https://api.stacspec.org/v1.0.0-rc.1/collection-search#sort`

### Fields

The fields extension can be implemented, too. It works as it does for Items.
See <https://github.com/stac-api-extensions/fields> for details.

Conformance class: `https://api.stacspec.org/v1.0.0-rc.1/collection-search#fields`

### Context

The context extension can be implemented, too. It works as it does for Items.
See <https://github.com/stac-api-extensions/context> for details.

Conformance class: `https://api.stacspec.org/v1.0.0-rc.1/collection-search#context`

These extensions provide additional functionality that enhances Item Search.