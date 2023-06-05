# STAC API - Collection Search

- **Title:** Collection Search
- **OpenAPI specification:** [openapi.yaml](openapi.yaml) (todo)
- **Conformance Classes:**
  - <https://api.stacspec.org/v1.0.0-rc.1/core> (required)
  - <https://api.stacspec.org/v1.0.0-rc.1/collection-search> (required)
  - <http://www.opengis.net/spec/ogcapi-common-2/1.0/conf/simple-query> (required)
  - Extensions (all optional):
    - Free-text search: <https://api.stacspec.org/v1.0.0-rc.1/collection-search#free-text>
    - Query/STACQL: <https://api.stacspec.org/v1.0.0-rc.1/collection-search#query>
    - Filter/CQL2: <https://api.stacspec.org/v1.0.0-rc.1/collection-search#filter>
    - Sort: <https://api.stacspec.org/v1.0.0-rc.1/collection-search#sort>
    - Fields: <https://api.stacspec.org/v1.0.0-rc.1/collection-search#fields>
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

- Conformance class: `http://www.opengis.net/spec/ogcapi-common-2/1.0/conf/simple-query`
- Requirement class in *OGC API - Common - Part 2: Geospatial Data*: [Simple Query](https://portal.ogc.org/files/99149#rc-simple-query-section)

A basic set of query parameters MUST be implemented.
These are aligned with the corresponding parameters in STAC API - Features and OGC API - Records:
- [`bbox`](https://docs.ogc.org/DRAFTS/20-004.html#_parameter_bbox) (intersection of the given `bbox` with any of the spatial extent provided in a STAC Collection at `extent.spatial.bbox`)
- [`datetime`](https://docs.ogc.org/DRAFTS/20-004.html#_parameter_datetime) (intersection of the given `datetime` with any of the temporal extent provided in a STAC Collection at `extent.temporal.interval`)
- [`limit`](https://docs.ogc.org/DRAFTS/20-004.html#_parameter_limit)

### Extensions

#### Free text search

- Conformance class: `https://api.stacspec.org/v1.0.0-rc.1/collection-search#free-text`

A free-text search parameter `q` based on OGC API - Records can be added.
See <https://docs.ogc.org/DRAFTS/20-004.html#_parameter_q> for details.
The specific set of text fields of a Collection to which the parameter is applied is left to the discretion of the implementation, but a recommendation is to at least consider `title`, `description` and `keywords`.
The search works case-insensitive.
Any of the search terms (i.e. all words separated by a space) must be present in the set of text fields (OR).

#### Filter (CQL)

- Conformance classes: `https://api.stacspec.org/v1.0.0-rc.1/collection-search#filter`
- Requirement class in *OGC API - Records*: [Local Resource Catalogue, CQL Filter](https://docs.ogc.org/DRAFTS/20-004.html#clause-local-resources-catalogue_cql2-filter)

The filter extension for CQL support can be implemented, too.
See <https://github.com/stac-api-extensions/filter> for details.
It works as it does for Items, except that the queryables link for Collection Search is located in the response of `GET /collections` (property `links`).
The path/endpoint for Collection Search queryables can be freely chosen, but SHOULD NOT conflict with `GET /queryables`.

#### Query (STACQL)

- Conformance class: `https://api.stacspec.org/v1.0.0-rc.1/collection-search#query`

The query extension for STACQL support can be implemented, too. It works as it does for Items.
See <https://github.com/stac-api-extensions/query> for details.

#### Sorting

- Conformance classes: `https://api.stacspec.org/v1.0.0-rc.1/collection-search#sort`
- Requirement class in *OGC API - Records*: [Local Resource Catalogue, Sorting](https://docs.ogc.org/DRAFTS/20-004.html#clause-local-resources-catalogue_sorting)

The sort extension can be implemented, too. It works as it does for Items.
See <https://github.com/stac-api-extensions/sort> for details.

#### Fields

- Conformance class: `https://api.stacspec.org/v1.0.0-rc.1/collection-search#fields`

The fields extension can be implemented, too. It works as it does for Items.
See <https://github.com/stac-api-extensions/fields> for details.
