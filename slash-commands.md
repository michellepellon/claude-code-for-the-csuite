# Slash Commands

Slash commands allow you to control Claude's behavior during an interactive
session. Claude Code has built-in slash commands for common tasks such as:

- ```/clear``` - Clear conversation history
- ```/mcp``` - Manage MCP server connections and OAuth authentication
- ```/model``` - Select or change the AI model

## Custom slash commands

Custom slash commands allow you to define frequently-used prompts as 
Markdown files that Claude Code can execute. Commands are organized by scope 
(project-specific or personal) and support namespacing through directory 
structures.

### Syntax

```/<command-name> [arguments]```

#### Parameters

- **<command-name>** - Name derived from the Markdown filename (without ```.md``` extension)
- **[arguments]** - Optional arguments passed to the command
