\# Lab 3 submission



Chosen path: GitHub Actions.



\## Task 1 — PR Gate



\### Green CI run



Link: TODO



\### Failed CI run evidence



Screenshot: `submissions/src/screenshots/lab03/failed\_ci.png`



Fix commit: TODO



\### Branch protection



Screenshot: `submissions/src/screenshots/lab03/branch\_protection.png`



\### Design questions



\*\*a) Why pin the runner version (`ubuntu-24.04`) instead of `ubuntu-latest`?\*\*



Pinning the runner version prevents unexpected changes in the CI environment. If `ubuntu-latest` moves to a new image, installed tools, system libraries, or defaults may change and break a previously green pipeline without any repository change.



\*\*b) Why split vet + test + lint into separate units?\*\*



Separate jobs make failures easier to diagnose and allow independent work to run in parallel. If everything is in one combined job, a fast failure can hide later failures, and the total pipeline becomes less informative and less parallel.



\*\*c) What real attack does SHA pinning prevent?\*\*



SHA pinning prevents supply-chain attacks where a third-party action tag is moved or compromised. A relevant example is the `tj-actions/changed-files` compromise from March 2025, where a popular GitHub Action was used as an attack path.



\*\*d) What is `permissions:` and what principle is behind it?\*\*



`permissions:` controls the default GitHub token permissions available to workflow jobs. Setting `contents: read` follows the principle of least privilege: the pipeline should receive only the access it actually needs.



\*\*e) GitLab stages/jobs question\*\*



I selected the GitHub Actions path, so GitLab-specific stages and dependencies are not applicable for my implementation.



\## Task 2 — Make It Fast and Smart



\### Timing table



| Scenario | Wall-clock |

|----------|------------|

| Baseline (no cache, single Go version, no path filter) | TODO |

| With cache | TODO |

| With cache + matrix | TODO |



\### Optimizations applied



\- Enabled Go cache via `actions/setup-go`.

\- Added a Go version matrix for `1.23` and `1.24`.

\- Added path filters for `app/\*\*` and `.github/workflows/ci.yml`.

\- Added a `ci-ok` aggregation job for stable branch protection.



\### Design questions



\*\*f) Why cache `go.sum`-keyed inputs and not build outputs?\*\*



Caches should be based on deterministic inputs, such as dependency lock files. Build outputs can depend on OS, compiler version, environment variables, and hidden state, so caching them blindly can cause stale or incorrect artifacts.



\*\*g) What does `fail-fast: false` change in a matrix run?\*\*



`fail-fast: false` allows all matrix jobs to finish even if one Go version fails. This is useful for diagnosis because we can see exactly which toolchain versions are affected. `fail-fast: true` is useful when saving CI minutes is more important than collecting complete failure information.



\*\*h) What is the cache poisoning risk?\*\*



If a malicious PR can write cache entries that protected branches later restore, the attacker may try to inject modified dependencies or build artifacts into trusted runs. GitHub mitigates this with cache scoping and access restrictions, but workflows should still avoid caching untrusted outputs and should use deterministic keys.



\## Bonus Task — Pipeline Performance Investigation



\### Before/after table



| Optimization applied | Before (s) | After (s) | Saving |

|----------------------|-----------:|----------:|-------:|

| Parallel jobs | TODO | TODO | TODO |

| Go cache | TODO | TODO | TODO |

| Path filters | TODO | TODO | TODO |



\### Bottleneck analysis



TODO

