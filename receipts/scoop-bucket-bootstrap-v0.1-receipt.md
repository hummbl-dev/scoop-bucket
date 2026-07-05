# Receipt: scoop-bucket bootstrap v0.1

- **Repo:** hummbl-dev/scoop-bucket
- **Event:** Bootstrap bucket policy and generated-manifest contract
- **Version:** v0.1
- **Date:** 2025-07-04

## Inputs

This receipt records the introduction of the bootstrap policy surface for
`hummbl-dev/scoop-bucket`. There is no upstream release artifact receipt
consumed at bootstrap time; this receipt exists to anchor the policy documents
themselves.

## Artifacts introduced

| Path                                                  | Purpose                                  |
|-------------------------------------------------------|------------------------------------------|
| `policy/bucket-policy-v0.1.md`                        | Bucket policy (non-canon, downstream)    |
| `policy/generated-manifest-contract-v0.1.md`          | Manifest generation contract             |
| `schemas/scoop-manifest-v0.1.json`                    | JSON schema for generated Scoop manifests|
| `receipts/scoop-bucket-bootstrap-v0.1-receipt.md`     | This receipt                             |

## Posture

- The bucket is declared a **downstream generated surface** from
  `hummbl-dev/packages`.
- No independent artifact identity is introduced in this repo.
- Manifests MUST be generated per `generated-manifest-contract-v0.1.md` and
  validate against `scoop-manifest-v0.1.json`.

## Validation

- [x] Policy documents present and versioned `v0.1`.
- [x] Manifest schema present and referenced by the generation contract.
- [x] No manifests generated yet (bootstrap only); future manifests must match
      a `packages` release artifact receipt exactly.

## Closes

- Closes #1 — Bootstrap bucket policy and generated-manifest contract
- Closes #2 — Add manifest schema expectations for HUMMBL packages
