# Obsidian CLI — Claude Code Skill

A [Claude Code](https://claude.ai/code) skill that enables AI-powered interaction with your [Obsidian](https://obsidian.md/) vaults via the built-in Obsidian CLI.

## Features

- **Create, read, and manage notes** — create, append, prepend, delete, move, rename
- **Search your vault** — full-text search with context, browse files and folders
- **Daily notes** — read, append, prepend, or get the path to today's daily note
- **Properties & tags** — set, read, and remove frontmatter properties; list and query tags
- **Tasks** — list, filter, and toggle task status across your vault
- **Templates** — insert templates and read template contents
- **Link analysis** — backlinks, outgoing links, unresolved links, dead ends, orphans
- **Plugin & theme management** — enable, disable, install, and uninstall plugins and themes
- **Commands & automation** — execute any Obsidian command by ID
- **Version history & sync** — browse, read, and restore file history and sync versions

## Requirements

- **Obsidian v1.12+** with the CLI enabled (*Settings → General → Command line interface → Register CLI*)
- **Obsidian desktop app** must be running (the CLI communicates via local IPC)
- **Claude Code** installed and configured

## Installation

### Manual install

Clone the repo and copy the skill to your Claude Code skills directory:

```bash
# Clone the repository
git clone <repo-url>

# Copy to project-level skills
mkdir -p /path/to/your-project/.claude/skills/obsidian-cli/references
cp <repo-dir>/SKILL.md /path/to/your-project/.claude/skills/obsidian-cli/
cp <repo-dir>/references/commands.md /path/to/your-project/.claude/skills/obsidian-cli/references/

# Or copy to global skills (available to all projects)
mkdir -p ~/.claude/skills/obsidian-cli/references
cp <repo-dir>/SKILL.md ~/.claude/skills/obsidian-cli/
cp <repo-dir>/references/commands.md ~/.claude/skills/obsidian-cli/references/
```

## Usage

Once installed, Claude will automatically detect Obsidian-related requests and use the CLI to interact with your vault.

### Examples

> **"Create a new note in my Obsidian vault called 'Meeting Notes' with content about the Q4 roadmap."**
> **"Search my vault for anything related to 'action items' and show the context."**
> **"Append a task to today's daily note: '- [ ] Review PR #42'."**
> **"List all incomplete tasks in my vault."**
> **"What files are in my Projects folder?"**
> **"Set the Status property to 'Done' on my Meeting Notes file."**
> **"Show me the backlinks for the file called 'Project Plan'."**

### Manual invocation

You can also manually invoke the skill with `/obsidian-cli` or `/obsidian`.

## Skill Structure

```
obsidian-cli-skill/
├── SKILL.md                    # Skill definition (YAML frontmatter + instructions)
├── references/
│   └── commands.md             # Complete command reference (80+ commands)
├── README.md                   # This file
├── LICENSE                     # MIT License
└── .gitignore
```

## Commands Reference

The skill covers **80+ commands** across these categories:

| Category | Key Commands |
|----------|-------------|
| File Operations | `create`, `read`, `append`, `prepend`, `delete`, `move`, `rename`, `file` |
| Search & Navigation | `search`, `search:context`, `open`, `random`, `recents`, `tabs` |
| Daily Notes | `daily`, `daily:read`, `daily:append`, `daily:prepend`, `daily:path` |
| Properties & Tags | `property:set/read/remove`, `properties`, `tags`, `tag` |
| Tasks | `tasks`, `task` (toggle/done/todo) |
| Templates | `templates`, `template:insert`, `template:read` |
| Links | `backlinks`, `links`, `unresolved`, `deadends`, `orphans` |
| Vault Info | `files`, `folders`, `vault`, `vaults`, `wordcount`, `outline` |
| Plugins & Themes | `plugins`, `plugin:enable/disable/install`, `themes`, `theme:set` |
| History, Sync & Diff | `history`, `sync`, `diff` — full version management |
| Aliases | `aliases` — list and query note aliases |
| Bases (Databases) | `bases`, `base:create/query/views` |
| Bookmarks | `bookmarks`, `bookmark` |
| CSS Snippets | `snippets`, `snippet:enable/disable` |
| Hotkeys | `hotkeys`, `hotkey` |
| Automation | `command`, `commands`, `workspace`, `reload`, `restart` |
| Developer | `eval`, `devtools`, `dev:cdp/console/css/debug/dom/errors/mobile/screenshot` |

See [references/commands.md](references/commands.md) for the complete reference.

## Technical Details

- **Backend**: Uses Obsidian's built-in CLI (v1.12+) — no additional plugins or tools required
- **Communication**: Local IPC — Obsidian app must be running
- **Platform support**: macOS, Windows, Linux
- **Skill type**: Knowledge skill — Claude dynamically constructs commands based on natural language requests

## License

MIT

## Related

- [Obsidian](https://obsidian.md/) — the knowledge base that powers this skill
- [Obsidian CLI Documentation](https://help.obsidian.md/cli) — official CLI reference
- [Claude Code Skills](https://code.claude.com/docs/en/skills.md) — official skill documentation
