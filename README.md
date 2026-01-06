# Postman MCP Server - Folder Execution Support

This is a fork of the official [Postman MCP Server](https://github.com/postmanlabs/postman-mcp-server) that adds support for executing specific folders within a Postman collection.

For complete documentation on the Postman MCP Server, including installation, configuration, and usage, see the [original README](https://github.com/postmanlabs/postman-mcp-server#readme).

---

## Changes in This Fork

### Folder Execution Support

The `runCollection` tool now accepts an optional `folderId` parameter that allows you to run only the requests within a specific folder of a collection, rather than the entire collection.

**Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `collectionId` | string | Yes | The ID of the collection to run |
| `folderId` | string | No | The ID or name of a folder to run. When provided, only requests within that folder will be executed |
| `environmentId` | string | No | The ID of an environment to use |
| `iterationCount` | number | No | Number of iterations to run |
| `requestTimeout` | number | No | Timeout for each request in milliseconds |
| `scriptTimeout` | number | No | Timeout for scripts in milliseconds |
| `stopOnFailure` | boolean | No | Whether to stop execution on first failure |

**Example usage:**

```json
{
  "collectionId": "17929829-80491fd9-90c6-4894-a852-392a2d9167df",
  "folderId": "17929829-d36b7575-71d6-4eee-8a48-cbda89c11af8"
}
```

The `folderId` parameter accepts either:
- A folder ID (e.g., `17929829-d36b7575-71d6-4eee-8a48-cbda89c11af8`)
- A folder name (e.g., `Users API`)

When a folder ID is provided, it is automatically resolved to the folder name that Newman expects.

### Bug Fixes

**Newman Node 22 Compatibility**

Updated Newman to version 6.2.1 to resolve compatibility issues with Node.js 22, which caused the server to crash due to undici-related errors.

**Request Delay Removed**

Removed the 1-second delay between requests (`delayRequest: 1000`) to improve execution speed. This change allows folder runs to complete within MCP timeout limits.

---

## Installation

This fork can be installed from source:

```bash
git clone https://github.com/jaredboynton/postman-mcp-server-folders.git
cd postman-mcp-server-folders
npm install
npm run build
```

### Cursor / Claude Desktop

Configure your MCP client to point to the local build:

```json
{
  "servers": {
    "postman": {
      "type": "stdio",
      "command": "node",
      "args": ["/path/to/postman-mcp-server-folders/dist/src/index.js"],
      "env": {
        "POSTMAN_API_KEY": "YOUR_API_KEY"
      }
    }
  }
}
```

### VS Code

VS Code uses a different MCP configuration schema. Create a `.vscode/mcp.json` file in your project:

```json
{
  "servers": {
    "postman-api-mcp": {
      "type": "stdio",
      "command": "node",
      "args": ["/path/to/postman-mcp-server-folders/dist/src/index.js"],
      "options": {
        "env": {
          "POSTMAN_API_KEY": "${input:postman-api-key}"
        }
      }
    }
  },
  "inputs": [
    {
      "id": "postman-api-key",
      "type": "promptString",
      "description": "Enter your Postman API key"
    }
  ]
}
```

On Windows, use backslashes in the path:

```json
"args": ["C:\\Users\\username\\git\\postman-mcp-server-folders\\dist\\src\\index.js"]
```

---

## License

This project is licensed under the Apache-2.0 License. See the [LICENSE](./LICENSE) file for details.
