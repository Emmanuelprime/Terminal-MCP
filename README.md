# Agent Terminal

Agent Terminal is a [Model Context Protocol (MCP)](https://modelcontextprotocol.io/)
server that gives an MCP client tools for running shell commands and Python code,
searching for files, and reading or writing files and folders.

The server communicates over **stdio**. It is built with
[FastMCP](https://github.com/jlowin/fastmcp) and requires Python 3.10 or later.

## Tools

| Tool | Parameters | Description |
| --- | --- | --- |
| `bash` | `command` | Runs a shell command and returns its output. |
| `python_code` | `code` | Runs Python code and returns its output. |
| `python_file` | `file` | Runs the specified Python file and returns its output. |
| `glob` | `pattern` | Returns paths matching a glob pattern. |
| `grep` | `pattern`, `file` | Searches a file for matching lines using the Windows `findstr` command. |
| `read_file` | `file` | Reads and returns a file's contents. |
| `write_file` | `file`, `content` | Writes content to a file, creating or replacing it. |
| `create_folder` | `folder` | Creates a folder, including any missing parent folders. |
| `delete_folder` | `folder` | Recursively deletes a folder and its contents. |

## Installation

Clone the repository and install it in a virtual environment. For example, with
[uv](https://docs.astral.sh/uv/):

```bash
git clone https://github.com/Emmanuelprime/Terminal-MCP.git
cd Terminal-MCP
uv sync
```

The `agent_terminal` command is installed by the project. You can also launch
the server from the project directory with:

```bash
uv run agent_terminal
```

## MCP client configuration

Configure your MCP client to start the server as a stdio subprocess. For
example, a client that accepts an `mcpServers` configuration can use:

```json
{
  "mcpServers": {
    "agent-terminal": {
      "command": "uv",
      "args": [
        "run",
        "--directory",
        "/absolute/path/to/Terminal-MCP",
        "agent_terminal"
      ]
    }
  }
}
```

Replace `/absolute/path/to/Terminal-MCP` with the directory where you cloned
the repository. If you install the package in the environment used by your MCP
client, you can instead set `command` to `agent_terminal` and omit `args`.

## Security

This server provides powerful access to the machine on which it runs. The
`bash` tool executes commands through a shell, the Python tools execute code,
and the file tools can modify or recursively delete data. Only connect it to
trusted agents, and run it with an account and working environment that do not
have access to sensitive files or systems. Review tool requests before
approving them, and avoid using it in environments where untrusted users can
invoke the server.

## Development

Install the project and its dependencies with `uv sync`, then start the server
using `uv run agent_terminal`. The project entry point is
`agent_terminal.main:main`.
