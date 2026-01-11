# Matter Server Helm Chart

A Helm chart for deploying [Python Matter Server](https://github.com/matter-js/python-matter-server) on Kubernetes.

This Chart is inspired by the great work of the [Open Home Foundation Matter community](https://github.com/matter-js/python-matter-server) but is not part of the project and independently maintained.
There is no guarantee of stability and you're using it on your own risk!


## Prerequisites

- Kubernetes 1.19+ 
- Helm 3.0+
- IPv6 enabled on cluster nodes
- Host network access (required for Matter mDNS)

## Tested

- Raspberry Pi4
- [K0s](https://k0sproject.io/) - v1.33
- [HomeAssitant](https://www.home-assistant.io/) - 2025.12.3

## Installation

```bash
helm install matter-server oci://ghcr.io/foexle/matter-server

# With custom values
helm install matter-server oci://ghcr.io/foexle/matter-server -f my-values.yaml
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
| `securityContext.privileged` | POD security context privileged | `false` | 

## With Bluetooth Support

```bash
helm install matter-server oci://ghcr.io/foexle/matter-server \
  --set matterServer.bluetoothAdapter=0 \
  --set securityContext.privileged=true
```

## Connect to Matter Server
To connect to the Matter Server:

  1. Find the node IP where the pod is running:
    
    kubectl get pods -l "app.kubernetes.io/name=matter-server" -o wide
    

  2. Connect via WebSocket:
   
    ws://<NODE_IP>:{{ add here .Values.matterServer.port }}/ws
  


When you run on RaspberryPI or on another single device, you can use ```localhost``` instead if an IP address.


For Home Assistant integration:
  - Add the Matter integration
  - Use the WebSocket URL above as the server address

# Note

- Host Network: ENABLED (required for Matter mDNS/multicast)
- Matter requires IPv6 to be enabled on your cluster nodes
- For Thread support, ensure you have a Thread Border Router
- D-Bus is required for Bluetooth support