---
name: json-canvas-skill
description: Create and edit JSON Canvas files (.canvas) with nodes, edges, groups, and connections. Use when working with .canvas files, creating visual canvases, mind maps, flowcharts, or when the user mentions Canvas files in Obsidian. Triggers on "canvas", "JSON Canvas", ".canvas", "infinite canvas", "Obsidian canvas".
license: MIT
activation: /json-canvas-skill
provenance:
  maintainer: pix
  version: 1.1.0
  created: 2026-09-21
  source_references:
    - references/examples.md
metadata:
  version: 1.1.0
  author: pix
  created: 2026-09-21
  last_reviewed: 2026-09-21
  review_interval_days: 90
---

# /json-canvas-skill

Create and edit valid JSON Canvas files (`.canvas`) used in Obsidian and other applications.

## When to Use

- User asks to create a `.canvas` file
- User wants to build a visual mind map, flowchart, or board in Obsidian
- User mentions JSON Canvas, infinite canvas, or Obsidian canvas
- User provides a `.canvas` file to edit or extend

## Workflow

1. Determine canvas purpose (mind map, board, flowchart, research map)
2. Design node layout with appropriate types and positions
3. Define edges with correct from/to references and optional labels
4. Generate valid JSON with unique 16-char hex IDs
5. Save as `<name>.canvas` in the target directory

## File Structure

A canvas file contains two top-level arrays:

```json
{
  "nodes": [],
  "edges": []
}
```

- `nodes` (optional): Array of node objects
- `edges` (optional): Array of edge objects connecting nodes

## Node Types

Four node types are supported:

| Type | Purpose | Key Attributes |
|------|---------|----------------|
| `text` | Markdown content | `text` (required) |
| `file` | Reference to files/attachments | `file` (required), `subpath` (optional) |
| `link` | External URL | `url` (required) |
| `group` | Visual container for other nodes | `label`, `background`, `backgroundStyle` |

### Z-Index Ordering

Nodes are ordered by z-index in the array:

- First node = bottom layer (displayed below others)
- Last node = top layer (displayed above others)

### Generic Node Attributes

All nodes share these attributes:

| Attribute | Required | Type | Description |
|-----------|----------|------|-------------|
| `id` | Yes | string | Unique identifier for the node |
| `type` | Yes | string | Node type: `text`, `file`, `link`, or `group` |
| `x` | Yes | integer | X position in pixels |
| `y` | Yes | integer | Y position in pixels |
| `width` | Yes | integer | Width in pixels |
| `height` | Yes | integer | Height in pixels |
| `color` | No | canvasColor | Node color (see Color section) |

### Text Nodes

Text nodes contain Markdown content.

```json
{
  "id": "6f0ad84f44ce9c17",
  "type": "text",
  "x": 0,
  "y": 0,
  "width": 400,
  "height": 200,
  "text": "# Hello World\n\nThis is **Markdown** content."
}
```

| Attribute | Required | Type | Description |
|-----------|----------|------|-------------|
| `text` | Yes | string | Plain text with Markdown syntax |

### File Nodes

File nodes reference files or attachments (images, videos, PDFs, notes, etc.).

```json
{
  "id": "a1b2c3d4e5f67890",
  "type": "file",
  "x": 500,
  "y": 0,
  "width": 400,
  "height": 300,
  "file": "Attachments/diagram.png"
}
```

```json
{
  "id": "b2c3d4e5f6789012",
  "type": "file",
  "x": 500,
  "y": 400,
  "width": 400,
  "height": 300,
  "file": "Notes/Project Overview.md",
  "subpath": "#Implementation"
}
```

| Attribute | Required | Type | Description |
|-----------|----------|------|-------------|
| `file` | Yes | string | Path to file within the system |
| `subpath` | No | string | Link to heading or block (starts with `#`) |

### Link Nodes

Link nodes display external URLs.

```json
{
  "id": "c3d4e5f678901234",
  "type": "link",
  "x": 1000,
  "y": 0,
  "width": 400,
  "height": 200,
  "url": "https://obsidian.md"
}
```

| Attribute | Required | Type | Description |
|-----------|----------|------|-------------|
| `url` | Yes | string | External URL |

### Group Nodes

Group nodes are visual containers for organizing other nodes.

