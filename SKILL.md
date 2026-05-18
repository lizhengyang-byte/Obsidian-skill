---
name: obsidian-cli
description: >-
  Interact with Obsidian vaults via the built-in Obsidian CLI (v1.12+).
  Create, read, search, and manage notes, daily notes, tasks, tags, properties,
  templates, plugins, themes, and more. Activate when the user mentions Obsidian,
  their vault, notes, or asks to interact with their knowledge base using the CLI.
allowed-tools: Bash, Read, Write
user-invocable: true
shell: bash
---

# Obsidian CLI Skill

This skill enables Claude to use the **Obsidian CLI** — the command-line interface
built into Obsidian v1.12+ — to interact with your Obsidian vault(s).

## Prerequisites

- **Obsidian v1.12 or later** installed
- **Obsidian app must be running** (the CLI communicates via local IPC)
- **CLI enabled** in Obsidian: *Settings → General → Command line interface → Register CLI*
- **`obsidian` command available** in your shell PATH

To verify:
```bash
obsidian version
```

If not found, add the Obsidian binary to your PATH:
- **macOS**: `/Applications/Obsidian.app/Contents/MacOS/obsidian`
- **Windows**: Typically added automatically after registering in settings
- **Linux**: Depends on installation method (AppImage, Snap, etc.)

## Syntax

```
obsidian <command> [options]
```

### Global Options

| Option | Description |
|--------|-------------|
| `vault=<name>` | Target a specific vault by name (required if multiple vaults) |
| `file=<name>` | Refer to a file by its name (wikilink-style resolution) |
| `path=<path>` | Refer to a file by its exact path (`folder/note.md`) |

### Important Rules

1. **File resolution**: `file=` resolves like wikilinks (by note name, first match wins);
   `path=` uses exact file paths. Most commands default to the **active file** when omitted.
2. **Quoting**: Wrap values containing spaces in quotes: `name="My Note"`.
3. **Escape sequences**: Use `\n` for newlines, `\t` for tabs in content values.
4. **Vault targeting**: If you have only one vault, `vault=` is usually optional.

## Command Reference

See `references/commands.md` for the complete list of all commands with their options.

### Quick Reference by Category

#### File Operations
| Command | Description |
|---------|-------------|
| `create name=<name> [content=] [template=] [open] [overwrite]` | Create a new note |
| `read [file=] [path=]` | Read a note's contents |
| `append [file=] content= [inline]` | Append content to a note |
| `prepend [file=] content= [inline]` | Prepend content (after frontmatter) |
| `delete [file=] [path=] [permanent]` | Delete a note |
| `move [file=] to=<path>` | Move or rename a note |
| `rename [file=] name=<name>` | Rename a note |

#### Search & Navigation
| Command | Description |
|---------|-------------|
| `search query=<text> [path=] [limit=] [case] [format=]` | Search vault contents |
| `search:context query=<text> [limit=] [case] [format=]` | Search with surrounding context |
| `open [file=] [path=] [newtab]` | Open a file in Obsidian |
| `random [folder=] [newtab]` | Open a random note |
| `recents` | List recently opened files |
| `tabs [ids]` | List open tabs |

#### Daily Notes
| Command | Description |
|---------|-------------|
| `daily [paneType=]` | Open today's daily note |
| `daily:read` | Read today's daily note |
| `daily:append content= [inline] [open]` | Append to daily note |
| `daily:prepend content= [inline] [open]` | Prepend to daily note |
| `daily:path` | Get path to daily note |

#### Properties & Tags
| Command | Description |
|---------|-------------|
| `property:set name= value= [type=] [file=]` | Set a property |
| `property:read name= [file=]` | Read a property value |
| `property:remove name= [file=]` | Remove a property |
| `properties [file=] [total] [counts] [format=]` | List properties in vault |
| `tags [file=] [total] [counts] [sort=] [format=]` | List tags |
| `tag name=<tag> [total] [verbose]` | Get tag info |

#### Tasks
| Command | Description |
|---------|-------------|
| `tasks [file=] [total] [done] [todo] [status=] [format=]` | List tasks |
| `task [file=] [line=] [toggle/done/todo] [status=]` | Update a task's status |

