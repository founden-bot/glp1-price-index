# Installing the GLP1Prices.co MCP server

This is a hosted, remote MCP server. There is nothing to download, build or run, and no API key.

- Transport: Streamable HTTP
- URL: https://glp1pricesco.founden.co/api/mcp
- Authentication: none

Add it to the client's MCP settings as a remote server, for example:

```json
{ "mcpServers": { "glp1prices": { "url": "https://glp1pricesco.founden.co/api/mcp" } } }
```

Then check it by calling `list_glp1_medications`, which takes no arguments and returns the price lists the index keeps.
