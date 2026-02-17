# ☸️ Kubernetes Mastery Guide
## *The Complete Encyclopedia of Kubernetes Components & Signals*

<div align="center">
  
  <img src="https://raw.githubusercontent.com/kubernetes/kubernetes/master/logo/logo.png" width="150">
  
  ### 📊 **Every Kubernetes Component, Signal, and Resource at a Glance**
  
  <table>
    <tr>
      <td align="center"><b>🏗️ Control Plane</b><br>5 Components</td>
      <td align="center"><b>🖥️ Data Plane</b><br>3 Components</td>
      <td align="center"><b>📦 Workloads</b><br>8 Resources</td>
      <td align="center"><b>🌐 Networking</b><br>7 Resources</td>
    </tr>
    <tr>
      <td align="center"><b>💾 Storage</b><br>9 Resources</td>
      <td align="center"><b>🔐 Security</b><br>10 Resources</td>
      <td align="center"><b>🎯 Scheduling</b><br>13 Resources</td>
      <td align="center"><b>⚡ Scaling</b><br>5 Resources</td>
    </tr>
    <tr>
      <td align="center"><b>🩺 Health</b><br>7 Probes</td>
      <td align="center"><b>📈 Pod Signals</b><br>70+ Signals</td>
      <td align="center"><b>📏 Policy</b><br>6 Resources</td>
      <td align="center"><b>🔧 Extensions</b><br>5 Resources</td>
    </tr>
  </table>
  
  <br>
  
  <img src="https://img.shields.io/badge/Total_Components-150+-326CE5?logo=kubernetes&logoColor=white&style=for-the-badge">
  <img src="https://img.shields.io/badge/CKA_CKAD_CKS_Ready-326CE5?style=for-the-badge&logo=cncf">
  
</div>

---

## 📑 **Complete Table of Contents**

