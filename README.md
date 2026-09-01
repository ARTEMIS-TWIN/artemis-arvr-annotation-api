# ARTEMIS AR/VR Services API

OpenAPI 3.0 specification for the AR/VR collaboration services of the
[ARTEMIS](https://www.artemis-twin.eu/) project, developed by
**ATHENA Research Center** within **WP9.2**.

📖 **[Browse the interactive documentation →](https://artemis-twin.github.io/artemis-arvr-annotation-api/)**

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
