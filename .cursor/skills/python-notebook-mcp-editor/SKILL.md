---
name: python-notebook-mcp-editor
description: Edit and manage Jupyter notebooks using the user-python-notebook-mcp server. Use when the user asks to create, read, or modify .ipynb files, notebook cells, or notebook outputs.
---

# Python Notebook MCP Editor

## Goal
Use MCP notebook tools (not raw JSON editing) for notebook work whenever possible.

## Trigger
Apply this skill when the user asks to:
- edit a Jupyter notebook (`.ipynb`)
- add or rewrite notebook cells
- inspect notebook cell outputs
- create a new notebook

## Required MCP servers
- `user-python-notebook-mcp` (primary notebook operations)
- `user-sequential-thinking` (for non-trivial edit plans)
- `user-desktop-commander` (optional file inspection helpers)
- `user-basic-memory` (optional: store repeatable notebook workflow notes)

## Workflow
1. **Plan briefly for non-trivial requests**
   - Use `user-sequential-thinking` to break the notebook task into steps.
2. **Initialize notebook MCP workspace first**
   - Call `initialize_workspace` with the project directory before any notebook tool.
3. **Discover notebook and target cells**
   - Use `list_notebooks` to find notebooks.
   - Use `read_notebook` or `read_cell` to locate cell indexes and content.
4. **Apply edits with MCP notebook tools**
   - Use `edit_cell` for existing cells.
   - Use `add_cell` for new cells.
   - Use `create_notebook` for new notebook files.
5. **Validate**
   - Re-read changed cells with `read_cell`.
   - If outputs matter, use `read_cell_output` or `read_notebook_outputs`.

## Tool usage notes
- Prefer notebook-aware MCP tools over direct file patching for `.ipynb`.
- Keep edits minimal and scoped to requested cells.
- Use exact cell indexes; confirm index before editing.
- If asked to make many related edits, do them incrementally and verify after each group.

## Suggested command sequence (conceptual)
1. `initialize_workspace(project_dir)`
2. `list_notebooks(directory=".")`
3. `read_notebook(filepath=...)` (or `read_cell`)
4. `edit_cell(...)` / `add_cell(...)`
5. `read_cell(...)` and `read_cell_output(...)` for verification

## Fallback rule
If notebook MCP is unavailable, explain the limitation and ask for permission before using a non-MCP fallback.
