# Elastic Agent Builder Lab Guide
## Workshop Title: Data Visualization and Analysis with Elastic Agent Builder

![Elastic](https://img.shields.io/badge/Elastic-Serverless-00BFB3?style=flat&logo=elastic)
![Level](https://img.shields.io/badge/Level-Beginner-green)
![Duration](https://img.shields.io/badge/Duration-30mins-blue)

> **Note:** This lab guide uses Elastic Serverless (Elasticsearch Serverless project) with a 14-day free trial. No prior Elastic or Elasticsearch experience required.

---

## Overview & Learning Objectives

This hands-on lab guide introduces you to Elastic Agent Builder's powerful data visualization and analysis capabilities using natural language. You will work with Kibana's sample datasets to explore how AI-powered agents can translate conversational queries into precise ES|QL queries, generate visualizations, and build comprehensive dashboards—all through simple conversation.

### What You Will Learn

- **Natural language interaction** with Elasticsearch data - ask questions in plain English and get instant insights
- **ES|QL query generation and refinement** - watch the agent translate your requests into precise queries
- **Automated visualization creation** - generate charts, graphs, and metrics without manual configuration
- **Dashboard building through conversation** - combine multiple visualizations into interactive dashboards
- **Custom agent creation** - build a specialized agent with a unique personality using system prompts
- **Extending capabilities with MCP servers** - integrate third-party tools like Excalidraw and Jina AI

### What You Will Build

- Natural language queries across ecommerce, flights, and web logs datasets
- Multiple visualization types: bar charts, line charts, pie charts, and metric cards
- A comprehensive analytics dashboard combining insights from all datasets
- A custom "Data Visualization Chef" agent with a culinary-themed personality
- Diagram generation capabilities using Excalidraw MCP server
- Content enrichment using Jina AI MCP server for web scraping and data augmentation

### Lab Modules

| Module | Title | Duration | Description |
|--------|-------|----------|-------------|
| Setup | Environment Setup | 5 mins | Create an Elastic Serverless trial and load sample datasets |
| Module 1.1 | Natural Language Queries | 5 mins | Query ecommerce data using conversational language |
| Module 1.2 | ES\|QL Query Exploration | 5 mins | Analyze flights and web logs with advanced ES\|QL queries |
| Module 1.3 | Visualizations & Dashboards | 5 mins | Build charts and dashboards from multiple data sources |
| Module 2.1 | Build Custom Agent | 4 mins | Create a food-themed Data Visualization Chef agent |
| Module 2.2 | Custom MCP Server Integration | 6 mins | Extend agent with Excalidraw diagrams and Jina AI external content |

### Prerequisites

- A modern web browser (Chrome recommended)
- An email address for Elastic Cloud account registration
- No prior Elasticsearch, Kibana, or Elastic experience required
- Basic familiarity with data concepts (tables, charts) is helpful but not required

---

## Environment Setup (5mins)
### Create Your Elastic Serverless Trial Project

> **Tip:** The Elasticsearch Serverless project type is perfect for this lab because it includes Agent Builder enabled by default, requires no cluster management, and comes with pre-built sample datasets.

### Step 1.1 — Sign Up for Elastic Cloud Trial

Navigate to the Elastic Cloud registration page and create your free 14-day trial account.

<img width="800" alt="image" src="https://github.com/user-attachments/assets/bbb8d092-3ffe-4c9e-9621-9d5d97bb0640" />

1. Go to [Elastic Serverless Trial](https://ela.st/build-your-agents) and click **Start free trial**
2. Enter your email address and create a password
3. Verify your email address by clicking the link sent to your inbox
4. Once logged in, you will see the Elastic Cloud home page

### Step 1.2 — Create a Serverless Elasticsearch Project

<img width="800" alt="image" src="https://github.com/user-attachments/assets/82b13aff-8df7-4628-9b90-0bccacc671dc" />

1. Click **Create project** from the Elastic Cloud home page
2. Select the **Elasticsearch** project type from the available options
3. Enter a project name, for example: `Data-Viz-Lab`
4. Select your preferred cloud provider and region (choose the closest to you for best performance)
5. Click **Create project** and wait for provisioning to complete (typically 1–2 minutes)

### (Optional) Step 1.3 — Save Your Connection Details

<img width="800" alt="image" src="https://github.com/user-attachments/assets/2efc0e99-fb18-4de3-86d0-9f75b292cb46" />

Once your project is ready, save the following details — you will need them throughout the lab:

| Item | Where to find it |
|------|-----------------|
| Kibana URL | Project Overview → Endpoints → Kibana URL |
| Elasticsearch URL | Project Overview → Endpoints → Elasticsearch URL |

> **Note:** For this lab, you won't need to generate API keys manually. The Agent Builder feature uses your authenticated Kibana session automatically.

### Step 1.4 — Load Kibana Sample Data

Kibana includes three excellent sample datasets that we'll use throughout this lab: ecommerce transactions, flight data, and web server logs.

<img width="800" alt="image" src="./assets/getting-started.png" />

1. From your Kibana instance, click on the **🚀** menu icon in the bottom-left corner
2. Click on **Add data** button and **Browse sample datasets**

<img width="800" alt="image" src="./assets/getting-started.png" />

3. You will see three sample datasets available. Click **Install data** for each of the following:
   - **Sample eCommerce orders** - Online store transaction data with products, customers, and revenue
   - **Sample flight data** - Flight delays, carriers, and destinations
   - **Sample web logs** - Web server access logs with response codes and geographic data

<img width="800" alt="image" src="./assets/install-datasets.png" />

4. Wait for each dataset to load (takes about 10-30 seconds each)

> ✅ **Expected Result:** You should see all three sample datasets with a status of "Installed" and each should contain:
> - `kibana_sample_data_ecommerce` - ~1,850 documents
> - `kibana_sample_data_flights` - ~2,700 documents  
> - `kibana_sample_data_logs` - ~2,200 documents

### Step 1.5 — Verify Data Ingestion

Let's confirm all three indices are available with the expected document counts.

<img width="800" alt="image" src="./assets/discover-ecommerce.png" />

1. Discover datasets

> ✅ **Expected Result:** You should see output similar to:

<img width="800" alt="image" src="./assets/ecommerce-dataset.png" />

---

## Part 1: Tool Use, ES|QL, and Data Visualization (15 mins)

In this section, you will learn how to interact with your data using natural language, understand how Agent Builder translates your questions into ES|QL queries, and create powerful visualizations—all through simple conversation.

---

## Module 1.1 — Getting Started with Natural Language Queries (5 mins)
### Conversational Data Analysis with Ecommerce Data

**Objective:** Learn how to ask questions in plain English and watch as Agent Builder translates them into precise ES|QL queries and visualizations.

**Dataset Focus:** `kibana_sample_data_ecommerce`

In this module, you'll explore ecommerce transaction data by asking simple business questions. You'll see how Agent Builder understands your intent, constructs the appropriate ES|QL query, executes it, and presents the results.

### Understanding the Ecommerce Dataset

The ecommerce sample dataset contains online store transactions with the following key fields:
- **Products**: product names, categories, prices
- **Customers**: customer names, demographics
- **Orders**: order dates, total revenue, tax amounts
- **Geography**: customer location data

### Step 1.1.1 — Query Top-Selling Products

Let's start with a common business question: which products are selling best?

1. In the chat interface with the Elastic AI Agent, enter the following prompt:

```
What are the top 5 best-selling products in the ecommerce data?
```

<img width="700" alt="image" src="./assets/top5-products-ecommerce.png" />

2. Observe the agent's response. You should see:
   - The ES|QL query that was generated (click to expand if collapsed)
   - A table or visualization showing the top 5 products by quantity sold
   - Product names with their total quantities

> **Key Learning:** Notice how your natural language question was translated into an ES|QL query similar to:
> ```esql
> FROM kibana_sample_data_ecommerce
> | STATS total_quantity = SUM(products.quantity) BY products.product_name
> | SORT total_quantity DESC
> | LIMIT 5
> ```

### Step 1.1.2 — Analyze Revenue by Category

Now let's look at revenue distribution across product categories.

1. Ask the agent:

```
Show me the total revenue by category from the kibana_sample_data_ecommerce index
```

<img width="700" alt="image" src="./assets/total-revenue-ecommerce.png" />

2. The agent will:
   - Generate an appropriate ES|QL query using aggregation
   - Return the revenue totals grouped by product category
   - May automatically create a bar chart or table visualization

> **Key Learning:** The agent understands "revenue" means summing the `taxful_total_price` field and groups results by `category`. The generated query might look like:
> ```esql
> FROM kibana_sample_data_ecommerce
> | STATS total_revenue = SUM(taxful_total_price) BY category
> | SORT total_revenue DESC
> ```

### Step 1.1.3 — Explore the dataset see ES|QL with Agent Builder for yourself! Some examples:

```
Which customers have spent the most money? Show me the top 10 customers by total purchase amount.
```

**Key Components of ES|QL queries you'll see:**
- `FROM` - specifies which index to query
- `STATS` - performs aggregations (SUM, COUNT, AVG, etc.)
- `BY` - groups results by specific fields
- `SORT` - orders results ascending or descending
- `LIMIT` - restricts the number of results returned
- `KEEP` or `DROP` - selects or excludes specific fields

> **Tip:** Try asking follow-up questions like "Can you explain this query to me?" or "What does the STATS command do?" - the agent can teach you ES|QL as you go!

---

## Module 1.2 — Exploring Flight and Web Logs datasets (5 mins)
### Advanced Query Patterns with Flights and Web Logs

**Objective:** Explore more complex ES|QL capabilities including time-based analysis, filtering, and multi-field aggregations across two different datasets.

**Dataset Focus:** `kibana_sample_data_flights` and `kibana_sample_data_logs`

In this module, you'll work with operational data that includes timestamps, status codes, and geographic information—key dimensions for analytics.

Start a new Agent Builder chat to have a fresh context window.

### Understanding the Datasets

**Flights Dataset:**
- Flight numbers, carriers, origins, and destinations
- Delay times, distances, timestamps
- Weather conditions and delay reasons

**Web Logs Dataset:**
- HTTP requests with timestamps
- Response codes (200, 404, 500, etc.)
- Client geo-location, user agents, and referrers

### Step 1.2.1 — Analyze Flight Delays by Carrier and Delay Types

Let's explore which airlines have the most delays and understand the common causes.


1. Ask the agent:

```
Which carriers have the highest average flight delay times, and what are the most common delay types?
```

<img width="700" alt="image" src="./assets/delays-flights.png" />

2. The agent will:
   - Calculate average delay times for each carrier
   - Group results by carrier/airline name
   - Analyze and count delay types across the dataset
   - Present results in tables or bar charts showing both metrics

> **Key Learning:** This query demonstrates ES|QL's ability to:
> - **Aggregations** - Using AVG() and COUNT() functions for statistical analysis
> - **Grouping** - BY clause to organize data by categorical fields (carrier, delay type)
> - **Statistical analysis** - Computing meaningful metrics from numeric data
> - **Handling null values** - Not all flights are delayed, ES|QL handles missing data gracefully

**What the agent might discover:**
- Carriers with average delays ranging from 15-60 minutes
- Common delay types: Weather, Carrier, Security, Late Aircraft
- Patterns showing which carriers have specific delay issues

### Step 1.2.2 — Route Performance with Multi-Field Analysis

Discover the busiest flight routes and their performance metrics.

```
Show me the top 10 most popular flight routes by number of flights, including their average ticket price and cancellation rate
```

### Additional Flight Analysis Examples

Want to explore more? Try these prompts on your own:

**Time-based Pattern Analysis:**
```
Analyze flight delays by day of week and time of day. When are flights most likely to be delayed?
```
- Showcases: Temporal analysis, time bucketing, pattern recognition, multi-dimensional grouping
- You might discover Monday mornings or Friday evenings have higher delays

**Weather Impact Analysis:**
```
How does weather at the origin airport affect flight delays? Compare delay rates across different weather conditions
```
- Showcases: Correlation analysis, filtering, percentage calculations, comparative analytics
- Reveals how fog, thunderstorms, or clear weather correlate with delays

**Geographic Revenue Analysis:**
```
What is the total revenue by destination country, and which countries have the highest average ticket prices?
```
- Showcases: Geographic aggregations, revenue calculations, sorting and ranking
- Useful for understanding international vs domestic pricing patterns












## Part 2: Building Custom Agent and Adding Custom Tools (5 mins)

In this section, you'll create your own specialized data analytics agent with a unique personality — a culinary-obsessed "Data Visualization Chef" who explains data insights using food metaphors! You'll then extend your custom agent with powerful external capabilities using Model Context Protocol (MCP) servers, adding visual diagramming and web content fetching tools.

---

## Module 2.1 — Build Your Custom Data Analytics Agent (4 mins)
### Create a Food-Themed Data Visualization Agent 

**Objective:** Build a custom agent with a distinctive personality for data visualization and analysis tasks. You'll learn how system prompts shape agent behavior and create an engaging, food-themed analyst that makes data analysis deliciously fun!

### Why Create a Custom Agent?

While the default Elastic AI Agent is powerful and versatile, creating a custom agent allows you to:
- **Add personality** that makes interactions more engaging and memorable (like our culinary theme!)
- **Focus the agent** on specific tasks (e.g., data visualization, analysis)
- **Customize behavior** with specialized instructions and tone
- **Limit tool access** to only relevant tools for better accuracy
- **Create domain expertise** through targeted system prompts
- **Improve response quality** by reducing scope and ambiguity

In this lab, you'll create a "Data Visualization Chef" agent that uses food metaphors to explain data insights — making analytics more fun and relatable while remaining accurate and helpful!

### Step 2.1.1 — Create Your Custom Agent

Let's build a Data Visualization Chef agent with a unique personality!

1. Navigate to **Agents** → **Elastic AI Agent**
2. Click **+ New**

<img width="700" alt="image" src="./assets/new-agent.png" />

3. Fill in the basic information:

| Field | Value |
|-------|-------|
| Agent ID | `foodie-agent` |
| Custom Instructions | `Refer to Step 2.1.2 below` |
| Display Name | Foodie Visualization Chef |
| Description | A culinary-obsessed data analyst who sees every dataset as a recipe. This specialized agent analyzes Kibana sample datasets with delicious food metaphors, creating insightful visualizations and ES\|QL queries while keeping things engaging and easy to digest. |

<img width="700" alt="image" src="./assets/foodie-agent.png" />

### Step 2.1.2 — Write Custom System Instructions

The system prompt shapes how your agent thinks and responds. This is where you give your agent personality and focus! Enter this creative custom instruction that gives your agent a unique food-themed personality:

```
You are a Data Visualization Chef — a culinary-obsessed data analyst who sees 
every dataset as a recipe and every insight as a dish worth savoring.

You LOVE food. Every answer you give has a food metaphor, analogy, or 
expression woven in naturally. Slow queries are "simmering nicely", 
a great chart is "beautifully plated", messy data is "a kitchen in chaos", 
and a breakthrough insight is "the secret ingredient we've been looking for".
Never force it — let the food flavour come through organically.


Your core capabilities:
- Translate business questions into precise ES|QL queries (your recipes)
- Create clear, insightful visualizations — bar charts, line charts, pie charts, 
  and metric cards (your plating style)
- Explain data patterns and trends in simple, non-technical language 
  (the tasting notes)
- Recommend the best chart type for each analysis 
  (choosing the right vessel for the dish)


Your data sources — today's menu:
- kibana_sample_data_ecommerce: Online store transactions, products, customers
  → Think of this as the front-of-house: orders, receipts, and happy diners
- kibana_sample_data_flights: Flight delays, carriers, routes, destinations
  → The supply chain: getting ingredients (and people) from A to B
- kibana_sample_data_logs: Web server logs, response codes, geographic traffic
  → The kitchen logs: every ticket, every fire, every table turn


Guidelines:
1. Always show the ES|QL query you generate so users can follow the recipe
2. When asked to visualize data, automatically suggest the best chart type 
   and explain why it's the right plate for this dish
3. Provide context with your answers — explain what the data means, 
   not just what it says
4. If a query could be ambiguous, ask a clarifying question before cooking — 
   a good chef always confirms the order before firing the grill
5. Keep responses concise but satisfying — no one likes a meal that 
   overstays its welcome
6. When appropriate, suggest follow-up analyses like recommending 
   a pairing or a next course


Remember: These are sample datasets for learning purposes — 
consider them the mise en place for your data skills. Bon appétit! 🍽️
```

4. Click on Tools tab and look at the 5 tools that it has selected.

<img width="700" alt="image" src="./assets/foodie-tools.png" />

5. Click **Save and Chat** to save your system instructions

> **Key Learning:** Notice how a well-crafted system prompt gives your agent a distinct personality and communication style. The food metaphors make the agent more engaging while still being helpful and accurate. This is the power of custom agents!

### Step 2.1.3 — Chat with your custom agent

Have a chat with your custom agent now. For example:

```
What is the total revenue by destination country, and which countries have the highest average ticket prices?
```

<img width="700" alt="image" src="./assets/foodie-agent-chat01.png" />



## Module 2.2 — Extend with Custom MCP Servers (6 mins)
### Add Visual Diagramming and External Content to Your Custom Agent

**Objective:** Integrate two powerful MCP servers—Excalidraw for visual diagrams and Jina AI for external web content—to dramatically expand your custom agent's capabilities.

### About MCP Servers

Model Context Protocol (MCP) enables agents to connect with external services and tools. In this module, you'll add:

**Excalidraw MCP** - Visual diagramming capabilities
- Create flowcharts showing processes and workflows
- Generate architecture diagrams for system design
- Build network diagrams showing relationships
- Design mind maps for brainstorming
- Produce custom sketches and illustrations

**Jina AI MCP** - External content fetching capabilities
- Fetch and process web pages
- Extract clean text from HTML
- Search the web for relevant information
- Enrich internal data with external sources
- Access real-time information from the internet

### Step 2.2.1 — Create the Excalidraw MCP Connector

First, configure the connection to the Excalidraw MCP server.

<img width="800" alt="image" src="https://github.com/user-attachments/assets/0529e04c-eeb8-473d-9a49-b3f92cf0beac" />

1. In Kibana, navigate to **Stack Management → Connectors**
2. Click **Create connector**
3. Search for **MCP** in the connector type list
4. Select **MCP Connector**

[Screenshot placeholder: MCP connector configuration form]

5. Configure with the following settings:

| Field | Value |
|-------|-------|
| Connector Name | `Excalidraw MCP` |
| MCP Server URL | `https://mcp.excalidraw.com` |
| Authentication | None |

6. Click **Save and test**
7. Verify the connector shows a green status indicator

<img width="700" alt="image" src="https://github.com/user-attachments/assets/4c030340-23ba-4de9-a095-9d6a1a1a64d6" />

> ✅ **Expected Result:** The connector test should succeed, confirming the Excalidraw MCP server is reachable.

### Step 2.2.2 — Import Excalidraw Tools

Now bulk-import the available tools from Excalidraw.

<img width="800" alt="image" src="https://github.com/user-attachments/assets/23c51759-1c75-499d-a847-2f3b8d0db3dc" />

1. Navigate to **Agents** → **Tools** tab
2. Click the **Manage MCP** dropdown
3. Select **Bulk import MCP tools**

 <img width="800" alt="image" src="https://github.com/user-attachments/assets/86901de5-74a1-48b5-bd46-4fa6faa29841" />

4. In the import dialog:
   - **MCP Server**: Select `Excalidraw MCP` (the connector you just created)
   - Wait for the tool list to populate (should show multiple diagram-related tools)
   - **Select all available tools** (typically includes create_diagram, add_shape, add_text, export_diagram, etc.)
   - **Namespace**: Enter `excalidraw`

5. Click **Import tools**

6. Verify the tools are imported:
   - Navigate to **Tools** tab
   - Filter or search for `excalidraw`
   - You should see multiple tools like:
     - `excalidraw.create_view`
     - `excalidraw.export_to_excalidraw`
     - `excalidraw.read_checkpoint`
     - `excalidraw.read_me`
     - `excalidraw.save_checkpoint`

7. Let's refine the instructions of the default tool (excalidraw.read_me) to make it more specific. Replace the default with the markdown below:

```
    Returns the Excalidraw element format reference with color palettes, examples, and tips. Call this **BEFORE** using create_view for the first time.

    It will look like:

    ```
    {
    "type": "excalidraw",
    "version": 2,
    "source": "https://excalidraw.com",
    "elements": [
        {
        "id": "title-001",
        "type": "text",
        "x": 400,
        "y": 50,
        "width": 400,
        "height": 45,
        "angle": 0,
        "strokeColor": "#1e1e1e",
        "backgroundColor": "transparent",
        "fillStyle": "solid",
        "strokeWidth": 1,
        "strokeStyle": "solid",
        "roughness": 0,
        "opacity": 100,
        "groupIds": [],
        "frameId": null,
        "roundness": null,
        "seed": 1234567890,
        "version": 1,
        "versionNonce": 1234567890,
        "isDeleted": false,
        "boundElements": null,
        "updated": 1708045200000,
        "link": null,
        "locked": false,
        "text": "System Architecture Diagram",
        "fontSize": 36,
        "fontFamily": 1,
        "textAlign": "center",
        "verticalAlign": "top",
        "baseline": 32,
        "containerId": null,
        "originalText": "System Architecture Diagram",
        "lineHeight": 1.25
        },
        {
        "id": "rect-presentation-layer",
        "type": "rectangle",
        "x": 100,
        "y": 150,
        "width": 200,
        "height": 100,
        "angle": 0,
        "strokeColor": "#1971c2",
        "backgroundColor": "#a5d8ff",
        "fillStyle": "solid",
        "strokeWidth": 2,
        "strokeStyle": "solid",
        "roughness": 0,
        "opacity": 100,
        "groupIds": ["layer-presentation"],
        "frameId": null,
        "roundness": {
            "type": 3,
            "value": 16
        },
        "seed": 1234567891,
        "version": 1,
        "versionNonce": 1234567891,
        "isDeleted": false,
        "boundElements": [
            {
            "type": "text",
            "id": "text-presentation-layer"
            },
            {
            "id": "arrow-pres-to-app",
            "type": "arrow"
            }
        ],
        "updated": 1708045200000,
        "link": null,
        "locked": false
        },
        {
        "id": "text-presentation-layer",
        "type": "text",
        "x": 135,
        "y": 187.5,
        "width": 130,
        "height": 25,
        "angle": 0,
        "strokeColor": "#1e1e1e",
        "backgroundColor": "transparent",
        "fillStyle": "solid",
        "strokeWidth": 2,
        "strokeStyle": "solid",
        "roughness": 0,
        "opacity": 100,
        "groupIds": ["layer-presentation"],
        "frameId": null,
        "roundness": null,
        "seed": 1234567892,
        "version": 1,
        "versionNonce": 1234567892,
        "isDeleted": false,
        "boundElements": null,
        "updated": 1708045200000,
        "link": null,
        "locked": false,
        "text": "Presentation Layer",
        "fontSize": 20,
        "fontFamily": 1,
        "textAlign": "center",
        "verticalAlign": "middle",
        "baseline": 18,
        "containerId": "rect-presentation-layer",
        "originalText": "Presentation Layer",
        "lineHeight": 1.25
        },
        {
        "id": "rect-application-layer",
        "type": "rectangle",
        "x": 400,
        "y": 150,
        "width": 200,
        "height": 100,
        "angle": 0,
        "strokeColor": "#862e9c",
        "backgroundColor": "#d0bfff",
        "fillStyle": "solid",
        "strokeWidth": 2,
        "strokeStyle": "solid",
        "roughness": 0,
        "opacity": 100,
        "groupIds": ["layer-application"],
        "frameId": null,
        "roundness": {
            "type": 3,
            "value": 16
        },
        "seed": 1234567893,
        "version": 1,
        "versionNonce": 1234567893,
        "isDeleted": false,
        "boundElements": [
            {
            "type": "text",
            "id": "text-application-layer"
            },
            {
            "id": "arrow-pres-to-app",
            "type": "arrow"
            },
            {
            "id": "arrow-app-to-data",
            "type": "arrow"
            }
        ],
        "updated": 1708045200000,
        "link": null,
        "locked": false
        },
        {
        "id": "text-application-layer",
        "type": "text",
        "x": 430,
        "y": 187.5,
        "width": 140,
        "height": 25,
        "angle": 0,
        "strokeColor": "#1e1e1e",
        "backgroundColor": "transparent",
        "fillStyle": "solid",
        "strokeWidth": 2,
        "strokeStyle": "solid",
        "roughness": 0,
        "opacity": 100,
        "groupIds": ["layer-application"],
        "frameId": null,
        "roundness": null,
        "seed": 1234567894,
        "version": 1,
        "versionNonce": 1234567894,
        "isDeleted": false,
        "boundElements": null,
        "updated": 1708045200000,
        "link": null,
        "locked": false,
        "text": "Application Layer",
        "fontSize": 20,
        "fontFamily": 1,
        "textAlign": "center",
        "verticalAlign": "middle",
        "baseline": 18,
        "containerId": "rect-application-layer",
        "originalText": "Application Layer",
        "lineHeight": 1.25
        },
        {
        "id": "rect-data-layer",
        "type": "rectangle",
        "x": 700,
        "y": 150,
        "width": 200,
        "height": 100,
        "angle": 0,
        "strokeColor": "#2f9e44",
        "backgroundColor": "#b2f2bb",
        "fillStyle": "solid",
        "strokeWidth": 2,
        "strokeStyle": "solid",
        "roughness": 0,
        "opacity": 100,
        "groupIds": ["layer-data"],
        "frameId": null,
        "roundness": {
            "type": 3,
            "value": 16
        },
        "seed": 1234567895,
        "version": 1,
        "versionNonce": 1234567895,
        "isDeleted": false,
        "boundElements": [
            {
            "type": "text",
            "id": "text-data-layer"
            },
            {
            "id": "arrow-app-to-data",
            "type": "arrow"
            }
        ],
        "updated": 1708045200000,
        "link": null,
        "locked": false
        },
        {
        "id": "text-data-layer",
        "type": "text",
        "x": 765,
        "y": 187.5,
        "width": 70,
        "height": 25,
        "angle": 0,
        "strokeColor": "#1e1e1e",
        "backgroundColor": "transparent",
        "fillStyle": "solid",
        "strokeWidth": 2,
        "strokeStyle": "solid",
        "roughness": 0,
        "opacity": 100,
        "groupIds": ["layer-data"],
        "frameId": null,
        "roundness": null,
        "seed": 1234567896,
        "version": 1,
        "versionNonce": 1234567896,
        "isDeleted": false,
        "boundElements": null,
        "updated": 1708045200000,
        "link": null,
        "locked": false,
        "text": "Data Layer",
        "fontSize": 20,
        "fontFamily": 1,
        "textAlign": "center",
        "verticalAlign": "middle",
        "baseline": 18,
        "containerId": "rect-data-layer",
        "originalText": "Data Layer",
        "lineHeight": 1.25
        },
        {
        "id": "arrow-pres-to-app",
        "type": "arrow",
        "x": 300,
        "y": 200,
        "width": 100,
        "height": 0,
        "angle": 0,
        "strokeColor": "#1971c2",
        "backgroundColor": "transparent",
        "fillStyle": "solid",
        "strokeWidth": 2,
        "strokeStyle": "solid",
        "roughness": 0,
        "opacity": 100,
        "groupIds": [],
        "frameId": null,
        "roundness": {
            "type": 2
        },
        "seed": 1234567897,
        "version": 1,
        "versionNonce": 1234567897,
        "isDeleted": false,
        "boundElements": null,
        "updated": 1708045200000,
        "link": null,
        "locked": false,
        "points": [
            [0, 0],
            [100, 0]
        ],
        "lastCommittedPoint": [100, 0],
        "startBinding": {
            "elementId": "rect-presentation-layer",
            "focus": 0,
            "gap": 1
        },
        "endBinding": {
            "elementId": "rect-application-layer",
            "focus": 0,
            "gap": 1
        },
        "startArrowhead": null,
        "endArrowhead": "arrow"
        },
        {
        "id": "arrow-app-to-data",
        "type": "arrow",
        "x": 600,
        "y": 200,
        "width": 100,
        "height": 0,
        "angle": 0,
        "strokeColor": "#862e9c",
        "backgroundColor": "transparent",
        "fillStyle": "solid",
        "strokeWidth": 2,
        "strokeStyle": "solid",
        "roughness": 0,
        "opacity": 100,
        "groupIds": [],
        "frameId": null,
        "roundness": {
            "type": 2
        },
        "seed": 1234567898,
        "version": 1,
        "versionNonce": 1234567898,
        "isDeleted": false,
        "boundElements": null,
        "updated": 1708045200000,
        "link": null,
        "locked": false,
        "points": [
            [0, 0],
            [100, 0]
        ],
        "lastCommittedPoint": [100, 0],
        "startBinding": {
            "elementId": "rect-application-layer",
            "focus": 0,
            "gap": 1
        },
        "endBinding": {
            "elementId": "rect-data-layer",
            "focus": 0,
            "gap": 1
        },
        "startArrowhead": null,
        "endArrowhead": "arrow"
        }
    ],
    "appState": {
        "gridSize": 20,
        "viewBackgroundColor": "#ffffff",
        "zoom": {
        "value": 1.1
        }
    },
    "files": {}
    }

    ```
```

> ✅ **Expected Result:** Multiple Excalidraw tools appear in your tools list with the `excalidraw.` prefix and no error indicators.

### Step 2.2.3 — Create the Jina AI MCP Connector

Now set up the second MCP server for external content fetching.

> **Note:** Jina AI provides a free tier that works without authentication. For higher rate limits and advanced features, you can sign up at [jina.ai](https://jina.ai) and add your API key.

<img width="800" alt="image" src="./assets/jina-key.png" />

1. Return to **Stack Management → Connectors**
2. Click **Create connector** again
3. Search for **MCP** and select **MCP Connector**

 <img width="800" alt="image" src="./assets/jina-mcp-connector.png" />

4. Configure with these settings:

| Field | Value |
|-------|-------|
| Connector Name | `Jina AI MCP` | |
| MCP Server URL | `https://mcp.jina.ai/v1` | |
| Authorization | **Bearer** *(Put in the Key)* | Secret | 

5. Click **Save and test**
6. Verify successful connection

<img width="800" alt="image" src="./assets/jina-connector-test.png" />

> ✅ **Expected Result:** Jina AI MCP server is reachable.

### Step 2.2.4 — Import Jina AI Tools

Bulk-import the web scraping and content extraction tools.

<img width="800" alt="image" src="./assets/jina-bulk-tools.png" />

1. Go to **Agents** → **Tools** tab
2. Click **Manage MCP** → **Bulk import MCP tools**
3. Configure:
   - **MCP Server**: Select `Jina AI MCP`
   - Wait for tools to load
   - **Select all available tools** (typically includes parallel_read_url, parallel_search_web, etc.)
   - **Namespace**: Enter `jina`

4. Click **Import specific tools**

5. Verify specific imported tools:
   - Filter tools list for `jina`
   - Import these tools like:
     - `jina.parallel_read_url` - Extract and convert web page content to clean, readable markdown format
     - `jina.parallel_search_web` - Search the web for relevant content

> ✅ **Expected Result:** Jina AI tools appear in your tools list with `jina.` prefix.

### Step 2.2.5 — Add Both MCP Tool Sets to Your Custom Agent

Now extend your Data Visualization Chef with BOTH diagramming and external content capabilities.

<img width="800" alt="image" src="./assets/foodie-excalidraw.png" />

1. Navigate to **Agents** → **More** (top right) → **View all agents** 
2. Click on **Data Visualization Chef** (your custom agent) to edit it
3. Go to the **Tools** section
4. Click **Add tools**
5. Search for `excalidraw` and select all the Excalidraw tools
6. Search for `jina` and select the imported Jina AI tools (parallel_read_url, parallel_search_web). If you want to add more tools, feel free to do so
7. Click **Save**

<img width="800" alt="image" src="./assets/agent-excalidraw-jina-tools.png" />

> **Note:** Your Data Visualization Chef now has data analysis, visual diagramming, AND external content fetching—a complete toolkit for comprehensive analysis!

### Step 2.2.6 — Test Diagram Generation (Excalidraw)

Let's create a visual diagram through conversation!

**Test Prompt 1: Flight Network Diagram**

<img width="800" alt="image" src="./assets/excalidraw-architecture.png" />

    Create a network diagram visualizing the top flight routes between cities based on the flights dataset. Export the diagram into Excalidraw.
    


**Test Prompt 2: Data Architecture Diagram**

<img width="800" alt="image" src="./assets/excalidraw-architecture.png" />

```
Draw and export a simple Elasticsearch architecture diagram
```

<img width="800" alt="image" src="./assets/excalidraw-advance-architecture.png" />


```
Use the drawing tool to create a clear network / data‑flow diagram showing Elastic Agent, Fleet Server, and Elasticsearch data nodes. This diagram will be used as supporting documentation for a change approval board (CAB), so it must be accurate and self‑explanatory. Provide the sharable link after drawing it.

Requirements:
Draw separate labeled boxes for Elastic Agent, Fleet Server, and Elasticsearch data nodes (show the Elasticsearch side as a small cluster of 2–3 data nodes).

Add text labels inside every box (no unlabeled shapes).

Draw directional arrows to show data flow: Elastic Agent → Fleet Server → Elasticsearch data nodes, and any return/response paths that are relevant.

On or next to each arrow, add text labels for the default network ports and protocols:

Elastic Agent :left_right_arrow: Fleet Server: TCP 8220 (HTTPS).

Fleet Server :left_right_arrow: Elasticsearch HTTP interface: TCP 9200 (HTTPS).

Elasticsearch internal cluster traffic (between data nodes, and between data nodes and master/cluster‑manager if shown): TCP 9300.

Arrange the layout left‑to‑right for readability in CAB materials: Elastic Agent on the left, Fleet Server in the middle, Elasticsearch data nodes on the right. Avoid line crossings, keep arrows straight.

Use color only to distinguish roles (e.g., one color for agents, one for Fleet Server, one for Elasticsearch), but never leave a shape without a descriptive text label.
```

Compare the diagrams! Prompting at its finest.

### Step 2.2.7 — Test External Content Integration (Jina AI)

Let's use your enhanced custom agent to enrich data analysis with external information!

**Test Prompt 1: Enrich Ecommerce Analysis with Trends**

<img width="800" alt="image" src="./assets/jina-chat-tool.png" />

```
Read https://en.wikipedia.org/wiki/Top_Chef and summarise give me a background of the hosts
```

The agent will:
- Use `jina.parallel_search_web` to search relevant tech trend articles
- Use `jina.parallel_fetch_url` to extract content from promising URLs
- Analyze the external content
- Compare trends to your internal ecommerce categories
- Provide insights on alignment or gaps
---

## Conclusion & Next Steps

Congratulations! You've completed the Elastic Agent Builder Data Visualization and Analysis lab. You've learned how to:

✅ **Interact with data using natural language** - Ask questions in plain English and get immediate insights  
✅ **Understand ES|QL query generation** - See how conversational requests translate to precise queries  
✅ **Create powerful visualizations** - Generate charts, graphs, and dashboards through conversation  
✅ **Build comprehensive dashboards** - Combine multiple visualizations into cohesive analytics views  
✅ **Build custom agents with personality** - Create a food-themed Data Visualization Chef with unique character  
✅ **Extend with MCP servers** - Add Excalidraw diagrams and Jina AI
✅ **Combine internal and external data** - Enrich analysis with visualizations and web content

### What You Built

- A complete analytics workflow using three different datasets
- Multiple visualization types (bar charts, line charts, pie charts, metrics)
- A multi-dataset dashboard combining ecommerce, flights, and web logs
- A custom "Data Visualization Chef" agent with culinary-themed personality
- Integrated MCP capabilities: Excalidraw diagrams and Jina AI MCP tools

### Key Takeaways

1. **Natural Language is Powerful**: You can perform complex data analysis without learning query syntax
2. **ES|QL is Accessible**: Agent Builder teaches you ES|QL as you work through natural interaction
3. **Iteration is Easy**: Refine queries and visualizations through conversation, not configuration
4. **Custom Agents Add Value**: System prompts create unique personalities that make interactions more engaging
5. **MCP Extends Capabilities**: Third-party integrations make your agent even more capable
6. **Context Matters**: The agent maintains conversation context for progressive refinement

### Recommended Next Steps

**Immediate Actions:**
1. **Experiment with your own data**: Try uploading custom datasets and querying them
2. **Create more dashboards**: Build domain-specific dashboards for your use cases
3. **Experiment with agent personalities**: Create agents with different themes (detective, scientist, coach)
4. **Explore more MCP servers**: Check the MCP directory for additional integrations
5. **Share your work**: Export dashboards and share insights with your team

**Advanced Learning:**
1. **ES|QL Deep Dive**: Study ES|QL documentation to understand advanced patterns
2. **Custom Tools**: Create custom ES|QL tools with pre-defined queries for your common use cases
3. **Advanced Agent Customization**: Build specialized agents for specific domains (security, finance, operations)
4. **System Prompt Engineering**: Experiment with different personalities and communication styles
5. **Programmatic Access**: Use the Kibana API to integrate agents into applications

**Explore More MCP Servers:**
- **GitHub MCP**: Integrate with GitHub repositories for code analysis
- **Slack MCP**: Post insights directly to Slack channels
- **Google Sheets MCP**: Export data to spreadsheets automatically
- **Custom MCP**: Build your own MCP server for proprietary tools

### Additional Resources

**Elastic Documentation:**
- [Elastic Agent Builder Official Guide](https://www.elastic.co/guide/en/kibana/current/agent-builder.html)
- [ES|QL Reference](https://www.elastic.co/guide/en/elasticsearch/reference/current/esql.html)
- [Kibana Visualizations](https://www.elastic.co/guide/en/kibana/current/visualize.html)

**MCP Resources:**
- [Model Context Protocol Specification](https://modelcontextprotocol.io/)
- [Excalidraw MCP Documentation](https://github.com/modelcontextprotocol/servers)
- [Jina AI Documentation](https://docs.jina.ai/)

**Community:**
- [Elastic Community Forums](https://discuss.elastic.co/)

---

### Feedback and Support

If you found this lab helpful or have suggestions for improvement:
- Provide feedback through your Elastic Cloud account
- Join the Elastic community forums
- Share your dashboards and insights with #ElasticAgentBuilder

**Need Help?**
- Check [Elastic Support Portal](https://support.elastic.co/) for documentation
- Visit the [Elastic Community Forums](https://discuss.elastic.co/) for community support
- Contact Elastic Support if you have an active subscription

---

> **Lab Guide Version:** 1.0  
> **Last Updated:** March 2026  
> **Compatibility:** Elastic Serverless (Elasticsearch project type), Agent Builder feature enabled  
> **Sample Data:** Kibana sample datasets (ecommerce, flights, web logs)

---

Thank you for completing this ElasticON Singapore lab! Happy analyzing! 🚀
