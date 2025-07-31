# 🧱 K8-Hardway: Automated Kubernetes the Hard Way

**K8-Hardway** is a DevOps infrastructure project that automates the complete "Kubernetes the Hard Way" workflow using **Jenkins**, **Terraform**, **Ansible**, and **GCP**. It encapsulates best practices in Infrastructure as Code, secure cloud provisioning, CI/CD orchestration, and fault-tolerant distributed system design.

> 📌 No packaged installers. No shortcuts. Just pure automation of the manual, hard way.

---

## 🗺️ Project Overview

This system automates:
- GCP infrastructure provisioning via reusable Terraform modules
- Certificate generation, TLS bootstrapping, and MySQL setup
- CI/CD pipeline orchestration through Jenkins
- Secure, idempotent Ansible playbooks for Kubernetes setup

---

## 🌩️ Cloud Infrastructure (Terraform + GCP)

- Modular IaC provisioning: VPC, firewall rules, controller/worker nodes, MySQL, and static IPs
- Centralized GCS bucket for PEM files & agent binaries
- Output variables propagate infra IDs across Terraform modules

---

## 🔧 CI/CD Orchestration (Jenkins)

- Jenkins master connects to provisioned GCP agents via SSH
- Agent bootstrapping via `agent.jar` with JNLP execution
- Jobs created programmatically using `jenkinsapi`
- Plugin setup (Ansible, BlueOcean) managed via install scripts
- Secrets fetched and authenticated via secure WebSocket connections

---

## 🔐 Secure Communication (PKI + TLS)

- **CFSSL** installed via Ansible for certificate/key generation
- Private credentials managed and distributed via `gsutil` securely
- TLS authentication for controller ↔ agent communication

---

## 🧪 Infrastructure Automation (Ansible)

- YAML playbooks install:
    - Kubernetes CLI tools (`kubectl`)
    - CFSSL binaries
- Targeted `k8main` group with elevated privileges
- Supports idempotency across reruns and failure states

---

## 🪄 CI Job Execution Flow

| Job Name           | Description                                              |
|--------------------|----------------------------------------------------------|
| `install-k8`       | Bootstraps cluster binaries and tools                    |
| `create-certificates`| Generates PKI materials for Kubernetes components     |
| `create-etcd`      | Initializes secure etcd cluster                          |
| `create-controllers`| Deploys and configures controller nodes                |
| `create-rbac`      | Sets up role-based access control                        |
| `create-workers`   | Deploys worker nodes and connects them to the cluster   |

- Jobs defined in XML, executed sequentially
- Built-in fail-fast logic halts pipeline on error
- Artifacts pulled from central GCS bucket via SSH + `gsutil`

---

## 📁 Technologies Used

| Tool           | Role                            |
|----------------|----------------------------------|
| **Go**         | TLS tools & shell utilities      |
| **Terraform**  | Cloud resource provisioning      |
| **Ansible**    | Configuration management         |
| **Jenkins**    | CI/CD pipeline orchestration     |
| **GCP**        | Infrastructure backend           |
| **CFSSL**      | Certificate generation           |
| **gsutil**     | Artifact distribution            |

---

## 📃 License

This project is licensed under the [MIT License](LICENSE).
