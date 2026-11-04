# OriginChainDB SDK support

Updated **4 November 2026**. This matrix covers the eight public SDK source
candidates below. Use each repository's README for installation, connection
settings, runnable examples, and language-specific behavior.

## Source versions and verification

Package publication is paused. These versions describe source candidates;
they do not indicate that a matching package is available on a registry.
Use the README's source-install instructions and pin a reviewed commit.

Maintainers: follow the [SDK publishing guide](SDK-PUBLISHING.md) for registry
setup, verified artifacts, release commands and post-publication checks.

| Language | Package or module | Source version | Verified source | CI |
| --- | --- | --- | --- | --- |
| Python | `originchain` | `1.0.0rc1` | [`bd50928dc938`](https://github.com/originchaindb/originchaindb-python/tree/bd50928dc9382889ab5c541ecdb99335fb6f4728) | [Passed](https://github.com/originchaindb/originchaindb-python/actions/runs/41867600397) |
| TypeScript / JavaScript | `@originchain/sdk` | `1.0.0-rc.1` | [`909a992c4d44`](https://github.com/originchaindb/originchaindb/tree/909a992c4d44af447754710c2920f154ecbda276) | [Passed](https://github.com/originchaindb/originchaindb/actions/runs/41823527178) |
| Go | `github.com/originchaindb/originchaindb-go` | `1.0.0-rc.1` source candidate | [`198c066dab66`](https://github.com/originchaindb/originchaindb-go/tree/198c066dab6678a65102a0f43640c52d860b4f5f) | [Passed](https://github.com/originchaindb/originchaindb-go/actions/runs/41823553618) |
| Rust | `originchaindb` | `1.0.0-rc.1` | [`96132665a0ea`](https://github.com/originchaindb/originchaindb-rust/tree/96132665a0ea8822de8bf7b2ffd2bf3c8ed0d292) | [Passed](https://github.com/originchaindb/originchaindb-rust/actions/runs/41823577397) |
| Java | `ai.originchain:originchaindb` | `1.0.0-rc.1` | [`475ded0442d1`](https://github.com/originchaindb/originchaindb-java/tree/475ded0442d1eb098c9aa338accda5554a69a955) | [Passed](https://github.com/originchaindb/originchaindb-java/actions/runs/41823605284) |
| C# / .NET | `OriginChainDB` | `1.0.0-rc.1` | [`45822b1d0954`](https://github.com/originchaindb/originchaindb-csharp/tree/45822b1d0954ad48a9a281ed51d24d63286f6616) | [Passed](https://github.com/originchaindb/originchaindb-csharp/actions/runs/41823633292) |
| PHP | `originchain/originchaindb` | Composer development branch | [`c1f568c25579`](https://github.com/originchaindb/originchaindb-php/tree/c1f568c25579d3fbabf297915d38f1a04b6315e1) | [Passed](https://github.com/originchaindb/originchaindb-php/actions/runs/41823661519) |
| Ruby | `originchaindb` | `1.0.0.rc.1` | [`6a99f36dda52`](https://github.com/originchaindb/originchaindb-ruby/tree/6a99f36dda5285924d5862fdda3c22d230b4f1db) | [Passed](https://github.com/originchaindb/originchaindb-ruby/actions/runs/41823678751) |

Python's import remains `originchain`; the TypeScript SDK also supports
JavaScript. Ruby uses the normalized gem version shown above. PHP has no
published candidate tag. Go's candidate label is not a published module tag;
use the commit-based installation in its README.

## Runtime coverage

The CI workflows linked above passed. This table lists tested configurations,
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
contract fixture and CI consumer. They also pass the shared **nine-request failure
contract**: expired/revoked responses, permission denial, rate limiting with
Retry-After, interrupted writes without replay, timeout, credential rotation,
observer isolation, and typed row conversion. Native tests cover cancellation
where the runtime supports it. CI checks all three fixtures against the
[pinned common contract](sdk-contract/v1/README.md).

All eight runnable catalog programs were also built or syntax-checked and
executed against an isolated integration database. They passed:

- Schema registration and row writes/read.
- SQL with bound parameters.
- Vector insertion and similarity search.
- Explicit full-text indexing and BM25 search.
- Graph neighbor traversal using the declared relation.

Each SDK README links to its complete example program and explains the setup,
result fields, and example commands.
The examples write data and require the corresponding database permissions.

## Bounded operations and build quality

All eight SDKs expose native lazy batch-write, page and row iterators. Batch
writes default to 1,000 rows / 1 MiB and report each acknowledged input range.
Failures preserve the cause and distinguish unsent from unknown outcomes;
unknown writes are never automatically replayed. Keyset reads default to 1,000
rows and require a unique immutable non-null string key with binary ordering.
They do not provide a snapshot across pages or a streaming JSON parser.

The common **21-request iteration contract** passes across the runtime matrix.
All eight complete bulk/read/resume programs also ran successfully on fresh
development namespaces; their inserted rows were removed after verification.
The READMEs link to setup, checkpoint, cancellation and recovery guides.

CI checks native public API signatures, installs each built artifact into a
fresh application, and measures connection reuse, concurrent requests, large
responses, latency and memory. Performance gates compare three-run medians with
reviewed Linux runner baselines. These measure SDK transport overhead, not
database performance. Deliberately removed APIs, extra latency and extra memory
were rejected by the corresponding guards.

Successful main builds carry GitHub OIDC/Sigstore provenance. Each downloaded
artifact was verified against its repository, exact source revision and CI
workflow; a tampered artifact was rejected. Build artifacts expire after 14 days
and are not registry releases. See any SDK's `docs/quality.md` for methodology,
thresholds, reproduction, verification commands and future registry installation.

## Isolated engine compatibility

All eight native SDK clients passed **14 real-engine checks each (112 total)**
on 4 November 2026. These cover newly provisioned hash tokens, invalid and expired
tokens, tenant isolation, enforcement of read-only and table permissions, rotation on an
existing client, revocation, valid-token recovery, bounded writes, keyset reads,
revocation and valid-token persistence after restart, and absence of denied writes.
The isolated engine shut down gracefully with no remaining test listeners.

The qualification records the exact engine revision, binary digest, SDK source
pins and runtime versions. The delivered SDKs retain the tested executable code;
subsequent changes affect documentation, build checks, CI, examples and a test clock tolerance.
It covers one dedicated engine using hash-token provisioning. This isolated run
did not change a production database or package registry. Separate live results
are described below, along with additional release and recovery checks.

## Live token lifecycle and row policies

All eight native SDKs passed **64 live checks** on 4 November 2026 against a
dedicated test instance: actual control-plane token issuance, exact row filtering
and column masking, ungranted-table and read-only write refusal, invalid-token
rejection, rotation on existing client objects, and deletion followed by a
confirmed database revocation receipt. Administrator reads established that the
hidden fixture rows existed and denied writes left no row. The temporary tokens,
linked identity, database user, role, grant, row policy, and tables were removed.

Activation, rotation and revocation required propagation; an accepted management
request did not mean every database target had applied it immediately. Each SDK
now documents the same activation and revocation-state checks in
`docs/authentication.md`. The SDK token provider supplies the current secret but
does not manage control-plane sessions or promise uninterrupted rotation.

This qualification covers canonical table grants and read-only tokens on the
tested deployment. It is not a general security certification, a statement that
all row-policy expressions are supported, or a guarantee for writes under RLS.
Package publication remains paused.

## Pinned engine revisions and recovery

All eight SDKs passed **120 additional checks** against three pinned engine
artifacts on 4 November 2026. Each SDK ran the same five checks on each artifact:
bound parameters, invalid-token refusal, three acknowledged write batches,
four keyset pages with exact row values, and a valid read on the existing client.

| Engine artifact | Revision | Result |
| --- | --- | --- |
| `rel25.2` | `5b6f62e2cb8e` | 40 checks passed |
| `rel25.1` | `f274503b0ca1` | 40 checks passed |
| Artifact promoted 25 October 2026 (`ci` build tag) | `ddf868579cf2` | 40 checks passed |

Artifact digests and the running binaries' reported revisions were verified.
This is SQL and row-operation compatibility on these exact artifacts, not a
supported version range. The control-plane token qualification above applies to
`rel25.2`; it is not inferred for the older artifacts.

Two isolated replication scenarios passed for every SDK on `rel25.2`:

- With a healthy follower, acknowledged writes survived a writer-process crash
  and manual follower promotion. The same client objects recovered and new
  writes were read back.
- With the follower stopped, each attempted write returned HTTP 503 once.
  After both processes were killed and the follower was promoted, every earlier
  acknowledged row remained and the unacknowledged test writes were absent.

The two scenarios preserved **160 acknowledged rows** across the eight clients.
They used separate engine processes and storage directories, TCP replication,
`fsync=always`, one required follower acknowledgement, and a fixed test proxy.
All nine test-engine processes exited; their storage and proxy listener were
removed. The existing database service was not restarted.

These checks exercise SDK behavior during isolated operator-driven recovery.
They do not certify automatic elections, multi-host cloud failover, regional
recovery, or a zero-loss guarantee for default asynchronous replication.

## Large datasets and sustained workload

All eight native SDKs passed a complete large-data and sustained-workload run
against the deployed test database on 4 November 2026:

- **1,000,000 rows:** 125,000 per SDK, with a deterministic key, integer value,
  and 256-byte payload. Each SDK wrote 125 batches of 1,000 rows and read its
  complete dataset through keyset pages of up to 10,000 rows. Every value,
  payload, row order, total count, and checksum matched.
- **Ten minutes at 24 requests/s:** all **14,400 requests** completed with exact
  responses, zero reported errors, and zero client-pool drops. Each SDK completed
  1,800 operations, including 90 writes. All **720 acknowledged writes** were
  verified against the stored ledgers.
- The preceding 8- and 24-request/s warm-ups also passed. All sixteen temporary
  tables and the dedicated test-client container were removed afterward.

The sustained mix was 95% point reads and 5% inserts into separate ledgers.
Clients ran on an existing Linux arm64 AWS runner, through the database's public
HTTPS endpoint. The client container was limited to two CPUs and 4 GiB of memory,
with four sequential client processes per SDK. Java used a 128 MiB maximum heap
per process. The serving database process, binary digest, and restart count
remained unchanged.

The following latency figures are from the original qualification run.
Observed native-call latency includes the SDK and network path; it excludes
the workload scheduler's queue and process IPC:

| SDK | p50 (ms) | p95 (ms) | p99 (ms) |
| --- | --- | --- | --- |
| Python | 4.38 | 6.44 | 6.89 |
| TypeScript / JavaScript | 4.16 | 6.55 | 6.93 |
| Go | 3.57 | 6.62 | 6.91 |
| Rust | 3.81 | 5.47 | 5.78 |
| Java | 5.22 | 7.46 | 10.23 |
| C# / .NET | 3.33 | 5.36 | 6.20 |
| PHP | 3.34 | 5.69 | 6.08 |
| Ruby | 3.95 | 6.68 | 7.27 |

Maximum sampled client-container memory was **1.55 GiB**. These samples
include runtime overhead, allocator retention, and container cache; they are not
a comparison of language memory efficiency.

Two earlier Windows runs also completed the million-row dataset stages. Across
the Windows and tunneled attempts, transport timeouts and client-pool drops
occurred; one response failure remains unclassified. Those failures are retained
in the qualification records with their causes unassigned. A small-data
50-request/s attempt completed 920 of 1,000 scheduled requests, with 80 client-pool
drops, and failed its offered-load target.

This is a bounded integration result at the stated workload and client location.
It does not establish server saturation capacity, a production latency objective,
or resolution of the earlier Windows-path failures. Package publication remains
paused.

## Repeatable qualification workflow

Maintainers can now repeat the full eight-SDK qualification through one command
on the existing private build pool. A successful follow-up on 4 November 2026
used the exact public source commits listed above and passed:

- **64 live token and scoped policy checks**, including rotation, confirmed
  revocation and removal of the temporary identities and policies.
- **120 compatibility checks** across the three pinned engine artifacts, plus
  both isolated manual-recovery scenarios described above.
- **1,000,000 exact dataset rows** and **14,400 of 14,400 soak requests** over
  ten minutes at 24 requests/s, with zero errors or client-pool drops. All
  720 soak writes matched their stored ledgers; both warm-ups also passed.

The job retained its two-CPU / 4-GiB limit; maximum sampled client memory was
**2.14 GiB**. All sixteen workload tables and the token fixtures were removed.
The serving database's process identity, binary digest and restart count stayed
unchanged. Failed trials are retained separately and are not counted as passes.

The workflow binds its receipt to the SDK commits, engine artifact digests,
runtime versions and evidence hashes. Incomplete coverage, mismatched pins,
unconfirmed revocation, dropped requests and failed cleanup block qualification.
Credentials use a one-use encrypted handoff and are excluded from retained
reports. The operator also checks the serving process before and after the run.

This workflow does **not** publish packages. Publication remains paused, and
broader production readiness still requires the engine security fixes to be
verified on an approved deployed build. The scoped policy tests above do not
replace that verification or resolve the historical Windows soak failures.

## Windows connection investigation

A follow-up on 4 November passed **81 valid-token read checks** from Windows:
15 direct HTTPS requests, 18 requests pinned across the resolved server
addresses, and 48 reads across all eight native SDKs. Every SDK also completed
a bulk-write/keyset-scan test; all **8,024 rows** and eight test tables were
verified removed.

A separate [clean Windows/Linux diagnostic](https://github.com/originchaindb/originchaindb-python/actions/runs/41867782773)
used deliberately invalid credentials. Both systems completed 12 of 15 requests
with HTTP 401, but all three 320-KB uploads were interrupted during sending.
A header-only control separately observed HTTP 401 with `Connection: close`
before sending the body. This explains an early authentication refusal path;
it does not identify the cause of the earlier valid-token soak timeouts. The
diagnostic remains recorded as a failed experiment, and no SDK runtime fix or
Windows endurance certification is claimed.

## Application integration

All eight SDKs include optional application-managed token providers, local request
observers, typed row conversion, and native HTTP configuration. Static-token
construction stays available. Method names and types follow each language's
conventions while sharing the same wire contract and failure-test scenarios.

Each repository has `docs/api/v1/README.md`, `docs/migration-v1.md`, and a complete
application integration example. All eight examples passed on the integration
database: a bound read-only SQL query, native HTTP configuration, a credential
provider, conversion into an application model, and a local request observer.
The .NET example uses `IHttpClientFactory` and dependency injection.

Observers expose only method, operation family, elapsed time, HTTP status,
request ID and error category. They do not include URLs, SQL, parameters,
credentials or response bodies, and they do not enable a telemetry exporter.

Each SDK also includes a manually dispatched `Live integration` workflow. These
workflows require the `sdk-integration` environment and its credentials; configure
a required reviewer and restrict deployments to `main` before adding secrets.
Their setup is documented in `docs/live-testing.md`. The evidence above comes from
local live execution; these GitHub environment workflows have not been enabled
or dispatched in this verification run.

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
- Live examples used an existing integration credential. The additional live
  qualification above separately verifies control-plane issuance, rotation,
  revocation, and the listed read-only row-policy scenarios.
- Live checks establish behavior on the integration database used for this run.
  They do not establish a supported server-version range or a production rollout.
  Availability still depends on server features and token permissions.

## Framework integrations and MCP

| Project | Scope | Compatibility status |
| --- | --- | --- |
| [LangChain](https://github.com/originchaindb/langchain-originchain) | Python vector-store adapter | `0.2.0rc1` unpublished source; rebuilt Python SDK `1.0.0rc1`; [verified commit](https://github.com/originchaindb/langchain-originchain/tree/a5a727985d6178b72d7c00f7f04f0d2730a12b8d); [CI](https://github.com/originchaindb/langchain-originchain/actions/runs/41744979600); proprietary license |
| [LlamaIndex](https://github.com/originchaindb/llama-index-vector-stores-originchain) | Python vector-store adapter | `0.2.0rc1` unpublished source; rebuilt Python SDK `1.0.0rc1`; [verified commit](https://github.com/originchaindb/llama-index-vector-stores-originchain/tree/eebe7433b5617353e010020013610dc6c3ef76eb); [CI](https://github.com/originchaindb/llama-index-vector-stores-originchain/actions/runs/41745009994); proprietary license |
| [MCP server](https://github.com/originchaindb/mcp-server) | SQL, vector search, full-text search, and schema discovery over stdio; Node 20+ | Separate project and verification; not included in the eight-SDK checks above; MIT license |

Follow each integration's own README for installation, permissions, and
limitations. This matrix does not certify additional language clients or mobile
platforms outside the configurations listed above.

Both adapter acceptance workflows pass four configurations: minimum framework on
Python 3.10, current framework on Python 3.12 and 3.14, and Python 3.12 on Windows.
The checks build wheel/sdist archives and repeat tests with installed wheels.
LangChain has 31 acceptance tests and LlamaIndex has 38. Both passed real-database
add/search/delete checks using supplied two-dimensional fixture embeddings; test
documents and vectors were verified absent afterward. Empty schema definitions
remain in their isolated integration namespaces. No external embedding service
was called. This is integration-instance evidence, not server-version certification.
