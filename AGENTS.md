# json-canvas-skill

Create and edit JSON Canvas files (.canvas) with nodes, edges, groups, and connections for Obsidian and other infinite canvas tools.

## Activation Triggers

Activate when the user mentions any of:
- `canvas`, `.canvas`, `JSON Canvas`
- `infinite canvas`, `Obsidian canvas`
- `mind map`, `flowchart`, `board` (in canvas context)

## Usage

Invoke with `/json-canvas-skill` or describe a canvas creation task. The skill generates valid JSON Canvas files with proper node types, edge connections, and layout.

## Supported Node Types

Text, File, Link, Group — with color presets, z-index ordering, and edge labels.

## Gotchas

- IDs must be exactly 16-char lowercase hex; other formats break Obsidian import
- Edges referencing non-existent node IDs are silently ignored
- Group nodes do NOT auto-contain children — position them manually inside bounds
- `file` paths are relative to vault root, not the `.canvas` file location
- Negative coordinates are valid but some viewers clip them

## Details

See [SKILL.md](./SKILL.md) for full specification, attributes, and validation rules.

Read [references/examples.md](references/examples.md) for complete canvas examples.