#### Templates
| Command | Description |
|---------|-------------|
| `templates [total]` | List available templates |
| `template:read name= [resolve] [title=]` | Read template content |
| `template:insert name=` | Insert template into active file |

#### Links
| Command | Description |
|---------|-------------|
| `backlinks [file=] [counts] [total] [format=]` | List backlinks to a file |
| `links [file=] [total]` | List outgoing links |
| `unresolved [total] [counts] [verbose] [format=]` | List unresolved links |
| `deadends [total] [all]` | Files with no outgoing links |
| `orphans [total] [all]` | Files with no incoming links |

#### Vault Info
| Command | Description |
|---------|-------------|
| `files [folder=] [ext=] [total]` | List vault files |
| `folders [folder=] [total]` | List vault folders |
| `vault [info=]` | Show vault info |
| `vaults [total] [verbose]` | List known vaults |
| `wordcount [file=] [words] [characters]` | Count words and characters |
| `outline [file=] [format=] [total]` | Show file headings |

#### Plugin & Theme Management
| Command | Description |
|---------|-------------|
| `plugins [filter=] [versions] [format=]` | List installed plugins |
| `plugin:enable id= [filter=]` | Enable a plugin |
| `plugin:disable id= [filter=]` | Disable a plugin |
| `plugin:install id= [enable]` | Install a community plugin |
| `plugin:uninstall id=` | Uninstall a plugin |
| `themes [versions]` | List installed themes |
| `theme:set name=` | Set active theme |
| `theme:install name= [enable]` | Install a community theme |
| `theme:uninstall name=` | Uninstall a theme |

#### Commands & Automation
| Command | Description |
|---------|-------------|
| `command id=<command-id>` | Execute any Obsidian command by ID |
| `commands [filter=]` | List all available commands |
| `workspace [ids]` | Show workspace tree |
| `reload` | Reload the vault |
| `restart` | Restart Obsidian |

#### History & Sync
| Command | Description |
|---------|-------------|
| `history [file=]` | List file history versions |
| `history:read [file=] version=<n>` | Read a specific version |
| `history:restore [file=] version=<n>` | Restore a specific version |
| `sync:status` | Show sync status |
| `sync [on/off]` | Pause or resume sync |

## Common Usage Patterns

### Creating notes
```bash
# Simple note
obsidian create name="Meeting Notes" content="Discussed Q4 roadmap"

# Note from a template
obsidian create name="Daily Journal" template="Journal Template" open

# Note in a specific vault
obsidian vault="Work" create name="Sprint Review" content="Sprint 42 completed"
```

### Reading and searching
```bash
# Read a specific file
obsidian read file="Meeting Notes"

# Search vault
obsidian search query="Q4 roadmap" limit=10

# Search with context
obsidian search:context query="action items" format=text
```

### Managing tasks
```bash
# List all incomplete tasks
obsidian tasks todo

# List tasks in a specific file
obsidian tasks file="Project Plan" done

# Toggle task status
obsidian task file="Project Plan" line=12 toggle
```

### Daily notes workflow
```bash
# Log to today's daily note
obsidian daily:append content="- Reviewed PR #42" inline

# Read yesterday's daily note
obsidian vault="Personal" daily:read
```

### Properties
```bash
# Add a property
obsidian property:set file="Meeting Notes" name="Status" value="Done" type=text

# Read a property
obsidian property:read file="Meeting Notes" name="Status"

# Remove a property
obsidian property:remove file="Meeting Notes" name="Old-Prop"
```

## Error Handling Tips

- **"Command not found"**: The Obsidian CLI is not in PATH or not enabled in Settings
- **"No vault specified"**: Use `vault=<name>` when multiple vaults exist or the active vault is unclear
- **"File not found"**: The file name/path doesn't exist. Try using `path=` with the full path if `file=` fails
- **"Obsidian is not running"**: The CLI requires the Obsidian desktop app to be open
- **"Cannot find file"**: Check spelling or use `files` to list available files first

## Slash Commands

When invoked via `/obsidian-cli`, the skill provides general guidance on using the
Obsidian CLI. For specific operations, just describe what you want to do in natural
language — the skill will determine the correct command automatically.