| Section | Components | Icon |
|---------|------------|:----:|
| [Control Plane](#-control-plane-components) | 5 Components | 🏗️ |
| [Data Plane (Nodes)](#-data-plane-node-components) | 3 Components | 🖥️ |
| [Workload Resources](#-workload-resources) | 8 Resources | 📦 |
| [Networking Resources](#-networking-resources) | 7 Resources | 🌐 |
| [Service Types](#-service-types) | 4 Types | 🎯 |
| [Storage Resources](#-storage-resources) | 9 Resources | 💾 |
| [Configuration & Security](#-configuration--security) | 10 Resources | 🔐 |
| [Scheduling & Placement](#-scheduling--placement) | 13 Resources | 🎯 |
| [Scaling & Resource Management](#-scaling--resource-management) | 5 Resources | ⚡ |
| [Health & Reliability](#-health--reliability) | 7 Resources | 🩺 |
| [Pod-Level Signals](#-pod-level-signals-comprehensive) | 70+ Signals | 📈 |
| [Policy & Governance](#-policy--governance) | 6 Resources | 📏 |
| [Extensibility](#-extensibility) | 5 Resources | 🔧 |
| [Complete API Reference](#-complete-kubernetes-api-reference) | All Resources | 📋 |
| [Production Failure Signals](#-production-failure-signals) | 15+ Signals | 🚨 |

---

## 🏗️ **Control Plane Components**

| # | Component | Description | Production Role |
|---|-----------|-------------|-----------------|
| 1 | **kube-apiserver** | Front-end to control plane; validates and configures data | All requests go through API server |
| 2 | **etcd** | Distributed key-value store; cluster brain | Backup every 30 mins; TLS encryption |
| 3 | **kube-scheduler** | Assigns pods to nodes based on constraints | Multiple schedulers for different workloads |
| 4 | **kube-controller-manager** | Runs controller processes | Leader election for HA |
| 5 | **cloud-controller-manager** | Interfaces with cloud provider APIs | Cloud-specific controllers |

---

## 🖥️ **Data Plane (Node) Components**

| # | Component | Description | Implementation |
|---|-----------|-------------|----------------|
| 1 | **kubelet** | Primary node agent; registers node with cluster | Systemd service; port 10250 |
| 2 | **kube-proxy** | Network proxy; maintains network rules | DaemonSet; iptables/IPVS modes |
| 3 | **container runtime** | Runs containers (containerd, CRI-O) | Pulls images; manages container lifecycle |

---

## 📦 **Workload Resources**

| # | Resource | API Version | Namespaced | Purpose |
|---|----------|-------------|:----------:|---------|
| 1 | **Pod** | v1 | ✅ | Smallest deployable unit |
| 2 | **ReplicaSet** | apps/v1 | ✅ | Maintains stable set of pods |
| 3 | **ReplicationController** | v1 | ✅ | Legacy replica management |
| 4 | **Deployment** | apps/v1 | ✅ | Declarative pod updates |
| 5 | **StatefulSet** | apps/v1 | ✅ | Stateful applications |
| 6 | **DaemonSet** | apps/v1 | ✅ | Run pod on every node |
| 7 | **Job** | batch/v1 | ✅ | Run to completion |
| 8 | **CronJob** | batch/v1 | ✅ | Scheduled jobs |

---

## 🌐 **Networking Resources**

| # | Resource | API Version | Namespaced | Purpose |
|---|----------|-------------|:----------:|---------|
| 1 | **Service** | v1 | ✅ | Pod network abstraction |
| 2 | **Ingress** | networking.k8s.io/v1 | ✅ | HTTP/S routing |
| 3 | **IngressClass** | networking.k8s.io/v1 | ❌ | Ingress controller class |
| 4 | **NetworkPolicy** | networking.k8s.io/v1 | ✅ | Pod firewall rules |
| 5 | **Endpoint** | v1 | ✅ | Service endpoint list (legacy) |
| 6 | **EndpointSlice** | discovery.k8s.io/v1 | ✅ | Scalable endpoint tracking |
| 7 | **CNI (Container Network Interface)** | - | ❌ | Networking plugin interface |

---

## 🎯 **Service Types**

| # | Service Type | Description | External Access |
|---|--------------|-------------|-----------------|
| 1 | **ClusterIP** | Internal cluster IP only | ❌ No |
| 2 | **NodePort** | Expose on each node's IP:port | ✅ `<NodeIP>:<NodePort>` |
| 3 | **LoadBalancer** | Cloud provider provisions LB | ✅ Cloud LB IP |
| 4 | **Headless Service** | No cluster IP; direct pod DNS | ✅ Direct pod DNS |

---

## 💾 **Storage Resources**

| # | Resource | API Version | Namespaced | Purpose |
|---|----------|-------------|:----------:|---------|
| 1 | **Volume** | v1 | ✅ | Pod storage |
| 2 | **PersistentVolume (PV)** | v1 | ❌ | Cluster storage resource |
| 3 | **PersistentVolumeClaim (PVC)** | v1 | ✅ | Storage request |
| 4 | **StorageClass** | storage.k8s.io/v1 | ❌ | Storage type definition |
| 5 | **VolumeSnapshot** | snapshot.storage.k8s.io/v1 | ✅ | Volume snapshot |
| 6 | **VolumeSnapshotClass** | snapshot.storage.k8s.io/v1 | ❌ | Snapshot class |
| 7 | **CSI (Container Storage Interface)** | - | ❌ | Storage plugin interface |
| 8 | **CSIDriver** | storage.k8s.io/v1 | ❌ | CSI driver registration |
| 9 | **CSINode** | storage.k8s.io/v1 | ❌ | CSI node info |

---

## 🔐 **Configuration & Security**

| # | Resource | API Version | Namespaced | Purpose |
|---|----------|-------------|:----------:|---------|
| 1 | **ConfigMap** | v1 | ✅ | Non-sensitive configuration |
| 2 | **Secret** | v1 | ✅ | Sensitive data |
| 3 | **ServiceAccount** | v1 | ✅ | Pod identity |
| 4 | **Role** | rbac.authorization.k8s.io/v1 | ✅ | Namespace permissions |
| 5 | **ClusterRole** | rbac.authorization.k8s.io/v1 | ❌ | Cluster permissions |
| 6 | **RoleBinding** | rbac.authorization.k8s.io/v1 | ✅ | Bind role to subjects |
| 7 | **ClusterRoleBinding** | rbac.authorization.k8s.io/v1 | ❌ | Bind cluster role |
| 8 | **PodSecurity** (PSA) | - | ✅ | Pod security standards |
| 9 | **SecurityContext** | v1 | ✅ | Container-level security |
| 10 | **PodSecurityPolicy** (deprecated) | policy/v1beta1 | ❌ | Legacy pod security |

---

## 🎯 **Scheduling & Placement**

| # | Resource/Concept | Type | Purpose |
|---|-----------------|------|---------|
| 1 | **Node** | Resource | Worker machine in cluster |
| 2 | **Namespace** | Resource | Resource isolation |
| 3 | **Label** | Concept | Key/value for organization |
| 4 | **Annotation** | Concept | Non-identifying metadata |
| 5 | **NodeSelector** | Scheduling | Simple node selection |
| 6 | **NodeAffinity** | Scheduling | Advanced node placement |
| 7 | **PodAffinity** | Scheduling | Co-locate pods |
| 8 | **PodAntiAffinity** | Scheduling | Separate pods |
| 9 | **Taints** | Scheduling | Node repel pods |
| 10 | **Tolerations** | Scheduling | Pod tolerate taints |
| 11 | **PriorityClass** | Resource | Pod priority |
| 12 | **TopologySpreadConstraints** | Scheduling | Even distribution |
| 13 | **Preemption** | Concept | Higher priority pods evict lower |

---

## ⚡ **Scaling & Resource Management**

| # | Resource | API Version | Namespaced | Purpose |
|---|----------|-------------|:----------:|---------|
| 1 | **HorizontalPodAutoscaler (HPA)** | autoscaling/v2 | ✅ | Scale pods by metrics |
| 2 | **VerticalPodAutoscaler (VPA)** | autoscaling.k8s.io/v1 | ✅ | Scale resources per pod |
| 3 | **ResourceQuota** | v1 | ✅ | Namespace resource limits |
| 4 | **LimitRange** | v1 | ✅ | Per-container/pod limits |
| 5 | **ClusterAutoscaler** | Addon | ❌ | Scale cluster nodes |

---

## 🩺 **Health & Reliability**

| # | Resource/Probe | Type | Purpose |
|---|---------------|------|---------|
| 1 | **LivenessProbe** | Probe | Is app alive? Restart if fails |
| 2 | **ReadinessProbe** | Probe | Is app ready for traffic? |
| 3 | **StartupProbe** | Probe | Has app started? For slow apps |
| 4 | **PodDisruptionBudget (PDB)** | Resource | Pod availability guarantee |
| 5 | **Lease** | Resource | Distributed locks |
| 6 | **ReadinessGates** | Concept | Additional readiness conditions |
| 7 | **PodConditions** | Status | Pod status conditions |

---

## 📈 **Pod-Level Signals (Comprehensive)**

### 🩺 **Health Signals**

| # | Signal | Description |
|---|--------|-------------|
| 1 | **LivenessProbe** | Container alive check |
| 2 | **ReadinessProbe** | Container ready for traffic |
| 3 | **StartupProbe** | Container started successfully |
| 4 | **ReadinessGates** | Extra readiness conditions |
| 5 | **PodConditions** | Overall pod conditions |
| 6 | **ContainersReady** | All containers ready status |
| 7 | **Ready** | Pod ready status |
| 8 | **Initialized** | Init containers completed |
| 9 | **PodScheduled** | Pod assigned to node |

### 🔄 **Lifecycle Signals**

| # | Signal | Description |
|---|--------|-------------|
| 10 | **PodPhase** | Current pod phase |
| 11 | **ContainerStateWaiting** | Container waiting reason |
| 12 | **ContainerStateRunning** | Container running |
| 13 | **ContainerStateTerminated** | Container terminated |
| 14 | **ContainerLastState** | Previous container state |
| 15 | **RestartCount** | Number of restarts |
| 16 | **ExitCode** | Container exit code |
| 17 | **TerminationMessage** | Why container terminated |
| 18 | **TerminationGracePeriodSeconds** | Grace period for shutdown |
| 19 | **DeletionTimestamp** | When pod marked for deletion |
| 20 | **Finalizers** | Pre-deletion cleanup |
| 21 | **PreStopHook** | Pre-termination hook |
| 22 | **PostStartHook** | Post-startup hook |

### 📊 **Resource Signals**

| # | Signal | Description |
|---|--------|-------------|
| 23 | **CPUUsage** | Current CPU usage |
| 24 | **MemoryUsage** | Current memory usage |
| 25 | **EphemeralStorageUsage** | Temporary storage usage |
| 26 | **ResourceRequests** | Requested resources |
| 27 | **ResourceLimits** | Maximum resources |
| 28 | **QoSClass** | Quality of Service class |
| 29 | **OOMKilled** | Killed due to memory |
| 30 | **CPUThrottling** | CPU throttled |
| 31 | **Evicted** | Pod evicted |
| 32 | **NodePressureEviction** | Evicted due to node pressure |
| 33 | **MemoryPressure** | Node memory pressure |
| 34 | **DiskPressure** | Node disk pressure |
| 35 | **PIDPressure** | Node PID pressure |

### 🌐 **Networking Signals**

| # | Signal | Description |
|---|--------|-------------|
| 36 | **PodIP** | Pod IP address |
| 37 | **HostIP** | Node IP address |
| 38 | **CNIAllocation** | CNI IP allocation status |
| 39 | **NetworkPolicyStatus** | Network policy applied |
| 40 | **ServiceEndpointRegistration** | Registered in service |
| 41 | **EndpointSliceUpdate** | EndpointSlice updated |

### 🧭 **Scheduling Signals**

| # | Signal | Description |
|---|--------|-------------|
| 42 | **NodeSelector** | Selected node |
| 43 | **NodeAffinity** | Node affinity rules |
| 44 | **PodAffinity** | Pod affinity rules |
| 45 | **PodAntiAffinity** | Pod anti-affinity rules |
| 46 | **Taints** | Node taints |
| 47 | **Tolerations** | Pod tolerations |
| 48 | **TopologySpreadConstraints** | Spread constraints |
| 49 | **PriorityClass** | Pod priority |
| 50 | **Preemption** | Preemption occurred |
| 51 | **Unschedulable** | Cannot schedule |
| 52 | **FailedScheduling** | Scheduling failed |

### 🔐 **Security Signals**

| # | Signal | Description |
|---|--------|-------------|
| 53 | **ServiceAccount** | Associated service account |
| 54 | **SecurityContext** | Security settings |
| 55 | **PrivilegedMode** | Running privileged |
| 56 | **SeccompProfile** | Seccomp profile applied |
| 57 | **AppArmorProfile** | AppArmor profile applied |
| 58 | **PodSecurityAdmission** | PSA admission result |
| 59 | **ImagePullSecret** | Registry credentials used |
| 60 | **ImagePullBackOff** | Failed to pull image |
| 61 | **ErrImagePull** | Error pulling image |

### 📦 **Storage Signals**

| # | Signal | Description |
|---|--------|-------------|
| 62 | **VolumeMountStatus** | Volume mounted |
| 63 | **PVCBound** | PVC bound to PV |
| 64 | **PVCPending** | PVC pending |
| 65 | **VolumeAttachStatus** | Volume attached |
| 66 | **VolumeDetachStatus** | Volume detached |
| 67 | **FailedMount** | Mount failed |
| 68 | **CSIDriverError** | CSI driver error |
| 69 | **ReadOnlyFilesystem** | Filesystem read-only |

### 📈 **Scaling Signals**

| # | Signal | Description |
|---|--------|-------------|
| 70 | **HPAStatus** | HPA status |
| 71 | **TargetCPUUtilization** | CPU target |
| 72 | **CustomMetricsStatus** | Custom metrics |
| 73 | **ReplicaSetScalingEvent** | RS scaled |
| 74 | **DeploymentScalingEvent** | Deployment scaled |

---

## 📏 **Policy & Governance**

| # | Resource | API Version | Namespaced | Purpose |
|---|----------|-------------|:----------:|---------|
| 1 | **ResourceQuota** | v1 | ✅ | Namespace resource limits |
| 2 | **LimitRange** | v1 | ✅ | Per-container/pod limits |
| 3 | **NetworkPolicy** | networking.k8s.io/v1 | ✅ | Pod firewall rules |
| 4 | **PodDisruptionBudget** | policy/v1 | ✅ | Pod availability guarantee |
| 5 | **PriorityClass** | scheduling.k8s.io/v1 | ❌ | Pod priority |
| 6 | **RuntimeClass** | node.k8s.io/v1 | ❌ | Container runtime |

---

## 🔧 **Extensibility**

| # | Resource | API Version | Namespaced | Purpose |
|---|----------|-------------|:----------:|---------|
| 1 | **CustomResourceDefinition (CRD)** | apiextensions.k8s.io/v1 | ❌ | Define custom resources |
| 2 | **Operator** | Pattern | - | Application lifecycle automation |
| 3 | **MutatingAdmissionWebhook** | admissionregistration.k8s.io/v1 | ❌ | Mutate requests |
| 4 | **ValidatingAdmissionWebhook** | admissionregistration.k8s.io/v1 | ❌ | Validate requests |
| 5 | **RuntimeClass** | node.k8s.io/v1 | ❌ | Container runtime configuration |

---

## 🚨 **Production Failure Signals**

| # | Signal | Description | Common Cause |
|---|--------|-------------|--------------|
| 1 | **CrashLoopBackOff** | Container crashes repeatedly | App error, bad config |
| 2 | **ImagePullBackOff** | Cannot pull image | Wrong image, registry issues |
| 3 | **ErrImagePull** | Error pulling image | Network, auth, missing image |
| 4 | **CreateContainerConfigError** | Config error | Missing ConfigMap/Secret |
| 5 | **CreateContainerError** | Cannot create container | Runtime issues |
| 6 | **ContainerCannotRun** | Container cannot start | Permission, binary missing |
| 7 | **BackOffRestartingContainer** | Backoff after crash | App repeatedly crashing |
| 8 | **ContextDeadlineExceeded** | Operation timeout | Network, slow operations |
| 9 | **NodeNotReady** | Node not ready | Kubelet down, network |
| 10 | **PodNotReady** | Pod not ready | Readiness probe failing |
| 11 | **ContainerStatusUnknown** | Unknown state | Node problem |
| 12 | **OOMKilled** | Out of memory killed | Memory limit too low |
| 13 | **Evicted** | Pod evicted | Resource pressure |
| 14 | **FailedScheduling** | Cannot schedule | Insufficient resources |
| 15 | **FailedMount** | Volume mount failed | Storage issues |
| 16 | **InvalidImageName** | Invalid image name | Typo in image |
| 17 | **NetworkPluginNotReady** | CNI not ready | Network plugin issues |

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

---

## 🎓 **Certification Coverage**

| Certification | Components Covered |
|--------------|-------------------|
| **CKA** | All Control Plane, Node Components, Workloads, Networking, Storage, Scheduling, Troubleshooting |
| **CKAD** | Workloads, Configuration, Observability, Pod Design, Services, Troubleshooting |
| **CKS** | Security (RBAC, Secrets, PSA), Network Policies, Runtime Security, Supply Chain |

---

## 📚 **Complete Component Count**

| Category | Count |
|----------|:-----:|
| Control Plane Components | 5 |
| Data Plane Components | 3 |
| Workload Resources | 8 |
| Networking Resources | 7 |
| Service Types | 4 |
| Storage Resources | 9 |
| Configuration & Security | 10 |
| Scheduling & Placement | 13 |
| Scaling & Resource Management | 5 |
| Health & Reliability | 7 |
| Pod-Level Signals | 74 |
| Policy & Governance | 6 |
| Extensibility | 5 |
| Production Failure Signals | 17 |
| **TOTAL COMPONENTS** | **173+** |

---

<div align="center">

## ✅ **Mastery Checklist - Every Component Covered**

| Category | Status |
|----------|:------:|
| Control Plane | ✅ Complete |
| Data Plane | ✅ Complete |
| Workloads | ✅ Complete |
| Networking | ✅ Complete |
| Storage | ✅ Complete |
| Security | ✅ Complete |
| Scheduling | ✅ Complete |
| Scaling | ✅ Complete |
| Health Probes | ✅ Complete |
| Pod Signals | ✅ Complete |
| Policy | ✅ Complete |
| Extensions | ✅ Complete |
| Failure Signals | ✅ Complete |

<br>

## 🎉 **You Now Have the Complete Kubernetes Encyclopedia!**

<table>
  <tr>
    <td align="center"><b>🏗️ Control Plane</b><br>5 Components</td>
    <td align="center"><b>🖥️ Data Plane</b><br>3 Components</td>
    <td align="center"><b>📦 Workloads</b><br>8 Resources</td>
  </tr>
  <tr>
    <td align="center"><b>🌐 Networking</b><br>7+ Resources</td>
    <td align="center"><b>💾 Storage</b><br>9 Resources</td>
    <td align="center"><b>🔐 Security</b><br>10 Resources</td>
  </tr>
  <tr>
    <td align="center"><b>🎯 Scheduling</b><br>13 Resources</td>
    <td align="center"><b>⚡ Scaling</b><br>5 Resources</td>
    <td align="center"><b>🩺 Health</b><br>7 Probes</td>
  </tr>
  <tr>
    <td align="center"><b>📈 Signals</b><br>74+ Signals</td>
    <td align="center"><b>📏 Policy</b><br>6 Resources</td>
    <td align="center"><b>🔧 Extensions</b><br>5 Resources</td>
  </tr>
</table>

<br>

**⭐ Star this repo** • **🔄 Share with peers** • **📚 Practice daily**

**Remember:** This is the complete Kubernetes reference - every component, signal, and resource in one place! 🚀

**[⬆ Back to Top](#-kubernetes-mastery-guide)**

</div>
