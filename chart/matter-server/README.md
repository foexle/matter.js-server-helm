# Matter Server Helm Chart

A Helm chart for deploying [Python Matter Server](https://github.com/matter-js/python-matter-server) on Kubernetes.

## Prerequisites

- Kubernetes 1.19+
- Helm 3.0+
- IPv6 enabled on cluster nodes
- Host network access (required for Matter mDNS)

## Installation

```bash
helm install matter-server ./matter-server

# With custom values
helm install matter-server ./matter-server -f my-values.yaml
```

## Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `image.repository` | Container image repository | `ghcr.io/matter-js/python-matter-server` |
| `image.tag` | Container image tag | `stable` |
| `hostNetwork` | Use host network (required) | `true` |
| `persistence.enabled` | Enable persistent storage | `true` |
| `persistence.size` | PVC size | `1Gi` |
| `matterServer.port` | WebSocket port | `5580` |
| `matterServer.bluetoothAdapter` | Bluetooth adapter (-1 to disable) | `-1` |
| `nodeSelector` | Node labels for pod assignment | `{}` |
| `usbDevices` | USB devices to pass through | `[]` |

## With Bluetooth Support

```bash
helm install matter-server ./matter-server \
  --set matterServer.bluetoothAdapter=0 \
  --set securityContext.privileged=true
```
