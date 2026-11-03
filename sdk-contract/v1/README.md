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
