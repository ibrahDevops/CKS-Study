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
| 14 | Cluster Hardening – RBAC | — | Cluster Hardening | ⏳ |

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
kubectl create secret tls <name> --cert=cert.pem --key=key.pem
curl -kv https://<host>:<port>/path --resolve <host>:<port>:<ip>
kube-bench run --targets master --check <id>
sha512sum <binary>                       # compare against release checksum
find /proc/<PID>/root/ | grep <binary>   # reach into a distroless container
```

### Exam-day reminders
- After editing a static pod manifest, wait for the apiserver to come back: `watch crictl ps`.
- Back up manifests before editing: `cp /etc/kubernetes/manifests/kube-apiserver.yaml ~/`.
- NetworkPolicy: test **both directions** and remember the **DNS egress** exception.