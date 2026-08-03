# moncface conventions — bootstrap repo

Name reservation + release-pipeline test for four npm packages:

| package | role |
|---|---|
| `@moncface/colophon` | Colophon convention (spec forthcoming) |
| `colophon-spec` | reserved, points to the scoped package |
| `@moncface/koine` | Koine format (spec forthcoming) |
| `koine-spec` | reserved, points to the scoped package |

When the specs are drafted, each convention may move to its own
repository; published versions from here remain valid (repository.url
is per-version).

Release: push a tag `v*` → dry-run job → publish job (npm Trusted
Publishing / OIDC, no tokens stored anywhere).
