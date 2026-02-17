# ☸️ Kubernetes Mastery Guide
## *The Complete Encyclopedia - From Zero to Production-Ready*

<div align="center">
  
  <img src="https://raw.githubusercontent.com/kubernetes/kubernetes/master/logo/logo.png" width="150">
  
  ### 📊 **Complete Kubernetes Ecosystem Coverage**
  
  <table>
    <tr>
      <td align="center"><b>🏗️ Control Plane</b><br>5 Components</td>
      <td align="center"><b>🖥️ Worker Nodes</b><br>3 Components</td>
      <td align="center"><b>📡 Addons</b><br>4+ Components</td>
      <td align="center"><b>📦 Workloads</b><br>8 Resources</td>
    </tr>
    <tr>
      <td align="center"><b>🌐 Networking</b><br>7+ Resources</td>
      <td align="center"><b>💾 Storage</b><br>9 Resources</td>
      <td align="center"><b>🔐 Security</b><br>10 Resources</td>
      <td align="center"><b>🎯 Scheduling</b><br>13 Resources</td>
    </tr>
    <tr>
      <td align="center"><b>⚡ Autoscaling</b><br>5 Resources</td>
      <td align="center"><b>🩺 Health</b><br>7 Probes</td>
      <td align="center"><b>📈 Signals</b><br>74+ Signals</td>
      <td align="center"><b>📏 Policy</b><br>6 Resources</td>
    </tr>
    <tr>
      <td align="center"><b>🔧 Extensions</b><br>5 Resources</td>
      <td align="center"><b>🚨 Failure</b><br>17+ Signals</td>
      <td align="center"><b>📝 Config</b><br>3 Resources</td>
      <td align="center"><b>🏷️ Metadata</b><br>6 Resources</td>
    </tr>
  </table>
  
  <br>
  
  <img src="https://img.shields.io/badge/Total_Components-180+-326CE5?logo=kubernetes&logoColor=white&style=for-the-badge">
  <img src="https://img.shields.io/badge/CKA_CKAD_CKS_Ready-326CE5?style=for-the-badge&logo=cncf">
  
</div>

---

## 📑 **Complete Table of Contents**

