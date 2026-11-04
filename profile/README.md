# OriginChainDB

**SQL, vector search, full-text search, and graph relationships in one database.**

Build with the official OriginChainDB SDKs. Each SDK follows its language's
conventions and shares the same core operations, connection settings, and
examples.

[Website](https://originchaindb.com/) | [Documentation](https://originchaindb.com/docs/) | [SDK support](https://github.com/originchaindb/.github/blob/main/SDK-SUPPORT.md)

## Choose your SDK

Every README includes installation, a first query, error handling, and a complete
product catalog example covering SQL, rows, vectors, full-text search, and graph
relationships.

| Language | Installation and guide |
| --- | --- |
| Python | [Python SDK](https://github.com/originchaindb/originchaindb-python#installation) |
| TypeScript / JavaScript | [TypeScript SDK](https://github.com/originchaindb/originchaindb#installation) |
| Go | [Go SDK](https://github.com/originchaindb/originchaindb-go#installation) |
| Rust | [Rust SDK](https://github.com/originchaindb/originchaindb-rust#installation) |
| Java | [Java SDK](https://github.com/originchaindb/originchaindb-java#installation) |
| C# / .NET | [.NET SDK](https://github.com/originchaindb/originchaindb-csharp#installation) |
| PHP | [PHP SDK](https://github.com/originchaindb/originchaindb-php#installation) |
| Ruby | [Ruby SDK](https://github.com/originchaindb/originchaindb-ruby#installation) |

**Current source: 1.0 release candidates.** Install from source using your SDK's
README. These candidates have not been published to package registries; existing
registry packages can have a different API. Exact versions, verified commits,
and CI results are in the [support matrix](https://github.com/originchaindb/.github/blob/main/SDK-SUPPORT.md).

## Connect and run a query

1. Install the SDK for your language using its guide above.
2. Copy the complete HTTPS database address from **Connect** into `OC_URL` and
   your database access token into `OC_TOKEN`. Use the address exactly as supplied.
3. Run the first-query example in your SDK's README.

For example, after installing the Python SDK and setting those environment variables:

```python
from originchain import Client

with Client.from_env() as db:
    result = db.sql.run("SELECT $1 AS value", [7])
    print(result.rows)
```

This query needs no tables and returns a row with `value` equal to `7`.
Keep credentials in environment variables or your application's secret manager.

## Explore the examples

The catalog examples use the same small dataset and schema in all eight
languages. They create data, so run them against a development database with
permission to register schemas and read/write the example data.

| Operation | What the example demonstrates | Walkthrough |
| --- | --- | --- |
| SQL | Bind parameters and read query results | [SQL example](https://github.com/originchaindb/originchaindb-python#query-with-sql) |
| Vector search | Store an embedding and find similar records | [Vector example](https://github.com/originchaindb/originchaindb-python#search-by-vector-similarity) |
| Full-text search | Index product descriptions and retrieve BM25 matches | [FTS example](https://github.com/originchaindb/originchaindb-python#search-text) |
| Graph relationships | Define a relation and traverse neighboring products | [Graph example](https://github.com/originchaindb/originchaindb-python#follow-graph-relationships) |

These walkthrough links use Python; the SDK guides above contain the equivalent
examples in every language, including schema setup and expected result fields.

## Integrations and tools

| Integration | Repository |
| --- | --- |
| LangChain vector store | [langchain-originchain](https://github.com/originchaindb/langchain-originchain) |
| LlamaIndex vector store | [llama-index-vector-stores-originchain](https://github.com/originchaindb/llama-index-vector-stores-originchain) |
| MCP server for SQL, vector search, full-text search, and schema discovery | [mcp-server](https://github.com/originchaindb/mcp-server) |

The LangChain and LlamaIndex source candidates are verified with the rebuilt
Python SDK, including real-database add/search/delete checks. They retain their
proprietary licenses and have separate installation requirements. See the
[SDK support matrix](https://github.com/originchaindb/.github/blob/main/SDK-SUPPORT.md) for exact commits, CI coverage, and package
availability; follow each integration's README and license.

## Support and contributions

Use the relevant SDK repository's Issues tab for bugs and feature requests.
Include the SDK version, runtime, and a minimal reproduction with credentials
removed. Each repository includes contribution instructions, a changelog,
and a security policy for reporting vulnerabilities.
