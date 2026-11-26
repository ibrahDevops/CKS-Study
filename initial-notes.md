# CKS Study Guide — Certified Kubernetes Security Specialist

> Personal study notes built from the **Killer Shell CKS Full Course** (YouTube, ~11h), mapped to the official CKS domains.
> Course: https://www.youtube.com/watch?v=d9xfB5qaOfg · Lab repo: https://github.com/killer-sh/cks-course-environment
>
> Format per topic: **Concept → Key points → Commands / YAML → Exam tips**. Updated as I progress through the course.

---

## Progress Tracker

| # | Course chapter | Timestamp | CKS Domain | Status |
|---|---|---|---|---|
| 1 | Introduction & Welcome | 0:00 | — | ✅ |
| 2 | K8s Security Best Practices | ~4:00 | All | ✅ |
| 3 | Create your course K8s cluster | ~24:00 | Lab | ✅ |
| 4 | Foundation – Kubernetes Secure Architecture | ~38:00 | Cluster Setup / Hardening | ✅ |
| 5 | Foundation – Containers under the hood | ~1:00:00 | System Hardening / Microservices | ✅ |
| 6 | Cluster Reset | 1:18:18 | Lab | ✅ |
| 7 | Cluster Setup – Network Policies | 1:18:53 | Cluster Setup | ✅ |
| 8 | Cluster Setup – GUI Elements | 1:46:54 | Cluster Setup | ✅ |
| 9 | K8s Docs Version | 2:02:06 | — | ✅ |
| 10 | Cluster Setup – Secure Ingress | 2:02:48 | Cluster Setup | ✅ |
| 11 | Cluster Setup – Node Metadata Protection | 2:23:54 | Cluster Setup | ✅ |
| 12 | Cluster Setup – CIS Benchmarks | 2:34:13 | Cluster Setup | ✅ |
| 13 | Cluster Setup – Verify Platform Binaries | 2:47:47 | Cluster Setup | ✅ |
| 14 | Cluster Hardening – RBAC | 2:59:32 | Cluster Hardening | ✅ |
| 15 | Cluster Hardening – Exercise caution in using ServiceAccounts | 3:31:26 | Cluster Hardening | ✅ |
| 16 | Cluster Hardening – Restrict API Access | 3:49:06 | Cluster Hardening | ✅ |
| 17 | Cluster Hardening – Upgrade Kubernetes | 4:17:25 | Cluster Hardening | ✅ |
| 18 | Microservice Vulnerabilities – Manage Kubernetes Secrets | 4:38:59 | Microservice Vulnerabilities | ✅ |
| 19 | Microservice Vulnerabilities – Container Runtime Sandboxes | 5:26:44 | Microservice Vulnerabilities | ✅ |
| 20 | Microservice Vulnerabilities – OS Level Security Domains | 5:55:20 | Microservice Vulnerabilities | ✅ |
| 21 | Microservice Vulnerabilities – mTLS | 6:12:xx | Microservice Vulnerabilities | ✅ |
| 22 | Open Policy Agent (OPA) | 6:29:xx | Microservice Vulnerabilities | ✅ |
| 23 | Supply Chain Security – Image Footprint | 7:06:xx | Supply Chain Security | ✅ |
| 24 | Supply Chain Security – Static Analysis | 7:28:xx | Supply Chain Security | ✅ |
| 25 | Supply Chain Security – Image Vulnerability Scanning | 7:52:41 | Supply Chain Security | ✅ |
| 26 | Supply Chain Security – Secure Supply Chain | 8:09:xx | Supply Chain Security | ✅ |
| 27 | Runtime Security – Behavioral Analytics at host and container level | 8:30:xx | Monitoring, Logging & Runtime | ✅ |
| 28 | Runtime Security – Immutability of containers at runtime | 9:17:xx | Monitoring, Logging & Runtime | ✅ |
| 29 | Runtime Security – Auditing | 9:34:xx | Monitoring, Logging & Runtime | ✅ |
| 30 | System Hardening – Kernel Hardening Tools (AppArmor, seccomp) | 10:06:xx | System Hardening | ⏳ |

---

## Table of Contents

