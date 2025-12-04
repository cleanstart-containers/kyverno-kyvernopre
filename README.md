# Container Documentation for Kyverno-Kyvernopre

Kyverno Pre-validation webhook container that performs policy validation checks before Kubernetes admission requests. This container is essential for implementing policy-as-code, ensuring compliance, and maintaining security standards in Kubernetes clusters. It provides pre-admission policy enforcement, validation of resources against custom policies, and integration with existing security workflows.

📌 **Base Foundation**: Security-hardened, minimal base OS designed for enterprise containerized environments from cleanstart Registry.

- *Image Path**: `ghcr.io/cleanstart-containers/kyverno-kyvernopre`  
- *Registry**: cleanstart Registry

## Key Features

Core capabilities and strengths of this container:

- Pre-admission policy validation for Kubernetes clusters
- Custom policy enforcement and compliance checking
- Integration with Kubernetes admission controllers
- Support for complex validation rules and policies

## Common Use Cases

Typical scenarios where this container excels:

- Kubernetes cluster policy enforcement
- Security compliance validation
- Resource configuration validation
- Automated policy checking in CI/CD pipelines

## Pull Latest Image

Download the container image from the registry:

```bash
docker pull ghcr.io/cleanstart-containers/kyverno-kyvernopre:latest
```

```bash
docker pull ghcr.io/cleanstart-containers/kyverno-kyvernopre:latest-dev
```

## Basic Run

Run the container with basic configuration:

```bash
docker run -it --name kyverno-pre \
  -e KYVERNO_NAMESPACE=kyverno \
  ghcr.io/cleanstart-containers/kyverno-kyvernopre:latest
```

## Production Deployment

Deploy with production security settings:

```bash
docker run -d --name kyverno-pre-prod \
  --read-only \
  --security-opt=no-new-privileges \
  --user 1000:1000 \
  -e KYVERNO_NAMESPACE=kyverno \
  ghcr.io/cleanstart-containers/kyverno-kyvernopre:latest
```

## Environment Variables

Configuration options available through environment variables:

| Variable | Default | Description |
|----------|---------|-------------|
| KYVERNO_NAMESPACE | kyverno | Namespace where Kyverno is installed |
| KYVERNO_METRICS_PORT | 9443 | Port for metrics endpoint |
| LOG_LEVEL | INFO | Logging level (DEBUG, INFO, WARNING, ERROR) |
| WEBHOOK_TIMEOUT | 10 | Webhook timeout in seconds |

## Security Best Practices

Recommended security configurations and practices:

- Use specific image tags for production deployments
- Implement proper RBAC policies
- Enable TLS for webhook communications
- Regular security scanning of policies
- Monitor policy validation metrics
- Implement proper backup strategies for policies

## Kubernetes Security Context

Recommended security context for Kubernetes deployments:

```yaml
securityContext:
  runAsNonRoot: true
  runAsUser: 1000
  runAsGroup: 1000
  readOnlyRootFilesystem: true
  allowPrivilegeEscalation: false
  capabilities:
    drop: ["ALL"]
```

## Resources

- **Official Documentation:** https://kyverno.io/docs/
- **Provenance / SBOM / Signature:** https://images.cleanstart.com/images/kyverno-kyvernopre
- **Docker Hub:** https://hub.docker.com/r/cleanstart/kyverno-kyvernopre
- **CleanStart All Images:** https://images.cleanstart.com
- **CleanStart Community Images:** https://hub.docker.com/u/cleanstart

### Vulnerability Disclaimer

CleanStart offers Docker images that include third-party open-source libraries and packages maintained by independent contributors. While CleanStart maintains these images and applies industry-standard security practices, it cannot guarantee the security or integrity of upstream components beyond its control.

Users acknowledge and agree that open-source software may contain undiscovered vulnerabilities or introduce new risks through updates. CleanStart shall not be liable for security issues originating from third-party libraries, including but not limited to zero-day exploits, supply chain attacks, or contributor-introduced risks.

Security remains a shared responsibility: CleanStart provides updated images and guidance where possible, while users are responsible for evaluating deployments and implementing appropriate controls.
