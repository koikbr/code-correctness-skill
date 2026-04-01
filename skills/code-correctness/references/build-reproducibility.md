---
description: Build and release review. Check reproducibility, dependency control, CI parity, hermeticity, caches, and provenance.
---

# Build Reproducibility

Use this lens for build system, CI, packaging, release, vendoring, registry, toolchain, and dependency changes.

## Terms

- **release mode**: the strict mode used to verify or produce release artifacts; exact dependency versions and sources, pinned toolchain, no undeclared network, no hidden host state
- **hermetic step**: a step that does not depend on undeclared files, environment, network, or host tools
- **provenance**: evidence of what built the artifact: source revision, dependency identities, toolchain, resolver, recipe, and builder identity

## Review points

### 1. Same inputs, same build result

A clean checkout with empty caches must build.

Prefer:

- lockfiles or equivalent resolved manifests
- pinned toolchain and resolver versions
- explicit build flags
- controlled environment variables
- deterministic generators

Do not rely on:

- `PATH`
- `$HOME`
- host locale, timezone, umask, or filesystem ordering
- current time
- undeclared network access

If exact byte-for-byte identity is not possible, the remaining variance must be named, bounded, justified, and checked.

### 2. Forks are now local ownership

A fork is acceptable only when all are true:

- the reason is documented
- the delta from upstream is visible
- the source is verifiable
- upstream status is known
- there is a path for upstream security fixes

Watch for:

- ad hoc changes on a long-lived fork branch
- different fork sources in CI and local builds
- multiple unexplained forks of the same upstream

### 3. Pins need intent and maintenance

Every pin needs:

- rationale
- scope
- owner
- review date or expiry
- removal condition where possible

Prefer narrow pins over freezing large parts of the graph.

### 4. CI must run the real recipe

CI and local development may use different wrappers, but they must drive the same underlying recipe.

Defects:

- CI installs undeclared tools
- CI sets undeclared environment variables
- green builds require pre-warmed runners
- release uses a recipe that tests never exercised

### 5. Control the dependency graph

Where the ecosystem allows it:

- lock the full transitive graph
- pin resolver version and config
- fetch through approved registries, mirrors, or proxies

If full locking is not available, require:

- pinned resolver version
- controlled registry path
- graph snapshot and diff
- explicit drift policy
- owner for exceptions

Signals worth flagging:

- lockfile churn with no clear reason
- unclear registry precedence
- runtime differences caused by different resolvers
- accidental duplication of the same library family

### 6. Undeclared inputs are bugs

Build steps must not depend on:

- home-directory content
- global config
- unpinned host packages or binaries
- hostname, username, or git dirtiness unless declared and normalized
- undeclared network calls

Normalize common sources of nondeterminism:

- locale and encoding
- timezone
- umask and file permissions
- filesystem ordering
- archive metadata
- absolute paths
- stable timestamps or `SOURCE_DATE_EPOCH`
- fixed random seeds when randomness affects output
- stable ordering of generated inputs and outputs

### 7. Caches speed up builds; they do not prove builds

Requirements:

- builds still work with caches disabled
- cache keys include all correctness-relevant inputs
- cache use is visible in logs or metrics
- there is a clean-room path in CI or release checks

Correctness bugs:

- warm cache fixes a fresh-checkout failure
- a step is skipped after a real input changed
- CI is green but a fresh clone fails

### 8. Verify sources and artifacts

Prefer signatures, checksums, pinned commits, and digests.

Unsafe patterns:

- unauthenticated dependency fetches
- mixed registries with unclear precedence
- unverifiable artifacts
- hand-downloaded binaries committed to the repo to "fix CI"

Record provenance for release artifacts when policy requires it.

### 9. Test the thing you ship

For a given target, prefer:

- build once
- test that artifact
- promote that artifact

Avoid:

- release-only flags or steps not covered by tests
- manual edits during release
- floating inputs such as `latest`

## Temporary exceptions

A workaround is acceptable only if it has:

- the assumption and reason
- tight bounds
- visibility
- an owner
- a review date
- an exit path

## Accept only if

- a fresh checkout builds with caches disabled
- toolchain and resolver behavior is deterministic
- release inputs resolve to exact versions and sources
- release mode is network-hermetic unless an exception is documented
- forks, pins, and workarounds have owner, scope, and review cadence
- CI and release use the same real recipe
- provenance exists where required
- the shipped artifact is the artifact that was tested