# OriginChainDB SDK publishing guide

Updated **5 November 2026**. This guide covers publishing the eight SDKs to
their language registries and verifying what users install. It does not deploy
the database engine. Application developers should use their SDK's README.

**Publishing is paused.** The commands below are a maintainer runbook for an
authorized release, not an announcement that these versions are available.
Current CI builds and verifies candidates; it does not publish them. Registry
accounts, release authentication and the remaining release gates must be ready
before running an upload or pushing a release tag.

## Package names and candidate versions

Use the reviewed commits and successful CI runs in [SDK support](SDK-SUPPORT.md#source-versions-and-verification).
The native version spellings below represent the same release candidate.

| SDK | Source repository | Registry coordinate | Candidate version |
| --- | --- | --- | --- |
| Python | [originchaindb-python](https://github.com/originchaindb/originchaindb-python) | PyPI `originchain` | `1.0.0rc1` |
| TypeScript / JavaScript | [originchaindb](https://github.com/originchaindb/originchaindb) | npm `@originchain/sdk` | `1.0.0-rc.1` |
| Go | [originchaindb-go](https://github.com/originchaindb/originchaindb-go) | `github.com/originchaindb/originchaindb-go` | Proposed tag `v1.0.0-rc.1` |
| Rust | [originchaindb-rust](https://github.com/originchaindb/originchaindb-rust) | crates.io `originchaindb` | `1.0.0-rc.1` |
| Java | [originchaindb-java](https://github.com/originchaindb/originchaindb-java) | Maven Central `ai.originchain:originchaindb` | `1.0.0-rc.1` |
| C# / .NET | [originchaindb-csharp](https://github.com/originchaindb/originchaindb-csharp) | NuGet `OriginChainDB` | `1.0.0-rc.1` |
| PHP | [originchaindb-php](https://github.com/originchaindb/originchaindb-php) | Packagist `originchain/originchaindb` | Proposed tag `v1.0.0-rc.1` |
| Ruby | [originchaindb-ruby](https://github.com/originchaindb/originchaindb-ruby) | RubyGems `originchaindb` | `1.0.0.rc.1` |

Check registry ownership and whether the version already exists before release.
An old package under the same name is not evidence that it contains this SDK.
Keep published versions and tags immutable; choose a new version when needed.

## One-time account setup

Use organization-controlled ownership, verified contact information and the
registry's required account security settings. GitHub access does not grant
publishing rights to a separate registry.

| Registry | Required setup | Official instructions |
| --- | --- | --- |
| PyPI | Owner or maintainer access to `originchain`; configure a trusted publisher or a project-scoped API token. | [Trusted publishers](https://docs.pypi.org/trusted-publishers/adding-a-publisher/) |
| npm | Permission to publish `@originchain/sdk` under the `originchain` scope; configure a trusted publisher or an authorized publishing credential. | [Trusted publishers](https://docs.npmjs.com/trusted-publishers/) |
| Go | Existing write access to the public GitHub module repository; no separate registry account or upload token. | [Publishing modules](https://go.dev/doc/modules/publishing) |
| crates.io | Account with a verified email and permission to publish the crate; configure registry authentication. | [First publication](https://doc.rust-lang.org/cargo/reference/publishing.html) |
| Maven Central | Central Portal account, verified `ai.originchain` namespace, Portal publishing-token credentials, and an organization-controlled PGP signing key. | [Account](https://central.sonatype.org/register/central-portal/), [namespace](https://central.sonatype.org/register/namespace/), [token](https://central.sonatype.org/publish/generate-portal-token/), [signing](https://central.sonatype.org/publish/requirements/) |
| NuGet | Account and package publishing rights; configure a trusted publishing policy or scoped API key for `OriginChainDB`. | [Trusted publishing](https://learn.microsoft.com/en-us/nuget/nuget-org/trusted-publishing) |
| Packagist | Account with rights to submit/manage `originchain/originchaindb`; submit the public GitHub repository and configure update synchronization. | [Submission and updates](https://packagist.org/about) |
| RubyGems | Account with gem publishing rights; satisfy MFA requirements and configure a trusted publisher or scoped publishing key. | [Publishing](https://guides.rubygems.org/publishing/), [trusted publishing](https://guides.rubygems.org/trusted-publishing/releasing-gems/) |

Prefer trusted publishing where the registry supports the selected release
environment. Configure its exact GitHub organization, repository, workflow
filename and environment; a configuration for another SDK does not authorize
this one. npm's documented GitHub OIDC flow uses GitHub-hosted runners, so do
not assume that the private qualification runner can publish through that flow.
See the registry instructions above for initial-package setup and eligibility.

Keep fallback tokens and signing material in the approved secret manager.
Inject credentials into the release job at runtime; never commit them or paste
them into issues, command history, release notes or this guide. Database access
tokens and management-account passwords are not package-publishing credentials.

## Before each release

1. Confirm the publishing hold has been lifted for the named packages and
   versions. A documentation update or green CI run is not release authorization.
2. Select an exact source commit for each SDK. Check its version, changelog,
   package contents, supported runtimes and migration guidance.
3. Require successful CI, installed-package checks and verified provenance for
   those commits. Obtain a fresh full qualification receipt for the approved
   SDK and engine pins. Clear the outstanding engine security verification gate;
   scoped read-only policy tests do not establish general RLS write safety.
4. Check account ownership, namespace approval and registry version availability.
   Authenticate with the intended organization account, not a personal fallback.
5. Prepare the registry upload or tag and its exact artifacts. Preserve hashes,
   source revisions and test receipts in the release record.
6. Publish only the approved candidate, verify installation from the registry,
   then update the support matrix and installation instructions.

The earlier Windows soak failures remain unresolved; successful bounded Windows
checks do not establish Windows endurance. Release notes must retain applicable
[compatibility limits](SDK-SUPPORT.md#windows-connection-investigation).

## Download and verify the build

Commands use Bash on Linux, macOS or WSL. Run them in a clean release workspace
with Python, GitHub CLI and the relevant language tooling. Set `REPO`, `CI_RUN`
and `SOURCE_SHA` to one approved SDK repository, successful **main** CI run and
full source SHA. Do not use a floating branch as the release identity.

For GitHub operations in this organization, use the authorized identity:

```bash
set -euo pipefail
set +x
export GH_TOKEN="$(gh auth token --hostname github.com --user Originchain-ops)"
test "$(gh api user --jq .login)" = "Originchain-ops"
gh run view "$CI_RUN" --repo "$REPO" --json headSha,status,conclusion
gh run download "$CI_RUN" --repo "$REPO" \
  --name sdk-verified-build --dir release-files
```

Require the displayed SHA to equal `SOURCE_SHA`, status `completed` and
conclusion `success`. Use a new empty `release-files` directory for each SDK.
Verify the provenance of the manifest and each package file:

```bash
for file in release-files/*; do
  gh attestation verify "$file" --repo "$REPO" \
    --signer-workflow "$REPO/.github/workflows/ci.yml" \
    --source-digest "$SOURCE_SHA" --source-ref refs/heads/main
done
```

Check `build.json`: its repository, source commit and workflow run must match
the selected release. Recalculate each listed file's SHA-256 and size and
compare them with the manifest. Attestations establish origin; the passing
checks and qualification establish the tested behavior. Details are in each
SDK's `docs/quality.md` and GitHub's
[verification guide](https://docs.github.com/en/actions/how-tos/secure-your-work/use-artifact-attestations/verify-artifact-attestations-with-the-github-cli).

CI artifacts expire after 14 days. If unavailable, run the approved build
process again and retain new verification evidence. Do not substitute an
unverified local build or an older registry package.

In the upload examples, `WHEEL`, `SDIST`, `TARBALL`, `NUPKG` and `GEM` are paths
to the exact verified files listed in `build.json`. Set them before use; do not
upload the manifest, provenance bundle or every file through an indiscriminate
wildcard. The commands below perform publication and belong after the release
gate, not in a routine build or pull-request job.

## Python to PyPI

The distribution and import name are both `originchain`. CI supplies a wheel
and source distribution. For the token-based operator path, obtain the
authorized token through the secret manager and inject `TWINE_PASSWORD`;
`TWINE_USERNAME` is `__token__`.

```bash
python -m twine check "$WHEEL" "$SDIST"
python -m twine upload --non-interactive \
  --repository-url https://upload.pypi.org/legacy/ "$WHEEL" "$SDIST"
```

For OIDC automation, configure PyPI's trusted publisher and its supported
publishing action instead of assuming a local Twine command obtains an OIDC
credential. TestPyPI is a separate service with separate credentials and does
not publish the production package. [Python packaging instructions](https://packaging.python.org/en/latest/tutorials/packaging-projects/).

## TypeScript and JavaScript to npm

Authenticate for `@originchain/sdk` through the configured trusted publisher or
approved npm credential. An environment variable alone does not configure npm
token authentication; use the registry's supported CLI/CI authentication setup.
Publish the verified tarball and keep the candidate on the `next` distribution
tag so it does not replace the stable `latest` selection.

```bash
npm publish "$TARBALL" --registry https://registry.npmjs.org/ \
  --access public --tag next --ignore-scripts
```

The tarball already contains the compiled CommonJS, ES module and type files.
Do not rebuild it during publication. A configured trusted GitHub publisher
can supply registry provenance; the existing GitHub artifact attestation is
a separate verification record. [npm publish](https://docs.npmjs.com/cli/v11/commands/npm-publish/).

## Go through a Git tag

The module path stays `github.com/originchaindb/originchaindb-go`. In a clean
checkout, confirm `SOURCE_SHA` is the approved commit and the proposed tag is
unused. Verify the `Originchain-ops` actor immediately before pushing, and pin
the credential helper:

```bash
set -euo pipefail
test "$(gh api user --jq .login)" = "Originchain-ops"
git tag -a v1.0.0-rc.1 "$SOURCE_SHA" -m "OriginChainDB Go 1.0.0-rc.1"
git -c credential.helper= -c 'credential.helper=!gh auth git-credential' \
  push origin refs/tags/v1.0.0-rc.1
```

Check the remote tag resolves to the reviewed commit. Fetch that version from
a fresh Go module to verify normal proxy/checksum resolution. Do not move a
published tag. [Go release procedure](https://go.dev/doc/modules/publishing).

## Rust to crates.io

Use a clean checkout of the approved SHA and its committed `Cargo.lock`, with
the release toolchain and authenticated registry. `CARGO_REGISTRY_TOKEN` can
be injected from the secret manager for token-based publication.

```bash
cargo publish --locked --registry crates-io --dry-run
# Verify the generated target/package/*.crate against the approved candidate.
cargo publish --locked --registry crates-io
```

Cargo packages the source when publishing; it does not accept an existing
`.crate` file as a positional upload argument. Before the real publish, compare
the generated archive with the approved artifact. Resolve any difference and
retain new package verification evidence rather than assuming byte identity.
Do not use `--no-verify` or allow unreviewed working-tree changes. Confirm the
registry's final version even if Cargo times out while waiting for its index.
[Cargo publish](https://doc.rust-lang.org/cargo/commands/cargo-publish.html).

## Java to Maven Central

**First-release packaging setup is still required.** The current POM builds
the main, source and Javadoc JARs. It does not yet configure Central publishing
or PGP signing, and the current CI artifact does not include a release POM.
Running `mvn deploy` on this checkout is not a complete Central release process.

After namespace and credential setup, prepare a reviewed release configuration:

1. Include the exact release POM in the verified CI artifacts alongside all
   three JARs; retain its source identity and checksum.
2. Configure PGP signing and the Central publishing plugin using the
   [official Maven integration](https://central.sonatype.org/publish/publish-portal-maven/),
   or assemble a Portal upload bundle from the verified files.
3. Put the POM, JARs, required checksums and detached signatures under
   `ai/originchain/originchaindb/1.0.0-rc.1/` in the bundle. Keep the signing
   secret outside the repository and build artifacts.
4. Upload through the configured plugin or Portal, review validation results,
   and explicitly publish only after release authorization. A validated
   deployment is not yet a published Maven Central version.
5. Resolve `ai.originchain:originchaindb:1.0.0-rc.1` in a fresh Maven project.

Use the [Central requirements](https://central.sonatype.org/publish/requirements/)
and [bundle layout](https://central.sonatype.org/publish/publish-portal-upload/).
Do not upload the unsigned CI JAR by itself or invent a `release` Maven profile
that is not present in the reviewed project.

## C# to NuGet

Publish the verified `.nupkg`. Obtain `NUGET_API_KEY` from the configured trusted
publisher exchange or scoped secret; never put its value in the command text.

```bash
dotnet nuget push "$NUPKG" --source https://api.nuget.org/v3/index.json \
  --api-key "$NUGET_API_KEY"
```

The CLI can also submit an adjacent matching `.snupkg`; retain and verify its
artifact if symbol publication is part of the release. Check NuGet's validation
and indexing state before announcing installation availability. Do not use a
skip-duplicate option to hide an unexplained existing version.
[NuGet publication](https://learn.microsoft.com/en-us/nuget/nuget-org/publish-a-package).

## PHP through Packagist

Submit `https://github.com/originchaindb/originchaindb-php` in the authorized
Packagist account and connect its GitHub update mechanism. The package already
has `composer.json`; keep its version omitted so Packagist derives versions
from repository tags.

```bash
set -euo pipefail
composer validate --strict
test "$(gh api user --jq .login)" = "Originchain-ops"
git tag -a v1.0.0-rc.1 "$SOURCE_SHA" -m "OriginChainDB PHP 1.0.0-rc.1"
git -c credential.helper= -c 'credential.helper=!gh auth git-credential' \
  push origin refs/tags/v1.0.0-rc.1
```

Check that the tag is unused before creating it and resolves to the approved
commit afterward. Request an update in Packagist if synchronization is pending,
then verify the version and source reference. The CI ZIP verifies package
contents; it is not an archive to upload to Packagist. [Packagist instructions](https://packagist.org/about).

## Ruby to RubyGems

The gem's native candidate version is `1.0.0.rc.1`. Publish the verified gem
using the configured RubyGems authentication. For a scoped token, the supported
`GEM_HOST_API_KEY` environment variable can supply it without embedding it in
the command. Satisfy the account's applicable MFA/OTP requirements.

```bash
gem push "$GEM" --host https://rubygems.org
```

Check the registry's recorded gem version and dependencies after publication.
[RubyGems publishing](https://guides.rubygems.org/publishing/) and
[command authentication](https://guides.rubygems.org/command-reference/#gem-push).

## Verify what users install

Run these only after the exact candidate is published. Each row belongs in a
fresh application or isolated environment with no local SDK path override.
Use the repository README for native runtime requirements and client setup.

| SDK | Install or resolve the exact candidate |
| --- | --- |
| Python | `python -m pip install --no-cache-dir "originchain==1.0.0rc1"` in a new virtual environment |
| TypeScript / JavaScript | `npm install --save-exact @originchain/sdk@1.0.0-rc.1` in a new npm project |
| Go | `go get github.com/originchaindb/originchaindb-go@v1.0.0-rc.1` in a new Go module |
| Rust | `cargo add originchaindb@=1.0.0-rc.1` in a new Cargo project |
| Java | Add Maven dependency `ai.originchain:originchaindb:1.0.0-rc.1`; resolve using an empty local Maven repository |
| C# / .NET | `dotnet add package OriginChainDB --version 1.0.0-rc.1` in a new .NET project with an empty package cache |
| PHP | `composer require originchain/originchaindb:1.0.0-rc.1` in a new Composer project |
| Ruby | `gem install originchaindb --version 1.0.0.rc.1 --no-document` in an isolated gem environment |

Confirm the resolved version and registry source, then compile/import the
public API. Using the approved non-production database and a separately
supplied `OC_TOKEN`, run the README's bound query `SELECT $1 AS value` with
parameter `7` and require the returned value to be `7`.

Repeat the installed-package contract checks and the SQL, vector, full-text
search, graph and bulk/pagination examples against owned fixtures. These
examples create data: record and remove only their resources afterward.
Retain package hashes, resolved dependency versions, test output and cleanup
receipts. A registry upload alone is not end-to-end verification.

## Complete or recover a release

After registry verification, create the approved GitHub release records with
the exact source SHA, version, migration notes, known limits and provenance.
Update the READMEs and [support matrix](SDK-SUPPORT.md) to show the versions
actually available. If only some registries succeeded, list each separately;
do not announce an eight-language release while others are pending.

If an upload times out, inspect the registry before repeating it. Stop on a
wrong artifact, authentication failure, validation failure or failed installed
test. Keep the failure evidence, use the registry's deprecation/yank controls
where appropriate, and issue a corrected version. Do not force-update published
tags or treat an existing version as proof that this upload succeeded.
