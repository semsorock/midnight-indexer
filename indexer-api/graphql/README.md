# Midnight Indexer MCP Server Guide

## 1. Prerequisite: Midnight Node

Before starting the MCP server, you must have a Midnight node running and available on your host machine at **localhost:9944**.

- You can easily run a Midnight node using the [midnight-node-docker](https://github.com/midnightntwrk/midnight-node-docker) GitHub repository.
- Follow the instructions in that repo to get a node up and running out of the box.

## 2. Starting the MCP Server

The MCP server is provided as a Docker service in this repository. It exposes a safe, read-only API for AI clients (like Cursor AI) to interact with the Midnight blockchain indexer.

**To start the MCP server and all dependencies:**

```sh
docker-compose up mcp-server
```

This will also start the required `indexer-api` and its dependencies.  
The MCP server will be available at:  
**http://localhost:5001/mcp**

---

## 3. Configuring MCP Server for Cursor AI

To connect Cursor AI to your local MCP server, use the provided `mcp.json` file in your project root:

```json
{
    "mcpServers": {
        "midnight-mcp": {
            "type": "streamable-http",
            "url": "http://localhost:5001/mcp",
            "note": "For Streamable HTTP connections, add this URL directly in your MCP Client"
        }
    }
}
```

- Make sure the URL matches your local MCP server endpoint.
- In Cursor AI, add this server as a new MCP connection using the above URL.

---

## 4. Available Tools (GraphQL Operations)

The MCP server exposes several tools (GraphQL operations) that you can use via Cursor AI or any compatible MCP client.  
Here are the main operations available:

### Queries

- **GetBlock**: Fetch a block by offset (height/hash).  
  _Returns block details, transactions, and contract actions._

- **GetTransactions**: Fetch transactions by offset (hash/identifier).  
  _Returns transaction details, block info, and contract actions._

- **GetUnshieldedUtxos**: Fetch unshielded UTXOs for a given address.  
  _Returns UTXO details, creation, and spending transactions._

- **GetContractActions**: Fetch contract actions for a given address.  
  _Returns contract call, deploy, or update actions and their state._

### Subscriptions

- **GetWalletSync**: Subscribe to wallet sync progress and updates for a session.  
  _Returns progress updates and relevant transaction/viewing updates._

---

## 5. How to Interact

- In Cursor AI, after connecting to the MCP server, you can use the above tools by their names (e.g., `GetBlock`, `GetTransactions`).
- Each tool accepts specific parameters (e.g., block offset, address, sessionId).
- You can run queries, fetch blockchain data, and subscribe to updates directly from your AI environment.

---

**For more details, see the `indexer-api/graphql/operations/` directory for the full GraphQL queries and parameters.** 
