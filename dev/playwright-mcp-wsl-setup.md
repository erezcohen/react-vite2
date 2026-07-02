# Setting Up Playwright MCP on WSL

## Why WSL needs a different approach

Chromium launched _inside_ WSL needs a display. WSLg provides one via a virtual GPU bridge, but that virtual GPU is prone to crashing Chromium's GPU process. The reliable options are: run headless (no display needed), or attach to a real Chrome instance running natively on Windows over the Chrome DevTools Protocol (CDP) instead of launching a browser from WSL at all.

This project uses the CDP-to-Windows-Chrome approach so the browser gets native GPU rendering and a visible window on the Windows desktop.

## 1. Start Chrome on Windows with remote debugging enabled

From a Windows terminal or shortcut:

```
"C:\Program Files\Google\Chrome\Application\chrome.exe" --remote-debugging-port=9222
```

WSL2 forwards `localhost` to the Windows host automatically, so this port is reachable from inside WSL without any extra networking setup. Verify with:

```bash
curl -s http://localhost:9222/json/version
```

## 2. Configure the MCP server

Add to `.mcp.json`:

```json
{
  "mcpServers": {
    "playwright": {
      "type": "stdio",
      "command": "npx",
      "args": [
        "-y",
        "@playwright/mcp@latest",
        "--cdp-endpoint",
        "http://localhost:9222"
      ],
      "env": {}
    }
  }
}
```

Use the official `@playwright/mcp` package (Microsoft-maintained), not a third-party WSL wrapper — it works fine under WSL2 as-is via the CDP flag above.

## 3. Reconnect

Run `/mcp` in Claude Code to connect. Chrome must already be running with the debug port open (step 1) before this will succeed.

## Alternative: headless mode

If you don't need a visible browser window, skip steps 1 and swap the args for:

```json
"args": ["-y", "@playwright/mcp@latest", "--headless"]
```

This runs Chromium inside WSL with no display or GPU involved — simplest and most isolated, at the cost of not being able to watch it interactively.
