# OriginChainDB SDK support

Updated 26 October 2026. Use each repository's README for installation and a
working first query. Repository names and package names are intentionally listed
separately: TypeScript uses the `originchaindb` repository and `@originchain/sdk`
package; Python uses `originchaindb-python` and imports `originchain`.

## Maintained language clients

The source versions below are unreleased. All eight passed fresh installed-
package checks for SQL, graph, vector and full-text operations on 25 October
against the same ephemeral development engine. Each repository's
`validation/DEVELOPMENT.md` records the scope and exact public runtime commit.
This establishes development-candidate behavior, not deployment to your database.

| Language | Repository | Package / module | Development version | Tested runtime source |
| --- | --- | --- | --- | --- |
| TypeScript / JavaScript | [originchaindb](https://github.com/originchaindb/originchaindb) | `@originchain/sdk` | `0.8.0-rc.1` | [`d870b52527a2`](https://github.com/originchaindb/originchaindb/tree/d870b52527a252c9c96339100af5ddeeebaccbd8) |
| Python | [originchaindb-python](https://github.com/originchaindb/originchaindb-python) | `originchain` | `0.11.0.dev0` | [`944cd410ffb1`](https://github.com/originchaindb/originchaindb-python/tree/944cd410ffb11110c00ce925c0c609e11862916b) |
| Go | [originchaindb-go](https://github.com/originchaindb/originchaindb-go) | `github.com/originchaindb/originchaindb-go` | `0.5.0-rc.1` | [`d7b6c85832f2`](https://github.com/originchaindb/originchaindb-go/tree/d7b6c85832f2f85c25a2db94d59eb5e660e6d4d2) |
| Java | [originchaindb-java](https://github.com/originchaindb/originchaindb-java) | `ai.originchain:originchaindb` | `0.5.0-rc.1` | [`f8b3e6e9f7b5`](https://github.com/originchaindb/originchaindb-java/tree/f8b3e6e9f7b549809c47c0ce8b6b36113e963e6b) |
| C# / .NET | [originchaindb-csharp](https://github.com/originchaindb/originchaindb-csharp) | `OriginChainDB` | `0.5.0-rc.1` | [`be39aae12ae2`](https://github.com/originchaindb/originchaindb-csharp/tree/be39aae12ae2124bdffdc6383d3b386fefaba251) |
| Ruby | [originchaindb-ruby](https://github.com/originchaindb/originchaindb-ruby) | `originchaindb` | `0.5.0.pre.rc.1` | [`d3e4c1b0b0c8`](https://github.com/originchaindb/originchaindb-ruby/tree/d3e4c1b0b0c871db25c3e5afc64ca56fd13bb5b4) |
| PHP | [originchaindb-php](https://github.com/originchaindb/originchaindb-php) | `originchain/originchaindb` | `development branch` | [`f3c6721d5b41`](https://github.com/originchaindb/originchaindb-php/tree/f3c6721d5b419385f56c053de8aea3e88046d9a3) |
| Rust | [originchaindb-rust](https://github.com/originchaindb/originchaindb-rust) | `originchaindb` | `0.5.0-rc.1` | [`a8488593201b`](https://github.com/originchaindb/originchaindb-rust/tree/a8488593201baedb382f78bbfe8b5d13fdf49e8d) |

Ruby's gem uses the normalized version `0.5.0.pre.rc.1`. PHP uses its Composer
development branch rather than an unpublished version tag. Earlier GitHub
previews remain separate from these source versions. Releases and registry
publication are paused; an unqualified package-manager install can select an
older package.

## Runtime checks

These are passing checks on the source snapshots above. They do not establish
support on untested runtimes or for every operation.

| Client | Verified runtime checks | Platform scope |
| --- | --- | --- |
| TypeScript / JavaScript | Node 18, 22, 24; Chromium, Firefox, WebKit request tests | Node: Linux, plus Node 22 on Windows/macOS; browser/worker fixtures do not grant safe use of embedded database credentials |
| Python | Python 3.9, 3.12, 3.14 | Linux, plus Python 3.12 on Windows/macOS |
| Go | Go 1.23 and 1.26 | Ubuntu 24.04, macOS 14 |
| Java | Temurin 17 and 21 | Ubuntu 24.04, macOS 14 |
| C# / .NET | .NET 8 | Ubuntu 24.04, macOS 14 |
| Ruby | Ruby 3.2, 3.3, 3.4 | Ubuntu 24.04, macOS 14 |
| PHP | PHP 8.2, 8.3, 8.4 | Ubuntu 24.04, macOS 14 |
| Rust | Stable compiler recorded by CI; native-tls and rustls | Ubuntu 24.04, macOS 14; no explicit minimum Rust compiler certified |

The installed-package engine checks cover bound SQL parameters, graph traversal,
vector search, boolean/BM25/highlight full-text responses, diagnostics and query
metrics with null idle latency. Build and fixture checks have broader runtime
coverage than live-operation checks. Server feature gates and permissions apply;
generated methods do not establish availability of every administrative API.

## Additional clients in development

Swift, Dart and Kotlin are not public package releases. Their current evidence
is narrower than the eight maintained clients above.

| Client | Passed checks | Still unverified |
| --- | --- | --- |
| Swift | HTTP fixtures on macOS and Ubuntu 24.04; iOS 26.2, tvOS 26.2 and watchOS 26.2 simulators; separate installed-consumer SQL check | Physical devices and minimum Apple OS versions |
| Dart | HTTP fixtures on Linux, macOS and Windows; nine Flutter 3.41.4 / Dart 3.11.1 integration tests on an Android 15 / API 35 AOSP ATD x86_64 emulator; separate installed-consumer SQL check | Browsers, physical Android devices, other Android versions and minimum Android version compatibility |
| Kotlin | JVM coroutine/HTTP fixtures on Linux, macOS and Windows; seven instrumented tests on an Android 15 / API 35 AOSP ATD x86_64 emulator; separate installed-consumer SQL check | Physical Android devices, other Android versions and minimum Android version compatibility |

The Android tests use a software emulator and a local HTTP fixture; they do
not test a deployed database or physical device. The same distinction applies
to the Apple simulator and desktop fixtures. A separate SQL consumer check does
not certify graph, vector or full-text behavior on all these platforms. Public
installation and distribution checks remain pending.

## Frameworks and MCP

| Library | Available scope | Distribution |
| --- | --- | --- |
| [LangChain](https://github.com/originchaindb/langchain-originchain) | Vector-store add, search, filtering and deletion; see README for transaction and MMR limits | Existing `0.1.0rc1` GitHub preview, proprietary license; not on PyPI |
| [LlamaIndex](https://github.com/originchaindb/llama-index-vector-stores-originchain) | Node storage, search, metadata filters and source-document deletion | Existing `0.1.0rc1` GitHub preview, proprietary license; not on PyPI |
| [MCP server](https://github.com/originchaindb/mcp-server) | SQL, vector search, full-text search and schema discovery over stdio; Node 20+ | `0.3.0-rc.1` development source; earlier `0.2.0-rc.1` GitHub preview |

The adapters' existing release verification remains attached to their release
pages. Their current README installs the maintained Python development source;
that change does not republish the adapters or establish a new live release
acceptance result. MCP has no dedicated graph tool.

## Data types and server versions

SDKs preserve the server's SQL value representation and optional output type
metadata. The development-engine tests include floating-point special values:
TypeScript keeps exact strings; Python converts them only when the SQL type
explicitly identifies a floating-point column. Ordinary text is unchanged.

These backend changes have not been deployed by the SDK verification work.
Stored microsecond timestamps, calendar intervals, protocol compatibility and
recovery remain under development; new storage formats stay disabled. Do not
infer a rollout or enable a format from a client version or generated method.