0. [Exam Overview](#0-exam-overview)
1. [Security Best Practices (Foundations)](#1-security-best-practices-foundations)
2. [Lab Setup](#2-lab-setup)
3. [Kubernetes Secure Architecture & PKI](#3-kubernetes-secure-architecture--pki)
4. [Containers Under the Hood](#4-containers-under-the-hood)
5. [Domain 1 — Cluster Setup](#5-domain-1--cluster-setup)
   - [5.1 Network Policies](#51-network-policies)
   - [5.2 GUI Elements (Dashboard)](#52-gui-elements-kubernetes-dashboard)
   - [5.3 Secure Ingress](#53-secure-ingress)
   - [5.4 Node Metadata Protection](#54-node-metadata-protection)
   - [5.5 CIS Benchmarks (kube-bench)](#55-cis-benchmarks-kube-bench)
   - [5.6 Verify Platform Binaries](#56-verify-platform-binaries)
6. [Domain 2 — Cluster Hardening](#6-domain-2--cluster-hardening)
   - [6.1 RBAC](#61-rbac--role-based-access-control)
   - [6.2 Accounts: ServiceAccounts vs users](#62-accounts-serviceaccounts-vs-normal-users)
   - [6.3 Exercise Caution in Using ServiceAccounts](#63-exercise-caution-in-using-serviceaccounts)
   - [6.4 Restrict API Access](#64-restrict-api-access)
   - [6.5 Upgrade Kubernetes](#65-upgrade-kubernetes-update-frequently)
7. [Domain 3 — System Hardening](#7-domain-3--system-hardening)
   - [7.1 Kernel Hardening Tools (AppArmor & seccomp)](#71-kernel-hardening-tools-apparmor--seccomp)
8. [Domain 4 — Minimize Microservice Vulnerabilities](#8-domain-4--minimize-microservice-vulnerabilities)
   - [8.1 Manage Kubernetes Secrets](#81-manage-kubernetes-secrets)
   - [8.2 Container Runtime Sandboxes](#82-container-runtime-sandboxes)
   - [8.3 OS Level Security Domains](#83-os-level-security-domains)
   - [8.4 mTLS](#84-mtls--mutual-tls)
   - [8.5 OPA Gatekeeper](#85-open-policy-agent-opa--gatekeeper)
   - [8.x etcd Encryption at Rest — Lab Incident & Recovery](#8x-etcd-encryption-at-rest--lab-incident--recovery-from-my-lab-nov-2026)
   - [8.y Container Runtime Sandbox (gVisor) — Lab Incident](#8y-container-runtime-sandbox-gvisor--runtimeclass--lab-incident--recovery-from-my-lab-nov-2026)
   - [8.z anonymous-auth restart loop](#8z-related-incident---anonymous-authfalse--apiserver-restart-loop)
12. [Troubleshooting Index](#12-troubleshooting-index)
9. [Domain 5 — Supply Chain Security](#9-domain-5--supply-chain-security)
   - [9.1 Image Footprint](#91-image-footprint)
   - [9.2 Static Analysis (kubesec, conftest)](#92-static-analysis-of-user-workloads)
   - [9.3 Image Vulnerability Scanning (Trivy)](#93-scan-images-for-known-vulnerabilities)
   - [9.4 Secure Supply Chain](#94-secure-supply-chain)
10. [Domain 6 — Monitoring, Logging & Runtime Security](#10-domain-6--monitoring-logging--runtime-security)
   - [10.1 Behavioral Analytics (strace, /proc, Falco)](#101-behavioral-analytics-at-host-and-container-level)
   - [10.2 Immutability of Containers](#102-immutability-of-containers-at-runtime)
   - [10.3 Auditing](#103-auditing)
11. [Quick Reference](#11-quick-reference)
   - [11.1 Shell & kubectl Tricks](#111-shell--kubectl-tricks)

---

## 0. Exam Overview

### Domains & weights

| Domain | Video (older curriculum) | **Current curriculum** |
|---|---|---|
| Cluster Setup | 10% | **15%** |
| Cluster Hardening | 15% | **15%** |
| System Hardening | 15% | **10%** |
| Minimize Microservice Vulnerabilities | 20% | **20%** |
| Supply Chain Security | 20% | **20%** |
| Monitoring, Logging & Runtime Security | 20% | **20%** |

> ⚠️ The video uses the older weights. Always check the current curriculum: https://github.com/cncf/curriculum

### Exam facts
- Performance-based, 2 hours, in a remote desktop (PSI) — same style as CKA.
- **Prerequisite:** an active CKA certification.
- Registration includes **2 killer.sh simulator sessions** (36h each) — save them for the final 1–2 weeks.
- Allowed docs: kubernetes.io docs plus the docs of the tools in scope (Falco, Trivy, AppArmor, etc.).

### Course approach
**Theory + Hands-on.** Every section ends with a lab on a real cluster, and every new section starts from a **fresh cluster** (reset the cluster between sections).

---

## 1. Security Best Practices (Foundations)

### 1.1 Security is complex and a process
- Security combines many different things.
- Environments change → security **cannot stay in a fixed state**.
- **Attackers have the advantage:**
  - They decide the **time**.
  - They pick **what** to attack — usually the weakest link.

### 1.2 Security Principles
| Principle | Meaning |
|---|---|
| **Defense in Depth** | Multiple independent layers of security |
| **Least Privilege** | Give only the permissions needed, nothing more |
| **Limiting the Attack Surface** | Remove/disable everything not needed |

### 1.3 Layered defence and redundancy
- **Redundancy is good… in security.** DRY ("Don't repeat yourself") applies to code, **not** to security.
- An attacker has to break through each layer in turn:

```
Attacker ──► Reduce Attack Surface ──► Redundant Security Layer ──► Least Privilege
```

### 1.4 Kubernetes Security Categories

```
┌─────────────────────────────────┐
│  Application Security    (🐳)   │
├─────────────────────────────────┤
│  Kubernetes Cluster Security (☸)│
├─────────────────────────────────┤
│  Host Operating System Sec. (🐧)│
└─────────────────────────────────┘
```

#### Host OS Security
- K8s nodes should only do **one thing: Kubernetes**.
- Reduce the attack surface:
  - Remove unnecessary applications.
  - Keep the OS up to date.
- Use runtime security tools (e.g. Falco).
- Find and identify malicious processes.
- Restrict IAM / SSH access.

#### Kubernetes Cluster Security
- Keep K8s components **running securely and up to date**: apiserver, kubelet, etcd.
- Restrict (external) access.
- Use **Authentication → Authorization**.
- Admission controllers:
  - `NodeRestriction`
  - Custom policies (OPA / Gatekeeper)
- Enable **audit logging**.
- Security benchmarking (CIS / `kube-bench`).
- **etcd:**
  - Encrypt etcd **at rest** (EncryptionConfiguration).
  - Encrypt **traffic** to etcd (TLS).
  - **Restrict access** to etcd (only the apiserver should talk to it). A compromised pod reaching etcd directly = full cluster compromise.

#### Application Security
- Use **Secrets** — no hardcoded credentials.
- RBAC.
- Container **sandboxing** (gVisor, Kata).
- Container **hardening**:
  - Reduce the attack surface (minimal images).
  - Run as a non-root user.
  - Read-only root filesystem.
- Vulnerability scanning (Trivy).
- mTLS / service meshes.

---

## 2. Lab Setup

### 2.1 Course cluster specs (2 VMs)

| | cks-master | cks-worker |
|---|---|---|
| OS | Ubuntu (video: 20.04; use a current LTS) | same |
| Disk | 50 GB | 50 GB |
| CPU | 2 | 2 |
| RAM | 4 GB | 4 GB |
| GCP machine type | e2-medium | e2-medium |
| AWS equivalent | t3.medium | t3.medium |

### 2.2 Install (run as root on each node)

```bash
# master
sudo -i; bash <(curl -s https://raw.githubusercontent.com/killer-sh/cks-course-environment/master/cluster-setup/latest/install_master.sh)

# worker (then run the printed kubeadm join command)
sudo -i; bash <(curl -s https://raw.githubusercontent.com/killer-sh/cks-course-environment/master/cluster-setup/latest/install_worker.sh)
```

> ⚠️ **Gotcha:** use `raw.githubusercontent.com`, **not** `github.com/.../blob/...`. The blob URL returns HTML, and bash fails with `syntax error near unexpected token 'newline'` / `<!DOCTYPE html>`.

### 2.3 Make the cluster reachable from outside (NodePorts)
The course opens ports **30000–40000** so NodePort services can be reached from your laptop.

```bash
# GCP (as in the video)
gcloud compute firewall-rules create nodeports --allow tcp:30000-40000

# AWS equivalent — restrict to your own IP
aws ec2 authorize-security-group-ingress --group-id <sg-id> --protocol tcp --port 30000-40000 --cidr $(curl -s https://checkip.amazonaws.com)/32
```

### 2.4 Lab hygiene
- Reset the cluster before each new section (re-run the install scripts).
- **Always stop your instances when you stop studying.**

---

## 3. Kubernetes Secure Architecture & PKI

### 3.1 What Kubernetes does
Takes container info and:
- Runs containers in a container engine.
- Schedules containers efficiently.
- Keeps containers alive and healthy.
- Allows container communication.
- Allows common deployment techniques.
- Handles volumes.

### 3.2 Architecture

```
                Control Plane                         Data Plane
 ┌──────────────────────────────────────┐   ┌──────────────────────────┐
 │  etcd        Scheduler               │   │ node1: kubelet, kube-proxy│
 │      ▲          │                    │◄─►│        pod[app]           │
 │      └── Apiserver ◄── kubectl       │   ├──────────────────────────┤
 │          ▲        ▲                  │◄─►│ node2: kubelet, kube-proxy│
 │ Cloud Ctrl Mgr   Controller Manager  │   │        pod[app + sidecar] │
 └──────────────────────────────────────┘   └──────────────────────────┘
```
- **Everything talks through the apiserver.** Only the apiserver talks to etcd.

### 3.3 Pod-to-pod communication
- By default, **every pod can reach every other pod**, across all nodes and namespaces. There is no isolation by default.
- The CNI provides this flat network. NetworkPolicies are what restrict it (see 5.1).

### 3.4 PKI — Certificate Authority (CA)
- The CA is the **trusted root** of all certificates inside the cluster.
- **All cluster certificates are signed by the CA.**
- Components use them to **validate each other** (mutual TLS).

```
                    CA
        ┌───────────┼───────────┐
  Apiserver cert  Kubelet cert  Scheduler cert
```

### 3.5 Who talks to whom (client vs server certs)

| Connection | Cert type | Location (kubeadm defaults) |
|---|---|---|
| CA | root | `/etc/kubernetes/pki/ca.crt` + `ca.key` |
| Apiserver (server) | srv | `/etc/kubernetes/pki/apiserver.crt` |
| etcd (server) | srv | `/etc/kubernetes/pki/etcd/server.crt` |
| Apiserver → etcd | client | `/etc/kubernetes/pki/apiserver-etcd-client.crt` |
| Apiserver → kubelet | client | `/etc/kubernetes/pki/apiserver-kubelet-client.crt` |
| Scheduler → apiserver | client | embedded in `/etc/kubernetes/scheduler.conf` |
| Controller-manager → apiserver | client | embedded in `/etc/kubernetes/controller-manager.conf` |
| Kubelet → apiserver | client | `/etc/kubernetes/kubelet.conf` → `/var/lib/kubelet/pki/kubelet-client-current.pem` |
| Kubelet (server) | srv | `/var/lib/kubelet/pki/kubelet.crt` |

### 3.6 Hands-on: find & inspect certificates

```bash
# list all control-plane certs
ls -l /etc/kubernetes/pki /etc/kubernetes/pki/etcd

# expiry overview
kubeadm certs check-expiration

# inspect a cert (subject, issuer, SANs, validity)
openssl x509 -in /etc/kubernetes/pki/apiserver.crt -noout -text

# decode a client cert embedded in a kubeconfig
grep client-certificate-data /etc/kubernetes/scheduler.conf | awk '{print $2}' | base64 -d | openssl x509 -noout -subject -issuer -dates

# kubelet certs on a node
ls -l /var/lib/kubelet/pki/
```

> 💡 **Exam tip:** If you're asked which cert component X uses to reach Y, check the component's static pod manifest (`/etc/kubernetes/manifests/*.yaml`) or its kubeconfig. The flags (`--etcd-certfile`, `--kubelet-client-certificate`, …) point straight to the files.

---

## 4. Containers Under the Hood

### 4.1 Containers vs VMs

| Containers | Virtual Machines |
|---|---|
| App process runs in a "kernel group" on the **shared host kernel** | App runs on its **own guest kernel + OS** |
| **Host kernel CAN access the app process** | Host kernel can **NOT** directly access the app process |
| Lightweight, weaker isolation | Heavier, stronger isolation |

> This is why **sandboxing** (gVisor, Kata) exists: it adds a kernel layer between the container and the host kernel.

### 4.2 Kernel space vs user space

```
USER SPACE     Applications (firefox, curl)  → Libraries (glibc, libxyz)
─────────────────────────── syscall interface (getpid(), reboot(), …)
KERNEL SPACE   Linux Kernel → Hardware
```
- Every container process makes **system calls directly to the host kernel**.
- So a kernel exploit from inside a container affects the whole host.
- → Restrict syscalls with **seccomp** and **AppArmor** (System Hardening domain).

### 4.3 Linux kernel isolation

| Mechanism | Purpose | Restricts |
|---|---|---|
| **Namespaces** | Restrict what processes can **see** | Other processes, users, filesystem, network |
| **cgroups** | Restrict the **resource usage** of processes | RAM, disk, CPU |

### 4.4 Linux kernel namespaces

| Namespace | What it isolates |
|---|---|
| **PID** | Isolates processes from each other. One process cannot see others. PID 10 can exist multiple times, once per namespace |
| **Mount** | Restricts access to mounts or the root filesystem |
| **Network** | Only certain network devices; own firewall, routing rules and socket ports. Can't see all traffic or contact all endpoints |
| **User** | Different set of user IDs. User 0 in one namespace can differ from user 0 in another. **Don't use the host root user (0) inside a container** |

### 4.5 Hands-on: namespace sharing

```bash
# Docker demo from the course: c2 joins c1's PID namespace → c1 sees c2's processes
docker run --name c1 -d ubuntu sh -c 'sleep 1d'
docker run --name c2 --pid=container:c1 -d ubuntu sh -c 'sleep 999d'
docker exec c1 ps aux

# Same idea in Kubernetes (containerd): shared PID namespace inside a pod
kubectl run share --image=busybox --overrides='{"spec":{"shareProcessNamespace":true}}' -- sleep 1d

# On the node: list namespaces of running processes
lsns
```

> 💡 **Security link:** `hostPID: true`, `hostNetwork: true` and `hostIPC: true` in a pod spec **break this isolation**. They're classic exam "find the insecure pod" targets.

---

## 5. Domain 1 — Cluster Setup

### 5.1 Network Policies

#### Concept
- **Firewall rules in Kubernetes.**
- Implemented by the **CNI plugin** (Calico, Cilium, Weave). Flannel does **not** enforce them.
- **Namespace-scoped.**
- Restrict **Ingress and/or Egress** for a group of pods based on rules and conditions.
- **Without any policy:** all pods can talk to all pods.

#### Policy anatomy

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: example
  namespace: default
spec:
  podSelector:              # ← policy APPLIES TO these pods
    matchLabels:
      id: frontend
  policyTypes:              # ← which directions are controlled
  - Egress
  egress:
  - to:                     # rule 1
    - namespaceSelector:
        matchLabels:
          id: ns1
    ports:                  #   ns1 AND port 80
    - protocol: TCP
      port: 80
  - to:                     # rule 2  (OR)
    - podSelector:
        matchLabels:
          id: backend       #   pods id=backend in SAME namespace
```

**Reading it:**
- Applies to pods with `id=frontend` in `default`.
- It controls outgoing traffic only.
- It allows **(namespace `id=ns1` AND TCP/80) OR (pods `id=backend` in the same namespace)**.

#### Key rules
| Rule | Meaning |
|---|---|
| `policyTypes` listed but **no rules** for that type | **Deny all** in that direction |
| Multiple **list items** under `egress`/`ingress` (`- to:` / `- from:`) | **OR** |
| `to/from` + `ports` in the **same** item | **AND** |
| `namespaceSelector` + `podSelector` in the **same** `- ` element | **AND** (pods X in namespace Y) |
| Same selectors as **separate** `- ` elements | **OR** |
| Multiple policies selecting the same pod | **Union** — they're additive, never conflicting |

> `example` = `example2a` (ns1:80) + `example2b` (backend): splitting rules across two policies gives the same result as one policy with two rules.

**Egress-only deny example:**
```yaml
spec:
  podSelector:
    matchLabels:
      id: frontend
  policyTypes:
  - Egress
# no egress rules → denies ALL outgoing traffic from id=frontend in default
```

#### Hands-on 1: Default deny

```yaml
# deny all ingress + egress for every pod in the namespace,
# but allow DNS so service names still resolve
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: deny
  namespace: default
spec:
  podSelector: {}
  policyTypes:
  - Egress
  - Ingress
  egress:
  - ports:
    - port: 53
      protocol: TCP
    - port: 53
      protocol: UDP
```

```bash
kubectl run frontend --image=nginx
kubectl run backend --image=nginx
kubectl expose pod frontend --port 80
kubectl expose pod backend --port 80
kubectl exec frontend -- curl -m2 backend     # works before policy, times out after
```

> ⚠️ Without the DNS exception, `curl backend` fails on **name resolution**, not on the policy you're testing. That's confusing to debug.

#### Hands-on 2: Allow frontend → backend (podSelector)
Traffic needs to be allowed on **both sides**: egress from frontend **and** ingress to backend.

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: frontend
  namespace: default
spec:
  podSelector:
    matchLabels:
      run: frontend
  policyTypes:
  - Egress
  egress:
  - to:
    - podSelector:
        matchLabels:
          run: backend
---
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: backend
  namespace: default
spec:
  podSelector:
    matchLabels:
      run: backend
  policyTypes:
  - Ingress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          run: frontend
```

```bash
kubectl exec frontend -- curl -m2 backend   # ✅ works
kubectl exec backend -- curl -m2 frontend   # ❌ times out (not allowed)
```

#### Hands-on 3: Allow backend → cassandra (namespaceSelector)

```bash
kubectl create ns cassandra
kubectl label ns cassandra ns=cassandra
kubectl label ns default ns=default
kubectl -n cassandra run cassandra --image=nginx
kubectl -n cassandra get pod cassandra -o wide          # note pod IP
```

```yaml
# default ns: backend egress → cassandra namespace
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: backend
  namespace: default
spec:
  podSelector:
    matchLabels:
      run: backend
  policyTypes:
  - Ingress
  - Egress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          run: frontend
  egress:
  - to:
    - namespaceSelector:
        matchLabels:
          ns: cassandra
---
# cassandra ns: default deny + allow ingress from default ns
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: cassandra
  namespace: cassandra
spec:
  podSelector:
    matchLabels:
      run: cassandra
  policyTypes:
  - Ingress
  ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          ns: default
```

```bash
kubectl exec backend -- curl -m2 <cassandra-pod-ip>   # ✅ works
```

> 💡 Every namespace automatically gets the label `kubernetes.io/metadata.name=<ns>`. Use it instead of adding custom labels: `namespaceSelector: {matchLabels: {kubernetes.io/metadata.name: cassandra}}`.

#### Exercise: "more?" — restrict backend ↔ cassandra further
**Task:** extend this so traffic is restricted on an **additional pod label** and an **additional port**.

<details>
<summary>Solution</summary>

```yaml
# cassandra ns: only pods with run=backend from ns default, only on port 80
ingress:
- from:
  - namespaceSelector:          # same element → AND
      matchLabels:
        ns: default
    podSelector:
      matchLabels:
        run: backend
  ports:
  - protocol: TCP
    port: 80
```
Note: `podSelector` is **not** prefixed with `-`. That makes it the **same** element as `namespaceSelector` → AND. Putting `-` before it would make it OR and open access far wider.
</details>

#### NetworkPolicy debugging checklist
```bash
kubectl get netpol -A
kubectl describe netpol <name> -n <ns>          # shows the parsed selectors — verify AND/OR
kubectl get pod --show-labels -n <ns>           # do labels actually match?
kubectl get ns --show-labels                    # for namespaceSelector
kubectl exec <pod> -- curl -m2 <target>         # timeout = blocked by policy; refused = no listener
```

#### Section summary
- NetworkPolicy
- Egress & Ingress
- Default deny
- Whitelist allow
- Selectors (pod / namespace, AND vs OR)

---

### 5.2 GUI Elements (Kubernetes Dashboard)

#### Concept
**Control access to GUI elements:**
- GUI elements and the Dashboard
- Methods of accessing them from outside
- Restricting access to them

**Rules:**
- Only expose services externally **if needed**.
- Cluster-internal services and dashboards can also be reached with `kubectl port-forward`, so they don't need to be exposed at all.

#### Real-world lesson: Tesla hack (2018)
- The Kubernetes Dashboard **had too many privileges** on the cluster (no RBAC, or roles that were too broad).
- The Dashboard **was exposed to the internet**, which isn't the default.
- Attackers used it to run crypto miners inside Tesla's cluster.

#### Access methods

| Method | How it works | Notes |
|---|---|---|
| `kubectl proxy` | Creates a proxy between localhost and the **API server**. Uses the connection configured in the kubeconfig. | Lets you reach the API locally over plain **HTTP without authentication**. URL: `http://localhost:8001/api/v1/...` |
| `kubectl port-forward` | Forwards connections from a **localhost port to a pod port** (tunnelled through the API server over HTTPS) | More generic than `kubectl proxy`, and works for **any TCP traffic**, not just HTTP |
| Ingress | `https://my-cluster/dashboard` → Nginx Ingress → Dashboard pod | Needs proper authentication in front of it |
| NodePort / LoadBalancer | Directly exposed | ❌ Avoid for dashboards |

```bash
kubectl proxy                                                           # then:
# http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
kubectl -n kubernetes-dashboard port-forward svc/kubernetes-dashboard 8443:443   # then https://localhost:8443
```

#### Install (2026 note)
> ⚠️ The Dashboard project is **archived** (moved to `kubernetes-retired`, Jan 2026). Its successor is **Headlamp**. For the course, use v2.7.0, which has the same args the video uses:

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes-retired/dashboard/v2.7.0/aio/deploy/recommended.yaml
```
- The old Helm repo URL `https://kubernetes.github.io/dashboard/` returns **404**. The chart now lives at `https://kubernetes-retired.github.io/dashboard/`.
- Headlamp: `helm repo add headlamp https://kubernetes-sigs.github.io/headlamp/ && helm install headlamp headlamp/headlamp -n kube-system`

#### Hands-on 1: Make the Dashboard available externally over HTTP (⚠️ don't do this at home!)
Always back up first:
```bash
kubectl -n kubernetes-dashboard get deploy kubernetes-dashboard -o yaml > dashboard-backup.yaml
kubectl -n kubernetes-dashboard edit deploy kubernetes-dashboard
```
```yaml
        args:
        # - --auto-generate-certificates     # removed → no TLS
        - --namespace=kubernetes-dashboard
        - --insecure-port=9090
        - --enable-insecure-login             # v2.7.0: needed for login over HTTP
        livenessProbe:
          httpGet:
            scheme: HTTP                      # was HTTPS
            path: /
            port: 9090                        # was 8443
```
```bash
kubectl -n kubernetes-dashboard edit svc kubernetes-dashboard     # type: NodePort, port/targetPort 9090
kubectl -n kubernetes-dashboard get svc                          # → 9090:3xxxx/TCP
```
Open `http://<node-PUBLIC-ip>:<nodeport>`. A private IP only works from inside the VPC, and the security group has to allow the NodePort range.

Restore the secure setup: `kubectl replace --force -f dashboard-backup.yaml`

#### Hands-on 2: Dashboard + RBAC
**Task:** give the Dashboard ServiceAccount more permissions, and check out other Dashboard arguments.

```bash
kubectl -n kubernetes-dashboard get sa
kubectl get clusterroles | grep view                 # view = read-only built-in role

# namespace-scoped read access (RoleBinding → only kubernetes-dashboard ns)
kubectl -n kubernetes-dashboard create rolebinding insecure --serviceaccount kubernetes-dashboard:kubernetes-dashboard --clusterrole view

# cluster-wide read access (ClusterRoleBinding → all namespaces)
kubectl create clusterrolebinding insecure --serviceaccount kubernetes-dashboard:kubernetes-dashboard --clusterrole view
```
> 💡 **RoleBinding + ClusterRole** = the ClusterRole's permissions, but **only in that namespace**. **ClusterRoleBinding** = cluster-wide. Never bind the Dashboard SA to `cluster-admin`.

Token for login (with a separate viewer SA):
```bash
kubectl -n kubernetes-dashboard create sa viewer && kubectl create clusterrolebinding viewer --clusterrole=view --serviceaccount=kubernetes-dashboard:viewer && kubectl -n kubernetes-dashboard create token viewer
```

#### Interesting Dashboard security arguments
| Arg | Risk |
|---|---|
| `--authentication-mode=basic` | Basic auth. Weak, and removed in newer versions |
| `--enable-skip-login=true` | ❌ Shows a "Skip" button. Anyone can log in as the Dashboard SA |
| `--enable-insecure-login` | Allows login over HTTP |
| `--insecure-port=9090` | Serves plain HTTP |
| `--auto-generate-certificates` | ✅ Serves HTTPS with a self-signed cert |

Full list: https://github.com/kubernetes-retired/dashboard/blob/master/docs/common/dashboard-arguments.md

#### Section summary
- Only expose externally if needed and if protected
- Restrict using RBAC
- Enforce authentication
- Change (harden) arguments

---

### 5.3 Secure Ingress

#### Concept
**Ingress objects with security control:**
- What is Ingress
- Setting up an Ingress with Services
- Securing an Ingress with TLS

#### What is Ingress
Service types build on each other: **LoadBalancer creates → NodePort, which creates → ClusterIP, which points to → the pod.**

An **Ingress controller** (e.g. ingress-nginx) is just a pod exposed through a NodePort/LoadBalancer service. It reads `Ingress` resources and routes HTTP(S) by host/path to ClusterIP services.

```
Request ──(HTTPS)──► NodePort ──► Nginx Ingress ──/service1──► ClusterIP ──► pod1
                                               └─/service2──► ClusterIP ──► pod2
```

> ⚠️ Before this lab, **delete all NetworkPolicies from previous sessions**: `kubectl delete netpol --all -A`
>
> ⚠️ **2026 note:** the community **ingress-nginx was retired in March 2026** (no more patches). It's fine for labs, but Gateway API is the successor. Options for exposing services: ClusterIP → NodePort → LoadBalancer → Ingress → Gateway API.

#### Hands-on 1: Set up an example Ingress
```bash
# install ingress-nginx (bare metal = NodePort)
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.15.1/deploy/static/provider/baremetal/deploy.yaml
kubectl -n ingress-nginx get svc     # note HTTP and HTTPS NodePorts, e.g. 80:32310, 443:30764

kubectl run pod1 --image=nginx && kubectl expose pod pod1 --port 80 --name service1
kubectl run pod2 --image=httpd && kubectl expose pod pod2 --port 80 --name service2
```
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: secure-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx
  rules:
  - http:
      paths:
      - path: /service1
        pathType: Prefix
        backend:
          service:
            name: service1
            port:
              number: 80
      - path: /service2
        pathType: Prefix
        backend:
          service:
            name: service2
            port:
              number: 80
```
```bash
curl http://<node-ip>:<http-nodeport>/service1    # nginx page
curl http://<node-ip>:<http-nodeport>/service2    # "It works!" (httpd)
```

#### Hands-on 2: Secure the Ingress with TLS
Without a TLS secret, the controller serves its default **"Kubernetes Ingress Controller Fake Certificate"** (`O=Acme Co`).

```bash
# 1. create a self-signed cert (CN must match the host)
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -nodes -subj "/CN=secure-ingress.com"
# 2. store it in a TLS secret
kubectl create secret tls secure-ingress --cert=cert.pem --key=key.pem
```
```yaml
spec:
  ingressClassName: nginx
  tls:
  - hosts:
    - secure-ingress.com
    secretName: secure-ingress
  rules:
  - host: secure-ingress.com        # host is required to match the TLS entry
    http:
      paths: ...                    # same as above
```
```bash
# 3. test (HTTPS NodePort; --resolve fakes DNS)
curl -kv https://secure-ingress.com:<https-nodeport>/service2 --resolve secure-ingress.com:<https-nodeport>:<node-ip>
```
Check `subject:` in the output:
- `CN=secure-ingress.com` → ✅ your cert is being served.
- `CN=Kubernetes Ingress Controller Fake Certificate` → ❌ secret name/namespace wrong, or the host doesn't match.

#### Gotchas (from my lab)
- `--resolve` only applies if **host AND port match exactly**. A typo like `secure.ingress.com` vs `secure-ingress.com` → `curl: (6) Could not resolve host`.
- Use the **HTTPS NodePort** (443:xxxxx), not the HTTP one.
- Once `tls:` is set, HTTP requests for that host get a **308 redirect** to HTTPS.
- `-k` is needed because the cert is self-signed. To verify properly, use `--cacert cert.pem` instead.
- A 404 with the fake cert = request didn't match any rule (wrong host header).

---

### 5.4 Node Metadata Protection

#### Concept
**Protect node metadata and endpoints:**
- Cloud platform node metadata
- Accessing sensitive node metadata
- Restricting access with NetworkPolicies

#### Cloud platform node metadata
- The metadata service API is **reachable from VMs by default** at `169.254.169.254`.
- It can contain **cloud credentials** for VMs/nodes (IAM role creds).
- It can contain **provisioning data** like kubelet credentials (bootstrap data).
- Pods on the node can reach it too, so a compromised pod can steal node or cloud credentials.

Goal: only specific pods may reach the metadata service, and all others are blocked.

#### Hands-on 1: Access from the instance and from a pod (AWS, IMDSv2)
```bash
# on the node
TOKEN=$(curl -s -X PUT http://169.254.169.254/latest/api/token -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")
curl -s -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/
curl -s -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/iam/security-credentials/   # 🔥 IAM creds

# from a pod
kubectl run test --image=curlimages/curl -it --rm --restart=Never -- sh -c 'T=$(curl -s -X PUT http://169.254.169.254/latest/api/token -H "X-aws-ec2-metadata-token-ttl-seconds: 60"); curl -s -H "X-aws-ec2-metadata-token: $T" http://169.254.169.254/latest/meta-data/'
```
| Cloud | Endpoint / header |
|---|---|
| AWS | `http://169.254.169.254/latest/meta-data/` (IMDSv2 needs a token via PUT) |
| GCP | `http://metadata.google.internal/computeMetadata/v1/` + `Metadata-Flavor: Google` |
| Azure | `http://169.254.169.254/metadata/instance?api-version=2021-02-01` + `Metadata: true` |

> 💡 On AWS, IMDSv2 with **hop limit 1** already blocks pods (the token reply can't make the extra network hop into the pod). Harden it with:
> `aws ec2 modify-instance-metadata-options --instance-id <id> --http-tokens required --http-put-response-hop-limit 1`

#### Hands-on 2: Deny all pods access to the metadata endpoint
```yaml
# all pods in namespace cannot access metadata endpoint
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: cloud-metadata-deny
  namespace: default
spec:
  podSelector: {}
  policyTypes:
  - Egress
  egress:
  - to:
    - ipBlock:
        cidr: 0.0.0.0/0
        except:
        - 169.254.169.254/32
```

#### Hands-on 3: Only allow pods with a certain label
```yaml
# only pods with label are allowed to access metadata endpoint
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: cloud-metadata-allow
  namespace: default
spec:
  podSelector:
    matchLabels:
      role: metadata-accessor
  policyTypes:
  - Egress
  egress:
  - to:
    - ipBlock:
        cidr: 169.254.169.254/32
```
```bash
kubectl label pod <pod> role=metadata-accessor      # this pod can reach metadata again
```
> 💡 Policies are **additive**. A labelled pod matches both policies, and the union of the egress rules = `0.0.0.0/0 except metadata` **+** `metadata` = everything. Unlabelled pods only get the deny policy.
>
> ⚠️ NetworkPolicies are namespaced, so you need the deny policy in **every** namespace.

#### Section summary
- Cloud metadata servers
- Access from the instance
- Access from a pod
- Restrict access using NetworkPolicy

---

### 5.5 CIS Benchmarks (kube-bench)

#### Concept
**Use the CIS benchmark to review security configuration:**
- Overview
- CIS Benchmarks in action
- kube-bench

#### CIS — Center for Internet Security
- Best practices for the **secure configuration** of a target system.
- Covers more than 14 technology groups.
- Developed through a consensus-based process by cybersecurity professionals and subject matter experts.

```
CIS Benchmarks (default K8s security rules)
        ├──► Use and apply
        └──► Customize (cloud providers: AWS/EKS, GCP/GKE, AKS) ──► Use and apply
```
- Video uses `CIS_Kubernetes_Benchmark_v1.6.0.pdf`. Rule numbers **change between versions**, so always read the rule text, not just the number.
- Download: https://www.cisecurity.org/benchmark/kubernetes (free with registration)

#### kube-bench
An open-source tool by Aqua Security that checks a cluster against the CIS benchmark.

```bash
# option A: run as a Job (no docker needed on containerd nodes)
kubectl apply -f https://raw.githubusercontent.com/aquasecurity/kube-bench/main/job-master.yaml
kubectl logs job/kube-bench

# option B: binary on the node
kube-bench run --targets master          # control plane
kube-bench run --targets node            # worker
kube-bench run --targets master --check 1.1.12     # single check
```
Output: `[PASS]` / `[FAIL]` / `[WARN]` / `[INFO]` per check. Each FAIL comes with a **Remediation** text at the bottom that tells you exactly what to change.

#### Hands-on: Fix rule 1.1.12 on the master node
**1.1.12 — Ensure that the etcd data directory ownership is set to `etcd:etcd`**
```bash
kube-bench run --targets master --check 1.1.12      # [FAIL]
ps -ef | grep etcd | grep data-dir                  # find data dir → /var/lib/etcd
stat -c %U:%G /var/lib/etcd                         # root:root
useradd etcd                                        # user may not exist
chown etcd:etcd /var/lib/etcd
kube-bench run --targets master --check 1.1.12      # [PASS]
```

#### Typical CIS fixes (exam favourites)
| Check | Fix |
|---|---|
| apiserver `--profiling` | `--profiling=false` in `/etc/kubernetes/manifests/kube-apiserver.yaml` |
| apiserver `--anonymous-auth` | `--anonymous-auth=false` |
| apiserver `--authorization-mode` | Must include `Node,RBAC`, never `AlwaysAllow` |
| apiserver audit logging | `--audit-log-path`, `--audit-log-maxage`, etc. |
| controller-manager / scheduler `--profiling` | `--profiling=false` |
| `--bind-address` | `127.0.0.1` for scheduler/controller-manager |
| kubelet `anonymous.enabled` | `false` in `/var/lib/kubelet/config.yaml` → `systemctl restart kubelet` |
| kubelet `authorization.mode` | `Webhook` |
| File permissions/ownership | `chmod 600` / `chown root:root` on manifests and kubeconfigs |

> 💡 **Exam tip:** back up the manifest before editing (`cp /etc/kubernetes/manifests/kube-apiserver.yaml ~/`), then `watch crictl ps` until the apiserver comes back. Re-run the single `--check` to confirm.

#### Section summary
- What CIS Benchmarks are
- How to use kube-bench
- How to apply and check fixes manually

---

### 5.6 Verify Platform Binaries

#### Concept
- Hashes
- Download and verify binaries
- Verify binaries from a container

#### Theory: hashes
`File ──(ONE WAY)──► Hash (fingerprint)`. Algorithms: SHA, MD5, …
- The same file always gives the same hash. Change a single bit and you get a completely different hash.
- You can't get back from the hash to the file.
- Use it to prove a downloaded or running binary is **untampered** (supply-chain protection).

#### Hands-on 1: Download and verify a Kubernetes release from GitHub
```bash
# match your cluster version
kubectl version
curl -LO https://dl.k8s.io/v1.xx.x/kubernetes-server-linux-amd64.tar.gz
sha512sum kubernetes-server-linux-amd64.tar.gz
# compare with the sha512 listed in the release CHANGELOG on GitHub (kubernetes/kubernetes/CHANGELOG)
```
Single binary with the official checksum file:
```bash
curl -LO https://dl.k8s.io/release/v1.xx.x/bin/linux/amd64/kubectl && curl -LO https://dl.k8s.io/release/v1.xx.x/bin/linux/amd64/kubectl.sha512
echo "$(cat kubectl.sha512)  kubectl" | sha512sum --check      # → kubectl: OK
```

#### Hands-on 2: Verify the binary running inside a container (kube-apiserver)
The apiserver image is **distroless**: there's no `sh` or `bash`, so `kubectl exec` fails (`executable file not found in $PATH`). Go through the **host's `/proc`** instead.

```bash
tar xzf kubernetes-server-linux-amd64.tar.gz
kubernetes/server/bin/kube-apiserver --version                     # must match running version
kubectl -n kube-system get pod kube-apiserver-<node> -o yaml | grep image
sha512sum kubernetes/server/bin/kube-apiserver > compare

# find the running process and its container filesystem
ps aux | grep kube-apiserver                                       # → PID e.g. 1843
find /proc/1843/root/ | grep kube-api                              # → /proc/1843/root/usr/local/bin/kube-apiserver
sha512sum /proc/1843/root/usr/local/bin/kube-apiserver >> compare

# compare: strip filenames, then uniq → 1 line = identical, 2 lines = tampered
cut -d' ' -f1 compare | uniq
```
> 💡 `/proc/<PID>/root/` = the root filesystem **as the process sees it**, so you can reach inside any container from the host. You'll use this trick again in runtime security.

#### Section summary
- Hashes of files
- Verify downloads using `sha512sum`
- Verify binaries on the server or in a container

🎉 **Domain 1 (Cluster Setup) complete.**

---

## 6. Domain 2 — Cluster Hardening

### 6.1 RBAC — Role Based Access Control

#### Concept
- Introduction to RBAC
- Create scenarios
- Certificates and "Users"

> "Role-based access control (RBAC) is a method of regulating access to computer or network resources based on the roles of individual users within your organization."

- Restricts access to Kubernetes resources when they're accessed by **Users or ServiceAccounts**.
- Works with **Roles and Bindings**.
- Specify what is **ALLOWED**; everything else is DENIED ("whitelisting").
- **Permissions are additive.** There are no "deny" rules.

```
--authorization-mode   Default: [AlwaysAllow]
Options: AlwaysAllow, AlwaysDeny, ABAC, Webhook, RBAC, Node
kubeadm default: --authorization-mode=Node,RBAC
```

#### Namespaced vs cluster resources
```bash
kubectl api-resources --namespaced=true     # pods, secrets, deployments, roles …
kubectl api-resources --namespaced=false    # nodes, PVs, namespaces, clusterroles …
```

#### Where permissions are available vs applied

| | Where **available**? (set of permissions) | | Where **applied**? (who gets them) |
|---|---|---|---|
| **Role** | in one namespace | **RoleBinding** | applied in one namespace |
| **ClusterRole** | in all namespaces + non-namespaced | **ClusterRoleBinding** | applied in all namespaces + non-namespaced |

- Role/ClusterRole = a set of permissions ("can edit pods", "can read secrets").
- Binding = **who** gets that set ("bind a Role/ClusterRole to something").

**Valid combinations:**
| Combo | Result |
|---|---|
| Role + RoleBinding | ✅ Permissions in one namespace |
| ClusterRole + ClusterRoleBinding | ✅ Permissions in all namespaces + non-namespaced |
| **ClusterRole + RoleBinding** | ✅ Reuse one ClusterRole, applied **only** in the RoleBinding's namespace. The user has the same permissions in multiple namespaces (one RoleBinding per namespace) |
| Role + ClusterRoleBinding | ❌ Not possible |

#### Permissions are additive
```
ClusterRole+ClusterRoleBinding ("get" & "delete" secrets)
  + Role+RoleBinding ("get" secrets)
  = "get" & "delete" secrets
```
> ⚠️ Be careful with ClusterRoles/ClusterRoleBindings. You can't take away in a namespace what a ClusterRoleBinding grants cluster-wide.

#### Hands-on 1: Role + RoleBinding (jane)
**Task:** create namespaces `red` and `blue`. User jane can only **get** secrets in `red`, and only **get + list** secrets in `blue`. Test with `auth can-i`.

```bash
k create ns red && k create ns blue
k -n red  create role secret-manager --verb=get --resource=secrets
k -n red  create rolebinding secret-manager --role=secret-manager --user=jane
k -n blue create role secret-manager --verb=get --verb=list --resource=secrets
k -n blue create rolebinding secret-manager --role=secret-manager --user=jane

# test (as admin, impersonating jane)
k -n red  auth can-i get secrets  --as jane        # yes
k -n red  auth can-i list secrets --as jane        # no
k -n blue auth can-i list secrets --as jane        # yes
k -n blue auth can-i delete secrets --as jane      # no
k -n default auth can-i get secrets --as jane      # no
k -n red auth can-i --list --as jane               # everything jane can do in red
```

#### Hands-on 2: ClusterRole + (Cluster)RoleBinding (jane & jim)
**Task:** create a ClusterRole `deploy-deleter` that allows deleting deployments. User jane can delete deployments in **all** namespaces. User jim can delete deployments **only in `red`**.

```bash
k create clusterrole deploy-deleter --verb=delete --resource=deployment
k create clusterrolebinding deploy-deleter --clusterrole=deploy-deleter --user=jane
k -n red create rolebinding deploy-deleter --clusterrole=deploy-deleter --user=jim

k auth can-i delete deploy --as jane -A             # yes
k -n red     auth can-i delete deploy --as jim      # yes
k -n default auth can-i delete deploy --as jim      # no
```

---

### 6.2 Accounts: ServiceAccounts vs "normal users"

| ServiceAccount | "normal user" |
|---|---|
| There **is** a ServiceAccount resource | There is **no** k8s User resource |
| Managed by the k8s API | Assumed to be managed by a **cluster-independent service** (AWS IAM, GCP, LDAP/OIDC) |
| Namespaced, has a token | A user is **someone with a cert and key** |

#### Users and certificates
A client cert is what makes someone a user:
- The **certificate must be signed by the cluster's CA**.
- The **username comes from the Common Name**: `/CN=jane`. Groups come from Organization: `/O=dev`.

#### CSR — CertificateSigningRequest flow
```
openssl → CSR ──(CertificateSigningRequest)──► API ◄── CA signs
                                                │
                     CRT ◄─(CertificateSigningRequest .status.certificate)
```
`create KEY → create CSR → API (approve) → download CRT from API → use CRT+KEY`

#### Hands-on 3: Create a certificate + key and authenticate as user jane
```bash
# 1. key + CSR (CN = username)
openssl genrsa -out jane.key 2048
openssl req -new -key jane.key -out jane.csr          # interactive: only fill "Common Name" = jane
# non-interactive alternative:
openssl req -new -key jane.key -out jane.csr -subj "/CN=jane"

# 2. send CSR to the API (template from k8s docs: "CertificateSigningRequest")
cat jane.csr | base64 -w 0                             # paste into spec.request
```
```yaml
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: jane
spec:
  request: <base64 of jane.csr, single line>
  signerName: kubernetes.io/kube-apiserver-client
  expirationSeconds: 86400            # optional, 1 day
  usages:
  - client auth
```
```bash
k apply -f csr.yaml
k get csr                                             # CONDITION: Pending
k certificate approve jane                            # → Approved,Issued

# 3. download the signed cert
k get csr jane -o jsonpath='{.status.certificate}' | base64 -d > jane.crt

# 4. add to kubeconfig
k config set-credentials jane --client-key=jane.key --client-certificate=jane.crt
#   → kubeconfig stores PATHS: client-certificate: /root/jane.crt
k config set-credentials jane --client-key=jane.key --client-certificate=jane.crt --embed-certs
#   → kubeconfig stores DATA: client-certificate-data: REDACTED (portable)
k config set-context jane --user=jane --cluster=kubernetes
k config get-contexts
k config use-context jane
k get ns                                              # Forbidden → authenticated, but no RBAC
k get secrets -n red                                  # works (from Hands-on 1)
k config use-context kubernetes-admin@kubernetes      # switch back!
```
> `k config view` shows certs as `REDACTED`/`DATA+OMITTED`. Use `k config view --raw` to see the actual base64 data.

**Exercise:** now do the same for user **jim**.

#### Leak + invalidation
- ⚠️ **There is no way to invalidate a certificate.**
- If a certificate has been leaked:
  - Remove all access via RBAC.
  - The username can't be safely used again until the cert expires.
  - Or create a new CA and re-issue **all** certs.
- → Use short `expirationSeconds` for user certs.

#### Gotchas (from my lab)
- `--client-certificate=jane-crt` → `could not stat client-certificate file`. It's a **typo** (`jane.crt`). Always `ls` first.
- The `openssl req` warning `Can't load /root/.rnd into RNG` is harmless.
- `spec.request` must be **one line** of base64 (`base64 -w 0`).

#### Section summary
- Roles / ClusterRoles
- RoleBinding / ClusterRoleBinding
- Combining Roles and Bindings
- "can-i" testing
- Created user certificate
- Used CSR k8s API signing

---

### 6.3 Exercise Caution in Using ServiceAccounts

#### Concept
- Overview
- Scenarios with SAs and Pods
- Limit ServiceAccount permissions with RBAC

#### ServiceAccounts and pods
- **Namespaced.**
- SA `default` exists in every namespace and is used by pods unless you set another.
- Can be used to talk to the k8s API.
- Token: since 1.24 it's a **projected, short-lived token**. No Secret is auto-created anymore. Create one manually with `k create token <sa>`.

#### Hands-on 1: Use a ServiceAccount token to connect to the API from inside a pod
```bash
k create sa accessor
k create token accessor                               # short-lived token (default 1h)
k run accessor --image=nginx -o yaml --dry-run=client > pod.yaml
# add spec.serviceAccountName: accessor   (SA must exist, else: error looking up service account default/test)
k -f pod.yaml create
k exec -it accessor -- bash
```
Inside the pod:
```bash
mount | grep ser                   # tmpfs on /run/secrets/kubernetes.io/serviceaccount (ro)
cd /run/secrets/kubernetes.io/serviceaccount && ls        # ca.crt  namespace  token
cat namespace                      # default
cat token                          # JWT → decode at jwt.io to see SA name, ns, expiry
env | grep KUBER                   # KUBERNETES_SERVICE_HOST=10.96.0.1, PORT=443

curl https://10.96.0.1                                         # (60) SSL cert problem → CA unknown
curl https://10.96.0.1 -k                                      # 403 User "system:anonymous" cannot get path "/"
curl https://10.96.0.1 -k -H "Authorization: Bearer $(cat token)"   # 403 User "system:serviceaccount:default:accessor"
curl --cacert ca.crt https://kubernetes.default -H "Authorization: Bearer $(cat token)"   # proper TLS verify
```
> Key insight: **any pod can read its SA token and talk to the API**. Who you are depends on the token. RBAC decides what you can do.

#### Hands-on 2: Disable the ServiceAccount token mount
Ask yourself: **does my pod need to talk to the Kubernetes API?** If not, don't mount the token.

```yaml
# on the ServiceAccount (affects all pods using it)
apiVersion: v1
kind: ServiceAccount
metadata:
  name: build-robot
automountServiceAccountToken: false
---
# on the Pod (takes PRECEDENCE over the SA setting)
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  serviceAccountName: build-robot
  automountServiceAccountToken: false
```
```bash
k exec accessor -- mount | grep ser        # nothing → token not mounted
```
Docs: kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/

#### Hands-on 3: Limit ServiceAccount permissions using RBAC
```bash
k auth can-i delete secrets --as system:serviceaccount:default:accessor     # no
k create clusterrolebinding accessor --clusterrole edit --serviceaccount default:accessor
k auth can-i delete secrets --as system:serviceaccount:default:accessor     # yes
```
> Impersonation format for SAs: `--as system:serviceaccount:<namespace>:<sa-name>`

#### POLP — Principle of Least Privilege
Only allow access to data or information that's necessary for the legitimate purpose.
- One SA per app; never give workloads the `default` SA with extra rights.
- Prefer RoleBinding (namespaced) over ClusterRoleBinding.
- Set `automountServiceAccountToken: false` unless the pod needs it.

#### Section summary
- SAs are resources with a token (projected, short-lived since 1.24)
- SAs and pods + mounting
- Create RBAC with SAs

---

### 6.4 Restrict API Access

#### Concept
- Authentication → Authorization → Admission
- Connect to the API in different ways
- Restrict API access in various ways

#### Request workflow
```
Human user / Pod (SA) ──► [1 Authentication]  ──► [2 Authorization]       ──► [3 Admission Control]          ──► etcd
                           "who are you?"         "are you allowed to         "has the limit of pods
                                                   create pods?"               been reached?"
```
- API requests are always tied to a **normal user**, a **ServiceAccount**, or they're **treated as anonymous**.
- Every request must authenticate, or it's treated as an anonymous user.

#### Restrictions (checklist)
1. **Don't allow anonymous access.**
2. **Close the insecure port.**
3. **Don't expose the apiserver to the outside.**
4. **Restrict access from nodes to the API (NodeRestriction).**
5. Prevent unauthorized access (RBAC).
6. Prevent pods from accessing the API (`automountServiceAccountToken: false`).
7. Put the apiserver port behind a firewall / allowed IP ranges (cloud provider).

#### Hands-on 1: Anonymous access
`kube-apiserver --anonymous-auth=true|false`
- Since 1.6, anonymous access is **enabled by default**:
  - if the authorization mode is other than AlwaysAllow,
  - but ABAC and RBAC require explicit authorization for anonymous.

```bash
curl https://localhost:6443 -k                        # 403 system:anonymous → anon auth ON, RBAC blocks
vim /etc/kubernetes/manifests/kube-apiserver.yaml     # add: - --anonymous-auth=false
curl https://localhost:6443 -k                        # 401 Unauthorized → anon auth OFF
```
> ⚠️ The apiserver's own **liveness probe uses anonymous access**. With `--anonymous-auth=false` in older versions the probe failed and the apiserver kept restarting. Newer versions handle this, but watch `crictl ps` after the change. Revert to the default for the following labs.

#### Hands-on 2: Insecure access (⚠️ removed in v1.24)
In the video: `kube-apiserver --insecure-port=8080`
- HTTP.
- The request **bypasses** the authentication and authorization modules.
- The admission controller still enforces.

> ❌ **On current clusters:** `--insecure-port` was deprecated in 1.20 and **removed in 1.24**. Adding it gives `Error: unknown flag: --insecure-port` and the apiserver won't start. Fix: `sed -i '/--insecure-port/d' /etc/kubernetes/manifests/kube-apiserver.yaml`.
>
> Modern equivalent for local unauthenticated HTTP: `kubectl proxy --port 8001 &` then `curl http://localhost:8001/api/v1/namespaces`. It authenticates with your kubeconfig and is localhost only.

#### HTTP / HTTPS access
```
kubectl + client cert ──Request──► (https) ──► API Server (server cert)
         ▲                                            │
         └──── verifies server cert via CA ◄──────────┘
API server verifies the client cert via the same CA.
```

#### Hands-on 3: Send a manual API request
Take the certs out of the kubeconfig and query the API with curl:
```bash
k config view --raw
k config view --raw -o jsonpath='{.clusters[0].cluster.certificate-authority-data}' | base64 -d > ca
k config view --raw -o jsonpath='{.users[0].user.client-certificate-data}' | base64 -d > crt
k config view --raw -o jsonpath='{.users[0].user.client-key-data}' | base64 -d > key
curl https://<apiserver-ip>:6443                                       # SSL problem
curl https://<apiserver-ip>:6443 --cacert ca                           # 403 anonymous
curl https://<apiserver-ip>:6443 --cacert ca --cert crt --key key      # ✅ full API access
```

#### Hands-on 4: Send an API request from outside
Make the Kubernetes API reachable from outside and access it with kubectl (copy the kubeconfig).
```bash
k edit svc kubernetes                         # type: NodePort (lab only!)
k get svc kubernetes                          # 443:3xxxx
# on your laptop: copy kubeconfig, set server: https://<node-public-ip>:<nodeport>
kubectl get ns --insecure-skip-tls-verify     # fails TLS verify without this flag: the public IP isn't in the cert SANs
openssl x509 -in /etc/kubernetes/pki/apiserver.crt -noout -text | grep -A1 "Subject Alternative"   # check SANs
```
> Fix the SAN issue properly with an `/etc/hosts` entry for `kubernetes` → public IP, and use `server: https://kubernetes:<nodeport>`.
> Revert: `k edit svc kubernetes` → `type: ClusterIP`.

#### Hands-on 5: NodeRestriction admission controller
`kube-apiserver --enable-admission-plugins=NodeRestriction` (on by default in kubeadm)
- Limits the Node labels a kubelet can modify.
- It can only modify **its own** Node and the pods bound to it.
- Secure workload isolation via labels: nobody can pretend to be a "secure" node and get secure pods scheduled.

**Verify NodeRestriction works:** use the worker node kubelet's kubeconfig to set labels.
```bash
# on the WORKER node
export KUBECONFIG=/etc/kubernetes/kubelet.conf
k get ns                                                     # Forbidden (kubelet ≠ admin)
k get node                                                   # works
k label node cks-master cks/test=yes                         # ❌ Forbidden: not its own node
k label node cks-worker cks/test=yes                         # ✅ own node, normal label OK
k label node cks-worker node-restriction.kubernetes.io/test=yes   # ❌ restricted prefix
unset KUBECONFIG
```
Real output from the video:
```
Error from server (Forbidden): namespaces is forbidden: User "system:node:cks-worker" cannot list resource "namespaces" ... at the cluster scope
Error from server (Forbidden): nodes "cks-master" is forbidden: node "cks-worker" is not allowed to modify node "cks-master"
node/cks-worker labeled
Error from server (Forbidden): nodes "cks-worker" is forbidden: is not allowed to modify labels: node-restriction.kubernetes.io/test
```
> The kubelet's identity is `system:node:<nodename>` (group `system:nodes`). The **Node authorizer** (`--authorization-mode=Node,...`) limits what it can read, and the **NodeRestriction admission plugin** limits what it can write.
> Labels with the prefix `node-restriction.kubernetes.io/` can only be set by admins. Use them for isolation (`nodeSelector`), because a compromised kubelet can't set them on itself.

#### Section summary
- Outside → API, Pod → API, Node → API
- Anonymous access
- Insecure access
- Certificates

---

### 6.5 Upgrade Kubernetes (Update Frequently)

#### Concept
- Release cycles
- Version differences of components
- Upgrade!

#### Why upgrade frequently?
- Support
- Security fixes
- Bug fixes
- Stay up to date for dependencies

#### Kubernetes release cycles
- Versioning: `major.minor.patch` → e.g. `1.19.2` = major 1, minor 19, patch 2.
- A new **minor version roughly every 3–4 months** (3 a year).
- **No LTS** (Long Term Support).
- **Support:** maintenance release branches for the **most recent three minor releases** (video: 1.19, 1.18, 1.17). Applicable fixes, including security fixes, may be backported to those three branches, depending on severity and feasibility.
- → If you're more than 3 minors behind, you get **no security patches**.

#### How to upgrade a cluster
- **First upgrade the master components:** apiserver, controller-manager, scheduler.
- **Then the worker components:** kubelet, kube-proxy.
- **Rule of thumb:** components may be the **same minor version as the apiserver or one below**. Never newer than the apiserver.
- Only upgrade **one minor version at a time** (1.36 → 1.37, not 1.35 → 1.37).

#### How to upgrade a node
1. `kubectl drain`
   - Safely evicts all pods from the node.
   - Marks the node as SchedulingDisabled (`kubectl cordon`).
2. Do the upgrade.
3. `kubectl uncordon`
   - Unmarks the node as SchedulingDisabled.

#### How to make your application survive an upgrade
- Pod `terminationGracePeriodSeconds` / Terminating state
- Pod lifecycle events (`preStop` hook)
- `PodDisruptionBudget` (limits how many pods drain can evict at once)

#### Hands-on 1: Create an outdated cluster
Install an earlier version of k8s in the cluster:
- Use the Killercoda playground (even just a patch upgrade like `1.25.2 → 1.25.3`), or
- Create new VMs with the previous install scripts, or
- Install the previous scripts over existing VMs.

#### Hands-on 2: Upgrade the control plane to one minor version up (kubeadm)
```bash
# ── control plane node ──
kubectl get node                                     # current versions
k drain cks-master --ignore-daemonsets
# point apt at the NEW minor repo (pkgs.k8s.io is per-minor!)
sed -i 's/v1.36/v1.37/' /etc/apt/sources.list.d/kubernetes.list && apt-get update
apt-cache madison kubeadm | head                     # list available versions
apt-mark unhold kubeadm && apt-get install -y kubeadm=1.37.1-1.1 && apt-mark hold kubeadm
kubeadm version
kubeadm upgrade plan                                 # shows what will be upgraded + target version
kubeadm upgrade apply v1.37.1                        # upgrades apiserver/cm/scheduler/etcd/coredns/kube-proxy
apt-mark unhold kubelet kubectl && apt-get install -y kubelet=1.37.1-1.1 kubectl=1.37.1-1.1 && apt-mark hold kubelet kubectl
systemctl daemon-reload && systemctl restart kubelet
k uncordon cks-master

# ── worker node ──
k drain cks-worker --ignore-daemonsets               # (run on master)
# on the worker:
sed -i 's/v1.36/v1.37/' /etc/apt/sources.list.d/kubernetes.list && apt-get update
apt-mark unhold kubeadm && apt-get install -y kubeadm=1.37.1-1.1 && apt-mark hold kubeadm
kubeadm upgrade node                                 # ← worker uses "upgrade node", not "apply"
apt-mark unhold kubelet kubectl && apt-get install -y kubelet=1.37.1-1.1 kubectl=1.37.1-1.1 && apt-mark hold kubelet kubectl
systemctl daemon-reload && systemctl restart kubelet
k uncordon cks-worker                                # (run on master)
k get node                                           # both on new version
```
> ⚠️ Since 2023 the k8s apt repo (`pkgs.k8s.io`) is **one repo per minor version**. If `apt-cache madison kubeadm` doesn't show the new minor, update the URL in `/etc/apt/sources.list.d/kubernetes.list` first. (My worker already has both v1.36 and v1.37 repos listed.)
> Version strings look like `1.37.1-1.1`. Copy them from `apt-cache madison`.

#### Section summary
- Why upgrade frequently
- K8s release cycles
- Version differences
- How to upgrade a cluster

#### Cross-reference: apiserver config files need a volume mount
Every new file/dir you point an apiserver flag at needs **flag + volumeMount + hostPath volume**. Applies to `--encryption-provider-config`, `--audit-policy-file`, `--audit-log-path`, `--admission-control-config-file`. Otherwise: `open /etc/kubernetes/...: no such file or directory` in `crictl logs`.
```yaml
    volumeMounts:
    - name: enc
      mountPath: /etc/kubernetes/enc
      readOnly: true
  volumes:
  - name: enc
    hostPath:
      path: /etc/kubernetes/enc
      type: DirectoryOrCreate
```
Debug a broken apiserver: `crictl ps -a | grep apiserver` → `crictl logs <id>`, or `ls /var/log/pods/kube-system_kube-apiserver-*/`.

## 7. Domain 3 — System Hardening
*Still TODO: AppArmor hands-on (Docker & K8s), seccomp, reduce host OS footprint, IAM*

### 7.1 Kernel Hardening Tools (AppArmor & seccomp)

#### Concept
- Overview
- AppArmor (Docker & K8s)
- Seccomp (Docker & K8s)

#### Linux kernel isolation (recap, see 4.3)
Container isolation = **Namespaces** (restrict what processes can see: other processes, users, filesystem) + **cgroups** (restrict resource usage: RAM, disk, CPU).

#### Where AppArmor & seccomp sit
```
USER SPACE    Applications (Firefox, Curl) → Libraries (glibc, libxyz)
              ┌─────── seccomp │ AppArmor ───────┐   ← extra layer before the kernel
KERNEL SPACE  Syscall Interface (getpid(), reboot()) → Linux Kernel → Hardware
```

#### AppArmor
- A **profile** per application (e.g. Firefox, kubelet) defines what the process may access: Filesystem ✅, Other processes ❌, Networks ❌.

**Profile modes**
| Mode | Effect |
|---|---|
| **Enforce** | Process **cannot escape** |
| **Complain** | Process can escape **but it will be logged** |
| **Unconfined** | Process can escape |

**Main commands**
```bash
aa-status          # show all profiles
aa-genprof         # generate a new profile (smart wrapper around aa-logprof)
aa-complain        # put profile in complain mode
aa-enforce         # put profile in enforce mode
aa-logprof         # update the profile if app produced some more usage logs (syslog)
apparmor_parser    # load a profile file into the kernel (exam!)
```
> `aa-*` tools come from the `apparmor-utils` package: `apt-get install -y apparmor-utils`. `aa-` alone is not a command — use Tab Tab.

#### Hands-on 1: Setup simple AppArmor profile for curl
```bash
apt-get install -y apparmor-utils
aa-genprof curl          # terminal 2: run `curl killer.sh -v` → back in terminal 1: S(can), allow/deny, F(inish)
aa-status | grep curl    # /usr/bin/curl in enforce mode
curl killer.sh -v        # now blocked (network denied)
cat /etc/apparmor.d/usr.bin.curl
aa-logprof               # update profile from new log entries
```
Hand-written alternative (fast, no wizard):
```
cat > /etc/apparmor.d/usr.bin.curl <<'EOF'
#include <tunables/global>
profile curl-deny /usr/bin/curl {
  #include <abstractions/base>
  deny network,
  /usr/bin/curl mr,
}
EOF
apparmor_parser -r /etc/apparmor.d/usr.bin.curl && aa-status | grep curl-deny
```

#### My lab issues with aa-genprof (Ubuntu 24.04, AppArmor 4.x)
`aa-genprof` parses **all** profiles in `/etc/apparmor.d/` first and failed on shipped ones:
| Error | Fix |
|---|---|
| `ERROR: Operation {'runbindable'} cannot have a source. Source = AARE('/')` | `grep -rln runbindable /etc/apparmor.d/` → `abstractions/passt` → move to `/root/aa-bak/` |
| `ERROR: Include file /etc/apparmor.d/abstractions/passt not found` | Move profiles including it: `grep -rl "abstractions/passt" /etc/apparmor.d/` → `usr.bin.passt`, `abstractions/pasta` |
| `ERROR: Include file /var/lib/snapd/apparmor/snap-confine not found` | `mkdir -p /var/lib/snapd/apparmor/snap-confine` |
Restore afterwards:
```bash
mv /root/aa-bak/passt /root/aa-bak/pasta /etc/apparmor.d/abstractions/ && mv /root/aa-bak/usr.bin.passt /etc/apparmor.d/ && apparmor_parser -QT /etc/apparmor.d/ 2>&1 | head
```
> Loaded profiles stay active in the kernel while files are moved. Exam: you'll use `apparmor_parser`, rarely `aa-genprof`.

#### Hands-on 2: AppArmor & Docker / K8s
*Task: Nginx Docker container uses AppArmor profile — TODO, next screenshots.*
K8s preview (1.30+):
```yaml
spec:
  securityContext:
    appArmorProfile:
      type: Localhost
      localhostProfile: <profile-name>     # profile must be loaded on the node running the pod
```

## 8. Domain 4 — Minimize Microservice Vulnerabilities
*Still TODO: Pod Security Admission, securityContext, mTLS/Cilium*

### 8.1 Manage Kubernetes Secrets

#### Concept
- Overview
- Create Secure Secret scenario
- Hack some Secrets 😈

#### Introduction to K8s Secrets
```
CODE   ──► git ──► API server ──► kubelet ──► Application POD
SECRET ──────────► API server ──► kubelet ──► Application POD
```
- Code goes through git/CI. **Secrets must not**: they go to the API server separately, and the kubelet injects them into the pod at runtime.
- Secret = a small amount of sensitive data (password, token, key). Decoupled from the pod spec and image.

#### Simple Secret scenario (hands-on)
`secret1` → **mounted as a file**, `secret2` → **env var**, both used in one pod.
```bash
k create secret generic secret1 --from-literal user=admin
k create secret generic secret2 --from-literal pass=12345678
k run pod --image=nginx -o yaml --dry-run=client > pod.yaml
```
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod
spec:
  containers:
  - name: pod
    image: nginx
    env:
    - name: PASSWORD
      valueFrom:
        secretKeyRef:
          name: secret2
          key: pass
    volumeMounts:
    - name: secret1
      mountPath: /etc/secret1
      readOnly: true
  volumes:
  - name: secret1
    secret:
      secretName: secret1
```
```bash
k -f pod.yaml create
k exec pod -- env | grep PASSWORD          # 12345678
k exec pod -- mount | grep secret1         # tmpfs on /etc/secret1 (ro)
k exec pod -- cat /etc/secret1/user        # admin
```
Shortcut: `k set env pod/<name> --from=secret/secret2 --dry-run=client -o yaml`

#### Hack 1: Secrets via the container runtime (node access)
Anyone with **root on the node** can read every secret used by pods on that node, **without any k8s permissions**.

*Diagram: NODE (kubelet + containerd) ↔ CONTROLPLANE (API server ↔ etcd with secret1, secret2)*

```bash
# on the worker node
crictl ps | grep pod                              # container id, e.g. bf4ba44613d63
crictl inspect bf4ba44613d63 | vim -              # full container config as JSON
```
In the `crictl inspect` output you can find:
- **env vars** → `PASSWORD=12345678` (secret2 in plain text!)
- **mounts** → `"containerPath": "/etc/secret1"`, `"hostPath": "/var/lib/kubelet/pods/<uid>/volumes/kubernetes.io~secret/secret1"`
- the SA token mount `/var/run/secrets/kubernetes.io/serviceaccount` → `.../kubernetes.io~projected/kube-api-access-xxxx`
- `"info": { "pid": 9467, "runtimeType": "io.containerd.runc.v2", ... }`

Read the mounted secret through `/proc`:
```bash
ps aux | grep 9467                                # nginx: master process
ls /proc/9467/root/                               # container's root filesystem
find /proc/9467/root/etc/secret1
cat /proc/9467/root/etc/secret1/user              # admin
# or directly from the kubelet volume dir:
cat /var/lib/kubelet/pods/<uid>/volumes/kubernetes.io~secret/secret1/user
```
> In the video this is shown with Docker (`docker inspect`). With containerd, use `crictl inspect`. Same idea.

#### Hack 2: Secrets via etcd (control plane access)
Secrets are stored **unencrypted (just base64) in etcd by default**. Anyone with etcd access can read them all.

*Diagram: KUBELET + docker (WORKER) ↔ API SERVER ↔ etcd with secret1, secret2 (MASTER)*

```bash
# etcdctl needs the apiserver's etcd client certs (find them in the apiserver manifest)
grep etcd /etc/kubernetes/manifests/kube-apiserver.yaml
ETCDCTL_API=3 etcdctl --cert /etc/kubernetes/pki/apiserver-etcd-client.crt --key /etc/kubernetes/pki/apiserver-etcd-client.key --cacert /etc/kubernetes/pki/etcd/ca.crt endpoint health
ETCDCTL_API=3 etcdctl --cert /etc/kubernetes/pki/apiserver-etcd-client.crt --key /etc/kubernetes/pki/apiserver-etcd-client.key --cacert /etc/kubernetes/pki/etcd/ca.crt get /registry/secrets/default/secret2
# → value 12345678 readable in plain text
```
Key path format: `/registry/<resource>/<namespace>/<name>`

#### Encrypt etcd: EncryptionConfiguration
*Diagram: API SERVER encrypts/decrypts → etcd stores only ciphertext (MASTER).*
The apiserver encrypts before writing to etcd. Flag: `--encryption-provider-config`

```yaml
apiVersion: apiserver.config.k8s.io/v1
kind: EncryptionConfiguration
resources:
- resources:
  - secrets
  providers:
  - identity: {}
  - aesgcm:
      keys:
      - name: key1
        secret: c2VjcmV0IGlzIHNlY3VyZQ==
      - name: key2
        secret: dGhpcyBpcyBwYXNzd29yZA==
  - aescbc:
      keys:
      - name: key1
        secret: c2VjcmV0IGlzIHNlY3VyZQ==
      - name: key2
        secret: dGhpcyBpcyBwYXNzd29yZA==
```
**In order!** The **first provider is used for encryption on save.** All providers are tried in order when reading.
- `identity` first (as above) → new secrets are saved **unencrypted**, but encrypted ones can still be read.

| Goal | Provider order | Then |
|---|---|---|
| **Encrypt all secrets** | `aesgcm`/`aescbc` first, `identity: {}` last | `kubectl get secrets --all-namespaces -o json \| kubectl replace -f -` |
| **Decrypt all secrets** | `identity: {}` first, `aescbc` (old key) after | same `replace` command |

#### Hands-on: Encrypt Secrets in etcd at rest and test it
Encrypt all existing secrets using **aescbc** and a password of our choice.
```bash
mkdir -p /etc/kubernetes/etcd
echo -n passwordpassword | base64          # 16 chars → valid key (16, 24 or 32 bytes)
# better: head -c 32 /dev/urandom | base64
```
```yaml
# /etc/kubernetes/etcd/ec.yaml
apiVersion: apiserver.config.k8s.io/v1
kind: EncryptionConfiguration
resources:
- resources:
  - secrets
  providers:
  - aescbc:
      keys:
      - name: key1
        secret: cGFzc3dvcmRwYXNzd29yZA==
  - identity: {}
```
apiserver manifest: flag `--encryption-provider-config=/etc/kubernetes/etcd/ec.yaml` **+ volumeMount + hostPath volume** (see 8.x).
```bash
k create secret generic very-secure --from-literal cc=1234
ETCDCTL_API=3 etcdctl <certs> get /registry/secrets/default/very-secure      # → k8s:enc:aescbc:v1:key1:...  (ciphertext)
k get secret very-secure -o yaml                                             # still readable via API ✅
k get secrets -A -o json | k replace -f -                                    # encrypt all existing ones
```

#### Video troubleshooting: apiserver won't start after adding the config
```bash
ps aux | grep apiserver                    # nothing → apiserver not running
cd /var/log/pods/ && ls                    # kube-system_kube-apiserver-cks-master_<uid>/
tail -f kube-system_kube-apiserver-cks-master_<uid>/kube-apiserver/2.log
```
```
Error: error while parsing encryption provider configuration file "/etc/kubernetes/etcd/ec.yaml":
resources[0].providers[0].aescbc.keys[0].secret: Invalid value: "REDACTED":
secret is not of the expected length, got 8, expected one of [16 24 32]
```
> **aescbc/aesgcm keys must decode to 16, 24 or 32 bytes.** `echo -n password | base64` = 8 bytes → invalid. Use `passwordpassword` (16) or `head -c 32 /dev/urandom | base64`.
> When the apiserver is down, kubectl is useless. Use `/var/log/pods/` or `crictl logs`.

#### K8s Secret risks (from the k8s docs)
- Secret data is stored in **etcd**, so:
  - Admins should enable **encryption at rest** (v1.13+).
  - Admins should **limit access to etcd** to admin users.
  - Admins may want to wipe/shred disks used by etcd when they're no longer in use.
  - With etcd in a cluster, use **TLS for etcd peer-to-peer** communication.
- A secret in a manifest (JSON/YAML) is only **base64**. Sharing it or checking it into a repo = compromised. **Base64 is not encryption.**
- Applications still need to protect the value after reading it (don't log it, don't send it to untrusted parties).
- Anyone who can **create a pod** in a namespace can mount (and read) any secret in that namespace.
- Root on a node = all secrets used by pods on that node (Hack 1).

#### Why do we even have ConfigMaps and Secrets?
- **ConfigMaps should stay as they are** (non-sensitive config). ⚠️ Don't encrypt them; see my incident in 8.x.
- **For Secrets, more secure ways can be implemented:**
  - Encryption at rest with a **KMS provider** (external key management, e.g. HashiCorp Vault, AWS KMS, Azure Key Vault):
```yaml
providers:
- kms:
    name: myKmsPlugin
    endpoint: unix:///tmp/socketfile.sock
    cachesize: 100
    timeout: 3s
- identity: {}
```
  - External secret stores: HashiCorp Vault, External Secrets Operator, Secrets Store CSI driver.

#### Section summary
- What are Secrets?
- Deploying and using Secrets
- Hacking Secrets (etcd and docker/containerd)
- How Secrets are stored (encrypted)

*"How secret are your most secret secrets?"*

---

### 8.2 Container Runtime Sandboxes

#### Concept
- Technical overview
- Break out of a container
- gVisor and Kata Containers
- OCI and containerd

#### "Containers are not contained"
Just because it runs in a container doesn't mean it's more protected.

#### Technical overview: containers / Docker
```
[App1 Process]   [App2 Process]   [App3 Process]
 KERNEL GROUP     KERNEL GROUP     KERNEL GROUP
════════════════════ Kernel ═══════════════════  ← SHARED
              OPERATING SYSTEM
```
- All containers share **one host kernel**. A process that exploits a kernel bug from App1 can reach App2 (red arrows in the slide).

#### Technical overview: sandbox
Sandbox?
- Playground when implementing an API
- Simulated testing environment
- Development server
- **Security layer to reduce attack surface** ← the CKS meaning

#### Containers and system calls
```
User Space    [App1] [App2]
              [SANDBOX] [SANDBOX]   ← sandbox for every container
              [System Calls]
Kernel Space  [Kernel]
              [Hardware]
```
The sandbox sits between the container and the kernel and intercepts/filters syscalls, so the app never talks to the real kernel directly.

#### Sandboxes don't come for free
- More resources needed
- Might be better for smaller containers
- Not good for syscall-heavy workloads
- No direct access to hardware

#### Hands-on: Why even sandbox? Contact the Linux kernel from inside a container
```bash
k run pod --image=nginx
k exec pod -it -- bash
uname -r                  # 5.4.0-1028-gcp  ← SAME as the host kernel!
exit
uname -r                  # 5.4.0-1028-gcp (on the node)
strace uname -r           # shows the syscall: uname({sysname="Linux", ...})
```
→ The container calls the **host kernel directly**.

**Dirty Cow** (CVE-2016-5195): a Linux kernel privilege-escalation bug. Exploitable **from inside a container** because the kernel is shared → container breakout to root on the host. That's why sandboxes exist.

#### OCI — Open Container Initiative
- Linux Foundation project to design **open standards** for virtualization.
- **Specification:** runtime, image, distribution.
- **Runtime:** `runc` (the container runtime that implements their spec).

```
dockerInc/docker ─use─► moby/moby ─use─► containerd/containerd ─use─► oci/runc ─► oci/runc/libcontainer
                                                                         ▲
                                           oci/runtime-spec ──impl──────┘
```
- Kubernetes (kubelet) → **CRI** → containerd → **OCI runtime** (`runc` by default, or `runsc` / `kata-runtime`).
- Because of these standards, you can swap `runc` for a sandboxed runtime **per pod**.

#### Kubernetes runtimes and CRI (Container Runtime Interface)
**Early days:** `kubelet → dockershim → dockerd → containerd → runc`. Kubelet could only talk to Docker. containerd is a CNCF project; runc follows the OCI spec.

**Today, with CRI:** the kubelet talks to **any** CRI-compatible runtime.
```
                          kubelet
   ┌──────────┬──────────┬──────┴─────┬──────────────┐
dockershim  cri-containerd  cri-o   containerd     singularity-cri
   │            │            │          │                │
 dockerd    containerd     runc     Shim API         singularity
 containerd   runc                 ┌───┼────────┐
 runc                            Kata Firecracker gVisor
```
- Choosing the runtime for the whole node: `kubelet --container-runtime-endpoint` (e.g. `unix:///run/containerd/containerd.sock`).
- Choosing it **per pod**: **RuntimeClass** (or annotations, in older setups).
- dockershim was **removed in k8s 1.24**. Docker images still run fine, because they're OCI images.

#### Kata Containers
- Strong separation layer.
- Runs **every container in its own private VM** (hypervisor based).
- **QEMU** as default. It needs virtualisation, like **nested virtualisation in the cloud** (often not available on EC2/GCE).
```
[VM: Process A | Namespaces | Linux Kernel A]  [VM: Process B | ... | Kernel B]  [VM: Process C | ... | Kernel C]
[Hardware Virtualization]                      [Hardware Virtualization]          [Hardware Virtualization]
══════════════════════════════════ Host Linux Kernel ══════════════════════════════════
           Additional isolation with a lightweight VM and individual kernels
```

#### gVisor — user-space kernel for containers
- Another layer of separation.
- **NOT** hypervisor/VM based.
- Simulates kernel syscalls with limited functionality.
- Runs in userspace, separated from the Linux kernel.
- Runtime is called **`runsc`**.
```
Application
──── System Calls ────
gVisor                 ← independent (user-space) kernel
──── Limited System Calls ────   ← virtualization-based strong isolation
Host Kernel
──── Hardware ────
```

#### Sandbox runtimes
| | **gVisor** (`runsc`, Google) | **Kata Containers** |
|---|---|---|
| How | **User-space kernel** written in Go. Intercepts syscalls and implements them itself | Each container runs in its **own lightweight VM** (own kernel, hypervisor) |
| Isolation | Strong (limited syscalls reach the host) | Strongest (hardware virtualization) |
| Needs | Nothing special | Nested virtualization / KVM, so often not possible on cloud VMs |
| Overhead | Syscall-heavy apps are slower | VM boot and memory overhead |

#### Hands-on: RuntimeClasses
Create and use a RuntimeClass for runtime `runsc` (gVisor).

*Installation of gVisor/runsc with containerd:* Worker node → KUBELET → containerd → `runc` (default) **or** `runsc (gvisor)`.

```yaml
apiVersion: node.k8s.io/v1
kind: RuntimeClass
metadata:
  name: gvisor
handler: runsc            # must match the runtime name in the containerd config
---
apiVersion: v1
kind: Pod
metadata:
  name: gvisor
spec:
  runtimeClassName: gvisor
  containers:
  - name: gvisor
    image: nginx
```
```bash
k get runtimeclass
k exec gvisor -- dmesg | head -3       # "Starting gVisor..." → sandboxed
k exec gvisor -- uname -r              # fake gVisor kernel, ≠ node kernel
```
Install and troubleshooting on my cluster (old runsc, cgroups v2, `hash -r`) → see **8.y** below.

> 💡 containerd shows the runtime per container: `crictl inspect <id> | grep runtimeType` → `io.containerd.runsc.v1` vs `io.containerd.runc.v2`.

#### Section summary
- Container sandboxes
- containerd
- kata-containers (VM)
- gVisor/runsc (kernel)
- k8s runtimes (CRI, RuntimeClass)

*"crictl rmp the-end"* 😄

---

### 8.3 OS Level Security Domains

#### Concept
- Security Contexts
- Pod Security Policies (⚠️ removed in v1.25, see below)
- Scenarios

#### Security Contexts
Define privilege and access control for a Pod/Container:
- userID and groupID
- Run privileged or unprivileged
- Linux capabilities

```yaml
spec:
  volumes:
  - name: vol
    emptyDir: {}
  securityContext:          # ← Pod level (all containers)
    runAsUser: 1000
    runAsGroup: 3000
    fsGroup: 2000
  containers:
  - command: ["sh", "-c", "sleep 1d"]
    image: busybox
    name: my-pod
    securityContext:        # ← Container level (overrides pod level)
      runAsUser: 0
```
```
/ $ id
uid=1000  gid=3000  groups=2000
     │        │           └─ fsGroup     = supplementary group id (owns mounted volumes)
     │        └─ runAsGroup = main group id
     └─ runAsUser = user id
```
> Pod-level fields apply to all containers. Container-level fields win if both are set. `capabilities`, `privileged`, `allowPrivilegeEscalation` and `readOnlyRootFilesystem` exist **only** at container level.

#### Hands-on 1: Security Contexts & UID/GID
Change the user and group under which the container processes are running.
```bash
k run pod --image=busybox --command -o yaml --dry-run=client -- sh -c 'sleep 1d' > pod.yaml
# add spec.securityContext: runAsUser 1000, runAsGroup 3000
k -f pod.yaml create
k exec -it pod -- sh
id                          # uid=1000 gid=3000
touch test                  # Permission denied (/ is owned by root)
cd /tmp && touch test && ls -lh   # works: owned by 1000:3000
```

#### Hands-on 2: Force container to run as non-root
```yaml
    securityContext:
      runAsNonRoot: true
```
- Image runs as root and no `runAsUser` is set → pod fails with `CreateContainerConfigError: container has runAsNonRoot and image will run as root`.
- Fix: also set `runAsUser: 1000` (pod or container level).

#### Privileged containers
- By default, Docker containers run **"unprivileged"**.
- You can run them as privileged to:
  - access all devices,
  - run the Docker daemon inside a container (`docker run --privileged`).
- **Privileged means that container user 0 (root) is directly mapped to host user 0 (root).**
- By default in Kubernetes, containers are **not** running privileged.
```yaml
    securityContext:
      privileged: true
```

#### Hands-on 3: Enable privileged and test using sysctl (his terminal)
```bash
k exec -it pod -- sh
/ # sysctl kernel.hostname=attacker
sysctl: error setting key 'kernel.hostname': Read-only file system     # not privileged
/ # id
uid=0(root) gid=0(root) groups=10(wheel)                              # root, but still blocked!

# set securityContext.privileged: true, recreate:
k -f pod.yaml delete --force --grace-period 0 && k -f pod.yaml create
k exec -it pod -- sh
/ # sysctl kernel.hostname=attacker
kernel.hostname = attacker                                             # ✅ kernel setting changed
```
> Key point: being **root inside a container ≠ privileged**. Without `privileged`, root is still restricted (read-only `/proc/sys`, limited capabilities).

#### PrivilegeEscalation
| **Privileged** | **PrivilegeEscalation** |
|---|---|
| Container user 0 (root) is directly mapped to host user 0 (root) | Controls whether a process can gain **more privileges than its parent** process |

- **By default, Kubernetes allows PrivilegeEscalation.**
```yaml
    securityContext:
      allowPrivilegeEscalation: false
```

#### Hands-on 4: Disable PrivilegeEscalation (his terminal)
```bash
k exec -it pod -- sh
/ # cat /proc/1/status | grep NoNewPrivs
NoNewPrivs:     0     # escalation allowed (default)
# set allowPrivilegeEscalation: false, recreate pod
NoNewPrivs:     1     # escalation blocked ✅
```
`/proc/1/status` also shows `CapEff` (effective capabilities, decode with `capsh --decode=<hex>`) and `Seccomp` (0 = disabled, 2 = filter).

#### Pod Security Policies → Pod Security Admission (2026 note)
> ⚠️ The video covers **PodSecurityPolicy (PSP)**. PSP was deprecated in 1.21 and **removed in 1.25**. Its replacement is **Pod Security Admission (PSA)**, which is built in and enabled by default.

PSA works with **namespace labels** and three levels: `privileged` (no restrictions) → `baseline` (blocks known escalations like privileged and hostPath) → `restricted` (non-root, drop ALL caps, seccomp, no escalation).
```bash
k label ns team-a pod-security.kubernetes.io/enforce=baseline        # reject violating pods
k label ns team-a pod-security.kubernetes.io/warn=restricted         # warn only
k label ns team-a pod-security.kubernetes.io/audit=restricted        # audit log only
k -n team-a run p --image=nginx --privileged                         # → Error: violates PodSecurity "baseline:latest"
```
> ⚠️ PSA only checks pods **at creation**. Existing pods keep running. Delete and recreate them (or `rollout restart` the deployment) to see the violation.

A pod that passes `restricted`:
```yaml
spec:
  securityContext:
    runAsNonRoot: true
    seccompProfile:
      type: RuntimeDefault
  containers:
  - name: app
    image: nginx
    securityContext:
      allowPrivilegeEscalation: false
      capabilities:
        drop: ["ALL"]
```

---

### 8.4 mTLS — Mutual TLS

#### Concept
- mTLS / pod-to-pod communication
- Service meshes
- Scenarios

#### mTLS — Mutual TLS
- Mutual authentication.
- Two-way (bilateral) authentication.
- Two parties authenticating each other at the same time.

#### K8s pod-to-pod communication
```
Request ──https──► [🔒 Ingress] ──► pod1 🔒 ─── pod2 🔒
                                     │  ╲     ╱  │
                                    pod4 🔒 ── pod3 🔒          😈 attacker inside the cluster
```
- By default, pod-to-pod traffic is **unencrypted**, and TLS usually terminates at the Ingress.
- If an attacker gets into the cluster, they can **intercept/sniff traffic between pods**.
- mTLS: **every pod** encrypts and authenticates its traffic.

#### mTLS between pods
```
                       CA
          pod1 [app 🔒] ⇄ [🔒 app] pod2
     client cert + server cert   server cert + client cert
                Rotate!
```
- Each app needs a **client cert** (to call others) and a **server cert** (to be called), all signed by a shared **CA**.
- Certs must be **rotated** regularly → painful to do inside every app.

#### ServiceMesh / Proxy
```
pod1 [app ↓ proxy 🔒] ────────► [🔒 proxy ↑ app] pod2
       ▲
 Service Mesh (external manager): CA + certs
```
- A **proxy sidecar** in every pod handles TLS. The app itself talks plain HTTP to localhost.
- A **service mesh** (Istio, Linkerd) manages the CA and issues/rotates the certs automatically.
- **iptables rules route traffic via the proxy**:
  - like by initContainer,
  - **needs the `NET_ADMIN` capability**.
- Alternative (current curriculum): **Cilium** with transparent encryption (WireGuard/IPsec), no sidecars.

#### Hands-on: Create a proxy sidecar with NET_ADMIN capability (his YAML)
```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: app
  name: app
spec:
  containers:
  - command:
    - sh
    - -c
    - ping google.com
    image: bash
    name: app
  - name: proxy
    image: ubuntu
    command:
    - sh
    - -c
    - 'apt-get update && apt-get install iptables -y && iptables -L && sleep 1d'
    securityContext:
      capabilities:
        add: ["NET_ADMIN"]
  dnsPolicy: ClusterFirst
  restartPolicy: Always
```
```bash
k -f app.yaml create
k logs app -c proxy          # iptables -L output → works
# without NET_ADMIN: "iptables: Permission denied (you must be root)" / "can't initialize iptables table"
```
> Capabilities are per container, so the `app` container still has no `NET_ADMIN`.

#### Section summary
- mTLS
- Pod-to-pod communication
- Create sidecar proxy
- `NET_ADMIN` capability

> **Cluster Reset:** every new section works with a fresh cluster.

---

### 8.5 Open Policy Agent (OPA) / Gatekeeper

#### Concept
- Introduction to OPA and Gatekeeper
- Enforce labels
- Enforce pod replica count

#### Request workflow (recap)
`Human user / Pod (SA)` → **1 Authentication** ("who are you?") → **2 Authorization** ("are you allowed to create pods?") → **3 Admission Control** ("has the pod all required labels?") → **4** etcd.
→ OPA Gatekeeper plugs in at step **3** as a validating (and mutating) **admission webhook**.

#### OPA — Open Policy Agent
> "The Open Policy Agent (OPA) is an open source, general-purpose policy engine that enables unified, context-aware policy enforcement across the entire stack."

- Not Kubernetes specific.
- Easy implementation of policies (**Rego** language).
- Works with JSON/YAML.
- In K8s it uses **Admission Controllers**.
- Does not know concepts like pods or deployments.

#### OPA Gatekeeper
- Provides **K8s CRDs** (Custom Resource Definitions) on top of OPA, so policies are k8s objects.
```
ConstraintTemplate  (apiVersion: templates.gatekeeper.sh/v1, kind: ConstraintTemplate, name: k8srequiredlabels)
   "search for necessary labels"  → contains the Rego + creates a new CRD kind: K8sRequiredLabels
        │                                   │
        ▼                                   ▼
Constraint (kind: K8sRequiredLabels)   Constraint (kind: K8sRequiredLabels)
 "Pods must have label X"               "Namespaces must have label Y"
```

#### Hands-on 1: Install OPA Gatekeeper
```bash
# make sure no other admission plugins interfere (course: only NodeRestriction)
grep admission /etc/kubernetes/manifests/kube-apiserver.yaml
kubectl apply -f https://raw.githubusercontent.com/open-policy-agent/gatekeeper/v3.18.0/deploy/gatekeeper.yaml
k -n gatekeeper-system get pod,svc
k get crd | grep gatekeeper
k get validatingwebhookconfigurations
```
> Check the latest release tag on GitHub (`open-policy-agent/gatekeeper`). The course repo pins an old version.

#### Hands-on 2: Deny All — Approve All policy
Course files: `cks-course-environment/course-content/opa/deny-all/`
```yaml
apiVersion: templates.gatekeeper.sh/v1
kind: ConstraintTemplate
metadata:
  name: k8salwaysdeny
spec:
  crd:
    spec:
      names:
        kind: K8sAlwaysDeny
      validation:
        openAPIV3Schema:
          properties:
            message:
              type: string
  targets:
  - target: admission.k8s.gatekeeper.sh
    rego: |
      package k8salwaysdeny
      violation[{"msg": msg}] {
        1 > 0                                  # always true → always violation
        msg := input.parameters.message
      }
---
apiVersion: constraints.gatekeeper.sh/v1beta1
kind: K8sAlwaysDeny
metadata:
  name: pod-always-deny
spec:
  match:
    kinds:
    - apiGroups: [""]
      kinds: ["Pod"]
  parameters:
    message: "ACCESS DENIED!"
```
```bash
k get constrainttemplates
k get k8salwaysdeny
k run pod --image=nginx       # Error from server (Forbidden): admission webhook "validation.gatekeeper.sh" denied the request: [pod-always-deny] ACCESS DENIED!
k describe k8salwaysdeny pod-always-deny     # shows "Total Violations" by EXISTING resources (audit)
```
Change the rule to `1 > 2` (never true) → **approve all**: pods can be created again.

His live edit (video uses `templates.gatekeeper.sh/v1beta1`; current Gatekeeper uses `v1`, both work):
```rego
violation[{"msg": msg}] {
  1 > 0 # true
  1 > 2 # false      ← adding this line makes the whole rule false → no violation → approve all
  msg := input.parameters.message
}
```
> A Rego rule produces a violation only if **all** lines inside it are true (implicit AND).

#### Hands-on 3: Enforce namespace/pod labels
Course files: `course-content/opa/namespace-labels/`. Library: `open-policy-agent/gatekeeper-library` → `requiredlabels`.
```yaml
apiVersion: constraints.gatekeeper.sh/v1beta1
kind: K8sRequiredLabels
metadata:
  name: ns-must-have-cks
spec:
  match:
    kinds:
    - apiGroups: [""]
      kinds: ["Namespace"]
  parameters:
    labels: ["cks"]
```
```bash
k create ns test              # denied: you must provide labels: {"cks"}
k create ns test --dry-run=client -o yaml | k label -f - cks=amazing --local -o yaml | k create -f -
```

#### Hands-on 4: Enforce Deployment replica count
Course files: `course-content/opa/deployment-replica-count/`. Constraint kind `K8sMinReplicaCount` with `parameters.min: 2`.
```bash
k create deploy test --image=nginx          # denied: replicas 1 < min 2
k create deploy test --image=nginx --replicas 3   # ✅
```

#### Rego Playground
https://play.openpolicyagent.org — test Rego with INPUT/OUTPUT before putting it in a template.
```rego
package play
default hello = false
hello {
    m := input.message
    m == "world"
}
```
INPUT `{"message": "world"}` → OUTPUT `{"hello": true}`. Change the input to `"hello"` → `false`. And more examples are under the "Examples" menu.

> ⚠️ Newer Rego (OPA v1) syntax requires `if`/`contains`: `violation contains {"msg": msg} if { ... }`. Gatekeeper still accepts the old syntax by default.

#### Section summary
- OPA overview
- Used Gatekeeper's CRDs
- Created templates and constraints
- Rego


### 8.x etcd Encryption at Rest — Lab Incident & Recovery (from my lab, Nov 2026)

#### Provider rules (memorise these)
- The **first** provider in the list is used to **write** (encrypt).
- **All** providers are tried in order to **read** (decrypt).
- `identity: {}` = plain text. Keep it in the list (last) while existing plain-text data still exists.
- Never remove an old key until all data has been **rewritten** with the new one.
- The apiserver reads `enc.yaml` **only at startup**, so restart it after every change.
- Prefix in etcd: `k8s:enc:aescbc:v1:<keyname>:` = encrypted; readable YAML/protobuf = plain text.

#### Setup (the correct way)
```bash
mkdir -p /etc/kubernetes/enc
head -c 32 /dev/urandom | base64          # → key
```
```yaml
# /etc/kubernetes/enc/enc.yaml
apiVersion: apiserver.config.k8s.io/v1
kind: EncryptionConfiguration
resources:
- resources:
  - secrets                 # ← secrets ONLY unless the task says otherwise
  providers:
  - aescbc:
      keys:
      - name: key1
        secret: <base64-32-byte-key>
  - identity: {}            # ← keep: allows reading existing plain-text secrets
```
apiserver manifest: **flag + volumeMount + hostPath volume** (missing mount → `open /etc/kubernetes/enc/enc.yaml: no such file or directory`)
```yaml
    - --encryption-provider-config=/etc/kubernetes/enc/enc.yaml
    volumeMounts:
    - name: enc
      mountPath: /etc/kubernetes/enc
      readOnly: true
  volumes:
  - name: enc
    hostPath:
      path: /etc/kubernetes/enc
      type: DirectoryOrCreate
```
```bash
# encrypt all existing secrets + verify
k get secrets -A -o json | k replace -f -
k create secret generic test-enc --from-literal=a=b
ETCDCTL_API=3 etcdctl --cacert /etc/kubernetes/pki/etcd/ca.crt --cert /etc/kubernetes/pki/apiserver-etcd-client.crt --key /etc/kubernetes/pki/apiserver-etcd-client.key get /registry/secrets/default/test-enc | hexdump -C | head
# → must show k8s:enc:aescbc:v1:key1
```

#### What went wrong
I had `configmaps` in `resources` as well, then changed the config. Configmaps that were written while that rule was active ended up encrypted, and the apiserver later couldn't read them.

**Symptoms:**
```
kube-controller-manager  CrashLoopBackOff
  err="StorageError: corrupt object ... Key: /configmaps/kube-system/kube-apiserver-legacy-service-account-token-tracking
       ... data from the storage is not transformable revision=0: no matching prefix found"
apiserver logs:
  cacher (configmaps): unexpected ListAndWatch error: failed to read one or more configmaps ... (and 17 more)
new pods (coredns, kube-proxy, weave-net) stuck ContainerCreating:
  FailedMount ... volume "kube-api-access-xxxx": failed to sync configmap cache: timed out waiting for the condition
  FailedMount ... volume "kube-proxy": failed to sync configmap cache
k -n kube-system get cm kube-proxy → InternalError: corrupt object ... no matching prefix found
```
**Why the whole cluster broke:** every pod mounts `kube-root-ca.crt` (SA token volume), and kube-proxy needs the `kube-proxy` configmap. If even **one** configmap can't be decrypted, the apiserver can't **list** configmaps at all, so every kubelet's configmap watch fails.

`no matching prefix found` = none of the configured providers can decode the stored object. Either:
1. No `identity: {}` → can't read **plain-text** data, or
2. The resource was removed from the config / the key changed → can't read **encrypted** data.

> ⚠️ Removing `configmaps` from `enc.yaml` does **not** decrypt anything. It only makes the already-encrypted objects unreadable.

#### The fix that worked: decrypt properly (identity FIRST, old key SECOND)
```yaml
apiVersion: apiserver.config.k8s.io/v1
kind: EncryptionConfiguration
resources:
- resources:
  - secrets
  providers:
  - aescbc:
      keys:
      - name: key1
        secret: <your key>
  - identity: {}
- resources:
  - configmaps
  providers:
  - identity: {}          # 1st → writes go out as plain text
  - aescbc:               # 2nd → can still read the encrypted ones
      keys:
      - name: key1
        secret: <same key as before>
```
```bash
# 1. restart apiserver (reload enc.yaml)
mv /etc/kubernetes/manifests/kube-apiserver.yaml /tmp/ && sleep 20 && mv /tmp/kube-apiserver.yaml /etc/kubernetes/manifests/
# 2. confirm everything is readable again
k get cm -A > /dev/null && echo ALL-CM-OK
# 3. rewrite all configmaps → stored as plain text
k get cm -A -o json | k replace -f -
# 4. optional: remove the configmaps block from enc.yaml, restart apiserver again
# 5. recover workloads
systemctl restart kubelet                                   # on BOTH nodes
k -n kube-system delete pod <stuck pods> --force --grace-period=0
k -n kube-system get pods -w
```
> This is also the **official way to disable encryption**: put `identity` first, rewrite, then remove the key.

#### Fallback if the old key is lost
Delete the unreadable objects directly in etcd (`kubectl delete` fails on corrupt objects):
```bash
E="etcdctl --cacert /etc/kubernetes/pki/etcd/ca.crt --cert /etc/kubernetes/pki/apiserver-etcd-client.crt --key /etc/kubernetes/pki/apiserver-etcd-client.key"; for k in $($E get /registry/configmaps --prefix --keys-only); do $E get "$k" --print-value-only </dev/null | grep -aq "k8s:enc:" && echo "$k" && $E del "$k" </dev/null; done
```
- `kube-root-ca.crt` (every ns) → recreated automatically by the controller-manager.
- `kube-proxy` → `kubeadm init phase addon kube-proxy --apiserver-advertise-address=<ip> --pod-network-cidr=<cidr>`
- `coredns` → `kubeadm init phase addon coredns`
- Leader-election locks (e.g. `ingress-nginx/ingress-controller-leader`) → just delete them, they get recreated.

> ⚠️ **Script gotcha:** in `etcdctl ... | while read k; do etcdctl get ...; done` the inner `etcdctl` **swallows stdin**, so only the first keys get checked. Use `for k in $(...)` and `</dev/null` on the inner commands, as above.

#### Lessons
- Encrypt **`secrets` only** unless the task explicitly says otherwise. Encrypting `configmaps` puts every system configmap behind one key.
- Always keep `identity: {}` and old keys in the list until you've rewritten all the data.
- Back up `enc.yaml` and the apiserver manifest before every change.
- Debug order: `crictl ps -a` → `crictl logs <apiserver>` → `k -n kube-system logs kube-apiserver-<node> | grep -i transform` → `k describe pod` events.

#### Related incident: anonymous auth — see below

### 8.y Container Runtime Sandbox (gVisor / RuntimeClass) — Lab Incident & Recovery (from my lab, Nov 2026)
*Course chapter: "Container Runtime Sandboxes" at 5:26:44. Screenshots still to come.*

#### Setup (the correct way)
```yaml
# RuntimeClass: handler must match the runtime name in the containerd config
apiVersion: node.k8s.io/v1
kind: RuntimeClass
metadata:
  name: gvisor
handler: runsc
---
apiVersion: v1
kind: Pod
metadata:
  name: gvisor
spec:
  runtimeClassName: gvisor
  containers:
  - name: gvisor
    image: nginx
```
containerd runtime entry (`/etc/containerd/config.toml` on the node that runs the pod):
```toml
# containerd 1.x (config version = 2)
[plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runsc]
  runtime_type = "io.containerd.runsc.v1"

# containerd 2.x (config version = 3)
[plugins."io.containerd.cri.v1.runtime".containerd.runtimes.runsc]
  runtime_type = "io.containerd.runsc.v1"
```
> containerd 2.x converts a version-2 config automatically when it loads it. Check what's actually loaded with `containerd config dump | grep -A2 "runtimes.runsc"`.

#### What went wrong (two errors, in sequence)
```
Warning  FailedCreatePodSandBox  kubelet  ... unable to get OCI runtime for sandbox "...": no runtime for "runsc" is configured
Warning  FailedCreatePodSandBox  kubelet  ... OCI runtime create failed: creating container: cgroups V2 is not yet supported. Enable cgroups V1 and retry
```
| Error | Cause | Fix |
|---|---|---|
| `secret is not of the expected length, got 8, expected one of [16 24 32]` | aescbc key too short | 8.1 |
| apiserver down, kubectl useless | Read `/var/log/pods/kube-system_kube-apiserver-*/` or `crictl logs` | 8.1 |
| `no runtime for "runsc" is configured` | containerd doesn't know the runtime: missing `runtimes.runsc` section, or containerd wasn't restarted | Add the section, `systemctl restart containerd` |
| `cgroups V2 is not yet supported` | **Old runsc binary.** The course install script pins `release-20210806.0`; Ubuntu 24.04 uses cgroup v2 | **Upgrade gVisor**, don't switch to cgroup v1 (new kubelets refuse to run on cgroup v1) |

#### The fix that worked (on the worker)
```bash
runsc --version; which -a runsc containerd-shim-runsc-v1
# → release-20210806.0, both in /usr/local/bin  (old, from the course script)

# install current gVisor from the official apt repo
curl -fsSL https://gvisor.dev/archive.key | gpg --dearmor -o /usr/share/keyrings/gvisor-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/gvisor-archive-keyring.gpg] https://storage.googleapis.com/gvisor/releases release main" > /etc/apt/sources.list.d/gvisor.list
apt-get update && apt-get install -y runsc            # → 20261116.0, installs to /usr/bin

# remove the old binaries (they come first in PATH)
rm -f /usr/local/bin/runsc /usr/local/bin/containerd-shim-runsc-v1
hash -r                                                # ← bash cached the old path!
runsc --version && which runsc containerd-shim-runsc-v1

containerd --version                                   # v2.4.2
containerd config dump | grep -A2 "runtimes.runsc"
systemctl restart containerd
```
On the master:
```bash
k delete pod gvisor --force --grace-period=0 && k apply -f gvisor-pod.yaml
k get pod gvisor -w
k exec gvisor -- dmesg | head -3        # → "Starting gVisor..."  = sandboxed ✅
k exec gvisor -- uname -r               # gVisor's fake kernel version, not the node's
```

#### Gotchas
- `-bash: /usr/local/bin/runsc: No such file or directory` after deleting the old binary: bash **caches command paths**. Fix: `hash -r`.
- The shim `containerd-shim-runsc-v1` must be in containerd's PATH, next to `runsc`. Upgrade or remove **both** together.
- The RuntimeClass `handler` name must **exactly** match the `runtimes.<name>` key in the containerd config.
- The pod only works on nodes where runsc is installed. Use a `nodeSelector` / RuntimeClass `scheduling.nodeSelector` if not every node has it.

#### Debug order
`k describe pod` (sandbox events) → `runsc --version` / `which -a runsc` on the node → `containerd config dump | grep runsc` → `journalctl -u containerd -n 30` → `systemctl restart containerd`.

---

### 8.z Related incident: `--anonymous-auth=false` → apiserver restart loop
The apiserver's startup/liveness/readiness probes (`/livez`, `/readyz`) are **anonymous** HTTPS requests from the kubelet. With `--anonymous-auth=false` they get 401, so the apiserver keeps restarting (`0/1`, restart count climbing), the controller-manager goes into `CrashLoopBackOff`, and pods go `Unknown`.
Fix: remove the flag (`sed -i '/--anonymous-auth=false/d' /etc/kubernetes/manifests/kube-apiserver.yaml`). To keep anonymous off properly, use an `AuthenticationConfiguration` that allows anonymous **only** for `/livez`, `/readyz` and `/healthz`.

## 9. Domain 5 — Supply Chain Security
*Still TODO: ImagePolicyWebhook, SBOM, image signing (cosign)*

### 9.1 Image Footprint

#### Concept
- Containers and Docker
- Reduce image footprint (multi-stage)
- Secure images

#### Containers vs VMs (recap, see 4.1)
- Containers: app process in a kernel group on the **shared** kernel → *kernel can access app process*.
- VMs: own kernel + OS → *host cannot access the app process*.

#### Containers and Docker — Layers
```dockerfile
FROM ubuntu                                       # ← import layers
RUN apt-get update && apt-get install -y golang-go   # ← add new layer
CMD ["sh"]
```
> **Only the instructions `RUN`, `COPY`, `ADD` create layers.** Other instructions create temporary intermediate images and do not increase the size of the build.

#### Why reduce the footprint
```
Debian      → Base base image ┐
Nginx       → Base image      │  Size adds up
install curl → My image        ┘
```
Every layer adds size **and** attack surface (more packages = more CVEs, more tools for an attacker).

#### Hands-on 1: Reduce image footprint via multi-stage build
We look at an example Golang Dockerfile and reduce the footprint via multi-stage build.
Course files: `cks-course-environment/course-content/supply-chain-security/image-footprint/`

**Before (single stage, his screen):**
```dockerfile
FROM ubuntu
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y golang-go
COPY app.go .
RUN CGO_ENABLED=0 go build app.go
CMD ["./app"]
```
```bash
docker build -t app .
docker run app
docker image ls | grep app        # ~700 MB (Ubuntu + Go toolchain)
```
**After (multi-stage, his screen):**
```dockerfile
FROM ubuntu
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y golang-go
COPY app.go .
RUN CGO_ENABLED=0 go build app.go

FROM alpine
COPY --from=0 /app .              # --from=0 = first stage (or name it: FROM ubuntu AS build → --from=build)
CMD ["./app"]
```
```bash
docker build -t app .
docker image ls | grep app        # ~7 MB — only alpine + the static binary
```
Docs: docs.docker.com → Dockerfile best practices → *Use multi-stage builds*.

#### Hands-on 2: Secure and harden images
1. **Use specific package versions**
2. **Don't run as root**
3. **Make filesystem read-only**
4. **Remove shell access**

```dockerfile
FROM ubuntu:20.04
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y golang-go=2:1.13~1ubuntu2      # 1. pinned versions (base image AND packages)
COPY app.go .
RUN pwd
RUN CGO_ENABLED=0 go build app.go

FROM alpine:3.12.0                                                       # 1. pinned base image
RUN chmod a-w /etc                                                       # 3. read-only (example: /etc)
RUN addgroup -S appgroup && adduser -S appuser -G appgroup -h /home/appuser   # 2. non-root user
RUN rm -rf /bin/*                                                        # 4. remove shell + tools (do this LAST)
COPY --from=0 /app /home/appuser/
USER appuser                                                             # 2. switch user
CMD ["/home/appuser/app"]
```
```bash
docker run -d --name app app
docker exec -it app sh        # OCI runtime exec failed: "sh": executable file not found → shell removed ✅
docker exec app whoami        # appuser (if whoami still existed)
```
> Order matters: `rm -rf /bin/*` must come after every `RUN` that needs a shell.
> In k8s, enforce the same at runtime: `securityContext.runAsNonRoot`, `readOnlyRootFilesystem: true` (8.3).

#### Section summary
- Reduce image footprint
- Use multi stage build
- Secure images with restrictions


#### Lab note: multi-stage build failed on `apt-get` (from my lab, Nov 2026)
```
E: Failed to fetch http://archive.ubuntu.com/.../liblocale-gettext-perl_1.07-4_amd64.deb  Connection failed [IP: 185.125.190.82 80]
The command '/bin/sh -c apt-get update && apt-get install -y golang-go=2:1.13~1ubuntu2' returned a non-zero code: 100
```
- **Cause:** a network drop during one package download. The Dockerfile itself was fine.
- **Fixes:** retry with `docker build --no-cache -t app .`, **or** point apt at the AWS regional mirror (`sed -i 's|http://archive.ubuntu.com|http://eu-west-1.ec2.archive.ubuntu.com|g' /etc/apt/sources.list`), **or** (best) use an official Go image for the build stage:
```dockerfile
# build container stage 1
FROM golang:1.23-alpine
WORKDIR /
COPY app.go .
RUN CGO_ENABLED=0 go build app.go

# app container stage 2
FROM alpine:3.20
RUN addgroup -S appgroup && adduser -S appuser -G appgroup -h /home/appuser
COPY --from=0 /app /home/appuser/
USER appuser
CMD ["/home/appuser/app"]
```
Same lesson (multi-stage → small image, non-root user) with supported base images. Check with `docker images | grep app`.

### 9.2 Static Analysis of User Workloads

#### Concept
- Static analysis
- Manual approach
- Tools for Kubernetes and scenarios

#### Static analysis
- Looks at **source code and text files** (k8s YAML, Dockerfiles).
- Checks against **rules**.
- Enforces rules.

#### Static analysis rules (examples)
- "Always define resource requests and limits" → *depends on use case and company or project*.
- "Pods should never use the default ServiceAccount".
- **Generally: don't store sensitive data plain in K8s/Docker files** (no passwords in `env:` or `ENV`).

#### Static analysis in CI/CD
```
Code → Commit → GitHub → Build → Test → Deploy → K8s
  ▲        ▲               ▲                       ▲
  └────────┴─ Static Analysis (kubesec, conftest) ┘
                                    PSP / OPA (admission) ─┘
```
Run it as early as possible (pre-commit, CI). Admission control (PSA/OPA Gatekeeper) is the last line in the cluster.

#### Manual approach
Read the YAML/Dockerfile yourself: secrets in plain text, `privileged`, `hostPath`, `runAsUser: 0`, `latest` tags, missing limits.

#### Kubesec
- Security risk analysis for Kubernetes resources.
- Open source.
- **Opinionated!** Fixed set of rules (security best practices).
- Run as: binary, docker container, kubectl plugin, admission controller (kubesec-webhook).
- Live demo: kubesec.io (hosted API `v2.kubesec.io/scan`).

#### Hands-on 1: Kubesec (my terminal)
```bash
k run nginx --image=nginx --dry-run=client -oyaml > nginx.yaml
docker run -i kubesec/kubesec:512c5e0 scan /dev/stdin < nginx.yaml
```
Output (JSON):
```json
"object": "Pod/nginx.default",
"valid": true,
"message": "Passed with a score of 0 points",
"score": 0,
"scoring": { "advise": [
  { "selector": "containers[] .securityContext .readOnlyRootFilesystem == true",
    "reason": "An immutable root filesystem can prevent malicious binaries being added to PATH and increase attack cost" },
  { "selector": "containers[] .securityContext .runAsNonRoot == true",
    "reason": "Force the running image to run as a non-root user to ensure least privilege" },
  { "selector": "containers[] .securityContext .runAsUser -gt 10000",
    "reason": "Run as a high-UID user to avoid conflicts with the host's user table" },
  { "selector": "containers[] .securityContext .capabilities .drop", ... },
  { "selector": "containers[] .resources .requests .cpu", ... }
]}
```
- **Score:** positive points for good settings (readOnlyRootFilesystem, runAsNonRoot …), **negative / critical** for dangerous ones (`privileged`, `SYS_ADMIN`, `hostNetwork`) → `"valid": false`.
- Exam pattern: run kubesec, fix the YAML until the critical findings are gone / score improves.
- Without docker (containerd nodes): use the binary: `kubesec scan pod.yaml`.

#### Conftest — OPA
- OPA = Open Policy Agent.
- **Unit test framework for Kubernetes configurations** (and Dockerfiles, Terraform, any JSON/YAML).
- Uses **Rego** language. Default package: `main`, rules: `deny[msg]`, `warn[msg]`.
```rego
package main
deny[msg] {
  input.kind = "Deployment"
  not input.spec.template.spec.securityContext.runAsNonRoot = true
  msg = "Containers must not run as root"
}
```

#### Hands-on 2: OPA conftest K8s Deployment (his terminal)
Course files: `course-content/supply-chain-security/static-analysis/conftest/kubernetes/` (`deploy.yaml`, `policy/deployment.rego`, `run.sh`)
```rego
# policy/deployment.rego   (from https://www.conftest.dev)
package main

deny[msg] {
  input.kind = "Deployment"
  not input.spec.template.spec.securityContext.runAsNonRoot = true
  msg = "Containers must not run as root"
}

deny[msg] {
  input.kind = "Deployment"
  not input.spec.selector.matchLabels.app
  msg = "Containers must provide app label for pod selectors"
}
```
```bash
git clone https://github.com/killer-sh/cks-course-environment.git
cd cks-course-environment/course-content/supply-chain-security/static-analysis/conftest/kubernetes
cat run.sh
docker run --rm -v $(pwd):/project instrumenta/conftest test deploy.yaml
# FAIL - deploy.yaml - Containers must not run as root
# 2 tests, 1 passed, 0 warnings, 1 failure, 0 exceptions
```
Fix: add `spec.template.spec.securityContext.runAsNonRoot: true` → re-run → 2 passed.

#### Hands-on 3: OPA conftest Dockerfile (his terminal)
Course files: `.../conftest/docker/` (`Dockerfile`, `policy/base.rego`, `policy/commands.rego`, `run.sh`)
```bash
cd ../docker && ls policy/        # base.rego  commands.rego
./run.sh
# FAIL - Dockerfile - unallowed image found ["ubuntu"]
# FAIL - Dockerfile - unallowed commands found ["apt-get update && apt-get install -y golang-go"]
# 2 tests, 0 passed, 0 warnings, 2 failures, 0 exceptions
vim Dockerfile   && ./run.sh     # change base image → 1 passed, 1 failure
vim policy/commands.rego && ./run.sh     # adjust denylist → 2 tests, 2 passed ✅
```
Typical Dockerfile policies (`base.rego` / `commands.rego`):
```rego
package main
denylist = ["ubuntu"]
deny[msg] {
  input[i].Cmd == "from"
  val := input[i].Value
  contains(val[i], denylist[_])
  msg = sprintf("unallowed image found %s", [val])
}
```
> ⚠️ `instrumenta/conftest` is the old image name; current: `openpolicyagent/conftest`. Binary: `conftest test <file> -p policy/`.

#### Section summary
- Static analysis
- Manual approach
- Kubesec
- OPA Conftest

---

### 9.3 Scan Images for Known Vulnerabilities

#### Concept
- Introduction
- Vulnerabilities
- Tools to scan images (Clair, Trivy)

#### Image vulnerabilities
Webservers or other apps can contain vulnerabilities (buffer overflows).
| Targets | Results |
|---|---|
| Remotely accessible application in container | Privilege escalation |
| Local application inside container | Information leaks |
| | DDoS |

#### Containers — Layers — Dependencies
```
ubuntu=14  → Layer 1-3
nginx=x.x  → Layer 4-5
curl=x.x   → Layer 6
```
Every layer/package version can carry known CVEs → pin and update versions.

#### Known image vulnerabilities
**Databases:**
- https://cve.mitre.org
- https://nvd.nist.gov

Vulnerabilities can be discovered in our own image and dependencies:
- Check during build
- Check at runtime

#### Where to check
```
Code → Commit → GitHub → Build → Test → Deploy → K8s
  ▲                        ▲                 ▲       ▲
  └──── Scan for known vulnerabilities ──────┘       │
                          Enforce registries/images (PSP / OPA) ┘
```
**Known image vulnerabilities — admission controllers:**
`API HTTP handler → AuthN/AuthZ → Mutating admission (→ mutating webhooks) → Object schema validation → Validating admission (→ validating webhooks) → persisted to etcd`
→ Tools that scan for known vulnerabilities can hook in as **webhooks** (e.g. ImagePolicyWebhook).

#### Clair
- Open source project.
- Static analysis of vulnerabilities in application containers.
- Ingests vulnerability metadata from a configured set of sources.
- Provides API.
- Heavier to set up (needs DB + server) → Trivy is the exam tool.

#### Trivy
- Open source project (Aqua Security).
- "A Simple and Comprehensive Vulnerability Scanner for Containers and other Artifacts, Suitable for CI."
- Simple, easy and fast.

#### Hands-on: Use Trivy to check some public images and our kube-apiserver image (his terminal)
```bash
docker run ghcr.io/aquasecurity/trivy:latest image nginx
docker run ghcr.io/aquasecurity/trivy:latest image nginx | grep CRITICAL
# Total: 148 (UNKNOWN: 0, LOW: 111, MEDIUM: 11, HIGH: 25, CRITICAL: 1)
# | libbsd0 | CVE-2019-20367 | CRITICAL | 0.9.1-2 | ... nlist.c in libbsd before ...
docker run ghcr.io/aquasecurity/trivy:latest image nginx:1.16-alpine
```
Output table columns: `LIBRARY | VULNERABILITY ID | SEVERITY | INSTALLED VERSION | FIXED VERSION | TITLE`.

**Exam-style usage (trivy binary is usually installed):**
```bash
trivy image nginx:1.19                                       # full report
trivy image --severity HIGH,CRITICAL nginx:1.19              # filter
trivy image -s CRITICAL -q nginx:1.19 | grep -c CRITICAL     # count, quiet
trivy image --ignore-unfixed nginx:1.19                      # only fixable CVEs
# check the kube-apiserver image used in the cluster:
grep image: /etc/kubernetes/manifests/kube-apiserver.yaml
trivy image registry.k8s.io/kube-apiserver:v1.37.1
# typical task: "find all pods whose image has CVE-XXXX / CRITICAL vulns and delete/scale them down"
k get pod -A -o jsonpath='{range .items[*]}{.metadata.namespace}/{.metadata.name}{" "}{.spec.containers[*].image}{"\n"}{end}'
```

#### Section summary
- Vulnerabilities
- Dependencies
- Databases
- Clair
- Trivy

---

### 9.4 Secure Supply Chain

#### Concept
- Supply chain
- K8s and container registries
- Validate and whitelist images and registries

#### Software supply chain
```
screw + wood → factory → chair → shop → customer
Tools → Software Development → [ Container → CI/CD Registry → K8s Cloud ] → Browser
                                └──────── CKS focus ────────┘
```

#### K8s and container registries — private registry with Kubernetes
```bash
# create secret
kubectl create secret docker-registry my-private-registry \
  --docker-server=my-private-registry-server \
  --docker-username=username \
  --docker-password=password \
  --docker-email=email
# add imagePullSecret to the SA → all pods using this SA can pull
kubectl patch serviceaccount default -p '{"imagePullSecrets": [{"name": "my-private-registry"}]}'
```
Or per pod: `spec.imagePullSecrets: [{name: my-private-registry}]`.

#### Use image digest instead of tag (his terminal)
Tags are mutable (`v1.19.3` can be re-pushed); a **digest** (`@sha256:...`) is immutable.
```bash
k -n kube-system get pod kube-apiserver-cks-master -o yaml | grep imageID
# imageID: k8s.gcr.io/kube-apiserver@sha256:...
vim /etc/kubernetes/manifests/kube-apiserver.yaml
#   image: k8s.gcr.io/kube-apiserver:v1.19.3
# → image: k8s.gcr.io/kube-apiserver@sha256:<digest>
```
> Video uses `k8s.gcr.io` (frozen). Current registry: `registry.k8s.io`.

#### Hands-on 1: Use image digest (his terminal)
**Task:** list all image registries used in the whole cluster, then use the image digest for kube-apiserver.
```bash
k get pod -A -oyaml | grep "image:"                  # includes managedFields noise (f:image: {})
k get pod -A -oyaml | grep "image:" | grep -v "f:"   # clean list
# image: k8s.gcr.io/coredns:1.7.0, k8s.gcr.io/etcd:3.4.13-0, k8s.gcr.io/kube-apiserver:v1.19.3 ...
# image: docker.io/weaveworks/weave-kube:2.7.0, weaveworks/weave-npc:2.7.0
# → registries in use: k8s.gcr.io, docker.io
```
Cleaner alternative:
```bash
k get pod -A -o jsonpath='{..image}' | tr ' ' '\n' | sort -u
```
Get the digest from the running pod and pin it in the manifest:
```bash
k -n kube-system get pod kube-apiserver-cks-master -oyaml | grep imageID
# imageID: docker-pullable://k8s.gcr.io/kube-apiserver@sha256:6ea8c40355df6c6c47050448e1f88cb4a5d618e9e96717818d4e11fcfe156ee0
vim /etc/kubernetes/manifests/kube-apiserver.yaml
#   image: k8s.gcr.io/kube-apiserver:v1.19.3
# → image: k8s.gcr.io/kube-apiserver@sha256:6ea8c40355df6c6c47050448e1f88cb4a5d618e9e96717818d4e11fcfe156ee0
```
> On containerd the `imageID` has no `docker-pullable://` prefix. Copy only `registry/name@sha256:...`.

#### Hands-on 2: Whitelist registries with OPA
**Task:** only images from `docker.io` and `k8s.gcr.io` can be used.
Course files: `course-content/supply-chain-security/secure-the-supply-chain/whitelist-registries/opa/`
```yaml
apiVersion: templates.gatekeeper.sh/v1
kind: ConstraintTemplate
metadata:
  name: k8strustedimages
spec:
  crd:
    spec:
      names:
        kind: K8sTrustedImages
  targets:
  - target: admission.k8s.gatekeeper.sh
    rego: |
      package k8strustedimages
      violation[{"msg": msg}] {
        image := input.review.object.spec.containers[_].image
        not startswith(image, "docker.io/")
        not startswith(image, "k8s.gcr.io/")
        msg := "not trusted image!"
      }
---
apiVersion: constraints.gatekeeper.sh/v1beta1
kind: K8sTrustedImages
metadata:
  name: pod-trusted-images
spec:
  match:
    kinds:
    - apiGroups: [""]
      kinds: ["Pod"]
```
```bash
k run test --image=nginx                    # denied: "nginx" has no registry prefix
k run test --image=docker.io/nginx          # ✅
```
> Short names like `nginx` are expanded to `docker.io/library/nginx` by the runtime, but the **pod spec** still says `nginx`, and that's what OPA sees.

#### ImagePolicyWebhook
```
Api Request → ApiServer → Allow/Deny
                 ↕
        AdmissionControllers
                 ↕
        ImagePolicyWebhook ──"kind":"ImageReview"──► External Service
```
The apiserver sends an `ImageReview` to an external service, which answers allow or deny:
```json
{
  "apiVersion":"imagepolicy.k8s.io/v1alpha1",
  "kind":"ImageReview",
  "spec":{
    "containers":[
      {"image":"myrepo/myimage:v1"},
      {"image":"myrepo/myimage@sha256:beb6bd6a68f114c1dc2ea4b28db81bdf91de202a9014972bec5e4d9171d90ed"}
    ],
    "annotations":{"mycluster.image-policy.k8s.io/ticket-1234": "break-glass"},
    "namespace":"mynamespace"
  }
}
```

#### Hands-on 3: Investigate ImagePolicyWebhook (his terminal)
**Task:** investigate ImagePolicyWebhook and use it up to the point where it calls an external service.

**Step 1: enable the plugin only → apiserver won't start**
```bash
vim /etc/kubernetes/manifests/kube-apiserver.yaml
#   - --enable-admission-plugins=NodeRestriction,ImagePolicyWebhook
k get pod
# The connection to the server 10.154.0.2:6443 was refused
cd /var/log/pods/
tail -f kube-system_kube-apiserver-cks-master_<uid>/kube-apiserver/0.log
# Error: failed to initialize admission: couldn't init admission plugin "ImagePolicyWebhook": no config specified
```
✅ **Reproduced on my cluster (v1.37.1):**
```
E1124 12:03:06.158704  1 run.go:72] "command failed" err="failed to apply admission: couldn't init admission plugin \"ImagePolicyWebhook\": no config specified"
```
> `tail` on a log number that no longer exists (`4.log`) → `No such file or directory`. The number increases with every restart, so `ls` the dir first or use `tail -f */kube-apiserver/*.log`.

**Step 2: copy the course files**
```bash
git clone https://github.com/killer-sh/cks-course-environment.git
cp -r cks-course-environment/course-content/supply-chain-security/secure-the-supply-chain/whitelist-registries/ImagePolicyWebhook/ /etc/kubernetes/admission
cd /etc/kubernetes/admission && ll
# admission_config.yaml  apiserver-client-cert.pem  apiserver-client-key.pem
# external-cert.pem      external-key.pem           kubeconf
```

**Step 3: `admission_config.yaml`**
```yaml
apiVersion: apiserver.config.k8s.io/v1
kind: AdmissionConfiguration
plugins:
  - name: ImagePolicyWebhook
    configuration:
      imagePolicy:
        kubeConfigFile: /etc/kubernetes/admission/kubeconf
        allowTTL: 50           # seconds to cache an "allow" answer
        denyTTL: 50            # seconds to cache a "deny" answer
        retryBackoff: 500      # ms between retries
        defaultAllow: false    # ← if the external service is unreachable: DENY all pods (fail closed)
```

**Step 4: `kubeconf`** (a kubeconfig that points at the external service, not at a cluster)
```yaml
apiVersion: v1
kind: Config
# clusters refers to the remote service.
clusters:
- cluster:
    certificate-authority: /etc/kubernetes/admission/external-cert.pem  # CA for verifying the remote service.
    server: https://external-service:1234/check-image                   # URL of remote service to query. Must use 'https'.
  name: image-checker
contexts:
- context:
    cluster: image-checker
    user: api-server
  name: image-checker
current-context: image-checker
preferences: {}
# users refers to the API server's webhook configuration.
users:
- name: api-server
  user:
    client-certificate: /etc/kubernetes/admission/apiserver-client-cert.pem  # cert for the webhook admission controller to use
    client-key:  /etc/kubernetes/admission/apiserver-client-key.pem          # key matching the cert
```

**Step 5: apiserver manifest: flag + volumeMount + hostPath** (same rule as 6.5/8.x!)
```yaml
    - --admission-control-config-file=/etc/kubernetes/admission/admission_config.yaml
    - --enable-admission-plugins=NodeRestriction,ImagePolicyWebhook
    volumeMounts:
    - mountPath: /etc/kubernetes/admission
      name: k8s-admission
      readOnly: true
  volumes:
  - hostPath:
      path: /etc/kubernetes/admission
      type: DirectoryOrCreate
    name: k8s-admission
```

**Step 6: test**
```bash
k -n kube-system get pod                     # apiserver back, all Running
k run test --image=nginx
# Error from server (Forbidden): pods "test" is forbidden: Post "https://external-service:1234/check-image?timeout=30s":
#   dial tcp: lookup external-service on 169.254.169.254:53: no such host
```
✅ That's the goal: the apiserver **calls the external service**. It doesn't exist, and with `defaultAllow: false` → pod denied.
Set `defaultAllow: true` → pods are allowed while the service is unreachable (fail open). Note: static pods/control plane already running are not affected.
Example real backend: https://github.com/flavio/kube-image-bouncer

> **Exam pattern:** "ImagePolicyWebhook is configured but incomplete, fix it": check the flag in `--enable-admission-plugins`, `--admission-control-config-file`, the volume mount, the `kubeConfigFile` path, the `server:` URL, and `defaultAllow` (usually must be `false`).

#### Section summary
- How k8s works with registries
- Image tags and digests
- Whitelist registries with OPA
- ImagePolicyWebhook

## 10. Domain 6 — Monitoring, Logging & Runtime Security

### 10.1 Behavioral Analytics at Host and Container Level

#### Concept
- Syscalls and processes
- strace and /proc
- Tools and scenarios: **Falco**

#### Kernel vs user space (recap, see 4.2)
```
USER SPACE    Applications (Firefox, Curl)  →  Libraries (glibc, libxyz)
              ─── seccomp / AppArmor ───   ← filter BEFORE the syscall reaches the kernel
KERNEL SPACE  Syscall Interface (getpid(), reboot()) → Linux Kernel → Hardware
```
Containers (docker/containerd) are just processes in user space making syscalls to the **same** kernel.
Full syscall list: https://man7.org/linux/man-pages/man2/syscalls.2.html ("the fundamental interface between an application and the Linux kernel").

#### strace
- Intercepts and logs **system calls** made by a process.
- Logs and displays **signals** received by a process.
- Diagnostic, learning, debugging.
```
strace
  -o filename    write output to a file
  -v             verbose
  -f             follow forks (child processes/threads)
  -cw            count and summarise (wall time)
  -p pid         attach to a running process
  -P path        only syscalls touching this path
```

#### Hands-on 1: strace "ls" (his terminal)
**Task:** investigate what it does and find all syscalls.
```bash
strace ls /              # every syscall line by line, ends with "+++ exited with 0 +++"
strace -cw ls /          # summary table
```
```
% time     seconds  usecs/call     calls    errors syscall
 19.51    0.001188          40        30           mmap
 16.08    0.000979          41        24           openat
 14.78    0.000900          36        25           fstat
 13.71    0.000835          32        26           close
  8.33    0.000507          42        12           mprotect
  ...
  5.24    0.000319          40         8         8 access
  3.68    0.000224         224         1           execve
  1.63    0.000099          50         2           getdents
100.00    0.006090                   156        10 total
```
> A simple `ls` makes ~156 syscalls. `openat` + `getdents` = open the directory and read the entries, `write` = print.

#### /proc directory
- Information and connections to processes and kernel.
- Study it to learn how processes work.
- Configuration and administrative tasks.
- Contains files that don't exist, yet you can access these (virtual filesystem, generated by the kernel).
- `/proc/<PID>/`: `exe` (binary), `cmdline`, `environ`, `fd/` (open files), `root/` (process root fs), `status`, `ns/`, `cgroup`.

#### Hands-on 2: strace and /proc on etcd (his terminal)
**Task:** strace Kubernetes etcd: 1. list syscalls, 2. find open files, 3. read secret value 😈
```bash
ps aux | grep etcd                         # PID e.g. 3502
strace -p 3502 -f                          # live syscalls (futex, epoll_pwait, read, write …)
strace -p 3502 -f -cw                      # Ctrl+C after a few seconds → summary
#  … write 223, fdatasync 16, read 180 (86 errors), pwrite64, epoll_ctl, accept4, openat …
cd /proc/3502 && ls -lh exe                # exe -> /usr/local/bin/etcd
cd fd && ls -l                             # 0..87 → sockets, pipes, and the etcd db file
ls -l | grep db                            # e.g. 7 -> /var/lib/etcd/member/snap/db
tail -f 7                                  # binary bbolt db: you can see node names, leases, member URLs
```
Read a secret straight from etcd's open db file:
```bash
k create secret generic credit-card --from-literal cc=1111222233334444
cat 7 | grep 1111222233334444              # "Binary file (standard input) matches"
cat 7 | strings | grep 1111222233334444    # 1111222233334444  ← plain text!
```
> Without **encryption at rest** (8.1/8.x), anyone with root on the control plane reads secrets via `/proc/<etcd-pid>/fd/`. With aescbc you'd only see `k8s:enc:aescbc:v1:...`.

#### Hands-on 3: /proc access env variables (his terminal)
**Task:** create an Apache pod with a secret as environment variable and read that secret from the host filesystem.
```bash
k run apache --image=httpd --env SECRET=5555666677778888 -oyaml --dry-run=client > apache.yaml
k -f apache.yaml create
k get pod apache -o wide                     # → node cks-worker
# on the worker:
ps aux | grep httpd                          # several httpd processes
pstree -p                                    # find the parent httpd PID, e.g. 28696
cd /proc/28696
ls -lh exe                                   # exe -> /usr/local/apache2/bin/httpd
ls -lha fd                                   # 0 -> /dev/null, 1/2 -> pipes, 3 -> socket
cat environ
# ...HTTPD_VERSION=2.4.46HOSTNAME=apache...SECRET=5555666677778888HTTPD_PREFIX=/usr/local/apache2...
cat environ | tr '\0' '\n' | grep SECRET     # readable version (entries are NUL-separated)
```
> 🔴 **Secrets as environment variables can be read from anyone who can access /proc on the host.** Prefer secret volumes (and restrict node access).

#### Falco
- Cloud-native runtime security (**CNCF**). Originally created by **Sysdig**.
- **ACCESS:** deep kernel tracing built on the Linux kernel.
- **ASSERT:** describe security rules against a system (+ default ones), detect unwanted behaviour.
- **ACTION:** automated response to security violations.

Docs: https://falco.org/docs · fields: https://falco.org/docs/rules/supported-fields · k8s docs "Auditing with Falco" (old v1.16 page).

#### Hands-on 4: Install Falco on the worker node
Course `Resources.md` (old, pinned):
```bash
curl -s https://falco.org/repo/falcosecurity-packages.asc | apt-key add -
echo "deb https://download.falco.org/packages/deb stable main" | tee -a /etc/apt/sources.list.d/falcosecurity.list
apt-get update -y
apt-get install -y linux-headers-$(uname -r)
apt-get install -y falco=0.32.1
```
**What I used (2026):** `apt-key` is deprecated, and current Falco uses the **modern eBPF** driver (no kernel headers/module needed):
```bash
curl -fsSL https://falco.org/repo/falcosecurity-packages.asc | gpg --dearmor -o /usr/share/keyrings/falco-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/falco-archive-keyring.gpg] https://download.falco.org/packages/deb stable main" > /etc/apt/sources.list.d/falcosecurity.list
apt-get update && apt-get install -y falco           # choose "Modern eBPF" in the dialog
systemctl status falco-modern-bpf --no-pager
```

#### My install log explained (Falco 0.45.2, Nov 2026)
| Log line | Meaning |
|---|---|
| `falco-modern-bpf.service` / `Opening 'syscall' source with modern BPF probe` | Modern eBPF driver: no kernel module |
| `Loaded plugin 'container@0.7.6'` + many runtime sockets | Since 0.41 container/k8s metadata comes from a plugin. It tries every socket; only `/run/containerd/containerd.sock` exists, the rest is harmless |
| `falco_rules.yaml \| schema validation: ok` | Default rules. **Don't edit**: overwritten on updates |
| `falco_rules.local.yaml \| schema validation: none` | **My custom rules / overrides.** `none` = empty file |
| `falcoctl ... falco-rules:5, check every: 168h` | Weekly automatic default-rule updates |
| `health webserver ... listening on 0.0.0.0:8765` | Health endpoint on all interfaces |

#### Hands-on 5: Look at existing Falco rules (his terminal, Falco 0.2x)
```bash
cd /etc/falco && ls
# falco.yaml  falco_rules.local.yaml  falco_rules.yaml  k8s_audit_rules.yaml  rules.available  rules.d
tail -f /var/log/syslog | grep falco
# Notice Privileged container started (user=root ... container:e30ae569fb2b k8s_weave-npc_weave-net-... image=weaveworks/weave-npc:2.7.0)
# Error File below /etc opened for writing (user=root ... command=vim falco.yaml ... file=/etc/falco/.falco.yaml.swp program=vim ... container_id=host)
```
> Just editing `falco.yaml` with vim on the host triggers "File below /etc opened for writing". Falco watches the **host** too (`container_id=host`).
> Better than syslog: `journalctl -u falco-modern-bpf -f`

#### Hands-on 6: Use Falco to find malicious processes inside containers
```bash
k exec -it apache -- bash                    # → Notice/Warning: "A shell was spawned in a container with an attached terminal"
k exec apache -- cat /etc/shadow             # → Warning: Sensitive file opened for reading by non-trusted program
k exec apache -- sh -c 'echo x >> /etc/passwd'   # → Error: File below /etc opened for writing
```

#### Hands-on 7: Change a Falco rule's output (from my lab)
Override in `/etc/falco/falco_rules.local.yaml` (copy the whole rule, change only the output):
```yaml
- rule: Terminal shell in container
  desc: A shell was used as the entrypoint/exec point into a container with an attached terminal.
  condition: >
    spawned_process
    and container
    and shell_procs
    and proc.tty != 0
    and container_entrypoint
    and not user_expected_terminal_shell_in_container_conditions
  output: A shell was spawned in a container with an attached terminal | %user.name, %evt.time, %container.id, %container.name
  priority: WARNING
  tags: [maturity_stable, container, shell, mitre_execution, T1059]
```
Or only replace the output (Falco ≥ 0.36):
```yaml
- rule: Terminal shell in container
  output: A shell was spawned | %evt.time,%container.id,%container.name,%user.name
  override:
    output: replace
```
Run in the foreground to see output directly: `falco` (Ctrl+C prints a summary: events detected, rule counts by severity).

**My result:**
```
08:53:44.924191745: Warning A shell was spawned in a container with an attached terminal | root, 08:53:44.924191745, 188e0dd5f672, apache container_id=188e0dd5f672 container_name=apache container_image_repository=docker.io/library/httpd container_image_tag=latest k8s_pod_name=apache k8s_ns_name=default
Events detected: 1 · WARNING: 1 · Terminal shell in container: 1
```
**In the video (Falco 0.26):** task slide = Rule *"A shell was spawned in a container with an attached terminal"*, Output format `TIME,USER-NAME,CONTAINER-NAME,CONTAINER-ID`, Priority `WARNING`.
```bash
cd /etc/falco && ls -lh
grep -r "A shell was spawned in a container with an attached terminal" .   # → ./falco_rules.yaml   find the rule by its output text!
falco --list=syscall | less                     # all available fields (or: falco.org/docs/rules/supported-fields)
```
His run ended with: `Events detected: 3 · NOTICE: 1 · INFO: 2 · Launch Privileged Container: 2 · Terminal shell in container: 1`.
Section summary: syscalls · pods/containers/processes · strace /proc · Falco.

**Gotcha: extra fields appended after my output.** It isn't the old rule. It's `append_output.suggested_output: true` in `falco.yaml` (the container plugin appends `container_id=… k8s_ns_name=…` to every rule).
```bash
grep -n "suggested_output:" /etc/falco/falco.yaml                   # line ~799
sed -i 's/suggested_output: true/suggested_output: false/' /etc/falco/falco.yaml
systemctl restart falco-modern-bpf
```
> Exam: tasks say "change the output format to `time,container-id,container-name,user-name`". Appended fields don't hurt. Useful fields: `%evt.time %container.id %container.name %user.name %proc.name %proc.cmdline %k8s.pod.name %k8s.ns.name`.

---

### 10.2 Immutability of Containers at Runtime

#### Concept
- Immutability
- Container and pod level enforcement
- Scenarios to ensure the pod's containers are immutable

#### Immutability
**Container won't be modified during its lifetime.**
| Mutable | Immutable |
|---|---|
| 1. SSH on VM instance | 1. Create new VM image |
| 2. Stop application | 2. Delete VM instance |
| 3. Update application | 3. Create new VM instance |
| 4. Start application | |

Containers: never patch a running container. Build a new image and redeploy.

**Why immutable?** We always know the state → advanced deployment methods, easy rollback, more reliability, better security (on container level).

#### Enforce on container image level
- Remove bash/shell
- Make filesystem read-only
- Run as user and non-root
→ See 9.1 image hardening. *But what if we have no control of the container (or are too lazy)?* → enforce at pod level:

#### Make manual changes to container — 3 ideas from the slides
| Idea | How | Verdict |
|---|---|---|
| **Command** | `command: chmod a-w -R / && nginx` (lock FS, then start app) | Hacky, mixes logic into the start command |
| **StartupProbe** | startupProbe runs `chmod a-w -R /` (or `rm /bin/bash`) while app container runs | Hacky — misuse of probes, but works |
| **InitContainer** | init container has **Read-Write** to a shared VOLUME, app container only **Read** | Cleaner: prepare data in init, app mounts it read-only |

#### Hands-on 1: StartupProbe for immutability
**Task:** use StartupProbe to remove `touch` and `bash` from container.
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: immutable
spec:
  containers:
  - image: httpd
    name: immutable
    startupProbe:
      exec:
        command:
        - rm
        - /bin/touch
        - /bin/bash
      initialDelaySeconds: 1
      periodSeconds: 5
```
```bash
k exec immutable -- touch /tmp/x       # executable file not found
k exec -it immutable -- bash           # executable file not found
```
> Probe must succeed (`rm` exits 0) or the container never becomes Ready. If files are already gone on a retry, `rm` fails → use `rm -f`.

#### Enforce Read-Only Root Filesystem
Using **SecurityContexts** and PodSecurityPolicies (→ today: PSA `restricted`, or OPA/Kyverno to require `readOnlyRootFilesystem`).

#### Hands-on 2: Enforce RO-filesystem (his vim)
**Task:** create Pod SecurityContext to make filesystem Read-Only; ensure some directories are still writeable using emptyDir volume.
```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: immutable
  name: immutable
spec:
  containers:
  - image: httpd
    name: immutable
    securityContext:
      readOnlyRootFilesystem: true
    volumeMounts:
    - name: cache-vol
      mountPath: /usr/local/apache2/logs      # httpd needs to write its pid/logs here
  volumes:
  - name: cache-vol
    emptyDir: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
```
```bash
k logs immutable        # without emptyDir: "Read-only file system: AH00099: could not create /usr/local/apache2/logs/httpd.pid" → CrashLoop
k exec immutable -- touch /test                           # Read-only file system ✅
k exec immutable -- touch /usr/local/apache2/logs/test    # works (emptyDir)
```
Docker equivalent shown on slide: `docker run --read-only --tmpfs /run my-container`

#### RBAC
With RBAC it should be ensured that **only certain people can even edit pod specs** — otherwise anyone can just remove `readOnlyRootFilesystem`.

#### Section summary
- Immutability
- Container image level
- Pod level via Security Context
- RBAC

---

---

### 10.3 Auditing

#### Concept
- Introduction
- Setup and configure Audit Logs
- Use Audit Logs to investigate access history

#### Audit Logs — Introduction
Every API request (create pod, new image for deployment, read secret) → **Kubernetes API** → **Audit Logs**. Later you query the history:
- Did someone access an important secret while it was not protected?
- When was the last time that user X did access cluster Y?
- Does my CRD work properly?

#### API Request Stages
Each request can be recorded with an associated "stage":
| Stage | When |
|---|---|
| `RequestReceived` | As soon as the audit handler receives the request, before it is delegated down the handler chain |
| `ResponseStarted` | Response headers sent, body not yet. **Only for long-running requests** (e.g. watch) |
| `ResponseComplete` | Response body completed, no more bytes will be sent |
| `Panic` | Events generated when a panic occurred |

#### Audit Policy — What data to store? (Rule LEVELS)
Many API requests → much data to store → *What events should be recorded and what data should these contain?*
| Level | Logs |
|---|---|
| `None` | don't log events that match this rule |
| `Metadata` | request metadata (user, timestamp, resource, verb…) but **not** request/response body |
| `Request` | metadata + **request body**, not response body (not for non-resource requests) |
| `RequestResponse` | metadata + request **and** response bodies (not for non-resource requests) |

#### Example policy (from the slides)
```yaml
apiVersion: audit.k8s.io/v1
kind: Policy
omitStages:
  - "RequestReceived"
rules:
# log no "read" actions
- level: None
  verbs: ["get", "watch", "list"]
# log nothing regarding events
- level: None
  resources:
  - group: "" # core
    resources: ["events"]
# log nothing coming from some groups
- level: None
  userGroups: ["system:nodes"]
- level: RequestResponse
  resources:
  - group: ""
    resources: ["secrets"]
# for everything else log
- level: Metadata
```
> ⚠️ **Checked in order: the first matching rule sets the "audit level" of the event.** Put `None` rules first and the catch-all last.
> ⚠️ `RequestResponse` on secrets writes **secret values into the log file** — in real life use `Metadata` for secrets.

#### Audit Backends — Where to store all that data?
- **Log** (JSON files) → ship with ElasticSearch / FileBeat / Fluentd
- **Webhook** (external API)
- **Dynamic backend** (AuditSink API — removed, no longer exists)

#### Overview
Event content (Pods, Secrets, "get", "delete") × **Level** (None/Metadata/Request/RequestResponse) × **Stages** (RequestReceived/ResponseStarted/ResponseComplete/Panic) → Audit Logs.

#### Hands-on 1: Setup Audit Logs (JSON)
**Task:** configure apiserver to store Audit Logs in JSON format.
```bash
mkdir -p /etc/kubernetes/audit
vim /etc/kubernetes/audit/policy.yaml
```
```yaml
apiVersion: audit.k8s.io/v1
kind: Policy
rules:
- level: Metadata
```
Apiserver manifest (course file `course-content/runtime-security/auditing/kube-apiserver_enable_auditing.yaml`):
```yaml
    - --audit-policy-file=/etc/kubernetes/audit/policy.yaml       # add
    - --audit-log-path=/etc/kubernetes/audit/logs/audit.log       # add
    - --audit-log-maxsize=500                                     # add  (MB per file)
    - --audit-log-maxbackup=5                                     # add  (rotated files kept)
...
    volumeMounts:
    - mountPath: /etc/kubernetes/audit                            # add  (NOT readOnly — apiserver writes logs)
      name: audit                                                 # add
...
  volumes:
  - hostPath:                                                     # add
      path: /etc/kubernetes/audit                                 # add
      type: DirectoryOrCreate                                     # add
    name: audit                                                   # add
```
```bash
watch crictl ps
tail -f /etc/kubernetes/audit/logs/audit.log
```
> ⚠️ His course file still contains `--insecure-port=0` → **delete it** on 1.24+ (unknown flag).

#### Hands-on 2: Create a secret and investigate the JSON audit log
```bash
k create secret generic very-secure --from-literal user=admin
cat /etc/kubernetes/audit/logs/audit.log | grep very-secure | tail -1 | jq
```
```json
{
  "kind": "Event", "apiVersion": "audit.k8s.io/v1",
  "level": "Metadata",
  "stage": "ResponseComplete",
  "requestURI": "/api/v1/namespaces/default/secrets?fieldManager=kubectl-create",
  "verb": "create",
  "user": { "username": "kubernetes-admin", "groups": ["system:masters","system:authenticated"] },
  "sourceIPs": ["10.156.0.25"],
  "userAgent": "kubectl/v1.19.3 (linux/amd64) kubernetes/1e11e4a",
  "objectRef": { "resource": "secrets", "namespace": "default", "name": "very-secure", "apiVersion": "v1" },
  "responseStatus": { "code": 201 },
  "annotations": { "authorization.k8s.io/decision": "allow" }
}
```
Who (`user.username`), what (`verb`, `objectRef`), from where (`sourceIPs`, `userAgent`), result (`responseStatus.code`, decision).

#### Hands-on 3: Restrict amount of Audit Logs to collect
**Task:** restrict logged data with an Audit Policy:
- Nothing from stage RequestReceived
- Nothing from "get", "watch", "list"
- From Secrets only metadata level
- Everything else RequestResponse level
```yaml
apiVersion: audit.k8s.io/v1
kind: Policy
omitStages:
  - "RequestReceived"
rules:
- level: None
  verbs: ["get", "watch", "list"]
- level: Metadata
  resources:
  - group: ""
    resources: ["secrets"]
- level: RequestResponse
```
Procedure (from slide):
1. Change policy file
2. Disable audit logging in apiserver, wait till restart
3. Enable audit logging in apiserver, wait till restart
   a. If apiserver doesn't start, check `/var/log/pods/kube-system_kube-apiserver*`
4. Test your changes
```bash
k create secret generic s2 --from-literal a=b
grep s2 /etc/kubernetes/audit/logs/audit.log | tail -1 | jq .level        # "Metadata"
k run x --image=nginx && grep '"name":"x"' /etc/kubernetes/audit/logs/audit.log | tail -1 | jq .level   # "RequestResponse"
```
> Simpler restart than disable/enable: `mv /etc/kubernetes/manifests/kube-apiserver.yaml /tmp/ && sleep 15 && mv /tmp/kube-apiserver.yaml /etc/kubernetes/manifests/`

#### My lab issues (Nov 2026)
1. **Apiserver never restarted after adding audit flags** → kubelet log:
   `Could not process manifest file ... couldn't parse as pod(yaml: line 118: did not find expected '-' indicator)`
   Cause: new volume indented with 3 spaces (`   - hostPath:`) instead of 2. YAML parse errors never reach `crictl logs` — only `journalctl -u kubelet`. Check before saving:
   `python3 -c "import yaml;yaml.safe_load(open('/etc/kubernetes/manifests/kube-apiserver.yaml'))" && echo YAML-OK`
2. **Apiserver crashed on start:**
   `loading audit policy file: unknown group version field audit.k8s.io/__internal, Kind=Policy in policy`
   Cause: a typo/misplaced field in `policy.yaml` (strict parsing). Fix by comparing with the known-good policy above; check for tabs with `cat -A`.

#### Section summary
- What are Audit Logs
- How to enable them (apiserver)
- How to filter these (Audit Policy)
- How to investigate logs

## 11. Quick Reference

### Key paths
| What | Path |
|---|---|
| Static pod manifests | `/etc/kubernetes/manifests/` |
| Control-plane PKI | `/etc/kubernetes/pki/` |
| etcd PKI | `/etc/kubernetes/pki/etcd/` |
| Component kubeconfigs | `/etc/kubernetes/{admin,scheduler,controller-manager,kubelet}.conf` |
| Kubelet config | `/var/lib/kubelet/config.yaml` |
| Kubelet certs | `/var/lib/kubelet/pki/` |

### Handy one-liners
```bash
kubeadm certs check-expiration
openssl x509 -in <cert> -noout -text
kubectl describe netpol -A
kubectl get pod -A -o jsonpath='{range .items[?(@.spec.hostPID==true)]}{.metadata.namespace}/{.metadata.name}{"\n"}{end}'
kubectl create secret tls <name> --cert=cert.pem --key=key.pem
curl -kv https://<host>:<port>/path --resolve <host>:<port>:<ip>
kube-bench run --targets master --check <id>
sha512sum <binary>                       # compare against release checksum
find /proc/<PID>/root/ | grep <binary>   # reach into a distroless container
k auth can-i <verb> <resource> --as <user> -n <ns>          # RBAC test
k auth can-i --list --as system:serviceaccount:<ns>:<sa>    # all SA permissions
k certificate approve <csr>
k get csr <name> -o jsonpath='{.status.certificate}' | base64 -d > x.crt
k config set-credentials <u> --client-key=x.key --client-certificate=x.crt --embed-certs
k config view --raw
k create token <sa>
crictl ps -a | grep apiserver && crictl logs <id>           # broken apiserver
crictl inspect <id>                                         # env vars, mounts, pid of a container
cat /proc/<pid>/root/<path>                                 # read files inside a container from the node
ETCDCTL_API=3 etcdctl --cert ... --key ... --cacert ... get /registry/secrets/<ns>/<name>
k get secrets -A -o json | k replace -f -                   # re-encrypt/decrypt all secrets
kubeadm upgrade plan / kubeadm upgrade apply vX.Y.Z / kubeadm upgrade node
k exec <pod> -- cat /proc/1/status | grep -E "NoNewPrivs|CapEff|Seccomp"
k label ns <ns> pod-security.kubernetes.io/enforce=restricted
k get constrainttemplates; k get constraints
docker run -i kubesec/kubesec:512c5e0 scan /dev/stdin < pod.yaml
docker run --rm -v $(pwd):/project openpolicyagent/conftest test deploy.yaml
trivy image -s HIGH,CRITICAL <image>
k get pod -A -o jsonpath='{..image}' | tr ' ' '\n' | sort -u     # all images/registries
strace -cw <cmd> / strace -p <pid> -f -cw
ls -l /proc/<pid>/{exe,fd}; cat /proc/<pid>/environ | tr '\0' '\n'
journalctl -u falco-modern-bpf -f                                 # Falco alerts
k create secret docker-registry <name> --docker-server=... --docker-username=... --docker-password=...
k drain <node> --ignore-daemonsets && k uncordon <node>
```

### Exam-day reminders
- After editing a static pod manifest, wait for the apiserver to come back: `watch crictl ps`.
- Back up manifests before editing: `cp /etc/kubernetes/manifests/kube-apiserver.yaml ~/`.
- NetworkPolicy: test **both directions** and remember the **DNS egress** exception.

---

### 11.1 Shell & kubectl Tricks

#### Shell setup (first 30 seconds of the exam)
```bash
alias k=kubectl                                   # usually preset
source <(kubectl completion bash)                 # tab-complete resources & names
complete -o default -F __start_kubectl k          # completion for the alias too
export do="--dry-run=client -o yaml"              # k run x --image=nginx $do > pod.yaml
export now="--force --grace-period 0"             # k delete pod x $now
```
Vim (`~/.vimrc`) — so YAML indentation doesn't break:
```
set expandtab tabstop=2 shiftwidth=2 number
```

#### Line editing & history
| Keys | Effect |
|---|---|
| `Tab Tab` | List options for a command / file / resource |
| `Ctrl+R` | Search history (again = next match); `→` to edit, `Ctrl+G` cancel |
| `Ctrl+A` / `Ctrl+E` | Jump to start / end of line |
| `Alt+B` / `Alt+F` | Jump word back / forward |
| `Ctrl+W` / `Ctrl+U` / `Ctrl+K` | Delete word back / to start / to end of line |
| `Ctrl+Y` | Paste back what you just deleted |
| `Ctrl+D` / `Alt+D` | Delete char / word forward |
| `!!` / `sudo !!` | Repeat last command (with sudo) |
| `!$` or `Alt+.` | Last argument of previous command |
| `cd -` | Previous directory |
| `Ctrl+L` | Clear screen |
| `Ctrl+Z`, `fg` | Pause foreground command, resume |
| `hash -r` | Clear bash's cached command paths (after moving/removing a binary) |
Vim: `D` delete to end of line, `X` char before cursor, `:set backspace=indent,eol,start` if Backspace doesn't work.

#### Generating YAML fast
```bash
k run pod1 --image=nginx $do > pod.yaml
k create deploy d1 --image=nginx --replicas=3 $do > d.yaml
k create secret generic s1 --from-literal=k=v $do
k create role r1 --verb=get,list --resource=pods $do
k expose pod pod1 --port 80 $do
k set env deploy/x --from=secret/s1 --dry-run=client -o yaml
k explain pod.spec.securityContext --recursive | less
```

#### Finding things
```bash
k get pod -A -o wide
k get pod -A --show-labels
k get pod -A -o jsonpath='{..image}' | tr ' ' '\n' | sort -u
k get pod -A --sort-by=.metadata.creationTimestamp
k api-resources --namespaced=false
k auth can-i --list --as system:serviceaccount:ns:sa
k get events -A --sort-by=.lastTimestamp | tail
```

#### Editing safely
```bash
cp /etc/kubernetes/manifests/kube-apiserver.yaml ~/apiserver.bak
k get deploy x -o yaml > x.bak.yaml
k replace --force -f pod.yaml
python3 -c "import yaml;yaml.safe_load(open('f.yaml'))"
```

#### Debugging the control plane
```bash
watch crictl ps
crictl ps -a | grep apiserver && crictl logs <id>
ls /var/log/pods/kube-system_kube-apiserver-*/kube-apiserver/
journalctl -u kubelet -f | grep -i manifest      # YAML errors only show here
systemctl cat kubelet
```

#### Container / process forensics
```bash
crictl ps | grep <name> && crictl inspect <id> | grep -i pid
ps aux | grep <proc>;  pstree -p
ls -l /proc/<pid>/exe /proc/<pid>/fd/
cat /proc/<pid>/environ | tr '\0' '\n'
cat /proc/<pid>/status | grep -E "NoNewPrivs|CapEff|Seccomp"
ls /proc/<pid>/root/
strace -p <pid> -f -cw
strings <binary-file> | grep <text>
```

#### Text tools
```bash
grep -rn "pattern" /etc/kubernetes/
grep -v "f:"
sed -i 's/old/new/' file
sed -i '/--flag/d' file
awk '{print $1}'   cut -d' ' -f1   sort -u   uniq -c   wc -l
tail -f file | grep --line-buffered x
base64 -d <<< "c2VjcmV0"
openssl x509 -in cert.crt -noout -text | grep -A1 "Subject Alternative"
sha512sum file
jq . file.json        # pretty-print audit log lines
```

#### tmux
`Ctrl+B %` vertical split · `Ctrl+B "` horizontal · `Ctrl+B` + arrow switch pane.

## 12. Troubleshooting Index
Quick lookup: **error message → section**. Every entry comes from my own lab.

| Error / symptom | Cause | Section |
|---|---|---|
| `syntax error near unexpected token 'newline'` / `<!DOCTYPE html>` | `github.com/.../blob/` URL instead of `raw.githubusercontent.com` | 2.2 |
| Dashboard/NodePort unreachable | Private IP, security group, or HTTP vs HTTPS | 5.2 |
| `curl: (6) Could not resolve host` with `--resolve` | Host/port typo, or HTTP NodePort used for HTTPS | 5.3 |
| `CN=Kubernetes Ingress Controller Fake Certificate` | TLS secret name/ns wrong, or host mismatch | 5.3 |
| `could not stat client-certificate file jane-crt` | Filename typo (`jane.crt`) | 6.2 |
| `Error: unknown flag: --insecure-port` | Flag removed in 1.24 | 6.4 |
| apiserver `open /etc/kubernetes/enc/enc.yaml: no such file or directory` | Missing volumeMount/hostPath | 6.5, 8.x |
| apiserver `0/1`, restart count climbing; controller-manager CrashLoopBackOff; pods `Unknown` | `--anonymous-auth=false` breaks the probes | 8.z |
| `corrupt object ... no matching prefix found` | Encryption config can't decrypt stored data | 8.x |
| `failed to sync configmap cache: timed out waiting for the condition` | Apiserver can't list configmaps (encryption) / broken kubelet watch | 8.x |
| `sysctl: error setting key ... Read-only file system` as root | Container not privileged (root ≠ privileged) | 8.3 |
| `container has runAsNonRoot and image will run as root` | `runAsNonRoot: true` without `runAsUser` | 8.3 |
| `violates PodSecurity "baseline:latest"` | PSA namespace label blocks the pod | 8.3 |
| `admission webhook "validation.gatekeeper.sh" denied the request` | Gatekeeper constraint violation | 8.5 |
| `couldn't init admission plugin "ImagePolicyWebhook": no config specified` | Plugin enabled without `--admission-control-config-file` | 9.4 |
| `pods "x" is forbidden: Post "https://external-service:1234/check-image..."` | ImagePolicyWebhook backend unreachable + `defaultAllow: false` | 9.4 |
| `tail: cannot open '.../kube-apiserver/4.log'` | Log number changed after restart; `ls` first | 9.4 |
| `Could not process manifest file ... did not find expected '-' indicator` | YAML indentation in static pod manifest (only in kubelet log) | 10.3 |
| `loading audit policy file: unknown group version field ... Kind=Policy` | Typo/misplaced field in audit policy | 10.3 |
| `AH00099: could not create .../httpd.pid` Read-only file system | readOnlyRootFilesystem without emptyDir for writable path | 10.2 |
| `aa-`: command not found | Prefix only; install `apparmor-utils`, use Tab Tab | 7.1 |
| `aa-genprof`: `Operation {'runbindable'} cannot have a source` / `Include file ... not found` | Shipped profiles unparseable by utils; move them aside | 7.1 |
| Falco output has extra `container_id=… k8s_ns_name=…` fields | `append_output.suggested_output: true` in falco.yaml | 10.1 |
| `cat <file> \| grep x` → `Binary file matches` | Use `strings` first | 10.1 |
| `"sh": executable file not found` on `docker exec` | Shell removed in hardened image (intended) | 9.1 |
| conftest `FAIL - ... unallowed image found` | Rego denylist matched | 9.2 |
| `apt-get ... Connection failed` during `docker build` | Network drop; retry / regional mirror / golang image | 9.1 |
| `no runtime for "runsc" is configured` | Missing containerd runtime entry | 8.y |
| `cgroups V2 is not yet supported` | Old gVisor binary | 8.y |
| `-bash: /usr/local/bin/runsc: No such file or directory` | bash path cache → `hash -r` | 8.y |

**General debug order for a broken control plane:**
`crictl ps -a` → `crictl logs <id>` → `k -n kube-system logs <pod>` → `k describe pod` events → `journalctl -u kubelet` / `-u containerd` → `systemctl restart kubelet` on all nodes → force-delete stuck `Unknown` pods.