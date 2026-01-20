# Camel Dashboard All-in-One Helm Chart

This is an umbrella Helm chart that deploys the complete Camel Dashboard solution, including:
- **Camel Dashboard Operator**: Manages Camel Dashboard instances
- **Camel Dashboard Console**: OpenShift Console plugin for Camel Dashboard
- **Hawtio Online Console Plugin**: Management and monitoring console for Java applications

## Prerequisites

- Kubernetes 1.19+
- Helm 3.0+
- OpenShift 4.10+ (for console plugin)

## Installation

### 1. Add the Helm Repository

```bash
helm repo add camel-dashboard https://camel-tooling.github.io/camel-dashboard/charts
helm repo update
```

### 2. Install the Chart

```bash
helm install camel-dashboard camel-dashboard/camel-dashboard-all -n camel-dashboard --create-namespace
```

Or install from source:

```bash
# Update dependencies first
helm dependency update

# Then install
helm install camel-dashboard . -n camel-dashboard --create-namespace
```

### 3. Customizing Installation

You can customize the installation by providing your own values:

```bash
helm install camel-dashboard camel-dashboard/camel-dashboard-all -n camel-dashboard --create-namespace -f my-values.yaml
```

## Configuration

### Enabling/Disabling Components

You can selectively enable or disable components:

```yaml
# values.yaml
camel-dashboard-operator:
  enabled: true  # Set to false to skip operator installation

camel-dashboard-console:
  enabled: true  # Set to false to skip console installation

hawtio-online-console-plugin:
  enabled: true  # Set to false to skip hawtio installation
```

### Operator Configuration

Override operator-specific values:

```yaml
camel-dashboard-operator:
  enabled: true
  operator:
    image: quay.io/camel-tooling/camel-dashboard-operator:latest
    logLevel: "debug"
    global: true
```

### Console Configuration

Override console-specific values:

```yaml
camel-dashboard-console:
  enabled: true
  plugin:
    image: quay.io/camel-tooling/camel-dashboard-console:0.2.2

  # Configure RBAC for viewing Camel apps
  camelAppRbac:
    - namespace: my-camel-apps
      subjects:
        - apiGroup: rbac.authorization.k8s.io
          kind: Group
          name: system:authenticated
```

### Hawtio Configuration

Override hawtio-specific values:

```yaml
hawtio-online-console-plugin:
  enabled: true
  plugin:
    image:
      name: quay.io/hawtio/online-console-plugin
      tag: 0.6.1
  gateway:
    image:
      name: quay.io/hawtio/online-console-plugin-gateway
      tag: 0.6.1
```

## Uninstalling

```bash
helm uninstall camel-dashboard -n camel-dashboard
```

## Development

### Testing Locally

```bash
# Lint the chart
helm lint .

# Test template rendering
helm template camel-dashboard . --debug

# Dry run installation
helm install camel-dashboard . --dry-run --debug
```

### Updating Dependencies

When the dependent charts change, update the dependencies:

```bash
helm dependency update
```

## More Information

- [Camel Dashboard Documentation](https://camel-tooling.github.io/camel-dashboard/)
- [Camel Dashboard Repository](https://github.com/camel-tooling/camel-dashboard)
- [Camel Dashboard Operator](https://github.com/camel-tooling/camel-dashboard-operator)
- [Camel Dashboard Console](https://github.com/camel-tooling/camel-dashboard-console)
- [Hawtio Console Plugin](https://github.com/hawtio/hawtio-online)
- [Apache Camel](https://camel.apache.org/)