| Section | Components | Icon |
|---------|------------|:----:|
| [Cluster Architecture](#-cluster-architecture) | Control Plane, Worker Nodes, Addons | 🏗️ |
| [Workload Resources](#-workload-resources) | Pods, Deployments, StatefulSets, DaemonSets, Jobs | 📦 |
| [Networking Deep Dive](#-networking-deep-dive) | Services, Ingress, Network Policies, CNI | 🌐 |
| [Storage & Persistence](#-storage--persistence) | Volumes, PV/PVC, StorageClass, CSI | 💾 |
| [Security & RBAC](#-security--rbac) | Authentication, Authorization, ServiceAccounts, Secrets | 🔐 |
| [Observability & Health](#-observability--health) | Probes, Metrics, Logging, Events | 📊 |
| [Autoscaling](#-autoscaling) | HPA, VPA, Cluster Autoscaler | ⚡ |
| [Advanced Scheduling](#-advanced-scheduling) | Node/Pod Affinity, Taints/Tolerations, PriorityClass | 🎯 |
| [Configuration & Secrets](#-configuration--secrets) | ConfigMap, Secrets, ServiceAccount | 📝 |
| [Policy & Governance](#-policy--governance) | ResourceQuota, LimitRange, NetworkPolicy, PDB | 📏 |
| [Pod-Level Signals](#-pod-level-signals-comprehensive) | 74+ Health, Lifecycle, Resource Signals | 📈 |
| [Production Failure Signals](#-production-failure-signals) | 17+ Common Failure Scenarios | 🚨 |
| [Extending Kubernetes](#-extending-kubernetes) | CRD, Operators, Admission Webhooks | 🔧 |
| [Complete API Reference](#-complete-kubernetes-api-reference) | All Resources at a Glance | 📋 |
| [Certification Path](#-certification-path) | CKA, CKAD, CKS Guide | 🎓 |
| [Essential Commands](#-essential-kubectl-commands) | 10+ Must-Known Commands | 🛠️ |

---

## 🏗️ **Cluster Architecture**

### 🎯 **Control Plane Components**

| # | Component | Description | Real-Time Use Case | Production Consideration |
|---|-----------|-------------|---------------------|---------------------------|
| 1 | **kube-apiserver** | Front-end to the control plane; validates and configures data | All `kubectl` commands hit this; authentication/authorization gateway | Run 3+ instances behind load balancer; supports 10k+ pods |
| 2 | **etcd** | Distributed key-value store; cluster brain | Stores all cluster state; network policies, config maps, secrets | Take etcd snapshots every 30 mins; enable TLS encryption |
| 3 | **kube-scheduler** | Assigns pods to nodes based on constraints | Scheduling ML workloads on GPU nodes; bin packing for cost saving | Run multiple schedulers for different workload types |
| 4 | **kube-controller-manager** | Runs controller processes | Node controller marking nodes unhealthy; replica controller maintaining pod count | Only one active controller manager at a time via leader election |
| 5 | **cloud-controller-manager** | Interfaces with cloud provider APIs | Provisioning LoadBalancers on AWS/Azure/GCP; managing node lifecycle | Separate controllers for each cloud provider |

### 🖥️ **Worker Node (Data Plane) Components**

| # | Component | Description | Real-Time Use Case | Implementation Details |
|---|-----------|-------------|---------------------|------------------------|
| 1 | **kubelet** | Primary node agent; registers node with cluster | Reports node status; ensures containers running; executes liveness probes | Runs as systemd service; communicates with API server on port 10250 |
| 2 | **kube-proxy** | Network proxy; maintains network rules | Implements Service abstraction via iptables/IPVS; handles nodePort connections | Runs as DaemonSet; supports iptables (default), IPVS (better performance) |
| 3 | **Container Runtime** | Runs containers (containerd, CRI-O) | Pulls images; starts/stops containers; manages container lifecycle | containerd is industry standard; CRI-O is lightweight & Kubernetes-native |

### 📡 **Addon Components**

| # | Addon | Purpose | Real-World Implementation |
|---|-------|---------|---------------------------|
| 1 | **CoreDNS** | Service discovery within cluster | Pods discover services via DNS names; internal load balancing |
| 2 | **Metrics Server** | Resource usage metrics | HPA relies on this for CPU/memory metrics; powers `kubectl top` commands |
| 3 | **Ingress Controller** | L7 load balancing | NGINX, HAProxy, Traefik routing external HTTP traffic to services |
| 4 | **Dashboard** | Web UI for cluster management | View workloads, scale deployments, view logs through browser |
| 5 | **CNI Plugin** | Container network interface | Calico, Cilium, Flannel for pod networking |
| 6 | **CSI Driver** | Container storage interface | AWS EBS, Azure Disk, Ceph for persistent storage |

---

## 📦 **Workload Resources**

| # | Resource | API Version | Namespaced | Description | Real-World Use Case |
|---|----------|-------------|:----------:|-------------|---------------------|
| 1 | **Pod** | v1 | ✅ | Smallest deployable unit | Web server + sidecar logging agent + metrics exporter |
| 2 | **ReplicaSet** | apps/v1 | ✅ | Maintains stable set of pods | Never directly used; Deployment manages it |
| 3 | **ReplicationController** | v1 | ✅ | Legacy replica management | Legacy systems, not recommended for new deployments |
| 4 | **Deployment** | apps/v1 | ✅ | Declarative pod updates | E-commerce site updating with zero downtime during Black Friday |
| 5 | **StatefulSet** | apps/v1 | ✅ | Stateful applications | Kafka brokers, MySQL clusters, Cassandra nodes |
| 6 | **DaemonSet** | apps/v1 | ✅ | Run pod on every node | Fluentd log collection, Prometheus Node Exporter |
| 7 | **Job** | batch/v1 | ✅ | Run to completion | Data processing, image resizing batch, database migration |
| 8 | **CronJob** | batch/v1 | ✅ | Scheduled jobs | Nightly backups, hourly report generation, weekly cleanup |

### 🐳 **Pod Deep Dive**

| Aspect | Description | Real-World Use Case |
|--------|-------------|---------------------|
| **Definition** | Smallest deployable unit; 1+ containers sharing network/storage | Web server + sidecar logging agent + metrics exporter |
| **Lifecycle** | Pending → Running → Succeeded/Failed → CrashLoopBackOff | Database pod with persistent volume surviving node failure |
| **Multi-Container** | Co-located helper containers with shared volumes | Main app + git-sync sidecar for content updates |
| **Init Containers** | Run to completion before main containers start | Database migration; permission setup; asset pre-processing |
| **Ephemeral Containers** | Temporary for debugging | Troubleshooting running pods without restarting them |

### 🚀 **Deployment Strategies**

| Scenario | Deployment Strategy | Business Impact | Real Example |
|----------|--------------------|-----------------|--------------|
| Critical production | RollingUpdate with zero downtime | No user impact | E-commerce site during Black Friday |
| Development | Recreate (simple, fast) | Acceptable downtime | Dev environment testing |
| A/B Testing | Canary (10% traffic) + monitoring | Risk mitigation | New feature rollout to 10% users |
| Database migration | Blue/Green with switch at LB | Instant rollback | MySQL version upgrade |

---

## 🌐 **Networking Deep Dive**

### 🎯 **Service Types & Use Cases**

| # | Service Type | Description | Real-World Implementation | When to Use |
|---|--------------|-------------|--------------------------|-------------|
| 1 | **ClusterIP** | Internal cluster IP only | Backend API consumed by frontend within cluster | Microservices communication |
| 2 | **NodePort** | Expose on each node's IP:port | Development testing; on-premise with fixed ports | Quick external access without LB |
| 3 | **LoadBalancer** | Cloud provider provisions LB | E-commerce site exposed to internet | Production web services |
| 4 | **ExternalName** | CNAME to external service | Point to legacy system outside cluster | Migration strategy |
| 5 | **Headless Service** | No cluster IP; direct pod DNS | StatefulSet discovery (Kafka, Cassandra) | When clients need direct pod access |

### 🌐 **Networking Resources**

| # | Resource | API Version | Namespaced | Purpose | Real Example |
|---|----------|-------------|:----------:|---------|--------------|
| 1 | **Service** | v1 | ✅ | Pod network abstraction | Load balance across 3 API pods |
| 2 | **Ingress** | networking.k8s.io/v1 | ✅ | HTTP/S routing | `api.example.com` → API service, `app.example.com` → Web service |
| 3 | **IngressClass** | networking.k8s.io/v1 | ❌ | Ingress controller class | Define NGINX as default ingress controller |
| 4 | **NetworkPolicy** | networking.k8s.io/v1 | ✅ | Pod firewall rules | Allow only API pods to access database |
| 5 | **Endpoint** | v1 | ✅ | Service endpoint list (legacy) | Track pod IPs for a service |
| 6 | **EndpointSlice** | discovery.k8s.io/v1 | ✅ | Scalable endpoint tracking | Better performance for large services |
| 7 | **CNI** | - | ❌ | Container Network Interface | Calico, Cilium, Flannel plugins |

### 🔒 **Network Policies - Zero-Trust Security Model**

| Layer | Allowed Traffic | Denied Traffic | Real Implementation |
|-------|-----------------|----------------|---------------------|
| **Internet → Ingress** | 80,443 | Everything else | Public access only to ingress |
| **Ingress → Web Pods** | 8080 | Everything else | Web pods receive traffic only from ingress |
| **Web Pods → API Pods** | 8080 | Everything else | API pods receive traffic only from web |
| **API Pods → DB Pods** | 5432 | Everything else | Database accessible only by API |

### 🌐 **CNI Plugin Comparison**

| CNI Plugin | Features | Best For | Real Implementation |
|------------|----------|----------|---------------------|
| **Calico** | NetworkPolicy, BGP, eBPF | Production, security-focused | Enterprise with strict security requirements |
| **Cilium** | eBPF, Hubble observability, service mesh | Advanced networking, observability | Microservices with service mesh |
| **Flannel** | Simple overlay network | Quick setup, basic needs | Development, small clusters |
| **Weave** | Simple, automatic mesh | Small clusters | Quick POC deployments |
| **Antrea** | Open vSwitch, NetworkPolicy | VMware environments | Existing VMware infrastructure |

---

## 💾 **Storage & Persistence**

### 📀 **Volume Types & Use Cases**

| # | Volume Type | Persistence | Use Case | Real Example |
|---|-------------|-------------|----------|--------------|
| 1 | **emptyDir** | Ephemeral (pod lifetime) | Cache, scratch space | Redis cache; build temporary files |
| 2 | **hostPath** | Node-bound | Node-level logs, Docker socket | Log collection; accessing host Docker |
| 3 | **configMap** | Ephemeral | Configuration injection | Nginx config; app properties |
| 4 | **secret** | Ephemeral | Sensitive data | Database passwords; API keys |
| 5 | **downwardAPI** | Ephemeral | Pod metadata | Expose pod IP, labels to container |
| 6 | **projected** | Ephemeral | Combine multiple sources | ServiceAccount token + configMap |
| 7 | **PersistentVolumeClaim** | Persistent | Production data | MySQL data; user uploads |

### 🗄️ **Storage Resources**

| # | Resource | API Version | Namespaced | Purpose | Real Example |
|---|----------|-------------|:----------:|---------|--------------|
| 1 | **Volume** | v1 | ✅ | Pod storage | Mount configmap as volume |
| 2 | **PersistentVolume (PV)** | v1 | ❌ | Cluster storage resource | 100Gi SSD provisioned by admin |
| 3 | **PersistentVolumeClaim (PVC)** | v1 | ✅ | Storage request | "I need 50Gi fast storage" |
| 4 | **StorageClass** | storage.k8s.io/v1 | ❌ | Storage type definition | SSD, HDD, encrypted, reclaim policy |
| 5 | **VolumeSnapshot** | snapshot.storage.k8s.io/v1 | ✅ | Volume snapshot | Point-in-time backup of database |
| 6 | **VolumeSnapshotClass** | snapshot.storage.k8s.io/v1 | ❌ | Snapshot class | Define snapshot retention policy |
| 7 | **CSI (Container Storage Interface)** | - | ❌ | Storage plugin interface | Standard for storage plugins |
| 8 | **CSIDriver** | storage.k8s.io/v1 | ❌ | CSI driver registration | Register EBS CSI driver |
| 9 | **CSINode** | storage.k8s.io/v1 | ❌ | CSI node info | Node-specific CSI information |

### 📊 **Access Modes**

| Access Mode | Abbreviation | Description | Example |
|-------------|--------------|-------------|---------|
| **ReadWriteOnce** | RWO | Single node read-write | MySQL database |
| **ReadOnlyMany** | ROX | Multiple nodes read-only | Static content |
| **ReadWriteMany** | RWX | Multiple nodes read-write | Shared filesystem |
| **ReadWriteOncePod** | RWOP | Single pod read-write | CSI only, strict isolation |

### 🔌 **CSI Driver Ecosystem**

| Storage Type | CSI Driver | Use Case | Real Example |
|--------------|------------|----------|--------------|
| **Cloud Block** | AWS EBS, GCE PD, Azure Disk | Database storage | MySQL, PostgreSQL, Cassandra |
| **Cloud File** | AWS EFS, Azure File, GCP Filestore | Shared storage | WordPress uploads, shared configs |
| **On-Premise** | Ceph RBD, Portworx, Longhorn | Private cloud | Enterprise on-premise storage |
| **Object Storage** | S3-compatible (MinIO) | Backups, logs | Backup storage, artifact repository |

---

## 🔐 **Security & RBAC**

### 👤 **Authentication & Authorization Flow**

| Step | Component | Description | Methods |
|------|-----------|-------------|---------|
| 1 | **User/ServiceAccount** | Identity requesting access | Human users, pods, CI/CD pipelines |
| 2 | **Authentication** | Verify identity | X509 certs, bearer tokens, OIDC, webhook |
| 3 | **Authorization** | Check permissions | RBAC, ABAC, Node, Webhook |
| 4 | **Admission Control** | Mutate/validate requests | Mutating/Validating webhooks |
| 5 | **Resource** | API server processes request | Create, get, update, delete |

### 🔐 **Security Resources**

| # | Resource | API Version | Namespaced | Purpose | Real Example |
|---|----------|-------------|:----------:|---------|--------------|
| 1 | **ConfigMap** | v1 | ✅ | Non-sensitive configuration | App properties, feature flags |
| 2 | **Secret** | v1 | ✅ | Sensitive data | Database passwords, API keys |
| 3 | **ServiceAccount** | v1 | ✅ | Pod identity | Give pod specific permissions |
| 4 | **Role** | rbac.authorization.k8s.io/v1 | ✅ | Namespace permissions | Developer can create pods in dev namespace |
| 5 | **ClusterRole** | rbac.authorization.k8s.io/v1 | ❌ | Cluster permissions | Admin can view all nodes |
| 6 | **RoleBinding** | rbac.authorization.k8s.io/v1 | ✅ | Bind role to subjects | Assign developer role to John |
| 7 | **ClusterRoleBinding** | rbac.authorization.k8s.io/v1 | ❌ | Bind cluster role | Give cluster-admin to team lead |
| 8 | **PodSecurity** (PSA) | - | ✅ | Pod security standards | Enforce restricted mode |
| 9 | **SecurityContext** | v1 | ✅ | Container-level security | Run as non-root user |
| 10 | **PodSecurityPolicy** (deprecated) | policy/v1beta1 | ❌ | Legacy pod security | Being replaced by PSA |

### 🎭 **RBAC Roles in Production**

| Role Type | Scope | Permissions | Real Implementation |
|-----------|-------|-------------|---------------------|
| **Viewer** | Namespace/Cluster | List/get pods, services | Auditors, read-only monitoring |
| **Developer** | Namespace | Create/update deployments | Dev team deploying to dev namespace |
| **CI/CD** | Namespace | Deploy from pipeline | Jenkins/GitLab automation |
| **SRE** | Cluster | Cluster-wide view, debug | Operations team troubleshooting |
| **Admin** | Cluster | Full access | Platform team managing cluster |

### 🔑 **ServiceAccounts - Pod Identity**

| Scenario | Implementation | Real Example |
|----------|----------------|--------------|
| **Pod needs API access** | Mounted token allows Kubernetes API calls | App that scales itself via API |
| **Cloud IAM integration** | AWS EKS Pod Identity, GCP Workload Identity | Pod accessing S3 buckets |
| **Pull private images** | ServiceAccount linked to image pull secrets | Pull from private registry |
| **Fine-grained permissions** | Different SAs for different microservices | Payment service has different permissions than frontend |

### 🔐 **Secret Types**

| Secret Type | Purpose | Example |
|-------------|---------|---------|
| **Opaque** | Arbitrary key-value | API keys, passwords |
| **kubernetes.io/tls** | TLS certificates | SSL certs for ingress |
| **kubernetes.io/dockerconfigjson** | Registry credentials | Pull from private registry |
| **kubernetes.io/basic-auth** | Basic auth credentials | HTTP basic auth |
| **kubernetes.io/ssh-auth** | SSH credentials | Git SSH keys |
| **kubernetes.io/service-account-token** | SA tokens | Legacy, now auto-generated |

### 🛡️ **Pod Security Standards**

| Level | Description | When to Use | Examples |
|-------|-------------|-------------|----------|
| **Privileged** | Unrestricted | System pods only | kube-proxy, CNI plugins |
| **Baseline** | Minimally restrictive | Most applications | Web servers, APIs |
| **Restricted** | Heavily restricted | PCI-DSS compliant workloads | Payment processing, healthcare data |

---

## 📊 **Observability & Health**

### 🩺 **Probes - Application Health**

| # | Probe Type | Purpose | Failure Consequence | Real Example |
|---|------------|---------|---------------------|--------------|
| 1 | **LivenessProbe** | Is app alive? | Restart container | Web server responding to /healthz |
| 2 | **ReadinessProbe** | Is app ready for traffic? | Remove from service | App loading cache; warming up |
| 3 | **StartupProbe** | Has app started? | Kill if too slow | Java app with 2-min startup |
| 4 | **ReadinessGates** | Extra readiness conditions | Pod not ready | External dependency check |
| 5 | **PodConditions** | Pod status conditions | Varies | Initialized, Ready, ContainersReady |
| 6 | **ContainersReady** | All containers ready | Pod not ready | All containers passed readiness |
| 7 | **PodScheduled** | Pod assigned to node | Pending state | Scheduler found a node |

### 📈 **Metrics Pipeline**

| Component | Purpose | Popular Tools | Real Implementation |
|-----------|---------|---------------|---------------------|
| **Node Metrics** | CPU, memory, disk per node | Node Exporter | Prometheus Node Exporter on each node |
| **Container Metrics** | Per-container resource usage | cAdvisor | Built into kubelet |
| **Kubernetes Objects** | Object counts, status | kube-state-metrics | Track deployment replicas, pod status |
| **Collection** | Scrape and store metrics | Prometheus | Scrape metrics every 30s |
| **Visualization** | Dashboards | Grafana | Create CPU/memory dashboards |
| **Alerting** | Notify on conditions | AlertManager | PagerDuty alert on high CPU |

### 📝 **Logging Architecture**

| Layer | Component | Purpose | Real Example |
|-------|-----------|---------|--------------|
| **Source** | Pod stdout/stderr | Application logs | `console.log` in Node.js app |
| **Collection** | Fluentd, Logstash | Gather logs from nodes | Fluentd DaemonSet on each node |
| **Aggregation** | Elasticsearch, Loki | Store and index logs | Elasticsearch cluster |
| **Visualization** | Kibana, Grafana | Search and explore logs | Kibana dashboards for debugging |

### 🔍 **Events - Cluster Activity**

| Event Type | What It Indicates | Troubleshooting Value |
|------------|-------------------|----------------------|
| **Scheduled** | Pod assigned to node | Check if scheduling worked |
| **Pulled** | Image pulled successfully | Image exists and accessible |
| **Created** | Container created | Container runtime working |
| **Started** | Container started | App starting successfully |
| **Killing** | Container being stopped | Scale down, eviction, OOM |
| **Unhealthy** | Probe failure | App not responding |

---

## ⚡ **Autoscaling**

### 📏 **Scaling Resources**

| # | Resource | API Version | Namespaced | Purpose | Real Example |
|---|----------|-------------|:----------:|---------|--------------|
| 1 | **HorizontalPodAutoscaler (HPA)** | autoscaling/v2 | ✅ | Scale pods by metrics | Scale from 3 to 20 pods at 70% CPU |
| 2 | **VerticalPodAutoscaler (VPA)** | autoscaling.k8s.io/v1 | ✅ | Scale resources per pod | Adjust CPU from 250m to 500m based on usage |
| 3 | **ClusterAutoscaler** | Addon | ❌ | Scale cluster nodes | Add nodes when pods pending |
| 4 | **ResourceQuota** | v1 | ✅ | Namespace resource limits | Team can use max 20 cores |
| 5 | **LimitRange** | v1 | ✅ | Per-container/pod limits | Each container max 4 cores |

### 📊 **HPA Metric Types**

| Metric Type | Description | Real-World Target |
|-------------|-------------|-------------------|
| **CPU Utilization** | Average CPU across pods | Target 70% utilization |
| **Memory Utilization** | Average memory across pods | Target 80% utilization |
| **Custom Metrics** | Requests per second, queue length | Scale based on business metrics |
| **External Metrics** | Cloud service metrics | Scale based on SQS queue depth |

**HPA Behavior:**
| Direction | Speed | Strategy | Real Example |
|-----------|-------|----------|--------------|
| **Scale Up** | Fast (double every 15s) | Handle traffic spikes quickly | Black Friday traffic surge |
| **Scale Down** | Slow (5-10 min window) | Avoid thrashing | After traffic returns to normal |

### 📊 **VPA Modes**

| Mode | Description | Use Case | Real Example |
|------|-------------|----------|--------------|
| **Off** | Recommendations only | Right-sizing analysis | Analyze 7-day usage pattern |
| **Initial** | Apply at creation only | New workloads | Set initial resources for new service |
| **Recreate** | Update by recreating pods | Stateful workloads | Update database pod resources |
| **Auto** | Automatically update | Optimize resource usage | Production workloads with varying load |

### 🏢 **Cluster Autoscaler Triggers**

| Trigger | Action | Cloud Implementation | Real Example |
|---------|--------|---------------------|--------------|
| **Pending pods** | Add nodes | AWS: ASG increase; GCP: MIG resize | 10 pending pods trigger 2 new nodes |
| **Underutilized nodes** | Remove nodes | Drain pods, terminate instances | Node <50% usage for 1 hour |
| **Spot instances** | Handle interruptions | Replace with on-demand if needed | AWS spot instance reclaim |

---

## 🎯 **Advanced Scheduling**

### 🎯 **Scheduling Resources**

| # | Resource/Concept | Type | Purpose | Real Example |
|---|-----------------|------|---------|--------------|
| 1 | **Node** | Resource | Worker machine in cluster | `node-1`, `node-2` in us-east-1 |
| 2 | **Namespace** | Resource | Resource isolation | `prod`, `staging`, `dev` namespaces |
| 3 | **Label** | Concept | Key/value for organization | `app=frontend`, `environment=prod` |
| 4 | **Annotation** | Concept | Non-identifying metadata | `build-version=1.2.3` |
| 5 | **NodeSelector** | Scheduling | Simple node selection | `disktype: ssd` |
| 6 | **NodeAffinity** | Scheduling | Advanced node placement | Prefer GPU nodes for ML workloads |
| 7 | **PodAffinity** | Scheduling | Co-locate pods | Cache pods on same node for low latency |
| 8 | **PodAntiAffinity** | Scheduling | Separate pods | Spread replicas across nodes for HA |
| 9 | **Taints** | Scheduling | Node repel pods | `gpu=true:NoSchedule` |
| 10 | **Tolerations** | Scheduling | Pod tolerate taints | Tolerate GPU taint for ML pods |
| 11 | **PriorityClass** | Resource | Pod priority | Critical pods get priority |
| 12 | **TopologySpreadConstraints** | Scheduling | Even distribution | Spread across zones |
| 13 | **Preemption** | Concept | Higher priority pods evict lower | Critical pod preempts batch job |

### 📍 **Node Affinity Types**

| Type | Description | Real-World Use |
|------|-------------|----------------|
| **requiredDuringScheduling** | Must match to schedule | GPU workloads must go to GPU nodes |
| **preferredDuringScheduling** | Try to match, but not required | Prefer SSD storage when available |

### 🚫 **Taint Effects**

| Taint Effect | Description | Real Implementation |
|--------------|-------------|---------------------|
| **NoSchedule** | Don't schedule new pods without toleration | Dedicated nodes for specific workloads |
| **PreferNoSchedule** | Try to avoid scheduling | Soft isolation |
| **NoExecute** | Evict existing pods without toleration | Spot instance handling |

### 🏢 **Multi-Tenant Cluster Design**

| Node Type | Taint | Workloads Allowed | Real Example |
|-----------|-------|-------------------|--------------|
| **Control Plane** | `node-role.kubernetes.io/master:NoSchedule` | System pods only | API server, scheduler, etcd |
| **GPU Nodes** | `gpu=true:NoSchedule` | ML workloads with toleration | TensorFlow training jobs |
| **Spot Nodes** | `spot=true:NoExecute` | Fault-tolerant batch jobs | CI/CD build agents |
| **Regular Nodes** | No taint | General workloads | Web servers, APIs |

### 🔄 **Pod Affinity/Anti-Affinity**

| Type | Description | Use Case | Real Example |
|------|-------------|----------|--------------|
| **Pod Affinity** | Co-locate pods | Low latency | Cache pods on same node as app |
| **Pod Anti-Affinity** | Separate pods | High availability | Spread 3 replicas across nodes |

### 📊 **PriorityClass Values**

| Priority Level | Value | Use Case | Real Example |
|----------------|-------|----------|--------------|
| **system-cluster-critical** | 2000000000 | Critical system pods | CoreDNS, metrics-server |
| **system-node-critical** | 2000001000 | Node-level critical pods | kube-proxy, CNI |
| **high-priority** | 1000000 | Production workloads | User-facing APIs |
| **medium-priority** | 100000 | Normal workloads | Batch processors |
| **low-priority** | 100 | Test workloads | CI/CD test jobs |

---

## 📏 **Policy & Governance**

### 📊 **Policy Resources**

| # | Resource | API Version | Namespaced | Purpose | Real Example |
|---|----------|-------------|:----------:|---------|--------------|
| 1 | **ResourceQuota** | v1 | ✅ | Namespace resource limits | Team can use max 20 cores, 40Gi memory |
| 2 | **LimitRange** | v1 | ✅ | Per-container/pod limits | Each container min 100m CPU, max 2 CPU |
| 3 | **NetworkPolicy** | networking.k8s.io/v1 | ✅ | Pod firewall rules | DB only accessible by API pods |
| 4 | **PodDisruptionBudget** | policy/v1 | ✅ | Pod availability guarantee | Always keep 2 database pods running |
| 5 | **PriorityClass** | scheduling.k8s.io/v1 | ❌ | Pod priority | Critical pods get priority |
| 6 | **RuntimeClass** | node.k8s.io/v1 | ❌ | Container runtime | Use gVisor for untrusted workloads |

### 📊 **ResourceQuota - Real-World Implementation**

| Environment | CPU Request | Memory Request | Pods | PVCs | Business Reason |
|-------------|-------------|----------------|------|------|-----------------|
| **Production** | 20 cores | 80 Gi | 50 | 10 | Mission-critical workloads |
| **Staging** | 10 cores | 40 Gi | 25 | 5 | Pre-production testing |
| **Development** | 5 cores | 20 Gi | 15 | 2 | Developer experimentation |
| **CI/CD** | 15 cores | 60 Gi | 30 | 0 | Build pipelines |

### 📏 **LimitRange - Environment Strategies**

| Environment | Min CPU | Max CPU | Default CPU | Ratio | Purpose |
|-------------|---------|---------|-------------|-------|---------|
| **Production** | 250m | 4 | 500m | 2:1 | Ensure performance |
| **Staging** | 100m | 2 | 250m | 4:1 | Balance cost vs testing |
| **Development** | 50m | 1 | 100m | 10:1 | Maximize resource sharing |

### 🛡️ **PodDisruptionBudget Strategies**

| Strategy | Setting | Use Case | Real Example |
|----------|---------|----------|--------------|
| **minAvailable** | Always keep 2 pods running | Critical stateful workloads | Database cluster |
| **maxUnavailable** | Allow only 1 pod to be down | During voluntary disruptions | Node drain operations |

---

## 📈 **Pod-Level Signals (Comprehensive)**

### 🩺 **Health Signals (9)**

| # | Signal | Description | Real-World Significance |
|---|--------|-------------|------------------------|
| 1 | **LivenessProbe** | Container alive check | Restart if app deadlocked |
| 2 | **ReadinessProbe** | Container ready for traffic | Remove from service if not ready |
| 3 | **StartupProbe** | Container started successfully | Give slow apps time to start |
| 4 | **ReadinessGates** | Extra readiness conditions | Wait for external dependency |
| 5 | **PodConditions** | Overall pod conditions | Track pod lifecycle stages |
| 6 | **ContainersReady** | All containers ready status | All containers passed readiness |
| 7 | **Ready** | Pod ready status | Pod can receive traffic |
| 8 | **Initialized** | Init containers completed | Setup completed successfully |
| 9 | **PodScheduled** | Pod assigned to node | Scheduling successful |

### 🔄 **Lifecycle Signals (13)**

| # | Signal | Description | When to Check |
|---|--------|-------------|---------------|
| 10 | **PodPhase** | Current pod phase | Pending, Running, Succeeded, Failed |
| 11 | **ContainerStateWaiting** | Container waiting reason | CrashLoopBackOff, ImagePullBackOff |
| 12 | **ContainerStateRunning** | Container running | Started successfully |
| 13 | **ContainerStateTerminated** | Container terminated | Completed or failed |
| 14 | **ContainerLastState** | Previous container state | Debug previous crash |
| 15 | **RestartCount** | Number of restarts | Detect crash loops |
| 16 | **ExitCode** | Container exit code | 0=success, non-zero=failure |
| 17 | **TerminationMessage** | Why container terminated | OOMKilled, Error message |
| 18 | **TerminationGracePeriodSeconds** | Grace period for shutdown | 30s default |
| 19 | **DeletionTimestamp** | When pod marked for deletion | Pod is terminating |
| 20 | **Finalizers** | Pre-deletion cleanup | Prevent immediate deletion |
| 21 | **PreStopHook** | Pre-termination hook | Graceful shutdown |
| 22 | **PostStartHook** | Post-startup hook | Post-deployment tasks |

### 📊 **Resource Signals (13)**

| # | Signal | Description | Action When High |
|---|--------|-------------|------------------|
| 23 | **CPUUsage** | Current CPU usage | Scale up or increase limit |
| 24 | **MemoryUsage** | Current memory usage | Check for leaks, increase limit |
| 25 | **EphemeralStorageUsage** | Temporary storage usage | Clean up logs, temp files |
| 26 | **ResourceRequests** | Requested resources | Minimum needed |
| 27 | **ResourceLimits** | Maximum resources | Hard limit |
| 28 | **QoSClass** | Quality of Service class | Guaranteed, Burstable, BestEffort |
| 29 | **OOMKilled** | Killed due to memory | Increase memory limit |
| 30 | **CPUThrottling** | CPU throttled | Increase CPU limit |
| 31 | **Evicted** | Pod evicted | Node pressure |
| 32 | **NodePressureEviction** | Evicted due to node pressure | Node memory/disk pressure |
| 33 | **MemoryPressure** | Node memory pressure | Node running out of memory |
| 34 | **DiskPressure** | Node disk pressure | Node running out of disk |
| 35 | **PIDPressure** | Node PID pressure | Too many processes |

### 🌐 **Networking Signals (6)**

| # | Signal | Description | Troubleshooting |
|---|--------|-------------|-----------------|
| 36 | **PodIP** | Pod IP address | Check network connectivity |
| 37 | **HostIP** | Node IP address | Which node is pod running on |
| 38 | **CNIAllocation** | CNI IP allocation status | Network plugin issues |
| 39 | **NetworkPolicyStatus** | Network policy applied | Policy enforcement |
| 40 | **ServiceEndpointRegistration** | Registered in service | Service discovery working |
| 41 | **EndpointSliceUpdate** | EndpointSlice updated | Service endpoints updated |

### 🧭 **Scheduling Signals (11)**

| # | Signal | Description | Impact |
|---|--------|-------------|--------|
| 42 | **NodeSelector** | Selected node | Node must match labels |
| 43 | **NodeAffinity** | Node affinity rules | Preferred/required node placement |
| 44 | **PodAffinity** | Pod affinity rules | Co-locate with other pods |
| 45 | **PodAntiAffinity** | Pod anti-affinity rules | Avoid co-location |
| 46 | **Taints** | Node taints | Nodes repel pods |
| 47 | **Tolerations** | Pod tolerations | Pods tolerate taints |
| 48 | **TopologySpreadConstraints** | Spread constraints | Even distribution |
| 49 | **PriorityClass** | Pod priority | Scheduling precedence |
| 50 | **Preemption** | Preemption occurred | Higher priority pod evicted lower |
| 51 | **Unschedulable** | Cannot schedule | No suitable node |
| 52 | **FailedScheduling** | Scheduling failed | Check resource constraints |

### 🔐 **Security Signals (9)**

| # | Signal | Description | Security Implication |
|---|--------|-------------|---------------------|
| 53 | **ServiceAccount** | Associated service account | Identity for pod |
| 54 | **SecurityContext** | Security settings | RunAsUser, capabilities |
| 55 | **PrivilegedMode** | Running privileged | Security risk |
| 56 | **SeccompProfile** | Seccomp profile applied | System call filtering |
| 57 | **AppArmorProfile** | AppArmor profile applied | Mandatory access control |
| 58 | **PodSecurityAdmission** | PSA admission result | Policy enforcement |
| 59 | **ImagePullSecret** | Registry credentials used | Private registry access |
| 60 | **ImagePullBackOff** | Failed to pull image | Registry/auth issues |
| 61 | **ErrImagePull** | Error pulling image | Network, image missing |

### 📦 **Storage Signals (8)**

| # | Signal | Description | Storage Issue |
|---|--------|-------------|---------------|
| 62 | **VolumeMountStatus** | Volume mounted | Storage ready |
| 63 | **PVCBound** | PVC bound to PV | Storage allocated |
| 64 | **PVCPending** | PVC pending | Storage not available |
| 65 | **VolumeAttachStatus** | Volume attached | Storage attached to node |
| 66 | **VolumeDetachStatus** | Volume detached | Storage detached |
| 67 | **FailedMount** | Mount failed | Filesystem issues |
| 68 | **CSIDriverError** | CSI driver error | Storage plugin problem |
| 69 | **ReadOnlyFilesystem** | Filesystem read-only | Permissions issue |

### 📈 **Scaling Signals (5)**

| # | Signal | Description | Autoscaling Insight |
|---|--------|-------------|---------------------|
| 70 | **HPAStatus** | HPA status | Current/desired replicas |
| 71 | **TargetCPUUtilization** | CPU target | Scaling threshold |
| 72 | **CustomMetricsStatus** | Custom metrics | Custom metric values |
| 73 | **ReplicaSetScalingEvent** | RS scaled | Replica count changed |
| 74 | **DeploymentScalingEvent** | Deployment scaled | Deployment updated |

---

## 🚨 **Production Failure Signals**

| # | Signal | Description | Common Cause | Resolution |
|---|--------|-------------|--------------|------------|
| 1 | **CrashLoopBackOff** | Container crashes repeatedly | App error, bad config | Check logs, fix app |
| 2 | **ImagePullBackOff** | Cannot pull image | Wrong image, registry issues | Verify image name/tag |
| 3 | **ErrImagePull** | Error pulling image | Network, auth, missing image | Check registry credentials |
| 4 | **CreateContainerConfigError** | Config error | Missing ConfigMap/Secret | Create missing resources |
| 5 | **CreateContainerError** | Cannot create container | Runtime issues | Check container runtime |
| 6 | **ContainerCannotRun** | Container cannot start | Permission, binary missing | Check SecurityContext |
| 7 | **BackOffRestartingContainer** | Backoff after crash | App repeatedly crashing | Debug application |
| 8 | **ContextDeadlineExceeded** | Operation timeout | Network, slow operations | Increase timeout |
| 9 | **NodeNotReady** | Node not ready | Kubelet down, network | Check node, restart kubelet |
| 10 | **PodNotReady** | Pod not ready | Readiness probe failing | Check app health |
| 11 | **ContainerStatusUnknown** | Unknown state | Node problem | Check node connectivity |
| 12 | **OOMKilled** | Out of memory killed | Memory limit too low | Increase memory limit |
| 13 | **Evicted** | Pod evicted | Resource pressure | Add resources, reduce load |
| 14 | **FailedScheduling** | Cannot schedule | Insufficient resources | Add nodes, reduce requests |
| 15 | **FailedMount** | Volume mount failed | Storage issues | Check PV/PVC, storage class |
| 16 | **InvalidImageName** | Invalid image name | Typo in image | Fix image name |
| 17 | **NetworkPluginNotReady** | CNI not ready | Network plugin issues | Restart CNI daemonset |

---

## 🔧 **Extending Kubernetes**

### 📦 **Custom Resources & Extensions**

| # | Resource | API Version | Namespaced | Purpose | Real Example |
|---|----------|-------------|:----------:|---------|--------------|
| 1 | **CustomResourceDefinition (CRD)** | apiextensions.k8s.io/v1 | ❌ | Define custom resources | Define `PostgreSQL` custom resource |
| 2 | **Operator** | Pattern | - | Application lifecycle automation | Prometheus Operator manages Prometheus |
| 3 | **MutatingAdmissionWebhook** | admissionregistration.k8s.io/v1 | ❌ | Mutate requests | Inject sidecar containers |
| 4 | **ValidatingAdmissionWebhook** | admissionregistration.k8s.io/v1 | ❌ | Validate requests | Enforce naming conventions |
| 5 | **RuntimeClass** | node.k8s.io/v1 | ❌ | Container runtime configuration | Use gVisor for untrusted workloads |

### 🤖 **Operator Maturity Levels**

| Level | Capability | Example | Real Implementation |
|-------|------------|---------|---------------------|
| **Level 1** | Basic install | Deploy application | Deploy Prometheus with defaults |
| **Level 2** | Upgrades | Handle version updates | Upgrade PostgreSQL version |
| **Level 3** | Full lifecycle | Backup, restore, failover | Automated database backup |
| **Level 4** | Deep insights | Metrics, alerts | Prometheus metrics export |
| **Level 5** | Auto-pilot | Auto-scaling, auto-healing | Automatic failover |

### 🪝 **Admission Webhook Examples**

| Webhook Type | Purpose | Real Example |
|--------------|---------|--------------|
| **Mutating** | Modify resources at creation | Inject Istio sidecar, add resource defaults |
| **Validating** | Validate before persistence | Prevent running privileged containers |

---

## 📋 **Complete Kubernetes API Reference**

### **All Resources with Namespace Status**

| Category | Resource | API Version | Namespaced |
|----------|----------|-------------|:----------:|
| **📦 Workloads** | Pod | v1 | ✅ |
| | ReplicaSet | apps/v1 | ✅ |
| | ReplicationController | v1 | ✅ |
| | Deployment | apps/v1 | ✅ |
| | StatefulSet | apps/v1 | ✅ |
| | DaemonSet | apps/v1 | ✅ |
| | Job | batch/v1 | ✅ |
| | CronJob | batch/v1 | ✅ |
| | | | |
| **🌐 Networking** | Service | v1 | ✅ |
| | Ingress | networking.k8s.io/v1 | ✅ |
| | IngressClass | networking.k8s.io/v1 | ❌ |
| | NetworkPolicy | networking.k8s.io/v1 | ✅ |
| | Endpoints | v1 | ✅ |
| | EndpointSlice | discovery.k8s.io/v1 | ✅ |
| | | | |
| **💾 Storage** | PersistentVolume | v1 | ❌ |
| | PersistentVolumeClaim | v1 | ✅ |
| | StorageClass | storage.k8s.io/v1 | ❌ |
| | VolumeAttachment | storage.k8s.io/v1 | ❌ |
| | VolumeSnapshot | snapshot.storage.k8s.io/v1 | ✅ |
| | VolumeSnapshotContent | snapshot.storage.k8s.io/v1 | ❌ |
| | VolumeSnapshotClass | snapshot.storage.k8s.io/v1 | ❌ |
| | CSIStorageCapacity | storage.k8s.io/v1 | ✅ |
| | CSIDriver | storage.k8s.io/v1 | ❌ |
| | CSINode | storage.k8s.io/v1 | ❌ |
| | | | |
| **📝 Config** | ConfigMap | v1 | ✅ |
| | Secret | v1 | ✅ |
| | ServiceAccount | v1 | ✅ |
| | | | |
| **🏷️ Metadata** | Namespace | v1 | ❌ |
| | Node | v1 | ❌ |
| | Event | v1 | ✅ |
| | LimitRange | v1 | ✅ |
| | ResourceQuota | v1 | ✅ |
| | Lease | coordination.k8s.io/v1 | ✅ |
| | ComponentStatus | v1 | ❌ |
| | Binding | v1 | ✅ |
| | | | |
| **🔐 Security** | Role | rbac.authorization.k8s.io/v1 | ✅ |
| | ClusterRole | rbac.authorization.k8s.io/v1 | ❌ |
| | RoleBinding | rbac.authorization.k8s.io/v1 | ✅ |
| | ClusterRoleBinding | rbac.authorization.k8s.io/v1 | ❌ |
| | PodSecurityPolicy (deprecated) | policy/v1beta1 | ❌ |
| | CertificateSigningRequest | certificates.k8s.io/v1 | ❌ |
| | TokenReview | authentication.k8s.io/v1 | ❌ |
| | SubjectAccessReview | authorization.k8s.io/v1 | ❌ |
| | | | |
| **📊 Autoscaling** | HorizontalPodAutoscaler | autoscaling/v2 | ✅ |
| | VerticalPodAutoscaler (CRD) | autoscaling.k8s.io/v1 | ✅ |
| | | | |
| **🎯 Scheduling** | PriorityClass | scheduling.k8s.io/v1 | ❌ |
| | RuntimeClass | node.k8s.io/v1 | ❌ |
| | | | |
| **🔧 Extensions** | CustomResourceDefinition | apiextensions.k8s.io/v1 | ❌ |
| | MutatingWebhookConfiguration | admissionregistration.k8s.io/v1 | ❌ |
| | ValidatingWebhookConfiguration | admissionregistration.k8s.io/v1 | ❌ |
| | APIService | apiregistration.k8s.io/v1 | ❌ |
| | FlowSchema | flowcontrol.apiserver.k8s.io/v1beta3 | ❌ |
| | PriorityLevelConfiguration | flowcontrol.apiserver.k8s.io/v1beta3 | ❌ |

### **What the Symbols Mean**

| Symbol | Meaning |
|:------:|---------|
| ✅ | **Namespaced** - Resource exists within a namespace (e.g., `default`, `kube-system`) |
| ❌ | **Cluster-wide** - Resource exists at cluster level, not in a namespace |

### **Namespaced vs Cluster-wide Explained**

| Type | Scope | Examples | Use Case |
|------|-------|----------|----------|
| **Namespaced (✅)** | Limited to a namespace | Pods, Services, Deployments | Isolate different teams/environments |
| **Cluster-wide (❌)** | Entire cluster | Nodes, PersistentVolumes, ClusterRoles | Global cluster configuration |

---

## 🎓 **Certification Path**

| Certification | Focus | Experience | Exam Format | Key Topics |
|--------------|-------|------------|-------------|------------|
| **CKA** (Certified Kubernetes Administrator) | Cluster administration, networking, troubleshooting | 6-12 months | Performance-based, 2 hrs | Control plane, etcd backup, networking, storage |
| **CKAD** (Certified Kubernetes Application Developer) | Application design, configuration, multi-container pods | 3-6 months | Performance-based, 2 hrs | Pod design, configmaps, secrets, probes |
| **CKS** (Certified Kubernetes Security Specialist) | Security, RBAC, policy enforcement | 1-2 years (CKA required) | Performance-based, 2 hrs | RBAC, network policies, runtime security |

---

## 🛠️ **Essential kubectl Commands**

| Command | Purpose | Example |
|---------|---------|---------|
| `kubectl get all -A` | List all resources in all namespaces | `kubectl get all -A \| grep crash` |
| `kubectl describe resource/name` | Detailed info about a resource | `kubectl describe pod web-1` |
| `kubectl logs pod-name -c container-name` | View container logs | `kubectl logs web-1 -c nginx --tail=50` |
| `kubectl exec -it pod-name -- /bin/sh` | Shell into a container | `kubectl exec -it web-1 -- sh` |
| `kubectl port-forward pod-name 8080:80` | Forward local port to pod | `kubectl port-forward web-1 8080:80` |
| `kubectl top pod` | Show pod resource usage | `kubectl top pod -n prod` |
| `kubectl get events --sort-by='.lastTimestamp'` | View recent events | `kubectl get events -n prod --watch` |
| `kubectl api-resources` | List all available resources | `kubectl api-resources \| grep storage` |
| `kubectl explain pod` | Documentation for resource | `kubectl explain pod.spec.containers` |
| `kubectl get quota -A` | View resource quotas | `kubectl get quota -n prod` |
| `kubectl get limitrange` | View limit ranges | `kubectl describe limitrange -n prod` |
| `kubectl auth can-i` | Check permissions | `kubectl auth can-i create pods` |

---

## 📚 **Learning Path**

```
Beginner ─────────────────────────────────────────────────────────────────► Expert
    │                                                                          │
    ├─ Week 1-2: Pods, Deployments, Services                                   ├─ Month 9-12: Custom Controllers
    ├─ Week 3-4: ConfigMaps, Secrets, Storage                                  ├─ Month 9-12: Operator Development
    ├─ Week 5-6: Ingress, Network Policies                                     ├─ Month 12+: Service Mesh (Istio)
    ├─ Week 7-8: ResourceQuota, LimitRange                                     ├─ Month 12+: Policy as Code (OPA)
    ├─ Week 9-10: Security, RBAC                                               ├─ Month 12+: Multi-Cluster Management
    └─ Week 11-12: Autoscaling, Scheduling                                     └─ Month 12+: Platform Engineering
```

---

## 🏁 **Quick Start Checklist**

- [ ] Understand cluster architecture (Control Plane, Nodes, Addons)
- [ ] Deploy your first pod with liveness/readiness probes
- [ ] Expose pod with Service (ClusterIP, NodePort, LoadBalancer)
- [ ] Create Deployment with rolling update strategy
- [ ] Add ConfigMap for configuration
- [ ] Store secrets securely (not in git!)
- [ ] Set ResourceQuota for namespace
- [ ] Configure LimitRange for containers
- [ ] Implement NetworkPolicy for zero-trust
- [ ] Set up monitoring (Prometheus + Grafana)
- [ ] Configure HPA for autoscaling
- [ ] Implement RBAC (Roles, RoleBindings)
- [ ] Practice troubleshooting common failure signals
- [ ] Understand pod lifecycle signals
- [ ] Backup etcd regularly

---

<div align="center">

## ✅ **Complete Component Coverage Checklist**

| Category | Components | Status |
|----------|------------|:------:|
| **🏗️ Control Plane** | API Server, etcd, Scheduler, Controller Manager, Cloud Controller | ✅ |
| **🖥️ Worker Nodes** | Kubelet, Kube-proxy, Container Runtime | ✅ |
| **📡 Addons** | CoreDNS, Metrics Server, Ingress Controller, Dashboard, CNI, CSI | ✅ |
| **📦 Workloads** | Pod, ReplicaSet, RC, Deployment, StatefulSet, DaemonSet, Job, CronJob | ✅ |
| **🌐 Networking** | Service (4 types), Ingress, IngressClass, NetworkPolicy, Endpoint, EndpointSlice, CNI | ✅ |
| **💾 Storage** | Volume (7 types), PV, PVC, StorageClass, VolumeSnapshot, VolumeSnapshotClass, CSI, CSIDriver, CSINode | ✅ |
| **🔐 Security** | ConfigMap, Secret (6 types), ServiceAccount, Role, ClusterRole, RoleBinding, ClusterRoleBinding, PSA, SecurityContext, PSP | ✅ |
| **🎯 Scheduling** | Node, Namespace, Label, Annotation, NodeSelector, NodeAffinity, PodAffinity, PodAntiAffinity, Taints, Tolerations, PriorityClass, TopologySpread, Preemption | ✅ |
| **⚡ Autoscaling** | HPA, VPA (4 modes), Cluster Autoscaler, ResourceQuota, LimitRange | ✅ |
| **🩺 Health** | Liveness, Readiness, Startup Probes, ReadinessGates, PodConditions, ContainersReady, PodScheduled | ✅ |
| **📈 Signals** | 74+ Pod-level signals (Health, Lifecycle, Resource, Networking, Scheduling, Security, Storage, Scaling) | ✅ |
| **🚨 Failure** | 17+ Production failure signals | ✅ |
| **📏 Policy** | ResourceQuota, LimitRange, NetworkPolicy, PDB, PriorityClass, RuntimeClass | ✅ |
| **🔧 Extensions** | CRD, Operator (5 levels), MutatingWebhook, ValidatingWebhook, RuntimeClass | ✅ |
| **📋 API** | 50+ API resources with namespace status | ✅ |

### **TOTAL COMPONENTS COVERED: 180+**

<br>

<table>
  <tr>
    <td align="center"><b>🏗️ Architecture</b><br>Control Plane → Worker Nodes → Addons</td>
    <td align="center"><b>📦 Workloads</b><br>Pod → Deployment → STS → DS → Jobs</td>
    <td align="center"><b>🌐 Networking</b><br>Service → Ingress → Policy → CNI</td>
  </tr>
  <tr>
    <td align="center"><b>💾 Storage</b><br>PVC → PV → StorageClass → CSI</td>
    <td align="center"><b>🔐 Security</b><br>RBAC → Secrets → PSA → ServiceAccount</td>
    <td align="center"><b>📏 Policy</b><br>ResourceQuota → LimitRange → PDB</td>
  </tr>
  <tr>
    <td align="center"><b>⚡ Autoscaling</b><br>HPA → VPA → CA</td>
    <td align="center"><b>🎯 Scheduling</b><br>Affinity → Taints → Priority</td>
    <td align="center"><b>📈 Signals</b><br>Health → Lifecycle → Resource → Failure</td>
  </tr>
  <tr>
    <td align="center"><b>🩺 Probes</b><br>Liveness → Readiness → Startup</td>
    <td align="center"><b>🔧 Extensions</b><br>CRD → Operators → Webhooks</td>
    <td align="center"><b>📋 API</b><br>50+ Resources → Namespaced → Cluster-wide</td>
  </tr>
</table>

<br>

| Resource Type | Check | Resource Type | Check | Resource Type | Check |
|--------------|:-----:|---------------|:-----:|---------------|:-----:|
| Pods | ✅ | Deployments | ✅ | StatefulSets | ✅ |
| Services | ✅ | Ingress | ✅ | NetworkPolicy | ✅ |
| ConfigMaps | ✅ | Secrets | ✅ | ServiceAccounts | ✅ |
| ResourceQuota | ✅ | LimitRange | ✅ | PodDisruptionBudget | ✅ |
| PersistentVolumes | ✅ | StorageClass | ✅ | CSI Drivers | ✅ |
| RBAC (Roles/Bindings) | ✅ | PodSecurity | ✅ | SecurityContext | ✅ |
| HPA | ✅ | VPA | ✅ | Cluster Autoscaler | ✅ |
| Node/Pod Affinity | ✅ | Taints/Tolerations | ✅ | PriorityClass | ✅ |
| Liveness/Readiness | ✅ | Startup Probe | ✅ | Pod Signals | ✅ |
| CRD | ✅ | Operators | ✅ | Admission Webhooks | ✅ |

<br>

**⭐ Star this repo** • **🔄 Share with peers** • **📚 Practice daily**

**Remember:** This is the complete Kubernetes reference - every component, resource, signal, and failure mode covered! 

**Total Components Covered: 180+** 🚀

**[⬆ Back to Top](#-kubernetes-mastery-guide)**

</div>
