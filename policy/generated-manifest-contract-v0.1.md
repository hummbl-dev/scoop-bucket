# Generated Manifest Contract v0.1

## Purpose

Defines the deterministic contract by which Scoop manifests in this bucket are
generated from canonical inputs in `hummbl-dev/packages`.

## Input

The generator consumes two inputs from `hummbl-dev/packages`:

1. **Package identity registry entry** — canonical metadata for a package
   (`packageId`, license, homepage, description, bin layout, checkver/autoupdate
   policy).
2. **Release artifact receipt** — the authoritative record of a specific
   release, including `version`, per-architecture `url` and `hash`.

A manifest is generated only when both inputs are present and consistent.

## Output

A single Scoop manifest JSON file per `(packageId, version)`, conforming to
`schemas/scoop-manifest-v0.1.json`.

## Field mapping

Manifest fields are populated exclusively from the inputs above. The mapping is
deterministic:

| Manifest field   | Source                                            |
|------------------|---------------------------------------------------|
| `version`        | release artifact receipt                          |
| `architecture`   | release artifact receipt (per-arch url/hash map) |
| `url`            | release artifact receipt (per-arch)              |
| `hash`           | release artifact receipt (per-arch)              |
| `bin`            | package identity registry                         |
| `checkver`       | package identity registry                         |
| `autoupdate`     | package identity registry                         |
| `license`        | package identity registry                         |
| `homepage`       | package identity registry                         |
| `description`    | package identity registry                         |
| manifest `name`  | `packageId` from the identity registry            |

The manifest filename is derived from `packageId`.

## Determinism

For a fixed `(identity registry entry, release artifact receipt)` pair, the
generator MUST produce byte-identical output across runs. Field ordering and
whitespace are fixed by the schema and generator.

## Validation

Before a manifest is accepted into the bucket it MUST pass validation:

1. **Schema validation** — the manifest validates against
   `schemas/scoop-manifest-v0.1.json`.
2. **Receipt match** — every artifact-derived field (`version`, `architecture`,
   `url`, `hash`) MUST equal the corresponding field in the release artifact
   receipt. Any mismatch is a hard failure.
3. **Identity match** — the manifest `name` MUST equal the `packageId` from the
   identity registry entry used as input.

A manifest that fails any check MUST NOT be committed.

## Non-goals

- This contract does not define how `packages` produces receipts; that is
  upstream policy.
- This contract does not define Scoop client behavior; it only constrains the
  shape and provenance of manifests in this bucket.
