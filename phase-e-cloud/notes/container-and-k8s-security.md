# 🐳 Container & Kubernetes Security (Docker/K8s)

> **Phase:** [phase-e-cloud](../) | **Tags:** `#docker` `#kubernetes` `#containers` `#breakout`  
> **Focus:** Docker socket mounts, Privileged containers, K8s RBAC

---

## 📑 Attack Vectors
- Docker Socket Escape: `/var/run/docker.sock` mounted inside container
- Privileged Container Escape (`--privileged`): Kernel module load or cgroups release_agent
- Kubernetes Service Account Tokens: `/var/run/secrets/kubernetes.io/serviceaccount/token`
