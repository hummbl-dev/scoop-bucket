# Bucket Policy v0.1

## Scope

This policy governs the `hummbl-dev/scoop-bucket` repository, the downstream
Scoop bucket surface for HUMMBL CLI tools distributed on Windows.

## Posture

**Non-canon.** This repository is a *downstream generated surface* derived from
the canonical source of truth in `hummbl-dev/packages`. It does not own package
identity, release artifacts, or versioning decisions.

## Source of truth

- **Canonical package metadata** lives in `hummbl-dev/packages` (the package
  identity registry).
- **Release artifact receipts** are produced by `hummbl-dev/packages` at
  publish time and are the authoritative record of what was built and released.
- This repo consumes those inputs and emits Scoop manifests. It does **not**
  introduce independent artifact identity.

## No independent artifact identity

- Manifests in this bucket MUST NOT define new package names, versions,
  checksums, or download URLs that do not originate from a package identity
  registry entry and a matching release artifact receipt.
- Every manifest field that describes an artifact (name, version, url, hash,
  architecture, bin) MUST be traceable to a receipt from `packages`.
- If a receipt is missing or contradicts the manifest, the manifest is invalid
  and MUST be regenerated or removed.

## Generation model

- Manifests are **generated**, not hand-edited. The generation process is
  defined in `policy/generated-manifest-contract-v0.1.md`.
- Edits to manifests MUST be the result of a regeneration run, not a manual
  patch. Manual drift between this repo and `packages` is a policy violation.

## Downstream guarantees

- This bucket guarantees that a given `(packageId, version)` manifest corresponds
  exactly to the receipted artifact for that release in `packages`.
- This bucket does NOT guarantee forward compatibility of manifest shape across
  schema versions; schema evolution is governed by `schemas/scoop-manifest-v0.1.json`
  and its successors.

## Versioning

This policy is versioned `v0.1`. Material changes require a new versioned
policy document and a bootstrap receipt referencing it.
