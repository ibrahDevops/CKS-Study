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
| 8 | Cluster Setup – GUI Elements | 1:46:54 | Cluster Setup | ⏳ |
| 9 | K8s Docs Version | 2:02:06 | — | ⏳ |
| 10 | Cluster Setup – Secure Ingress | 2:02:48 | Cluster Setup | ⏳ |
| 11 | Cluster Setup – Node Metadata Protection | 2:23:54 | Cluster Setup | ⏳ |
| 12 | Cluster Setup – CIS Benchmarks | 2:34:13 | Cluster Setup | ⏳ |
| 13 | Cluster Setup – Verify Platform Binaries | — | Cluster Setup | ⏳ |

---

## Table of Contents

0. [Exam Overview](#0-exam-overview)
1. [Security Best Practices (Foundations)](#1-security-best-practices-foundations)
2. [Lab Setup](#2-lab-setup)
3. [Kubernetes Secure Architecture & PKI](#3-kubernetes-secure-architecture--pki)
4. [Containers Under the Hood](#4-containers-under-the-hood)
5. [Domain 1 — Cluster Setup](#5-domain-1--cluster-setup)
   - [5.1 Network Policies](#51-network-policies)
   - 5.2 GUI Elements (Dashboard) — *TODO*
   - 5.3 Secure Ingress — *TODO*
   - 5.4 Node Metadata Protection — *TODO*
   - 5.5 CIS Benchmarks (kube-bench) — *TODO*
   - 5.6 Verify Platform Binaries — *TODO*
6. [Domain 2 — Cluster Hardening](#6-domain-2--cluster-hardening) — *TODO*
7. [Domain 3 — System Hardening](#7-domain-3--system-hardening) — *TODO*
8. [Domain 4 — Minimize Microservice Vulnerabilities](#8-domain-4--minimize-microservice-vulnerabilities) — *TODO*
9. [Domain 5 — Supply Chain Security](#9-domain-5--supply-chain-security) — *TODO*
10. [Domain 6 — Monitoring, Logging & Runtime Security](#10-domain-6--monitoring-logging--runtime-security) — *TODO*
11. [Quick Reference](#11-quick-reference)

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
*TODO — next screenshots*

### 5.3 Secure Ingress
*TODO*

### 5.4 Node Metadata Protection
*TODO*

### 5.5 CIS Benchmarks (kube-bench)
*TODO*

### 5.6 Verify Platform Binaries
*TODO*

---

## 6. Domain 2 — Cluster Hardening
*TODO — RBAC, ServiceAccounts, restrict API access, upgrades*

## 7. Domain 3 — System Hardening
*TODO — AppArmor, seccomp, reduce the host OS footprint, IAM*

## 8. Domain 4 — Minimize Microservice Vulnerabilities
*TODO — Pod Security Admission, securityContext, Secrets/etcd encryption, RuntimeClass (gVisor), mTLS/Cilium*

## 9. Domain 5 — Supply Chain Security
*TODO — Trivy, kubesec, ImagePolicyWebhook, SBOM, static analysis*

## 10. Domain 6 — Monitoring, Logging & Runtime Security
*TODO — Falco, audit logs, immutable containers*

---

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
```

### Exam-day reminders
- After editing a static pod manifest, wait for the apiserver to come back: `watch crictl ps`.
- Back up manifests before editing: `cp /etc/kubernetes/manifests/kube-apiserver.yaml ~/`.
- NetworkPolicy: test **both directions** and remember the **DNS egress** exception.