# ARTEMIS AR/VR Services API

OpenAPI 3.0 specification for the AR/VR collaboration services of the
[ARTEMIS](https://www.artemis-twin.eu/) project, developed by
**ATHENA Research Center** within **WP9.2**.

📖 **[Browse the interactive documentation →](https://artemis-twin.github.io/artemis-arvr-annotation-api/)**

---

## What this is

A REST API for immersive, multi-user collaborative annotation of cultural
heritage 3D digital twins. The primary consumer is the VAX Annotator
Unity/AR client running on Meta Quest headsets.

This repository publishes the **24 endpoints** of the four AR/VR collaboration
service categories defined in **Deliverable D9.2, Section 3.3**:

| Category | Endpoints | Purpose |
|---|---:|---|
| **Annotations** | 7 | Create, query, validate, export and delete annotations of six types — POI, LASSO, PRIMITIVE, MEASUREMENT, ENTITY, IMAGE |
| **Annotation Groups** | 8 | Group related annotations within a workspace; membership management and export |
| **Workspaces** | 7 | The collaborative session container: one asset, one composer, its annotations and groups |
| **Save Logs** | 2 | Append-only audit trail of save events; no update or delete by design |

## Files

| File | Contents |
|---|---|
| `artemis-arvr-annotation-openapi.json` | The OpenAPI 3.0 specification |
| `index.html` | Swagger UI page, served by GitHub Pages |

## Design notes

**Annotation model.** Type-specific spatial data is held in a nested sub-object
named after the annotation type (`poi`, `lasso`, `primitive`, `measurement`,
`entity`, `image`). Exactly one is populated per annotation, selected by the
`annotationType` discriminator; sub-objects belonging to other types are
discarded on write to prevent cross-type pollution.

**Persistence model.** The API is **not real-time**. The AR client creates
annotations locally and commits them only when the composer saves. The save flow
is N+1 REST calls — new groups, new annotations, updates, deletions, and finally
one save-log entry carrying a user-written summary. There is no batch endpoint:
the composer's client is the controller, the server is a store.

**Validation strategy.** The schema enforces type correctness only. Per-type
completeness ("a LASSO needs at least three points") is delegated to the
dedicated `POST /annotations/validate` endpoint, so partial work can be stored
while a deliberate completeness check remains available.

**Archived workspaces are read-only.** Archiving a workspace makes its content
fully immutable: every content mutation is rejected with `400`, while reads,
exports and save-log listings stay open.

## Out of scope

**Authentication and asset management** are, per D9.2 §3.3.1, provided by
supporting infrastructure and are not specified here. Every endpoint below
nevertheless requires a valid JWT Bearer token. Integration with the project
INDIGO-IAM OIDC provider (`https://chnet-iam.cloud.cnaf.infn.it`) is planned, so
that authentication is delegated to the platform and the API performs
authorization from the token claims.

**Active-workspace runtime endpoints** are excluded from this first publication.
They are backed by an in-memory registry whose state does not survive a restart
and would diverge across replicas, which makes them unsuitable for a cloud
deployment as they stand.

## Server

The `servers` entry points at the ARTEMIS Development Platform VM hosted by
CNR-INO, which is reachable **over the project VPN only**. "Try it out" in the
Swagger UI will therefore not reach it from the public internet. A publicly
reachable endpoint will be added once ports and SSL certificates are provisioned.

## Reference

Deliverable D9.2 — *Services Implementation*, Section 3.3
DOI: [10.5281/zenodo.20750197](https://doi.org/10.5281/zenodo.20750197)

Grant Agreement No. 101188009 — ARTEMIS — HORIZON-INFRA-2024-TECH-01

## Licence

MIT
