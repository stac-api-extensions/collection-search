# STAC API - Collection Search

- **Title:** Collection Search
- **Conformance Classes:**
  - **REQUIRED conformance classes:**
    - <https://api.stacspec.org/v1.0.0/core>
    - <https://api.stacspec.org/v1.0.0/collection-search>
    - <http://www.opengis.net/spec/ogcapi-common-2/1.0/conf/simple-query>
  - **OPTIONAL conformance classes**:
    - Free-text search: <https://api.stacspec.org/v1.0.0-rc.1/collection-search#free-text>
    - Filter/CQL2: <https://api.stacspec.org/v1.0.0-rc.4/collection-search#filter>
    - Sort: <https://api.stacspec.org/v1.1.0/collection-search#sort>
    - Fields: <https://api.stacspec.org/v1.0.0/collection-search#fields>
- **Scope:** STAC API - Core
- **[Extension Maturity Classification](https://github.com/radiantearth/stac-api-spec/tree/main/README.md#maturity-classification):** Candidate
- **Dependencies:**
  - [STAC API - Core](https://github.com/radiantearth/stac-api-spec/blob/main/core)
  - [STAC API - Collections](https://github.com/radiantearth/stac-api-spec/tree/main/ogcapi-features)
  - [OGC API - Common - Part 2: Geospatial Data](https://portal.ogc.org/files/99149)
  - [OGC API - Records - Part 1: Local Resource Catalogue](https://docs.ogc.org/is/20-004r1/20-004r1.html#clause-local-resources-catalog)
- **Owner**: @m-mohr

This extension extends the STAC API `GET /collections` endpoint to support advanced search capabilities
for STAC [Collections](https://github.com/radiantearth/stac-spec/blob/master/collection-spec/README.md).

The [`GET /collections` endpoint](https://api.stacspec.org/v1.0.0/collections/#tag/Collections/operation/getCollections)
doesn't provide any query parameters by default, so a basic set of query parameters is defined in [Basics](#basics)
for some rudimentary search functionality that is similar to the default items endpoint in STAC API.
Additional, more advanced behavior is defined as [optional conformance classes](#optional-conformance-classes).
These extensions can be composed by an implementer to cover only the set of functionality the implementer requires.

## Relation to OGC APIs

This extension is based on *[OGC API - Common - Part 2: Geospatial Data](https://portal.ogc.org/files/99149#rc-simple-query-section)*
and aligned to *[OGC API - Records - Part 1: Local Resource Catalogue](https://docs.ogc.org/is/20-004r1/20-004r1.html#clause-local-resources-catalog)*
and selectively implements a subset of their "requirements classes".

All functionality in *OGC API - Records - Part 1: Local Resource Catalogue* is only defined for the `GET` method (i.e. `GET /collections`).

> [!NOTE]  
> STAC may add behavior for `POST /collections` or [`QUERY /collections`](https://httpwg.org/http-extensions/draft-ietf-httpbis-safe-method-w-body.html)
> for more complex queries in the future. We'd prefer to use QUERY as POST would conflict with the Transaction Extension,
> where specific rules for content negotiation might be required.

## Versioning

The version of this extension only defines the version of the <https://api.stacspec.org/v1.0.0/collection-search>
conformance class. All other conformance classes define their own version numbers, e.g.
the [STAC API Sort Extension](https://github.com/stac-api-extensions/sort)
is current at v1.1.0 and as such the conformance class is
<https://api.stacspec.org/v1.1.0/collection-search#sort>.

## Pagination

Pagination for collections works as defined in STAC in general, through links with relation types `first`, `previous` (or `prev`), `next`, and `last`.
As such, this is also how it works for the `GET /collections` endpoint. For details, please see the chapter
[Collection Pagination](https://github.com/radiantearth/stac-api-spec/blob/main/ogcapi-features/README.md#collection-pagination).

## Basics

- Conformance classes:
  - `http://www.opengis.net/spec/ogcapi-common-2/1.0/conf/simple-query`
  - `https://api.stacspec.org/v1.0.0/collection-search`
- Requirement class in *OGC API - Common - Part 2: Geospatial Data*: [Simple Query](https://portal.ogc.org/files/99149#rc-simple-query-section)

This defines a basic set of query parameters for spatial and temporal querying, plus pagination.
Implementations of Collection Search MUST implemented the following three parameters:

- [`bbox`](https://docs.ogc.org/is/20-004r1/20-004r1.html#core-query-parameters-bbox): Spatial filtering, i.e. the intersection of the given `bbox` with any of the spatial extents provided in a STAC Collection for the path `extent.spatial.bbox`.
- [`datetime`](https://docs.ogc.org/is/20-004r1/20-004r1.html#core-query-parameters-datetime): Temporal filtering, i.e. the intersection of the given `datetime` with any of the temporal extents provided in a STAC Collection for the path `extent.temporal.interval`.
- [`limit`](https://docs.ogc.org/is/20-004r1/20-004r1.html#core-query-parameters-limit): Limits the page size.

The parameters are all aligned with the corresponding parameters in STAC API - Features and OGC API - Records.

### Optional conformance classes

#### Free-Text Search

- Conformance classes:
  - Basic: `https://api.stacspec.org/v1.0.0-rc.1/collection-search#free-text`
  - Advanced: `https://api.stacspec.org/v1.0.0-rc.1/collection-search#advanced-free-text`
- Requirement class in *OGC API - Records*: [Parameter q](https://docs.ogc.org/is/20-004r1/20-004r1.html#core-query-parameters-q)

This defines a new parameter, `q` that allows the user to perform free-text queries against STAC Collection metadata.
You have to choose to **either** implement Basic or Advanced free-text search.
This extension recommends implementation of the Basic free-text search due to its simplicity.

The authoritative specification is the [STAC API - Free-Text Search Extension](https://github.com/stac-api-extensions/freetext-search)
and this chapter is only a summary of the extension in the context of the Collection Search Extension.
The basic (but *not* the advanced) free-text search is also aligned with [OGC API - Records](https://docs.ogc.org/is/20-004r1/20-004r1.html#core-query-parameters-q).

The specific set of text fields of a Collection to which the parameter is applied is left to the discretion of the implementation, but a recommendation is to at least consider `title`, `description` and `keywords`.

For basic free-text search, the search is case-insensitive and spaces have no special meaning, which means
any of the search terms must be present in the set of text fields.
Commas act as separator between terms and reflect an *OR* operator.
For example, `q=EO,Earth Observation` would search for "Earth Observation" or "EO".

#### Filter (CQL2)

- Conformance classes: `https://api.stacspec.org/v1.0.0/collection-search#filter`
- Requirement class in *OGC API - Records*: [Local Resource Catalogue, CQL Filter](https://docs.ogc.org/is/20-004r1/20-004r1.html#clause-local-resources-catalog_filtering)

The Filter extension provides an expressive mechanism for searching based on Collection fields.

The authoritative specification is the [STAC API - Filter Extension](https://github.com/stac-api-extensions/filter)
and this chapter is only a summary of the extension in the context of the Collection Search Extension.

In general, the extension works for Collections exactly as it works for Items, with the following notable differences:

- It is implemented only for `GET /collections` and returns STAC Collections accordingly
- The link to the queryables endpoint for Collection Search is located in the response of `GET /collections` (property `links`)
- The path/endpoint for Collection Search queryables can be freely chosen, but SHOULD NOT conflict with `GET /queryables`. We propose `GET /collection-queryables`.

#### Query (STACQL)

The Query extension is not available for Collection Search as it's targeted for `POST` endpoints that accept JSON.
The Collection Search extension currently does not use the HTTP `POST` method, as such Query can't be implemented.
Please use [Filter](#filter-cql2) instead.

#### Sort

- Conformance classes: `https://api.stacspec.org/v1.1.0/collection-search#sort`
- Requirement class in *OGC API - Records*: [Local Resource Catalogue, Sorting](https://docs.ogc.org/is/20-004r1/20-004r1.html#clause-local-resources-catalog_sorting)

The Sort extension provides a simple but extensible mechanism to order search results according to certain properties.

The authoritative specification is the [STAC API - Sort Extension](https://github.com/stac-api-extensions/sort)
and this chapter is only a summary of the extension in the context of the Collection Search Extension.

In general, the extension works for Collections exactly as it works for Items, with the following notable differences:

- It is implemented for `GET /collections` and sorts STAC Collections accordingly
- The link to the sortables endpoint for Collection Search is located in the response of `GET /collections` (property `links`)
- The path/endpoint for Collection Search sortables can be freely chosen, but SHOULD NOT conflict with `GET /sortables`. We propose `GET /collection-sortables`.

Additionally, Items and Collections have differences in (common) availability of properties and their location within the document.
You can find some examples below:

| Use Case              | STAC Collection | STAC Item           |
| --------------------- | --------------- | ------------------- |
| Sort by id            | id              | id                  |
| Sort by title         | title           | properties.title    |
| Temporal sorting      | extent.temporal | properties.datetime |
| Sort by creation time | created         | properties.created  |

#### Fields

- Conformance class: `https://api.stacspec.org/v1.0.0/collection-search#fields`
- Requirement class in *OGC API - Records*: n/a

The Fields extension provides a way to explicitly include or exclude specific Collection fields in the API response,
usually to reduce the response size.

The authoritative specification is the [STAC API - Fields Extension](https://github.com/stac-api-extensions/fields)
and this chapter is only a summary of the extension in the context of the Collection Search Extension.

In general, the extension works for Collections exactly as it works for Items,
with the only difference that the paths must fit to the structure of a Collection instead of an Item.
