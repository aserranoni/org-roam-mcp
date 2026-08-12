

# Org-roam MCP Server

A Model Context Protocol (MCP) server that enables Claude Code and other MCP clients to interact with your org-roam knowledge base.

## Features

- **Read Operations**: Search and retrieve org-roam nodes, explore backlinks and relationships
- **Write Operations**: Create new notes, update existing content, add links between nodes
- **Full Integration**: Respects org-roam's file structure, UUID system, and conventions
- **Auto-Detection**: Automatically finds your org-roam database and directory
- **Fast Search**: Leverages org-roam's SQLite database for efficient queries
- **Robust Error Handling**: Comprehensive input validation and descriptive error messages
- **Path Normalization**: Handles quoted/unquoted file paths and node IDs consistently
- **Database Synchronization**: Automatic refresh after file operations for immediate updates

## Recent Improvements

### v0.2.0 - Enhanced Reliability (Latest)

- **🔧 Fixed File Path Handling**: Resolved issues with quoted file paths from the org-roam database
- **🆔 Improved Node ID Support**: Now handles both quoted and unquoted node ID formats seamlessly  
- **🔄 Enhanced Search Synchronization**: Database automatically refreshes after create/update operations
- **✅ Better Error Handling**: Added comprehensive input validation with clear error messages
- **🧹 Path Cleaning**: Automatic normalization of file paths with quote removal
- **📝 Improved Logging**: Better error reporting and debugging information

## Installation

```bash
# Using uvx (recommended)
uvx org-roam-mcp

# Or with pip in a virtual environment
pip install org-roam-mcp
```

## Configuration

### Auto-Detection

The server automatically detects your org-roam setup by looking for:

**Database locations:**
- `~/.emacs.d/org-roam.db`
- `~/org-roam.db`
- `~/.config/emacs/org-roam.db`
- `~/Documents/org-roam/org-roam.db`

**Directory locations:**
- `~/org-roam/`
- `~/Documents/org-roam/`
- `~/Dropbox/org-roam/`
- `~/Notes/`

### Manual Configuration

Override auto-detection with environment variables:

```bash
export ORG_ROAM_DB_PATH="/path/to/your/org-roam.db"
export ORG_ROAM_DIR="/path/to/your/org-roam-directory"
export ORG_ROAM_MAX_SEARCH_RESULTS=100  # Default: 50
```

## Usage with Claude Code

Add to your Claude Code MCP configuration (`~/.claude_code/mcp.json`):

```json
{
  "mcpServers": {
    "org-roam": {
      "command": "uvx",
      "args": ["org-roam-mcp"]
    }
  }
}
```

Or for local development:

```json
{
  "mcpServers": {
    "org-roam": {
      "command": "python",
      "args": ["-m", "org_roam_mcp.server"],
      "env": {
        "ORG_ROAM_DB_PATH": "/path/to/your/org-roam.db",
        "ORG_ROAM_DIR": "/path/to/your/org-roam"
      }
    }
  }
}
```

## Available Tools

### Search and Discovery

- **`search_nodes`**: Search for nodes by title, tags, or aliases
- **`get_node`**: Get detailed information about a specific node
- **`get_backlinks`**: Find all nodes that link to a specific node
- **`list_files`**: List all org files in your org-roam directory

### Content Creation and Modification

- **`create_node`**: Create a new org-roam node with title, content, and tags
- **`update_node`**: Update the content of an existing node
- **`add_link`**: Create a link from one node to another

## Resources

The server provides these resources for read-only access:

- **`org-roam://nodes`**: List of all nodes in your database
- **`org-roam://stats`**: Database statistics (node count, file count, etc.)
- **`org-roam://node/{id}`**: Detailed information about a specific node

## Examples

### Creating a New Note

```bash
# Through Claude Code
"Create a new org-roam note titled 'Machine Learning Concepts' with the tag 'AI'"
```

### Searching Your Knowledge Base

```bash
# Through Claude Code  
"Search my org-roam notes for anything related to 'neural networks'"
```

### Exploring Connections

```bash
# Through Claude Code
"Show me all the notes that link to my 'Deep Learning' note"
```

## Development

### Running from Source

1. Clone the repository:
```bash
git clone <repository-url>
cd org-roam-mcp
```

2. Create virtual environment and install:
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -e .
```

3. Run tests:
```bash
pytest tests/ -v
```

4. Run the server:
```bash
python -m org_roam_mcp.server
```

### Project Structure

```
org-roam-mcp/
├── src/org_roam_mcp/
│   ├── __init__.py
│   ├── server.py          # Main MCP server implementation
│   ├── database.py        # SQLite database interface
│   ├── file_manager.py    # Org file operations
│   └── config.py          # Configuration management
├── tests/
│   ├── test_database.py
│   └── test_file_manager.py
├── pyproject.toml
└── README.md
```

## File Format Compatibility

The server creates org-roam files with the standard structure:

```org
:PROPERTIES:
:ID: 12345678-1234-1234-1234-123456789abc
:END:
#+title: Your Note Title
#+filetags: :tag1: :tag2:

Your note content here...
```

## Requirements

- Python 3.10+
- Existing org-roam setup with SQLite database
- Read/write access to your org-roam directory

## Troubleshooting

### Database Not Found

If you get a "Database not found" error:

1. Check that org-roam is set up and has created a database
2. Run `M-x org-roam-db-sync` in Emacs to ensure the database exists
3. Set `ORG_ROAM_DB_PATH` environment variable to the correct path

### Permission Issues

Ensure the MCP server has read/write access to:
- Your org-roam database file
- Your org-roam directory and all subdirectories

### Integration Issues

1. Restart Claude Code after adding the MCP server configuration
2. Check Claude Code logs for any connection errors
3. Verify the server starts correctly by running it manually

## Contributing

1. Fork the repository
2. Create a feature branch
3. Add tests for new functionality
4. Ensure all tests pass
5. Submit a pull request

## License

MIT License - see LICENSE file for details
