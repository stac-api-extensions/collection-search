# STAC API - Collection Search

- **Title:** Collection Search
- **OpenAPI specification:** [openapi.yaml](openapi.yaml) (todo)
- **Conformance Classes:**
  - <https://api.stacspec.org/v1.0.0-rc.1/core>
  - <https://api.stacspec.org/v1.0.0-rc.1/collection-search>
  - <http://www.opengis.net/spec/ogcapi-common-2/1.0/req/simple-query>
- **Scope:** STAC API - Core
- **[Extension Maturity Classification](https://github.com/radiantearth/stac-api-spec/tree/main/README.md#maturity-classification):** Proposal
- **Dependencies:**
  - [STAC API - Core](https://github.com/radiantearth/stac-api-spec/blob/main/core)
  - [STAC API - Collections](https://github.com/radiantearth/stac-api-spec/tree/main/ogcapi-features)
  - [OGC API - Common - Part 2: Geospatial Data](https://portal.ogc.org/files/99149)
  - [OGC API - Records - Part 1: Local Resource Catalogue](https://docs.ogc.org/DRAFTS/20-004.html#clause-local-resources-catalogue)
- **Owner**: @m-mohr

A search endpoint provides the ability to query
STAC [Collections](https://github.com/radiantearth/stac-spec/blob/master/collection-spec/README.md)
objects across collections.
It retrieves a group of Collection objects that match the provided parameters and provides them as
the `GET /collections` endpoint does.

The Collection Search endpoint by default doesn't provide any query parameters to filter and all
additional behavior will be defined in [Extensions](#extensions). These extensions can be composed
by an implementer to cover only the set of functionality the implementer requires.

This extension is based on *[OGC API - Common - Part 2: Geospatial Data](https://portal.ogc.org/files/99149#rc-simple-query-section)*
and aligned to *[OGC API - Records - Part 1: Local Resource Catalogue](https://docs.ogc.org/DRAFTS/20-004.html#clause-local-resources-catalogue)*
and selectively implements a subset of their "requirements classes".

All functionality in *OGC API - Records - Part 1: Local Resource Catalogue* is only defined for the `GET` method (i.e. `GET /collections`).
*Note:* STAC may add behavior for `POST /collections` in the future, but due to a potential conflict
with the Transaction Extension, specific rules for content negotiation might be required.

## Pagination

Pagination for collections works exactly as it defined for Collections in general.
See [Collection Pagination](https://github.com/radiantearth/stac-api-spec/blob/main/ogcapi-features/README.md#collection-pagination)
for details.

## Query Parameters and Fields

### Basics

A basic set of query parameters MUST be implemented.
These are aligned with the corresponding parameters in STAC API - Features and OGC API - Records:
- [`bbox`](https://docs.ogc.org/DRAFTS/20-004.html#_parameter_bbox) (intersection of the given `bbox` with any of the spatial extent provided in a STAC Collection at `extent.spatial.bbox`)
- [`datetime`](https://docs.ogc.org/DRAFTS/20-004.html#_parameter_datetime) (intersection of the given `datetime` with any of the temporal extent provided in a STAC Collection at `extent.temporal.interval`)
- [`limit`](https://docs.ogc.org/DRAFTS/20-004.html#_parameter_limit)

Requirement class in *OGC API - Common - Part 2: Geospatial Data*: [Simple Query](https://portal.ogc.org/files/99149#rc-simple-query-section)

### Extensions

#### Free text search

A free-text search parameter `q` based on OGC API - Records can be added.
See <https://docs.ogc.org/DRAFTS/20-004.html#_parameter_q> for details.
It is case-insensitive and matches the string given strictly.
The specific set of text keys/fields/properties of a Collection to which the parameter is applied is left to the discretion of the implementation, but a recommendation is to at least consider `title`, `description` and `keywords`.

- Requirement class in *OGC API - Records*: n/a
- Conformance class: `https://api.stacspec.org/v1.0.0-rc.1/collection-search#free-text`

#### Query (STACQL)

The query extension for STACQL support can be implemented, too. It works as it does for Items.
See <https://github.com/stac-api-extensions/query> for details.

- Requirement class in *OGC API - Records*: n/a
- Conformance class: `https://api.stacspec.org/v1.0.0-rc.1/collection-search#query`

#### Filter (CQL)

The filter extension for CQL support can be implemented, too.
See <https://github.com/stac-api-extensions/filter> for details.
It works as it does for Items, except that the queryables link for Collection Search is located in the response of `GET /collections` (property `links`).
The path/endpoint for Collection Search queryables can be freely chosen, but SHOULD NOT conflict with `GET /queryables`.

- Requirement class in *OGC API - Records*: CQL Filter
- Conformance classes:
  - `https://api.stacspec.org/v1.0.0-rc.1/collection-search#filter`
  - `http://www.opengis.net/spec/ogcapi-records-1/1.0/req/cql-filter`

#### Sorting

The sort extension can be implemented, too. It works as it does for Items.
See <https://github.com/stac-api-extensions/sort> for details.

- Requirement class in *OGC API - Records*: Sorting
- Conformance classes:
  - `https://api.stacspec.org/v1.0.0-rc.1/collection-search#sort`
  - `http://www.opengis.net/spec/ogcapi-records-1/1.0/req/sorting`

#### Fields

The fields extension can be implemented, too. It works as it does for Items.
See <https://github.com/stac-api-extensions/fields> for details.

- Requirement class in *OGC API - Records*: n/a
- Conformance class: `https://api.stacspec.org/v1.0.0-rc.1/collection-search#fields`
