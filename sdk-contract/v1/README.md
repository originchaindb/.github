# OriginChainDB SDK contract v1

This is the shared behavioral contract for the eight maintained language SDKs.
It is maintained alongside the organization SDK directory. It does not generate
client implementations or language-specific APIs.

- `sdk-contract.json`: common connection defaults and application integration behavior.
- `run.py`: 24 HTTP requests covering SQL, rows, schemas, vector, FTS, graph and failures.
- `resilience.py`: nine requests covering auth errors, permissions, rate limits,
  interrupted writes, timeout, credential rotation, local observers and row mapping.

SDK repositories pin a reviewed commit of this directory in CI. Their
`scripts/check_contract.py` verifies that their manifest and fixtures match this
reference. Native consumers execute the operations and assert the language's
types and error behavior; exact request counts reject skipped cases and retries.
No test credentials or live service access are required for these fixtures.

Change the shared contract here, review the effect on all eight implementations,
then update every repository's pin and native consumer together. New optional
features may extend API v1; incompatible wire behavior needs a new contract
version and migration guidance. A fixture pass is not live server certification.

## Bounded iteration and quality checks

`iteration.py` drives the same 21-request contract in every SDK: bounded JSON
batches, explicit unconfirmed ranges, no replay after a lost response, mutable
producer snapshots, bound keyset continuation, response validation and early stop.
The API uses each language's native iterator/stream and cancellation mechanisms.

`benchmark.py` measures a warm sequential connection, eight concurrent workers and
1 MiB query results over loopback. Each measurement uses three fresh processes.
It records latency distributions, throughput, peak process RSS and connection
counts. Transport gates always run. Timing/memory regression gates compare a
reviewed baseline from the same CI platform; the initial calibration is explicit.
PHP uses eight processes; Ruby uses eight clients in native threads.

`check_api.py` compares each SDK's compiler/reflection/AST surface with its reviewed
baseline. Removals and signature changes fail. Additions pass. This conservative
check complements behavioral contracts; it does not prove every semantic-versioning
case compatible. Baseline changes require maintainer review.

No fixture uses production data, real tokens, or a package publishing credential.
