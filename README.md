# Cloudflare MCP Server Monorepo

Model Context Protocol (MCP) is a [new, standardized protocol](https://modelcontextprotocol.io/introduction) for managing context between large language models (LLMs) and external systems. In this repository, we provide an installer as well as an MCP Server for [Cloudflare's API](https://api.cloudflare.com).

This lets you use Claude Desktop, or any MCP Client, to use natural language to accomplish things on your Cloudflare account, e.g.:

* `Please deploy me a new Worker with an example durable object.`
* `Can you tell me about the data in my D1 database named '...'?`
* `Can you copy all the entries from my KV namespace '...' into my R2 bucket '...'?`

## Access the remote MCP server from Claude Desktop

Open Claude Desktop and navigate to Settings -> Developer -> Edit Config. This opens the configuration file that controls which MCP servers Claude can access.

Replace the content with the following configuration. Once you restart Claude Desktop, a browser window will open showing your OAuth login page. Complete the authentication flow to grant Claude access to your MCP server. After you grant access, the tools will become available for you to use. 

```
{
  "mcpServers": {
    "math": {
      "command": "npx",
      "args": [
        "mcp-remote",
        "https://mcp-cloudflare-staging.r.workers.dev/sse"
      ]
    }
  }
}
```

## Paid Features

Some features in this MCP server require a paid Cloudflare Workers plan:

- **Observability**: The prefixed `worker_logs_` tools require a paid Workers plan to access these metrics

Ensure your Cloudflare account has the necessary subscription level for the features you intend to use.

## Features

### Workers Management
- `worker_list`: List all Workers in your account
- `worker_get`: Get a Worker's script content

### Workers Logs
- `worker_logs_`: Retrieve workers observability logs for your worker by name, or rayId

## Developing

### Apps
- [workers-observability](apps/workers-observability/): The Workers Observability MCP server

### Packages

- eslint-config: Eslint config used by all apps and packages.
- typescript-config: tsconfig used by all apps and packages.
- mcp-common: Shared common tools and scripts to help manage this repo.

For more details on development in this monorepo, take a look at apps/workers-observability

## Testing

The project uses Vitest as the testing framework with MSW (Mock Service Worker) for API mocking.

### Running Tests

To run all tests:

```bash
pnpm test
```

To run a specific test file:

```bash
pnpm test -- tests/tools/queues.test.ts
```

To run tests in watch mode (useful during development):

```bash
pnpm test:watch
```

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.
