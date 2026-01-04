# Spring AI MCP Client & Server (SSE) Demo

This repository contains a demonstration of the **Model Context Protocol (MCP)** using **Spring AI**, consisting of a Client and a Server communicating via Server-Sent Events (SSE).

## 🏗️ Architecture

The project is divided into two main components:

1.  **MCP Server** (`spring-ai-mcp-server-sse-main`):
    *   Exposes a set of tools using the Model Context Protocol.
    *   Currently implements a `StockService` that provides stock prices for specific companies (Infosys, TCS, Samsung).
    *   Uses **SSE (Server-Sent Events)** for communication.
    *   Runs on port **8040**.

2.  **MCP Client** (`spring-ai-mcp-client-sse-main`):
    *   Connects to the MCP Server to discover and use the available tools.
    *   Integrates with an LLM (via **Ollama**) to process user queries.
    *   Exposes a REST API for users to interact with the system.
    *   Runs on port **8080**.

## 🚀 Prerequisites

Before running the application, ensure you have the following installed:

*   **Java**: The project is configured for **Java 25** (as per `pom.xml`). Ensure you have a compatible JDK installed.
*   **Maven**: For building and running the Spring Boot applications.
*   **Ollama**: For providing the LLM capabilities.
    *   You need to have Ollama installed and running.
    *   Pull the required model: `qwen3:1.7b` (or update the configuration to use a different model).

    ```bash
    ollama pull qwen3:1.7b
    ```

## 🛠️ Configuration

### Server Configuration
Located in: `spring-ai-mcp-server-sse-main/src/main/resources/application.properties`
*   **Port**: 8040
*   **Base Path**: `/mcp-server`
*   **Service Name**: `my-server-sse`

### Client Configuration
Located in: `spring-ai-mcp-client-sse-main/src/main/resources/application.properties`
*   **Context Path**: `/mcp-client`
*   **Ollama URL**: `http://localhost:11434`
*   **Ollama Model**: `qwen3:1.7b`
*   **MCP Server URL**: `http://localhost:8040/mcp-server`

## 🏃‍♂️ How to Run

### 1. Start Ollama
Ensure Ollama is running and accessible at `http://localhost:11434`.

### 2. Start the MCP Server
Open a terminal, navigate to the server directory, and run the application:

```bash
cd spring-ai-mcp-server-sse-main
mvn spring-boot:run
```
The server will start on `http://localhost:8040`.

### 3. Start the MCP Client
Open a **new** terminal, navigate to the client directory, and run the application:

```bash
cd spring-ai-mcp-client-sse-main
mvn spring-boot:run
```
The client will start on `http://localhost:8080`.

## 💡 Usage

Once both applications are running, you can test the system using a web browser or `curl`.

The client exposes a chat endpoint at:
`http://localhost:8080/mcp-client/chat`

### Example Query
Ask the client about a stock price. The client will query the LLM, which will in turn use the MCP Server's `StockService` tool to fetch the data.

**Request:**
```http
http://localhost:8080/mcp-client/chat?query=What is the stock price of Infosys?
```

**Expected Response:**
```text
The stock price of Infosys in Rs 100. Valuations are attractive now.
```

**Other supported companies:**
*   TCS
*   Samsung

## ⚠️ Troubleshooting

*   **Connection Refused**: Ensure the MCP Server is fully started before making requests to the Client.
*   **404 Not Found**: Check if the context paths (`/mcp-client`, `/mcp-server`) are correctly configured in `application.properties`.
*   **Ollama Error**: Ensure the model `qwen3:1.7b` is pulled and Ollama is running on port 11434.
