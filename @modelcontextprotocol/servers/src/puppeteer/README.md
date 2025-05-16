# Puppeteer

A Model Context Protocol server that provides browser automation capabilities using Puppeteer. This server enables LLMs to interact with web pages, take screenshots, and execute JavaScript in a real browser environment.

> [!CAUTION]
> This server can access local files and local/internal IP addresses since it runs a browser on your machine. Exercise caution when using this MCP server to ensure this does not expose any sensitive data.

## Components

### Tools

- **puppeteer_navigate**

  - Navigate to any URL in the browser
  - Inputs:
    - `url` (string, required): URL to navigate to
    - `launchOptions` (object, optional): PuppeteerJS LaunchOptions. Default null. If changed and not null, browser restarts. Example: `{ headless: true, args: ['--user-data-dir="C:/Data"'] }`
    - `allowDangerous` (boolean, optional): Allow dangerous LaunchOptions that reduce security. When false, dangerous args like `--no-sandbox`, `--disable-web-security` will throw errors. Default false.

- **puppeteer_screenshot**

  - Capture screenshots of the entire page or specific elements
  - Inputs:
    - `name` (string, required): Name for the screenshot
    - `selector` (string, optional): CSS selector for element to screenshot
    - `width` (number, optional, default: 800): Screenshot width
    - `height` (number, optional, default: 600): Screenshot height
    - `encoded` (boolean, optional): If true, capture the screenshot as a base64-encoded data URI (as text) instead of binary image content. Default false.

- **puppeteer_click**

  - Click elements on the page
  - Input: `selector` (string): CSS selector for element to click

- **puppeteer_hover**

  - Hover elements on the page
  - Input: `selector` (string): CSS selector for element to hover

- **puppeteer_fill**

  - Fill out input fields
  - Inputs:
    - `selector` (string): CSS selector for input field
    - `value` (string): Value to fill

- **puppeteer_select**

  - Select an element with SELECT tag
  - Inputs:
    - `selector` (string): CSS selector for element to select
    - `value` (string): Value to select

- **puppeteer_evaluate**
  - Execute JavaScript in the browser console
  - Input: `script` (string): JavaScript code to execute

### Resources

The server provides access to two types of resources:

1. **Console Logs** (`console://logs`)

   - Browser console output in text format
   - Includes all console messages from the browser

2. **Screenshots** (`screenshot://<name>`)
   - PNG images of captured screenshots
   - Accessible via the screenshot name specified during capture

## Key Features

- Browser automation
- Console log monitoring
- Screenshot capabilities
- JavaScript execution
- Basic web interaction (navigation, clicking, form filling)
- Customizable Puppeteer launch options

## Configuration to use Puppeteer Server

### Usage with Claude Desktop

Here's the Claude Desktop configuration to use the Puppeter server:

### Docker

**NOTE** The docker implementation will use headless chromium, where as the NPX version will open a browser window.

```json
{
  "mcpServers": {
    "puppeteer": {
      "command": "docker",
      "args": [
        "run",
        "-i",
        "--rm",
        "--init",
        "-e",
        "DOCKER_CONTAINER=true",
        "mcp/puppeteer"
      ]
    }
  }
}
```

### NPX

```json
{
  "mcpServers": {
    "puppeteer": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-puppeteer"]
    }
  }
}
```

### Usage with VS Code

For quick installation, use one of the one-click install buttons below...

[![Install with NPX in VS Code](https://img.shields.io/badge/VS_Code-NPM-0098FF?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=puppeteer&config=%7B%22command%22%3A%22npx%22%2C%22args%22%3A%5B%22-y%22%2C%22%40modelcontextprotocol%2Fserver-puppeteer%22%5D%7D) [![Install with NPX in VS Code Insiders](https://img.shields.io/badge/VS_Code_Insiders-NPM-24bfa5?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=puppeteer&config=%7B%22command%22%3A%22npx%22%2C%22args%22%3A%5B%22-y%22%2C%22%40modelcontextprotocol%2Fserver-puppeteer%22%5D%7D&quality=insiders)

[![Install with Docker in VS Code](https://img.shields.io/badge/VS_Code-Docker-0098FF?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=puppeteer&config=%7B%22command%22%3A%22docker%22%2C%22args%22%3A%5B%22run%22%2C%22-i%22%2C%22--rm%22%2C%22--init%22%2C%22-e%22%2C%22DOCKER_CONTAINER%3Dtrue%22%2C%22mcp%2Fpuppeteer%22%5D%7D) [![Install with Docker in VS Code Insiders](https://img.shields.io/badge/VS_Code_Insiders-Docker-24bfa5?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=puppeteer&config=%7B%22command%22%3A%22docker%22%2C%22args%22%3A%5B%22run%22%2C%22-i%22%2C%22--rm%22%2C%22--init%22%2C%22-e%22%2C%22DOCKER_CONTAINER%3Dtrue%22%2C%22mcp%2Fpuppeteer%22%5D%7D&quality=insiders)

For manual installation, add the following JSON block to your User Settings (JSON) file in VS Code. You can do this by pressing `Ctrl + Shift + P` and typing `Preferences: Open User Settings (JSON)`.

Optionally, you can add it to a file called `.vscode/mcp.json` in your workspace. This will allow you to share the configuration with others.

> Note that the `mcp` key is not needed in the `.vscode/mcp.json` file.

For NPX installation (opens a browser window):

```json
{
  "mcp": {
    "servers": {
      "puppeteer": {
        "command": "npx",
        "args": ["-y", "@modelcontextprotocol/server-puppeteer"]
      }
    }
  }
}
```

For Docker installation (uses headless chromium):

```json
{
  "mcp": {
    "servers": {
      "puppeteer": {
        "command": "docker",
        "args": [
          "run",
          "-i",
          "--rm",
          "--init",
          "-e",
          "DOCKER_CONTAINER=true",
          "mcp/puppeteer"
        ]
      }
    }
  }
}
```

### Launch Options

You can customize Puppeteer's browser behavior in two ways:

1. **Environment Variable**: Set `PUPPETEER_LAUNCH_OPTIONS` with a JSON-encoded string in the MCP configuration's `env` parameter:

   ```json
   {
     "mcpServers": {
       "mcp-puppeteer": {
         "command": "npx",
         "args": ["-y", "@modelcontextprotocol/server-puppeteer"],
         "env": {
           "PUPPETEER_LAUNCH_OPTIONS": "{ \"headless\": false, \"executablePath\": \"C:/Program Files/Google/Chrome/Application/chrome.exe\", \"args\": [] }",
           "ALLOW_DANGEROUS": "true"
         }
       }
     }
   }
   ```

2. **Tool Call Arguments**: Pass `launchOptions` and `allowDangerous` parameters to the `puppeteer_navigate` tool:

   ```json
   {
     "url": "https://example.com",
     "launchOptions": {
       "headless": false,
       "defaultViewport": { "width": 1280, "height": 720 }
     }
   }
   ```

## Build

Docker build:

```bash
docker build -t mcp/puppeteer -f src/puppeteer/Dockerfile .
```

## License

This MCP server is licensed under the MIT License. This means you are free to use, modify, and distribute the software, subject to the terms and conditions of the MIT License. For more details, please see the LICENSE file in the project repository.
