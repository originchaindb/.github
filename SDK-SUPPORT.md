# OriginChainDB SDK support

Updated **4 November 2026**. This matrix covers the eight public SDK source
candidates below. Use each repository's README for installation, connection
settings, runnable examples, and language-specific behavior.

## Source versions and verification

Package publication is paused. These versions describe source candidates;
they do not indicate that a matching package is available on a registry.
Use the README's source-install instructions and pin a reviewed commit.

| Language | Package or module | Source version | Verified source | CI |
| --- | --- | --- | --- | --- |
| Python | `originchain` | `1.0.0rc1` | [`194088399f27`](https://github.com/originchaindb/originchaindb-python/tree/194088399f277fa105fd9691c8d9b3637eec3c4b) | [Passed](https://github.com/originchaindb/originchaindb-python/actions/runs/41737112031) |
| TypeScript / JavaScript | `@originchain/sdk` | `1.0.0-rc.1` | [`a884c3a24eea`](https://github.com/originchaindb/originchaindb/tree/a884c3a24eeaf91b5311fbcfc2c60adfa066ac94) | [Passed](https://github.com/originchaindb/originchaindb/actions/runs/41737131569) |
| Go | `github.com/originchaindb/originchaindb-go` | `1.0.0-rc.1` source candidate | [`494448f8fbbb`](https://github.com/originchaindb/originchaindb-go/tree/494448f8fbbbf637fcee4f92b4e0fd740e7f48ef) | [Passed](https://github.com/originchaindb/originchaindb-go/actions/runs/41737146352) |
| Rust | `originchaindb` | `1.0.0-rc.1` | [`1060394037ef`](https://github.com/originchaindb/originchaindb-rust/tree/1060394037ef631833f709c4bd432af088027d3d) | [Passed](https://github.com/originchaindb/originchaindb-rust/actions/runs/41737166332) |
| Java | `ai.originchain:originchaindb` | `1.0.0-rc.1` | [`f3eb55fe3745`](https://github.com/originchaindb/originchaindb-java/tree/f3eb55fe3745a1f245a5c3b3918788d4bc55fe03) | [Passed](https://github.com/originchaindb/originchaindb-java/actions/runs/41737182682) |
| C# / .NET | `OriginChainDB` | `1.0.0-rc.1` | [`1b49700f6496`](https://github.com/originchaindb/originchaindb-csharp/tree/1b49700f64969fcac9fe8117e4008f059ee263d6) | [Passed](https://github.com/originchaindb/originchaindb-csharp/actions/runs/41737197849) |
| PHP | `originchain/originchaindb` | Composer development branch | [`0a502d297c6d`](https://github.com/originchaindb/originchaindb-php/tree/0a502d297c6d797055ea8213a07ff5d09d330b85) | [Passed](https://github.com/originchaindb/originchaindb-php/actions/runs/41737214570) |
| Ruby | `originchaindb` | `1.0.0.rc.1` | [`17cb1d36498e`](https://github.com/originchaindb/originchaindb-ruby/tree/17cb1d36498e38bdbfd9176b440ea68c1342919d) | [Passed](https://github.com/originchaindb/originchaindb-ruby/actions/runs/41737232155) |

Python's import remains `originchain`; the TypeScript SDK also supports
JavaScript. Ruby uses the normalized gem version shown above. PHP has no
published candidate tag. Go's candidate label is not a published module tag;
use the commit-based installation in its README.

## Runtime coverage

All **30 CI jobs** linked above passed. This table lists tested configurations,
not an assertion that every other runtime or operating system is supported.
The `stable` entries refer to the toolchain resolved by the linked CI run.

| SDK | Runtime checks | Operating systems |
| --- | --- | --- |
| Python | Python 3.10, 3.12, 3.14 | Linux; Windows and macOS on 3.12 |
| TypeScript / JavaScript | Installed HTTP consumer on Node 18, 22, 24; build and unit tests on Node 24; Chromium, Firefox, WebKit page and worker tests | Linux; Node 22 consumers on Windows and macOS; browsers on Linux |
| Go | Go 1.23 and stable | Linux; stable on Windows and macOS |
| Rust | Rust 1.88.0 and stable, with rustls | Linux; stable on Windows |
| Java | Temurin 17, 21, 25 | Linux |
| C# / .NET | .NET 8 | Linux and Windows |
| PHP | PHP 8.2, 8.4 | Linux |
| Ruby | Ruby 3.2, 3.4, 4.0 | Linux on 3.2/3.4; Windows on 4.0 |

Browser tests verify the transport and package. Applications still need an
appropriate credential delivery model and server CORS configuration; do not
embed privileged database credentials in frontend bundles.

## Shared behavior and live examples

The SDKs are maintained implementations with native language conventions.
Their common core covers SQL, schemas and rows, vector operations, full-text
indexing/search, graph queries/traversal, and usage/query metrics. Python and
TypeScript also have additional helpers outside this common core.

All eight SDKs passed the same **24-request HTTP contract**, covering request
shapes, response handling, precision, partial batch failures, authentication
errors, server errors, and redirect refusal. Each repository includes the
contract fixture and CI consumer.

All eight runnable catalog programs were also built or syntax-checked and
executed against an isolated integration database. They passed:

- Schema registration and row writes/read.
- SQL with bound parameters.
- Vector insertion and similarity search.
- Explicit full-text indexing and BM25 search.
- Graph neighbor traversal using the declared relation.

The [organization page](https://github.com/originchaindb) links to every catalog
program. Each SDK README explains the setup, result fields, and example commands.
The examples write data and require the corresponding database permissions.

## Compatibility boundaries

- Set `OC_URL` to the complete database address from **Connect**, and `OC_TOKEN`
  to the database access token. The SDK does not infer a tenant path.
- HTTPS is required except for loopback development. The default request timeout
  is 60 seconds; Ruby uses network inactivity timeouts. Redirects are refused,
  and automatic SDK retries are disabled by default.
- SQL results retain the server's value representation. Use the result and
  precision guidance in your language's README when mapping values into models.
- Cancellation, concurrency, and client lifetime follow native runtime behavior.
  Ruby and PHP clients are intended for sequential use per client instance.
- Live examples used an existing integration credential. Activation, expiry,
  rotation, and revocation of the new token lifecycle are not certified by these
  checks.
- Live checks establish behavior on the integration database used for this run.
  They do not establish a supported server-version range or a production rollout.
  Availability still depends on server features and token permissions.

## Framework integrations and MCP

| Project | Scope | Compatibility status |
| --- | --- | --- |
| [LangChain](https://github.com/originchaindb/langchain-originchain) | Python vector-store adapter | Earlier Python SDK source pin; compatibility with the 1.0 candidate has not been verified; proprietary license |
| [LlamaIndex](https://github.com/originchaindb/llama-index-vector-stores-originchain) | Python vector-store adapter | Earlier Python SDK source pin; compatibility with the 1.0 candidate has not been verified; proprietary license |
| [MCP server](https://github.com/originchaindb/mcp-server) | SQL, vector search, full-text search, and schema discovery over stdio; Node 20+ | Separate project and verification; not included in the eight-SDK checks above; MIT license |

Follow each integration's own README for installation, permissions, and
limitations. This matrix does not certify additional language clients or mobile
platforms outside the configurations listed above.
