# Managing Claude's Memory

Claude Code can remember your preferences across sessions, such as style 
guidelines and common commands in your workflow.

## Memory Locations

### User Memory

- **Location**: ```~/.claude/CLAUDE.md```
- **Purpose**: Personal preferences for all projects
- **Use Case Examples**: Code styling preferences, personal tooling shortcuts

### Project Memory

- **Location**: ```~/.CLAUDE.md```
- **Purpose**: Team-shared instructions for the project
- **Use Case Examples**: Project architecture, coding standards, common workflows

All memory files are automatically loaded into Claude Code’s context when 
launched.

### How Claude looks up memories

When you launch Claude Code, it tries to load “memories” (prompt snippets that guide the model) by walking up the directory tree from your current working directory (cwd). Here’s exactly what happens:

1. **Start at the cwd**

Claude Code first looks in the directory where you invoked it for two special files:

    - ```CLAUDE.md``` – general memories for that folder or module
    - ```CLAUDE.local.md``` – developer‑ or machine‑specific memories that shouldn’t be committed to source control

2. Walk upward one level at a time

After processing the cwd, Claude Code moves to that directory’s parent, repeats the search for the same two files, then continues moving upward again. It collects every matching file it finds along the way.

3. Stop before the filesystem root

The traversal halts as soon as it would reach the root directory ```/```; Claude Code **never** attempts to read memory files from ```/``` itself. This prevents accidentally loading global or irrelevant files.

4. Merge the memories in order

The files are read in ascending order *from the deepest directory to the highest* (i.e., cwd first, then its parent, and so on). This lets higher‑level memories act as broad defaults, while lower‑level memories can supplement or override them.

**Why this matters in large repositories**

- **Layered configuration** You can keep organization‑wide guidelines in a top‑level ```CLAUDE.md```, while individual packages or sub‑projects add more specific guidance in their own ```CLAUDE.md``` or ```CLAUDE.local.md```.

- **Minimal setup per folder** Running Claude Code from ```foo/bar/``` automatically brings in memories from both ```foo/bar/``` and ```foo/```. There’s no need to copy files or adjust paths manually.

- **Safe, predictable scope** By stopping at ```/```, Claude Code ensures it only pulls context that lives inside your project hierarchy, avoiding surprises from system‑wide files.

In short, the recursive search gives you a **hierarchical, yet bounded** way to provide both global and local context to Claude Code—perfect for complex codebases with many nested modules.
