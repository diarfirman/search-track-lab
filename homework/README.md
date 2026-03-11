# Elastic Agent Builder Lab Guide
### Build AI Agents on Elastic Serverless Using Synthetic Financial Data

![Elastic](https://img.shields.io/badge/Elastic-Serverless-00BFB3?style=flat&logo=elastic)
![Level](https://img.shields.io/badge/Level-Intermediate-yellow)
![Duration](https://img.shields.io/badge/Duration-~2%20Hours-blue)

> **Note:** This lab guide uses Elastic Serverless (Elasticsearch Serverless project) with a 14-day free trial. No prior Elastic experience required, but familiarity with REST APIs is helpful.

---

## Overview & Learning Objectives

This hands-on lab guide walks you through building AI-powered agents with Elastic Agent Builder on the Elastic Serverless platform. Using a realistic synthetic financial dataset, you will progress from setting up a trial environment to deploying fully capable custom agents with multiple tool types.

### What You Will Build

- A financial data analysis agent with natural language query capabilities
- Custom ES|QL tools for precise, parameterized financial queries
- An Index Search tool for semantic exploration of financial data
- An MCP tool integration using the free Excalidraw MCP server
- Programmatic access to your agents via the Kibana API

### Lab Modules

| Module | Title | Description |
|--------|-------|-------------|
| Lab 1 | Environment Setup | Create an Elastic Serverless trial and configure your Elasticsearch project |
| Lab 2 | Enable Agent Builder | Activate the Agent Builder feature and explore the default AI agent |
| Lab 3 | Ingest Financial Data | Load synthetic financial data using Kibana Dev Tools |
| Lab 4 | Build Custom Tools | Create ES\|QL and Index Search tools for financial data analysis |
| Lab 5 | Configure MCP Tools | Integrate the free Excalidraw MCP server for diagram generation |
| Lab 6 | Create a Custom Agent | Build and configure a Financial Analyst agent with all your tools |
| Lab 7 | Programmatic Access | Access your agent via the Kibana REST API |

### Prerequisites

- A modern web browser (Chrome, Firefox, or Edge recommended)
- An email address for account registration
- No prior Elastic Cloud experience required

---

## Lab 1 — Environment Setup
### Create your Elastic Serverless Trial Project

> **Tip:** The Elasticsearch Serverless project type is ideal for this lab because it includes the Agent Builder feature enabled by default with the Complete tier, and no cluster management is required.

### Step 1.1 — Sign Up for Elastic Cloud Trial

Navigate to the Elastic Cloud registration page and create your free 14-day trial account.

<img width="800" alt="image" src="https://github.com/user-attachments/assets/bbb8d092-3ffe-4c9e-9621-9d5d97bb0640" />

1. Go to [Elastic Serverless Trial](http://cloud.elastic.co/serverless-registration) and click **Start free trial**
2. Enter your email address and create a password, then verify your email.
3. Once logged in, you will see the Elastic Cloud home page. Click **Create project**.

### Step 1.2 — Create a Serverless Elasticsearch Project

<img width="800" alt="image" src="https://github.com/user-attachments/assets/82b13aff-8df7-4628-9b90-0bccacc671dc" />

1. Select the **Elasticsearch** project type from the available options.
2. Enter a project name, for example: `Financial-AI-Lab`
3. Select your preferred cloud provider and region (choose the closest to you for best performance).
4. Click **Create project** and wait for provisioning to complete (typically 1–2 minutes).

### Step 1.3 — Save Your Connection Details

<img width="800" alt="image" src="https://github.com/user-attachments/assets/2efc0e99-fb18-4de3-86d0-9f75b292cb46" />


Once your project is ready, save the following details — you will need them throughout the lab:

| Item | Where to find it |
|------|-----------------|
| Elasticsearch URL | Project Overview → Endpoints → Elasticsearch URL |
| Kibana URL | Project Overview → Endpoints → Kibana URL |
| API Key | Security → API Keys → Create API Key |

### Step 1.4 — Generate an API Key

<img width="800" alt="image" src="https://github.com/user-attachments/assets/80880139-9dee-4444-bc2c-6348e6eafa05" />

> **Important:** The API Key is only displayed once at creation time. Store it in a safe place before closing the dialog. You will need this key in Lab 3 when running the data generator.

1. From the Kibana left navigation, click on **Stack Management**.
2. Navigate to **Security → API Keys**.
3. Click **Create API key**. Give it the name: `lab-api-key`
4. Copy and save the API key value — it will only be shown once.

---

## Lab 2 — Enable Agent Builder
### Activate AI Features and Meet the Default Agent

> **Note:** The built-in Elastic AI Agent uses an Elastic Managed LLM by default — no external API keys are required for the trial. This is included at no extra cost during your 14-day trial period.

### Step 2.1 — Navigate to Agent Builder

<img width="800" alt="image" src="https://github.com/user-attachments/assets/8122db69-cbb6-4c9a-b31c-9eed26d58a4b" />

1. Log in to your Kibana instance using the URL saved in Lab 1.
2. In the left navigation panel, search for or scroll to find **Agents** under the AI-powered features section.
3. Alternatively, use the global search (the magnifying glass icon) and type `Agents`.

### Step 2.2 — Explore the Default Agent

<img width="800" alt="image" src="https://github.com/user-attachments/assets/e496e86c-d734-499e-a259-e5ab688fbbc4" />
<img width="800" alt="image" src="https://github.com/user-attachments/assets/e37be978-42be-45cf-b581-0edc6a253423" />

You will see the Agent Builder landing page. The default Elastic AI Agent is pre-configured and ready to use.

1. Click on **More** on the to right of the page and click **Agents** tab to see the list of available agents.
2. Currently you only have **Elastic AI Agent**.
3. To see the configured tools, you can click on **Manage Tools** — built-in agent has access to all built-in Elastic tools.

<img width="800" alt="image" src="https://github.com/user-attachments/assets/5e6eeef6-5e8b-4ba9-9092-2deca944ed3b" />

> **Tip:** Before adding your own data, the agent can only respond with its built-in knowledge and tools. In the next lab, you will load financial data so the agent can provide meaningful domain-specific answers.

### Step 2.3 — Start a Conversation

<img width="800" alt="image" src="https://github.com/user-attachments/assets/2d1654d3-e35b-4d5e-a471-28f28c792f14" />

1. Click the chat icon or the **Open chat** button next to the Elastic AI Agent.
2. Try this first query to test the agent's capabilities without any custom data:
<img width="700" alt="image" src="https://github.com/user-attachments/assets/19d149af-df31-4652-b8b0-d7c01caa5145" />

```
What indices are available in this Elasticsearch cluster?
```

3. The agent will use its built-in tools to inspect your cluster and respond.
4. Try a follow-up question:

```
What are the built-in tools available to you?
```
<img width="700" alt="image" src="https://github.com/user-attachments/assets/08247e71-fe97-4ed8-89a9-3f439d854b37" />

---

## Lab 3 — Ingest Financial Data
### Load Sample Financial Data via Kibana Dev Tools

In this lab you will load three financial indices directly into Elasticsearch using Kibana Dev Tools — no Python, no terminal, no external tools required. All commands are ready to copy and paste.

The dataset covers six fictional customer accounts trading 17 real-world tickers across five sectors over January 2025.

| Index | Docs | Contents |
|-------|------|----------|
| `financial-transactions` | 20 | Buy/sell trades — ticker, price, quantity, broker, status |
| `portfolio-snapshots` | 18 | Account holdings — market value, unrealized P&L, risk profile |
| `market-events` | 15 | Earnings, analyst upgrades, FDA approvals, macro events |

> **Note:** Open Kibana → Management → Dev Tools to access the console. Run each step in order. Steps 1, 3, and 5 create the index mapping — run them before the bulk insert. If you re-run a bulk insert, documents with the same `_id` are overwritten (safe to repeat).

---

### Section A — `financial-transactions`

#### Step 1 — Create the index mapping

Run this first to define field types. The `timestamp` field must be mapped as `date` so ES|QL date filters work correctly.

```json
PUT financial-transactions
{
  "mappings": {
    "properties": {
      "transaction_id":   { "type": "keyword" },
      "timestamp":        { "type": "date" },
      "account_id":       { "type": "keyword" },
      "customer_name":    { "type": "text",    "fields": { "keyword": { "type": "keyword" } } },
      "ticker":           { "type": "keyword" },
      "company_name":     { "type": "text",    "fields": { "keyword": { "type": "keyword" } } },
      "sector":           { "type": "keyword" },
      "transaction_type": { "type": "keyword" },
      "quantity":         { "type": "integer" },
      "price":            { "type": "float" },
      "total_value":      { "type": "float" },
      "currency":         { "type": "keyword" },
      "broker":           { "type": "keyword" },
      "status":           { "type": "keyword" },
      "notes":            { "type": "text" }
    }
  },
  "settings": {
    "number_of_shards": 1,
    "number_of_replicas": 0
  }
}
```

> ✅ **Expected Result:** `{ "acknowledged": true, "shards_acknowledged": true, "index": "financial-transactions" }`

#### Step 2 — Bulk insert 20 transaction records

Each line is an action object concatenated immediately with its document — no space or newline between them.

See [`data/financial-transactions-bulk.ndjson`](data/financial-transactions-bulk.ndjson) for the full bulk insert payload, or copy the command below into Kibana Dev Tools:

```
POST /financial-transactions/_bulk
```

> ✅ **Expected Result:** `errors: false` with 20 items, each showing `result: "created"`.

---

### Section B — `portfolio-snapshots`

#### Step 3 — Create the index mapping

```json
PUT portfolio-snapshots
{
  "mappings": {
    "properties": {
      "snapshot_id":      { "type": "keyword" },
      "snapshot_date":    { "type": "date" },
      "account_id":       { "type": "keyword" },
      "customer_name":    { "type": "text", "fields": { "keyword": { "type": "keyword" } } },
      "risk_profile":     { "type": "keyword" },
      "ticker":           { "type": "keyword" },
      "company_name":     { "type": "text" },
      "sector":           { "type": "keyword" },
      "shares_held":      { "type": "integer" },
      "avg_cost_basis":   { "type": "float" },
      "current_price":    { "type": "float" },
      "market_value":     { "type": "float" },
      "unrealized_pnl":   { "type": "float" },
      "pnl_pct":          { "type": "float" },
      "currency":         { "type": "keyword" }
    }
  },
  "settings": { "number_of_shards": 1, "number_of_replicas": 0 }
}
```

> ✅ **Expected Result:** `{ "acknowledged": true, "index": "portfolio-snapshots" }`

#### Step 4 — Bulk insert 18 portfolio snapshot records

All 18 snapshots share `snapshot_date: 2025-01-17` — a single point-in-time view of each account's holdings.

See [`data/portfolio-snapshots-bulk.ndjson`](data/portfolio-snapshots-bulk.ndjson) for the full bulk insert payload.

> ✅ **Expected Result:** `errors: false` with 18 items.

---

### Section C — `market-events`

#### Step 5 — Create the index mapping

```json
PUT market-events
{
  "mappings": {
    "properties": {
      "event_id":           { "type": "keyword" },
      "event_date":         { "type": "date" },
      "ticker":             { "type": "keyword" },
      "company_name":       { "type": "text", "fields": { "keyword": { "type": "keyword" } } },
      "sector":             { "type": "keyword" },
      "event_type":         { "type": "keyword" },
      "headline":           { "type": "text" },
      "impact":             { "type": "keyword" },
      "price_before":       { "type": "float" },
      "price_after":        { "type": "float" },
      "price_change_pct":   { "type": "float" },
      "analyst_rating":     { "type": "keyword" },
      "source":             { "type": "keyword" }
    }
  },
  "settings": { "number_of_shards": 1, "number_of_replicas": 0 }
}
```

> ✅ **Expected Result:** `{ "acknowledged": true, "index": "market-events" }`

#### Step 6 — Bulk insert 15 market event records

Events include earnings beats and misses, analyst upgrades and downgrades, product announcements, an FDA approval, and a macro energy event.

See [`data/market-events-bulk.ndjson`](data/market-events-bulk.ndjson) for the full bulk insert payload.

> ✅ **Expected Result:** `errors: false` with 15 items.

---

### Section D — Verify Data Ingestion

#### Step 7 — Confirm document counts

Run this single command to confirm all three indices are present with the expected document counts.

```
GET _cat/indices/financial-transactions,portfolio-snapshots,market-events?v&h=index,docs.count,store.size
```

> ✅ **Expected Result:**
> ```
> index                    docs.count  store.size
> financial-transactions   20          ~80kb
> portfolio-snapshots      18          ~70kb
> market-events            15          ~60kb
> ```

---

### Dataset Reference — Accounts & Tickers

> **Important:** All data is synthetic and generated for workshop purposes only. Prices, P&L values, and news headlines do not reflect actual market activity.

| Account ID | Customer | Risk Profile | Key Holdings |
|------------|----------|--------------|--------------|
| ACC-001 | Alice Nguyen | aggressive | AAPL, NVDA, GOOGL, LLY |
| ACC-002 | Brian Okafor | moderate | MSFT, META, TSLA, AMZN |
| ACC-003 | Carmen Lopez | conservative | JPM, XOM, AAPL, JNJ |
| ACC-004 | David Kim | aggressive | TSLA, BRK.B, AMD |
| ACC-005 | Elena Rossi | conservative | JNJ, PFE, DIS, V |
| ACC-006 | Frank Zhang | moderate | V, CVX, MSFT |

---

## Lab 4 — Build Custom Tools
### Create ES|QL and Index Search Tools

Custom tools are the heart of a useful AI agent. In this lab, you will create two types of custom tools: an ES|QL tool for precise parameterized queries, and an Index Search tool for semantic exploration of your financial data.

### Tool Types Reference

| Type | Best For | Query Control |
|------|----------|---------------|
| **ES\|QL Tool** | Precise, repeatable analytics with known schema | Pre-defined query with parameters — you control the query |
| **Index Search Tool** | Open-ended semantic search and exploration | LLM constructs the query dynamically |
| **MCP Tool** | External service integration (covered in Lab 5) | Delegated to external MCP server |
| **Workflow Tool** | Triggering pre-built Kibana workflows | Delegates to Kibana Workflow engine |

> **Tip:** ES|QL tools are ideal for precise analytics where you know the query structure in advance. Index Search tools are better for open-ended questions where the LLM needs flexibility. Using both types together gives your agent maximum capability.

### Step 4.1 — Create an ES|QL Tool: Get Transactions by Ticker

This tool will allow the agent to look up financial transactions for a specific stock ticker symbol.

1. In Kibana, navigate to **Agents** and click the **Tools** tab.
2. Click **New tool** and fill in the following fields:

| Field | Value |
|-------|-------|
| Tool ID | `finance.get_transactions_by_ticker` |
| Name | Get Transactions by Ticker |
| Type | ES\|QL |
| Description | Retrieves recent financial transactions for a specific stock ticker symbol. Use this when the user asks about trading activity, transaction history, or buy/sell orders for a specific stock. Parameter: ticker (required) — the stock symbol e.g. AAPL, GOOG. |


3. Enter the following ES|QL query:

```esql
FROM financial-transactions
| WHERE ticker == ?ticker
| SORT timestamp DESC
| LIMIT 20
| KEEP timestamp, ticker, transaction_type, quantity, price, total_value
```

4. Add a parameter by clicking **Add parameter**:

| Parameter Name | Type | Description | Required |
|----------------|------|-------------|----------|
| `ticker` | string | Stock ticker symbol (e.g., AAPL, GOOG, MSFT) | Yes |

<img width="800" alt="image" src="https://github.com/user-attachments/assets/a6e35cf5-0257-4bad-9fec-390260bb07a9" />


5. Click **Save and test**. Enter `AAPL` as the test ticker — you should see 3 results (TXN-001, TXN-009, TXN-015 from Alice and Carmen).

### Step 4.2 — Create an ES|QL Tool: Portfolio Summary

This tool provides portfolio performance summaries aggregated by sector.

1. Click **New tool** and configure:

| Field | Value |
|-------|-------|
| Tool ID | `finance.get_portfolio_summary` |
| Name | Get Portfolio Summary by Account |
| Type | ES\|QL |
| Description | Retrieves a portfolio summary for a specific account, showing holdings grouped by sector with total value. Use this when the user asks about portfolio composition, asset allocation, or account holdings. Parameter: account_id (required). |

2. Enter the following ES|QL query:

```esql
FROM portfolio-snapshots
| WHERE account_id == ?account_id
| STATS total_value = SUM(market_value), holdings_count = COUNT(*)
        BY sector
| SORT total_value DESC
```

3. Add a parameter for `account_id` (string, required) and the description.
4. Save and test with `account_id = ACC-001` (Alice Nguyen). You should see sectors: Technology, Healthcare.
   <img width="700" alt="image" src="https://github.com/user-attachments/assets/648c31e2-205e-4fc9-b7c7-a320c0076165" />


### Step 4.3 — Create an Index Search Tool: Financial Data Explorer

This tool gives the agent the ability to semantically search across all financial data.

1. Click **New tool** and configure:

| Field | Value |
|-------|-------|
| Tool ID | `finance.search_financial_data` |
| Name | Search Financial Data |
| Type | Index Search |
| Index Pattern | `financial-transactions,portfolio-snapshots,market-events` |
| Description | Performs a semantic search across all financial data including transactions, portfolio data, and market events. Use this for open-ended exploratory questions about financial data when you don't have a specific account or ticker in mind. The LLM will construct the appropriate query based on the user's request. |

2. Click **Save**. You do not need to add parameters — the LLM dynamically builds the query.

---

## Lab 5 — Configure MCP Tools
### Integrate Excalidraw for Free Diagram Generation

Model Context Protocol (MCP) tools allow your agent to call external services. In this lab, you will integrate the free Excalidraw MCP server, enabling your financial agent to generate diagrams and visual representations of data on demand.

### About Excalidraw MCP

- Excalidraw is a free, open-source virtual whiteboard tool
- The Excalidraw MCP server (`mcp.excalidraw.com`) is publicly accessible and free to use
- It allows AI agents to create and manipulate diagrams programmatically
- Your financial agent can use it to visualize portfolio allocations, create flowcharts, and draw data models

### Step 5.1 — Create an MCP Connector

First, you need to create an MCP connector in Kibana that points to the Excalidraw MCP server.

<img width="800" alt="image" src="https://github.com/user-attachments/assets/0529e04c-eeb8-473d-9a49-b3f92cf0beac" />


1. In Kibana, navigate to **Stack Management → Connectors**.
2. Click **Create connector** and search for **MCP** in the connector type list.
3. Configure the connector with the following settings:

| Field | Value |
|-------|-------|
| Connector Name | Excalidraw MCP |
| MCP Server URL | `https://mcp.excalidraw.com` |
| Authentication | None (public server) |

<img width="700" alt="image" src="https://github.com/user-attachments/assets/b846edca-5166-4c34-90fd-c6f3ac5f90b9" />


4. Click **Save and Test** and verify the connector shows a green status indicator.

   <img width="700" alt="image" src="https://github.com/user-attachments/assets/4c030340-23ba-4de9-a095-9d6a1a1a64d6" />


> **Note:** The Excalidraw MCP server provides tools for creating diagrams, adding shapes, text, and arrows, and exporting diagrams in various formats. These tools give your agent powerful visual output capabilities.

### Step 5.2 — Import Excalidraw MCP Tools

Now you will bulk-import the available tools from the Excalidraw MCP server.

<img width="800" alt="image" src="https://github.com/user-attachments/assets/23c51759-1c75-499d-a847-2f3b8d0db3dc" />

1. Navigate back to the **Agents** page and click the **Tools** tab.
2. Click the **Manage MCP** dropdown and select **Bulk import MCP tools**.
3. In the **MCP Server** field, select the Excalidraw MCP connector you just created.
4. Wait for the tool list to populate, then select all available tools.
5. Set the namespace to `excalidraw` (the tool IDs will be generated as `excalidraw.tool-name`).
   <img width="800" alt="image" src="https://github.com/user-attachments/assets/86901de5-74a1-48b5-bd46-4fa6faa29841" />
7. Click **Import tools**.

### Step 5.3 — Verify MCP Tool Health

1. After importing, check the Tools list for the `excalidraw.*` tools.
2. All imported tools should display without warning icons.
3. If a tool shows an error icon, verify the connector URL is correct and that the MCP server is reachable.


---

## Lab 6 — Create a Custom Agent
### Build a Financial Analyst Agent

Now that you have created tools, you will build a custom Financial Analyst agent that combines all your tools with tailored instructions. A well-configured agent provides focused, accurate responses by constraining its behavior through custom system prompts.

### Step 6.1 — Create the Custom Agent

1. Navigate to **Agents → Agents** tab.
2. Click **New agent**.
3. Fill in the basic information:

| Field | Value |
|-------|-------|
| Agent ID | financial_analyst_agent |
| Display Name | Financial Analyst Agent |
| Description | An AI agent specialized in analyzing synthetic financial data including transactions, portfolio holdings, and market events. |

### Step 6.2 — Write the System Prompt

The system prompt (custom instructions) shapes how the agent behaves and responds. Here is a recommended system prompt for the Financial Analyst Agent:

```
You are a Financial Analyst AI with expertise in portfolio management and market analysis.

Your primary data sources are synthetic financial datasets stored in Elasticsearch:
- financial-transactions: individual buy/sell trade records
- portfolio-snapshots: account holdings grouped by sector
- market-events: price movements and market announcements

Guidelines:
1. Always clarify the time period or account when relevant
2. Use the ES|QL tools for precise lookups with known parameters
3. Use the Index Search tool for exploratory or open-ended analysis
4. Use Excalidraw tools to create visual diagrams when explaining
   portfolio composition or data relationships
5. Present numerical data in a clear, structured format
6. Always note that this is SYNTHETIC data for demonstration purposes
```
<img width="800" alt="image" src="https://github.com/user-attachments/assets/c11e94e2-17ec-4b3b-9c53-adc5305bfd3c" />


### Step 6.3 — Assign Tools to the Agent

> **Note:** Limiting the number of tools to only what is relevant improves agent accuracy. An agent with too many tools may become confused about which tool to use for a given query.

1. Click the **Tools** tab in the agent editor.
2. Add the following tools to this agent:
   - `finance.get_transactions_by_ticker`
   - `finance.get_portfolio_summary`
   - `finance.search_financial_data`
   - All `excalidraw.*` tools from Lab 5
     
  <img width="800" alt="image" src="https://github.com/user-attachments/assets/92fa9e90-4a78-473e-948a-b2f77237ef78" />

3. Click **Save agent**.
   

### Step 6.4 — Test Your Custom Agent

> **Tip:** If the agent uses the wrong tool for a query, improve your tool descriptions. The description is how the LLM decides which tool to call — make them precise and distinct from each other.

Open the chat interface for your new Financial Analyst Agent and try these test prompts:

**Test the ES|QL tool — Get Transactions by Ticker:**
```
Show me all AAPL transactions
```

**Test the ES|QL tool — Portfolio Summary:**
```
Give me a portfolio summary for account ACC-004
```

**Test the Index Search tool with a cross-index question:**
```
Which Technology stocks had both market events and buy transactions in January 2025?
```

**Test multi-tool reasoning:**
```
Look up NVDA transactions for Alice Nguyen, then check what market events happened for NVDA, and summarise the situation
```

**Test the Excalidraw MCP tool:**
```
Create a diagram showing all 6 customer accounts grouped by risk profile and return the shareable link
```
<img width="700" alt="image" src="https://github.com/user-attachments/assets/e5f150fc-11fc-4087-93ec-d79a192dc4f6" />

Click on the link, you will be redirect to excalidraw page.

<img width="800" alt="image" src="https://github.com/user-attachments/assets/f2aedc4b-1142-4a73-bd26-3914bb205d82" />

---

## Lab 7 — Programmatic Access
### Call Your Agent via the Kibana REST API

Elastic Agent Builder exposes programmatic interfaces that allow you to integrate your agents into applications, automate workflows, and build custom frontends. In this lab, you will use the Kibana REST API to interact with your Financial Analyst Agent programmatically.

### Available Interfaces

| Interface | Use Case | Best For |
|-----------|----------|----------|
| **Kibana REST API** | Manage agents, tools, and conversations via HTTP | Backend integrations, automation scripts |
| **MCP Server** | Expose your agent as an MCP server for external clients | Claude Desktop, Cursor, custom MCP clients |
| **A2A Server** | Agent-to-agent communication via A2A protocol | Multi-agent orchestration, agent chaining |

> **Note:** For production use, consider using the MCP Server interface if you want to connect external MCP-compatible clients like Claude Desktop or Cursor directly to your Elastic Agent Builder agents.

> **Tip:** The MCP Server and A2A Server features were in Preview as of early 2026. Check the Elastic documentation for the latest status and capabilities of these interfaces.

### Step 7.1 — Find Your Agent ID

1. In Kibana, navigate to **Agents** and open your Financial Analyst Agent.
2. Copy the agent ID from the URL bar or from the agent details panel. It will look similar to: `agent_abcdef123456`

### Step 7.2 — Start a Conversation via the API

The Kibana REST API allows you to create a new conversation and send messages to any agent.

1. Open the Kibana Dev Tools console (or use curl from your terminal).
2. Create a new conversation:

```json
POST kbn:/api/chat/conversations
{
  "agentId": "your-agent-id-here",
  "title": "API Test Conversation"
}
```

3. Note the `conversation ID` from the response.

4. Send a message to the conversation:

```json
POST kbn:/api/chat/conversations/{conversationId}/messages
{
  "content": "Show me the last 5 transactions for AAPL"
}
```

5. Review the response — you should receive the agent's reply including the results from the ES|QL tool.

### Step 7.3 — Test with curl (Optional)

You can also call the API from outside Kibana using curl and your API key:

```bash
curl -X POST 'https://your-kibana-url/api/chat/conversations' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: ApiKey your-api-key' \
  -d '{"agentId": "your-agent-id", "title": "External Test"}'
```

### Step 7.4 — Explore MCP Server Access

Your Elastic Agent Builder can act as an MCP server, exposing your agents and tools to any MCP-compatible client.

1. Navigate to **Agents → Programmatic access → MCP server**.
2. Follow the instructions to get your MCP server URL and authentication details.
3. You can connect this MCP server URL to any compatible client, such as Claude Desktop, by adding it to your MCP client configuration.

---

## Appendix: ES|QL Query Reference & Troubleshooting

### A. ES|QL Query Reference for the Sample Dataset

All queries below target the three indices loaded in Lab 3 and can be tested directly in Kibana Dev Tools (Management → Dev Tools) using the `POST /_query` endpoint.

#### Group 1 — Transaction Analysis

**Q1 — Latest trades for a specific ticker**
```esql
POST /_query
{
  "query": """
    FROM financial-transactions
      | WHERE ticker == "AAPL"
      | SORT timestamp DESC
      | LIMIT 10
  """
}
```

**Q2 — Total buy volume by sector**
```esql
POST /_query
{
  "query": """
    FROM financial-transactions
      | WHERE transaction_type == "buy"
      | STATS total_bought = SUM(total_value) BY sector
      | SORT total_bought DESC
  """
}
```

**Q3 — Net flow per customer (CASE inside STATS)**
```esql
POST /_query
{
  "query": """
    FROM financial-transactions
      | STATS
          buy_vol  = SUM(CASE(transaction_type == "buy",  total_value, 0)),
          sell_vol = SUM(CASE(transaction_type == "sell", total_value, 0)),
          net      = SUM(CASE(transaction_type == "buy",  total_value, -total_value))
        BY customer_name
      | SORT net DESC
  """
}
```

**Q4 — Activity in the last 30 days**
```esql
POST /_query
{
  "query": """
    FROM financial-transactions
      | WHERE timestamp >= NOW() - 30 days
      | STATS trades = COUNT(*), total_value = SUM(total_value)
          BY customer_name, transaction_type
      | SORT customer_name ASC
  """
}
```

**Q5 — Trade volume by broker**
```esql
POST /_query
{
  "query": """
    FROM financial-transactions
      | STATS trade_count = COUNT(*), total_value = SUM(total_value) BY broker
      | SORT total_value DESC
  """
}
```

#### Group 2 — Portfolio Analysis

**Q6 — Portfolio summary by account**
```esql
POST /_query
{
  "query": """
    FROM portfolio-snapshots
      | STATS
          total_market_value = SUM(market_value),
          positions          = COUNT(*),
          total_pnl          = SUM(unrealized_pnl)
        BY account_id, customer_name
      | SORT total_market_value DESC
  """
}
```

**Q7 — Top gaining positions**
```esql
POST /_query
{
  "query": """
    FROM portfolio-snapshots
      | WHERE pnl_pct > 0
      | SORT pnl_pct DESC
      | KEEP ticker, customer_name, shares_held,
             avg_cost_basis, current_price, unrealized_pnl, pnl_pct
      | LIMIT 10
  """
}
```

**Q8 — Sector exposure breakdown**
```esql
POST /_query
{
  "query": """
    FROM portfolio-snapshots
      | STATS
          sector_value = SUM(market_value),
          positions    = COUNT(*),
          avg_return   = AVG(pnl_pct)
        BY sector
      | SORT sector_value DESC
  """
}
```

**Q9 — All holdings for one account**
```esql
POST /_query
{
  "query": """
    FROM portfolio-snapshots
      | WHERE account_id == "ACC-001"
      | KEEP ticker, company_name, sector,
             shares_held, market_value, unrealized_pnl, pnl_pct
      | SORT pnl_pct DESC
  """
}
```

**Q10 — Portfolio value by risk profile**
```esql
POST /_query
{
  "query": """
    FROM portfolio-snapshots
      | STATS
          total_value = SUM(market_value),
          avg_pnl_pct = AVG(pnl_pct)
        BY risk_profile
      | SORT total_value DESC
  """
}
```

#### Group 3 — Market Event Analysis

**Q11 — Recent positive events**
```esql
POST /_query
{
  "query": """
    FROM market-events
      | WHERE impact == "positive"
      | SORT event_date DESC
      | KEEP event_date, ticker, event_type, headline, price_change_pct
      | LIMIT 10
  """
}
```

**Q12 — Average price move by event type**
```esql
POST /_query
{
  "query": """
    FROM market-events
      | STATS avg_move = AVG(price_change_pct), event_count = COUNT(*)
          BY event_type
      | SORT avg_move DESC
  """
}
```

> ✅ **Expected Result:** `fda_approval` and `analyst_upgrade` show the largest average `price_change_pct` in this dataset.

**Q13 — Stocks with bullish analyst ratings**
```esql
POST /_query
{
  "query": """
    FROM market-events
      | WHERE analyst_rating IN ("buy", "strong_buy")
      | KEEP event_date, ticker, company_name, headline,
             analyst_rating, price_change_pct
      | SORT price_change_pct DESC
  """
}
```

**Q14 — Biggest single-day price movers**
```esql
POST /_query
{
  "query": """
    FROM market-events
      | WHERE price_change_pct IS NOT NULL
      | SORT price_change_pct DESC
      | KEEP event_date, ticker, event_type, headline,
             price_before, price_after, price_change_pct
      | LIMIT 5
  """
}
```

#### Group 4 — Advanced Patterns

**Q15 — Most traded tickers by spend**
```esql
POST /_query
{
  "query": """
    FROM financial-transactions
      | STATS my_trades = COUNT(*), my_spend = SUM(total_value) BY ticker
      | SORT my_spend DESC
      | LIMIT 10
  """
}
```

**Q16 — Full position analysis for one ticker**

Demonstrates the `CASE`-inside-`STATS` pattern. In an Agent Builder tool, replace `"TSLA"` with a `?ticker` parameter.

> **Tip:** To turn Q16 into an Agent Builder tool: replace `WHERE ticker == "TSLA"` with `WHERE ticker == ?ticker`, then add `ticker` as a required string parameter in the tool definition.

```esql
POST /_query
{
  "query": """
    FROM financial-transactions
      | WHERE ticker == "TSLA"
      | STATS
          total_bought = SUM(CASE(transaction_type == "buy",  total_value, 0)),
          total_sold   = SUM(CASE(transaction_type == "sell", total_value, 0)),
          net_shares   = SUM(CASE(transaction_type == "buy",  quantity, -quantity))
        BY ticker
  """
}
```

---

### B. Tool Description Best Practices

A strong tool description answers four questions for the LLM:

- **WHAT:** What does this tool actually do?
- **WHEN:** When should the agent call this tool?
- **HOW:** What parameters are needed?
- **LIMITS:** What are the constraints or caveats?

---

### C. Suggested Agent Chat Prompts

Use these natural language prompts to test your Financial Analyst Agent after completing Lab 6:

> **Tip:** Account IDs: ACC-001 Alice, ACC-002 Brian, ACC-003 Carmen, ACC-004 David, ACC-005 Elena, ACC-006 Frank.

#### ES|QL Tool Tests
- `"Show me all AAPL transactions"` → triggers Get Transactions by Ticker
- `"What did Alice Nguyen buy this week?"` → triggers Get Transactions by Ticker with customer filter
- `"Give me a portfolio breakdown for ACC-004"` → triggers Portfolio Summary
- `"How much is David Kim's TSLA position worth?"` → tests portfolio + reasoning

#### Index Search Tests
- `"Find any transactions related to AI companies"`
- `"Which customer has the most aggressive trading pattern?"`
- `"What sectors have the most trading activity?"`

#### Market Event Tests
- `"Which stocks got analyst upgrades recently?"`
- `"Show me all earnings events and their price impact"`
- `"What happened to NVDA stock in January 2025?"`

#### Multi-Tool Tests (Most Impressive)
- `"Look up AAPL transactions for Alice, check AAPL market events, then draw a timeline diagram"`
- `"Compare the portfolios of Alice (ACC-001) and David (ACC-004) and create a comparison diagram"`
- `"Which of Elena's holdings had negative market events? Draw a risk diagram."`

---

### D. Common Issues & Solutions

| Issue | Solution |
|-------|----------|
| Bulk insert returns `errors: true` | Check that you ran the PUT mapping step first. If index was auto-created, delete it and run PUT then POST `_bulk` again. |
| `timestamp` field mapped as text | This happens if you skipped Step 1. Delete the index and re-run the PUT mapping before the bulk insert. |
| Agent Builder not visible | Ensure you are in an Elasticsearch Serverless project with the Complete tier enabled. |
| ES\|QL tool returns no results | Verify index name matches exactly. Run `GET _cat/indices` to confirm. Index names are case-sensitive. |
| MCP connector shows unhealthy | Verify the MCP server URL is correct and publicly accessible. Test `mcp.excalidraw.com` in a browser. |
| Agent uses wrong tool | Improve tool descriptions. Add explicit `WHEN:` and `DO NOT use this when:` clauses to make tools distinctive. |
| API returns 403 Forbidden | Check that the API key has sufficient privileges for Agent Builder and the `.kibana` indices. |

---

### E. Reference Links

- **Elastic Agent Builder Docs:** [elastic.co/docs/explore-analyze/ai-features/elastic-agent-builder](https://elastic.co/docs/explore-analyze/ai-features/elastic-agent-builder)
- **ES|QL Reference:** [elastic.co/docs/explore-analyze/query-filter/languages/esql](https://elastic.co/docs/explore-analyze/query-filter/languages/esql)
- **Excalidraw MCP:** [mcp.excalidraw.com](https://mcp.excalidraw.com)
- **Elastic Serverless Trial:** [cloud.elastic.co/serverless-registration](https://cloud.elastic.co/serverless-registration)

---

*End of Lab Guide*
