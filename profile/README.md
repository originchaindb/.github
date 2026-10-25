# OriginChainDB

Official SDKs and developer tools for **OriginChainDB**: a database for SQL,
vector search, full-text search, graph queries, and AI applications.

[Website](https://originchaindb.com/) · [Documentation](https://originchaindb.com/docs/) ·
[SDK guide](https://originchaindb.com/docs/sdk/)

## SDKs

The default branches contain unreleased SDK updates for SQL, full-text search,
vector search and graph operations. Follow each repository's README for current
source-install instructions. TypeScript also supports JavaScript. New package
releases are paused; the preview links below refer to earlier releases.

| Language | Source repository | Earlier GitHub preview |
| --- | --- | --- |
| TypeScript / JavaScript | [originchaindb](https://github.com/originchaindb/originchaindb) | [v0.7.0-rc.6](https://github.com/originchaindb/originchaindb/releases/tag/v0.7.0-rc.6) |
| Python | [originchaindb-python](https://github.com/originchaindb/originchaindb-python) | Development source (`0.11.0.dev0`) |
| Go | [originchaindb-go](https://github.com/originchaindb/originchaindb-go) | [v0.3.0-rc.1](https://github.com/originchaindb/originchaindb-go/releases/tag/v0.3.0-rc.1) |
| Java | [originchaindb-java](https://github.com/originchaindb/originchaindb-java) | [v0.3.0-rc.1](https://github.com/originchaindb/originchaindb-java/releases/tag/v0.3.0-rc.1) |
| C# / .NET | [originchaindb-csharp](https://github.com/originchaindb/originchaindb-csharp) | [v0.3.0-rc.1](https://github.com/originchaindb/originchaindb-csharp/releases/tag/v0.3.0-rc.1) |
| Ruby | [originchaindb-ruby](https://github.com/originchaindb/originchaindb-ruby) | [v0.4.0-rc.1](https://github.com/originchaindb/originchaindb-ruby/releases/tag/v0.4.0-rc.1) |
| PHP | [originchaindb-php](https://github.com/originchaindb/originchaindb-php) | [v0.3.0-rc.1](https://github.com/originchaindb/originchaindb-php/releases/tag/v0.3.0-rc.1) |
| Rust | [originchaindb-rust](https://github.com/originchaindb/originchaindb-rust) | [v0.3.0-rc.1](https://github.com/originchaindb/originchaindb-rust/releases/tag/v0.3.0-rc.1) |

Preview release pages include archive installation instructions, checksums and
verification results. They can differ from current development source. An
unqualified package-manager install may select an older stable version; a GitHub
preview does not imply publication on a language's package registry.

Python development continues in `originchaindb-python`. Its distribution and
import name remain `originchain`; existing PyPI packages predate the replacement
repository. Follow the new repository's README to install from source.

The Go, Java, C#/.NET, Ruby, PHP, and Rust clients are generated from the API
specification. Their API shapes differ from the maintained TypeScript and Python
clients; use each repository's own examples.

## Framework integrations

Use LangChain or LlamaIndex to store and retrieve documents through the Python
SDK. **These two adapters are proprietary.** Their READMEs cover supported
operations, client ownership and handling partial failures.

| Framework | Source | GitHub preview |
| --- | --- | --- |
| LangChain | [langchain-originchain](https://github.com/originchaindb/langchain-originchain) | [v0.1.0-rc.1](https://github.com/originchaindb/langchain-originchain/releases/tag/v0.1.0-rc.1) |
| LlamaIndex | [llama-index-vector-stores-originchain](https://github.com/originchaindb/llama-index-vector-stores-originchain) | [v0.1.0-rc.1](https://github.com/originchaindb/llama-index-vector-stores-originchain/releases/tag/v0.1.0-rc.1) |

Install the wheels from the linked GitHub releases; these adapters are not on PyPI.

## MCP server

Connect an MCP host over stdio using the [OriginChainDB MCP server](https://github.com/originchaindb/mcp-server).
The current source provides SQL, vector search, full-text search and schema
discovery tools, with request deadlines, cancellation and database-user
authentication. Its README covers source installation, configuration and
permissions. The earlier [v0.2.0-rc.1 preview](https://github.com/originchaindb/mcp-server/releases/tag/v0.2.0-rc.1) remains available separately.

## Get started

1. Read the [quickstart](https://originchaindb.com/docs/quickstart/).
2. Choose an SDK and follow its repository's installation instructions.
3. Use your tenant endpoint and keep database credentials in your secret store.
4. Report SDK problems in the corresponding repository's Issues tab, including
   the SDK version and a minimal reproduction without credentials.
