# AWS Diagrams Reference

## Import paths

Icons live under `diagrams.aws.<category>`. Common categories:

| Category | Common imports |
|----------|----------------|
| `compute` | `EC2`, `Lambda`, `ECS`, `EKS`, `ECR`, `Fargate`, `Batch` |
| `database` | `RDS`, `Dynamodb`, `ElastiCache`, `Aurora` |
| `network` | `VPC`, `ALB`, `ELB`, `NLB`, `CloudFront`, `Route53`, `NATGateway`, `InternetGateway`, `ClientVpn`, `Endpoint`, `APIGateway` |
| `storage` | `S3`, `EFS`, `EBS`, `Glacier` |
| `security` | `IAM`, `IdentityAndAccessManagementIam`, `WAF`, `Shield`, `Guardduty`, `SecretsManager`, `KMS` |
| `management` | `Organizations`, `Cloudtrail`, `Cloudwatch`, `Config`, `SystemsManager`, `TrustedAdvisor` |
| `integration` | `SQS`, `SNS`, `Eventbridge`, `StepFunctions` |
| `devtools` | `Codebuild`, `Codepipeline`, `Codecommit` |
| `general` | `Users`, `Client`, `InternetAlt1` |

## On-prem / hybrid imports

| Module | Common imports |
|--------|----------------|
| `diagrams.onprem.vcs` | `Github`, `Gitlab` |
| `diagrams.onprem.ci` | `GithubActions`, `Jenkins`, `GitlabCI` |
| `diagrams.onprem.gitops` | `ArgoCD`, `Flux` |
| `diagrams.onprem.monitoring` | `Datadog`, `Splunk`, `Prometheus`, `Grafana` |
| `diagrams.onprem.client` | `Users` |

**Common import fix:** `ArgoCD` is in `diagrams.onprem.gitops`, not `diagrams.onprem.ci`.

## Diagram context options

```python
with Diagram(
    name="Title",              # default filename (slugified)
    filename="output",         # override filename (no extension)
    direction="TB",            # LR | RL | TB | BT
    curvestyle="spline",       # spline | ortho | curved | polyline
    outformat="png",           # png | jpg | svg | pdf | dot | list
    show=False,
    strict=False,              # merge duplicate edges
    graph_attr={},             # graphviz graph attrs
    node_attr={},
    edge_attr={},
):
```

Useful `graph_attr` values:

- `{"bgcolor": "transparent"}` — embed in docs/slides
- `{"concentrate": "true", "splines": "spline"}` — reduce edge clutter

## Edge attributes

```python
Edge(label="text", color="#hex or X11 name", style="solid|dashed|dotted|bold")
```

## Custom / missing icons

```python
from diagrams import Node
from diagrams.custom import Custom

generic = Node("External Secrets\nOperator")
custom = Custom("My Service", "./icons/service.png")  # 256x256 PNG
```

## Cluster limitations

- **Cannot** use `>>` to connect to a `Cluster` object
- **Can** connect to nodes defined inside a cluster
- Use `Node("Account")` inside account clusters as a connection anchor

## Color palette template

```python
COLOR_MANAGEMENT = "#009688"
COLOR_DEV        = "#03A9F4"
COLOR_STAGING    = "#FFC107"
COLOR_PROD       = "#F44336"
COLOR_NETWORK    = "#8BC34A"
COLOR_COMPUTE    = "#673AB7"
COLOR_DATABASE   = "#FF9800"
COLOR_FRONTEND   = "#9C27B0"
COLOR_CICD       = "#795548"
COLOR_MONITORING = "#607D8B"
COLOR_SECURITY   = "#FF5722"
COLOR_USER       = "#000000"
```

Apply with `graph_attr={"bgcolor": COLOR_PROD + "10"}` for subtle fills (`10`–`30` alpha suffix on hex).

## Data flow operators

```python
a >> b              # directed left-to-right
a << b              # directed right-to-left
a - b               # undirected
a >> [b, c, d]      # fan-out
a >> b >> c         # chain
```

## Discovering available icons

```bash
python3 -c "from diagrams.aws import compute; print(dir(compute))"
```

Or browse: https://diagrams.mingrammer.com/docs/nodes/aws

## CI/CD integration

```yaml
# Example GitHub Actions step
- run: |
    sudo apt-get install -y graphviz
    pip install diagrams
    python3 docs/diagrams/architecture.py
- uses: actions/upload-artifact@v4
  with:
    name: architecture-diagram
    path: docs/diagrams/*.png
```
