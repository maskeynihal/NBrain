---
name: aws-diagram
description: Creates AWS architecture diagrams as Python code using the mingrammer/diagrams library and Graphviz. Use when the user asks to draw, generate, or update AWS architecture diagrams, cloud infrastructure visuals, or diagram-as-code; when working with diagrams.aws imports; or when following the Diatom Labs / Cursor diagram workflow.
---

# AWS Architecture Diagrams

Generate professional AWS diagrams as version-controlled Python scripts using [mingrammer/diagrams](https://diagrams.mingrammer.com/) + Graphviz.

**Primary tool:** Python `diagrams` library (not draw.io, Lucidchart, or manual icon placement).
**Fallback:** Mermaid flowcharts for quick account/VPC overviews when official icons are not needed.

## Workflow

```
Task Progress:
- [ ] Step 1: Gather requirements
- [ ] Step 2: Verify prerequisites
- [ ] Step 3: Scaffold diagram script
- [ ] Step 4: Build iteratively (accounts → VPCs → services → edges)
- [ ] Step 5: Run script and fix errors one at a time
- [ ] Step 6: Report output path
```

### Step 1: Gather requirements

Before writing code, clarify:

- **Scope:** Which accounts, environments, regions, or layers to show
- **Audience:** DevOps (full topology) vs management (simplified data flow)
- **Components:** Compute, database, network, CI/CD, security, monitoring
- **Layout:** Account clusters, VPC/subnet nesting, external systems (GitHub, Datadog)

If the user has architecture docs, extract a requirements outline first. Prefer showing **data flow** over exhaustive topology. Split complex systems into multiple diagram files.

### Step 2: Verify prerequisites

```bash
command -v dot >/dev/null || brew install graphviz   # macOS
python3 -c "import diagrams" 2>/dev/null || pip install diagrams
```

Without Graphviz (`dot`), scripts fail with cryptic errors. Always verify first.

### Step 3: Scaffold diagram script

Create a `.py` file in the user's working directory (e.g. `docs/diagrams/architecture.py` or project root).

**Mandatory defaults:**

```python
with Diagram(
    "Architecture Title",
    show=False,           # ALWAYS — non-interactive, CI-safe
    direction="TB",       # TB for layered; LR for pipelines
    outformat=["png"],    # or ["png", "svg"] for docs
    filename="architecture",
):
    ...
```

Use PEP 723 for standalone scripts when `uv run` is available:

```python
# /// script
# dependencies = ["diagrams"]
# ///
```

### Step 4: Build iteratively

Follow the [Diatom Labs Cursor workflow](https://blog.diatomlabs.com/creating-aws-architecture-diagrams-with-python-and-cursor-a-step-by-step-guide-c88a0aa16298):

1. **Imports + color constants** — define `COLOR_*` hex values for account/env groups
2. **External actors** — `Users`, GitHub, CI/CD outside AWS
3. **Account clusters** — one `Cluster` per AWS account with `graph_attr={"bgcolor": COLOR + "10"}`
4. **VPC nesting** — public/private subnets, compute, data groups
5. **Edges last** — connect nodes with labeled `Edge` objects
6. **Simplify** — comment out non-essential envs if the diagram is cluttered

**Cluster styling pattern:**

```python
with Cluster("Production Account", graph_attr={"bgcolor": COLOR_PROD + "10"}):
    account = Node("Account")  # represent account boundary as a node
    with Cluster("VPC", graph_attr={"bgcolor": COLOR_NETWORK + "20"}):
        ...
```

**Edge pattern:**

```python
lb >> Edge(label="HTTPS", color="darkgreen") >> web
monitor >> Edge(color=COLOR_MON, style="dashed", label="Metrics") >> eks
```

### Step 5: Run and fix

```bash
python3 architecture.py
```

Fix errors **one at a time**, re-run after each fix. Common issues:

| Error | Fix |
|-------|-----|
| `ImportError` for a service icon | Check correct module path; use `Node("Label")` if no icon exists |
| Cannot connect to `Cluster` | Connect to a **node inside** the cluster, not the cluster itself |
| `dot` not found | Install Graphviz |
| Duplicate nodes | Assign shared resources to variables |

### Step 6: Report output

Tell the user the generated file path (e.g. `architecture.png` in the script's working directory).

## Design rules

- **Show data flow**, not every resource — ALB → EKS → RDS beats listing every security group
- **Nested clusters** for accounts, VPCs, subnets, logical groups
- **Meaningful edge labels** — protocol, purpose, or data type
- **Color coding** — consistent palette per account/environment/security domain
- **Version control** — keep `.py` diagram sources in the repo alongside infrastructure code
- **Direction:** `TB` for hierarchical multi-account; `LR` for request/response pipelines

## Operator precedence trap

`-` binds tighter than `>>` in Python. Wrap when mixing:

```python
(A >> B) - C   # correct
A >> B - C     # parses as A >> (B - C)
```

## When to use Mermaid instead

Use Mermaid for quick account-relationship sketches or when the user explicitly wants markdown-embeddable flowcharts. Use `diagrams` when official AWS icons and publication-quality output matter.

## Additional resources

- AWS icon imports and categories: [reference.md](reference.md)
- Full multi-account example patterns: [examples.md](examples.md)
- Official docs: https://diagrams.mingrammer.com/docs/guides/diagram
