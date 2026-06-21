# Kubernetes

Setup files for a Kubernetes cluster on CentOS 7.

## Files

- `kubernetes.repo` — Yum repository definition for Kubernetes packages
- `dockerfile` — Docker image with kubelet, kubeadm, and kubectl installed
- `join.txt` — Example `kubeadm join` command to add a worker node to the cluster

## Usage

```bash
# On master node
kubeadm init

# On worker nodes (use the token from join.txt)
kubeadm join <master-ip>:6443 --token <token> --discovery-token-ca-cert-hash <hash>
```
