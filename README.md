# STAC API - Collection Search

- **OpenAPI specification:** [openapi.yaml](openapi.yaml) (todo)
- **Conformance URIs:**
  - <https://api.stacspec.org/v1.0.0-rc.1/core>
  - <https://api.stacspec.org/v1.0.0-rc.1/collection-search>
  - <http://www.opengis.net/spec/ogcapi-common-2/1.0/req/simple-query>
- **[Maturity Classification](../README.md#maturity-classification):** Proposal
- **Dependencies**:
  - [STAC API - Core](https://github.com/radiantearth/stac-api-spec/blob/main/core)
  - [STAC API - Collections](https://github.com/radiantearth/stac-api-spec/tree/main/ogcapi-features)
  - [OGC API - Common - Part 2: Geospatial Data](https://portal.ogc.org/files/99149)
  - [OGC API - Records - Part 2: Collections](https://github.com/opengeospatial/ogcapi-records/tree/master/extensions/collections)

A search endpoint provides the ability to query
STAC [Collections](https://github.com/radiantearth/stac-spec/blob/master/collection-spec/README.md)
objects across collections.
It retrieves a group of Collection objects that match the provided parameters and provides them as
the `GET /collections` endpoint does.

The Collection Search endpoint by default doesn't provide any query parameters to filter and all
additional behavior will be defined in [Extensions](#extensions). These extensions can be composed
by an implementer to cover only the set of functionality the implementer requires.

This extension is based on *[OGC API - Common - Part 2: Geospatial Data](https://portal.ogc.org/files/99149#rc-simple-query-section)* and *[OGC API - Records - Part 2: Collections](https://github.com/opengeospatial/ogcapi-records/tree/master/extensions/collections)*
and selectively implements a subset of their "requirements classes".

All functionality is only defined for `GET /collections` in *OGC API - Records - Part 2: Collections*.

*Note:* STAC may add behavior for `POST /collections` in the future, but due to a potential conflict
with the Transaction Extension, specific rules for content negotiation might be required.

## Pagination

Pagination for collections works exactly as it defined for Collections in general.
See [Collection Pagination](https://github.com/radiantearth/stac-api-spec/blob/main/ogcapi-features/README.md#collection-pagination)
for details.

## Query Parameters and Fields

### Basics

A basic set of query parameters MUST be implemented.
These are aligned with the corresponding parameters in STAC API - Features:
- `bbox`
- `datetime`
- `limit`

Requirement class in *OGC API - Common - Part 2: Geospatial Data*: [Simple Query](https://portal.ogc.org/files/99149#rc-simple-query-section)

### Extensions

#### Query (STACQL)

The query extension for STACQL support can be implemented, too. It works as it does for Items.
See <https://github.com/stac-api-extensions/query> for details.

- Requirement class in *OGC API - Records - Part 2: Collections*: n/a
- Conformance class: `https://api.stacspec.org/v1.0.0-rc.1/collection-search#query`

#### Filter (CQL)

The filter extension for CQL support can be implemented, too. It works as it does for Items.
See <https://github.com/stac-api-extensions/filter> for details.

- Requirement class in *OGC API - Records - Part 2: Collections*: CQL Filter
- Conformance classes:
  - `https://api.stacspec.org/v1.0.0-rc.1/collection-search#filter`
  - `http://www.opengis.net/spec/ogcapi-records-1/1.0/req/cql-filter`

#### Sorting

The sort extension can be implemented, too. It works as it does for Items.
See <https://github.com/stac-api-extensions/sort> for details.

- Requirement class in *OGC API - Records - Part 2: Collections*: Sorting
- Conformance classes:
  - `https://api.stacspec.org/v1.0.0-rc.1/collection-search#sort`
  - `http://www.opengis.net/spec/ogcapi-records-1/1.0/req/sorting`

#### Fields

The fields extension can be implemented, too. It works as it does for Items.
See <https://github.com/stac-api-extensions/fields> for details.

- Requirement class in *OGC API - Records - Part 2: Collections*: n/a
- Conformance class: `https://api.stacspec.org/v1.0.0-rc.1/collection-search#fields`

#### Context

The context extension can be implemented, too. It works as it does for Items.
See <https://github.com/stac-api-extensions/context> for details.

- Requirement class in *OGC API - Records - Part 2: Collections*: n/a
- Conformance class: `https://api.stacspec.org/v1.0.0-rc.1/collection-search#context`