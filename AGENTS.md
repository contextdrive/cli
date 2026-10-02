# contextdrive-cli

This repository is `ctx`, the ContextDrive command-line client. Quarkus Picocli, shipped as a GraalVM native image. v1 commands are `ctx login`, `ctx ingest`, and `ctx status`.

The client walks, hashes, and uploads. It does not parse, embed, run SQL, or decide policy. Multipart parts are transport chunks. Embedding chunks are produced by `contextdrive/server` after a manifest is posted. File bytes go to the platform prefix `/{organization_id}/{namespace_id}/` with presigned URLs the origin assigns. They are not posted to the origin as a request body. This client does not accept a customer bucket.

## Vision repository

Product vision, architecture, and decisions live in the private repository `contextdrive/vision`, on branch `main`. Read `PRODUCT_VISION.md` and `ARCHITECTURE.md` there before designing or implementing. This repository does not keep a product vision. Do not add one. Server contracts this client calls are the issue bodies in `contextdrive/server` (`#8` presign onto the platform prefix, `#17` manifest and job status, `#3` JWT). `#9` (customer buckets) is closed and is not a client contract. If a note in this repository disagrees with the vision repository, the vision repository wins.

Reach `contextdrive/vision` in this order:

1. Local sibling checkout. From this repository root, `../vision` is the vision repository when the repos share a parent folder. Use that checkout only when it exists and `git -C ../vision rev-parse --abbrev-ref HEAD` prints `main`. Any other branch is not the source of truth.
2. Remote `main`, with the `gh` CLI. The logged-in token is the credential. The repository is private.

```shell
gh api repos/contextdrive/vision/contents/PRODUCT_VISION.md?ref=main --jq .content | base64 -d
gh api repos/contextdrive/vision/contents/ARCHITECTURE.md?ref=main --jq .content | base64 -d
```

`gh repo clone contextdrive/vision` and checking out `main` is the same remote read.

## Packages

The sample on `main` is `io.contextdrive.GreetingCommand`. Real commands live in `io.contextdrive.cli`. Each GitHub issue in this repository is one command. Do not take on server work (storage SPI, parsers, workers, policy) in a CLI change.
