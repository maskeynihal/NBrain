---
name: mermaid-aws-diagram
description: Converts Mermaid flowcharts into AWS architecture diagrams as Python code using mingrammer/diagrams and Graphviz. Use when the user provides a Mermaid chart and wants official AWS icons, when converting mermaid to diagrams, or when upgrading a markdown flowchart to publication-quality AWS visuals.
---

# Mermaid → AWS Architecture Diagrams

Convert Mermaid `flowchart` charts into version-controlled Python scripts that render with [mingrammer/diagrams](https://diagrams.mingrammer.com/) + Graphviz (official AWS icons).

**Input:** Mermaid `flowchart` source (inline, markdown file, or `.mmd`).
**Output:** Runnable `.py` script + PNG/SVG (same toolchain as the `aws-diagram` skill).

## Workflow

```
Task Progress:
- [ ] Step 1: Obtain and validate Mermaid source
- [ ] Step 2: Parse structure (subgraphs, nodes, edges)
- [ ] Step 3: Map nodes to AWS icons
- [ ] Step 4: Generate diagrams Python script
- [ ] Step 5: Run script and fix errors one at a time
- [ ] Step 6: Report output path; optionally keep Mermaid source alongside
```

### Step 1: Obtain and validate Mermaid source

Accept Mermaid from:
- Fenced ` ```mermaid ` blocks in markdown
- Standalone `.mmd` files
- User-pasted flowchart text

**Supported:** `flowchart` / `graph` with `TB`, `TD`, `BT`, `LR`, `RL`.

**Not supported** (ask user to simplify or draw manually with `aws-diagram`):
- `sequenceDiagram`, `classDiagram`, `erDiagram`, `stateDiagram`, `gantt`
- Mermaid `style`/`classDef` CSS (ignored; use `Cluster` colors instead)
- `&` multi-target edges (split into separate edges)

If the chart mixes AWS and non-AWS services, map externals via `diagrams.onprem.*` or `Node("Label")`.

### Step 2: Parse structure

Extract in order:

| Mermaid | diagrams equivalent |
|---------|----------------------|
| `flowchart TB` / `TD` | `direction="TB"` |
| `flowchart LR` | `direction="LR"` |
| `flowchart BT` | `direction="BT"` |
| `flowchart RL` | `direction="RL"` |
| `subgraph id["Title"]` | `with Cluster("Title"):` |
| `nodeId["Label"]` | variable + icon from label |
| `nodeId[(Label)]` | database-shaped → prefer RDS/DynamoDB |
| `A --> B` | `a >> b` |
| `A -->|text| B` | `a >> Edge(label="text") >> b` |
| `A -.-> B` | `Edge(style="dashed")` |
| `A ==> B` | `Edge(style="bold")` |
| `A --- B` | `a - b` (undirected; rare in AWS diagrams) |

**Parsing rules:**
- Use the **label text** (inside brackets), not the node ID, for icon lookup.
- Nested `subgraph` → nested `Cluster`.
- Preserve subgraph titles verbatim as cluster labels.
- Assign each node ID a unique Python variable (snake_case of ID).
- Record edges only after all nodes are defined.

### Step 3: Map nodes to AWS icons

Match label text (case-insensitive, substring) to icons. See [mermaid-mapping.md](mermaid-mapping.md) for the full keyword table.

**Priority:**
1. Exact AWS service acronym in label (`EKS`, `RDS`, `S3`)
2. Keyword match (`load balancer` → `ALB`, `postgres` → `RDS`)
3. Node shape hint: `[(...)]` → database icon
4. Fallback: `Node("Label")` with the Mermaid label text

When ambiguous (e.g. `DB`), prefer the service named in the label or ask the user.

**Imports:** collect only the icon classes actually used; group by `diagrams.aws.*` and `diagrams.onprem.*`.

### Step 4: Generate diagrams Python script

Follow `aws-diagram` conventions. Create e.g. `docs/diagrams/from-mermaid.py`.

**Mandatory defaults:**

```python
with Diagram(
    "Architecture Title",   # from Mermaid title comment or user request
    show=False,
    direction="TB",         # from Mermaid flowchart direction
    outformat=["png"],
    filename="architecture",
):
```

**Generation order:**
1. Imports + optional `COLOR_*` constants (mirror subgraph groupings)
2. Nested `Cluster` blocks (outer subgraphs first)
3. Node declarations inside appropriate clusters
4. Edges last — use labeled `Edge` for Mermaid `|label|` syntax

**Cluster styling** (when subgraphs imply environments):

```python
with Cluster("Production Account", graph_attr={"bgcolor": COLOR_PROD + "10"}):
    ...
```

Add a comment at the top linking to the source Mermaid file.

PEP 723 when `uv run` is available:

```python
# /// script
# dependencies = ["diagrams"]
# ///
```

### Step 5: Run and fix

```bash
command -v dot >/dev/null || brew install graphviz   # macOS
python3 -c "import diagrams" 2>/dev/null || pip install diagrams
python3 docs/diagrams/from-mermaid.py
```

Fix errors **one at a time**. Common issues:

| Error | Fix |
|-------|-----|
| Wrong icon for a label | Adjust keyword mapping; use `Node()` if no icon |
| `ImportError` | Correct module path; see [mermaid-mapping.md](mermaid-mapping.md) |
| Cannot connect to `Cluster` | Connect to a **node inside** the cluster |
| Operator precedence | Wrap chains: `(a >> b) >> c` not `a >> b >> c` when mixing `-` |
| Duplicate Mermaid IDs | One Python variable per ID |

### Step 6: Report output

Tell the user:
- Generated image path (e.g. `architecture.png`)
- Python script path
- Any nodes that fell back to `Node()` (no AWS icon match)
- Suggested manual tweaks if Mermaid semantics were lossy

## Design rules

- **Faithful structure** — subgraph nesting and edge topology should match Mermaid
- **Smarter icons** — upgrade generic labels (`API`, `Cache`) to appropriate AWS services when context is clear
- **Labeled edges** — preserve all Mermaid edge labels
- **Simplify if cluttered** — collapse parallel nodes or comment out non-essential subgraphs (note in script)
- **Keep both formats** — store `.mmd` or markdown source next to `.py` for diff-friendly iteration

## Operator precedence trap

`-` binds tighter than `>>`. Wrap when mixing:

```python
(a >> b) - c   # correct
a >> b - c     # parses as a >> (b - c)
```

## When to stay in Mermaid

Keep Mermaid (do not convert) when:
- User wants markdown-embeddable diagrams only
- Chart is a non-flowchart type
- No AWS services involved and icons do not matter

## Additional resources

- Mermaid syntax → diagrams mapping: [mermaid-mapping.md](mermaid-mapping.md)
- Mermaid input → Python output examples: [examples.md](examples.md)
- AWS icon imports and CI patterns: [../aws-diagram/reference.md](../aws-diagram/reference.md)
