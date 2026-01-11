# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a course repository for "MCP: Build Rich-Context AI Apps with Anthropic" - a tutorial series on Model Context Protocol (MCP). The lessons progressively build an MCP-based research assistant chatbot that searches arXiv papers.

## Repository Structure

Each `L*` directory contains a lesson with progressively more complex implementations:
- **L3**: Basic chatbot example (no MCP)
- **L4**: MCP server creation with FastMCP (`research_server.py` with tools only)
- **L5**: MCP client implementation (single server connection via stdio)
- **L6**: Multi-server connections (adds filesystem and fetch servers via `server_config.json`)
- **L7**: Full MCP features (adds prompts and resources to the server)
- **L9**: Remote server deployment (SSE transport instead of stdio)

## Running the Projects

Each lesson's project is in `L*/mcp_project/`. Projects use `uv` for dependency management.

```bash
# From within a lesson's mcp_project directory:
cd L5/mcp_project

# Run the MCP chatbot client
uv run mcp_chatbot.py

# Run the MCP server standalone (for testing)
uv run research_server.py
```

## Key Dependencies

- `anthropic` - Claude API client
- `mcp` - Model Context Protocol library (FastMCP for servers, ClientSession for clients)
- `arxiv` - arXiv paper search API
- `nest-asyncio` - Enables nested async event loops

## Architecture Patterns

### MCP Server (FastMCP)
Servers expose tools, resources, and prompts via the `@mcp.tool()`, `@mcp.resource()`, and `@mcp.prompt()` decorators. Transport is either `stdio` (local) or `sse` (remote).

### MCP Client
Clients connect via `stdio_client` for local servers. Multi-server support uses `AsyncExitStack` for connection lifecycle management. The `tool_to_session` or `sessions` dict maps tool names to their originating sessions.

### Server Configuration
L6+ uses `server_config.json` to define multiple MCP servers (research, filesystem, fetch) with their connection parameters.

## Environment

Requires `.env` file with `ANTHROPIC_API_KEY` for the Claude API.