```json
{
  "id": "d4e5f6789012345a",
  "type": "group",
  "x": -50,
  "y": -50,
  "width": 1000,
  "height": 600,
  "label": "Project Overview",
  "color": "4"
}
```

```json
{
  "id": "e5f67890123456ab",
  "type": "group",
  "x": 0,
  "y": 700,
  "width": 800,
  "height": 500,
  "label": "Resources",
  "background": "Attachments/background.png",
  "backgroundStyle": "cover"
}
```

| Attribute | Required | Type | Description |
|-----------|----------|------|-------------|
| `label` | No | string | Text label for the group |
| `background` | No | string | Path to background image |
| `backgroundStyle` | No | string | Background rendering style |

#### Background Styles

| Value | Description |
|-------|-------------|
| `cover` | Fills entire width and height of node |
| `ratio` | Maintains aspect ratio of background image |
| `repeat` | Repeats image as pattern in both directions |

## Edges

Edges are lines connecting nodes.

```json
{
  "id": "f67890123456789a",
  "fromNode": "6f0ad84f44ce9c17",
  "toNode": "a1b2c3d4e5f67890"
}
```

```json
{
  "id": "0123456789abcdef",
  "fromNode": "6f0ad84f44ce9c17",
  "fromSide": "right",
  "fromEnd": "none",
  "toNode": "b2c3d4e5f6789012",
  "toSide": "left",
  "toEnd": "arrow",
  "color": "1",
  "label": "leads to"
}
```

| Attribute | Required | Type | Default | Description |
|-----------|----------|------|---------|-------------|
| `id` | Yes | string | - | Unique identifier for the edge |
| `fromNode` | Yes | string | - | Node ID where connection starts |
| `fromSide` | No | string | - | Side where edge starts |
| `fromEnd` | No | string | `none` | Shape at edge start |
| `toNode` | Yes | string | - | Node ID where connection ends |
| `toSide` | No | string | - | Side where edge ends |
| `toEnd` | No | string | `arrow` | Shape at edge end |
| `color` | No | canvasColor | - | Line color |
| `label` | No | string | - | Text label for the edge |

### Side Values

| Value | Description |
|-------|-------------|
| `top` | Top edge of node |
| `right` | Right edge of node |
| `bottom` | Bottom edge of node |
| `left` | Left edge of node |

### End Shapes

| Value | Description |
|-------|-------------|
| `none` | No endpoint shape |
| `arrow` | Arrow endpoint |

## Colors

The `canvasColor` type can be specified in two ways:

### Hex Colors

```json
{
  "color": "#FF0000"
}
```

### Preset Colors

```json
{
  "color": "1"
}
```

| Preset | Color |
|--------|-------|
| `"1"` | Red |
| `"2"` | Orange |
| `"3"` | Yellow |
| `"4"` | Green |
| `"5"` | Cyan |
| `"6"` | Purple |

Note: Specific color values for presets are intentionally undefined, allowing applications to use their own brand colors.

## Validation Rules

1. All `id` values must be unique across nodes and edges
2. `fromNode` and `toNode` must reference existing node IDs
3. Required fields must be present for each node type
4. `type` must be one of: `text`, `file`, `link`, `group`
5. `backgroundStyle` must be one of: `cover`, `ratio`, `repeat`
6. `fromSide`, `toSide` must be one of: `top`, `right`, `bottom`, `left`
7. `fromEnd`, `toEnd` must be one of: `none`, `arrow`
8. Color presets must be `"1"` through `"6"` or valid hex color

## Gotchas

- IDs must be exactly 16-character lowercase hex strings; other formats silently break Obsidian import
- `edges` that reference non-existent node IDs are silently ignored, not errored
- Group nodes do NOT automatically contain child nodes — you must position children inside the group's bounds manually
- Canvas coordinates use pixels, not grid units; misalignment is common when mixing manual and generated positions
- `file` paths are relative to the vault root, not the `.canvas` file location
- Negative coordinates are valid but some viewers clip them; prefer positive origins for portability
- Empty `nodes` and `edges` arrays produce a valid but invisible canvas

## References

- [JSON Canvas Spec 1.0](https://jsoncanvas.org/spec/1.0/)
- [JSON Canvas GitHub](https://github.com/obsidianmd/jsoncanvas)
- Read `references/examples.md` for complete canvas examples (Simple Canvas, Project Board, Research Canvas, Flowchart)
