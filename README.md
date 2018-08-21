# Microsoft DevOps Workshop
Code for the demos and labs for this workshop

by Jerry Meisner and Christoph Champ for Redapt, Inc. (August 2018)

## Table of Contents

1. [Introduction](01_introduction)
1. [Docker](02_docker)
   1. Benefits of Containers
   2. Containerization
      1. Java [JVM Specifics]
      2. .NET (Windows Base Containers)
      3. Ruby
      4. NodeJs
      5. Python
   3. Configurability
   4. Security
2. [Kubernetes / AKS](03_kubernetes_aks)
   1. [Overview / Moving Parts](03_kubernetes_aks/01_overview)
      1. [ETCD](03_kubernetes_aks/01_overview/01_etcd.md)
      2. [Kubernetes API](03_kubernetes_aks/01_overview/02_kubernetes_api.md)
      3. [Controller Manager / Scheduler / Kubelet / Proxy](03_kubernetes_aks/01_overview/03_controller_manager-scheduler-kubelet-proxy.md)
      4. [Networking (Flannel/Calico)](03_kubernetes_aks/01_overview/04_networking.md)
      5. [Primitives (Deployments / Services / Pods / etc)](03_kubernetes_aks/01_overview/05_k8s_primitives.md)
      6. [RBAC / Service Accounts](03_kubernetes_aks/01_overview/06_rbac.md)
   2. [Environment Strategies (NonProd/Prod vs Dev/Test/Cert/Prod)](03_kubernetes_aks/02_environment_strategies)
      1. [Environments](03_kubernetes_aks/02_environment_strategies/01_environments.md)
      2. [Service Discovery / KubeDNS](03_kubernetes_aks/02_environment_strategies/02_service_discovery.md)
      3. [Workloads](03_kubernetes_aks/02_environment_strategies/03_workloads.md)
   3. Standing up your First Cluster
      1. MiniKube (Local)
      2. AKS
   4. Deploying Stateless Applications
      1. Hello-World
      2. Config Maps / Secrets
   5. Deploying Stateful Applications
      1. ELK Stack
      2. PVC
      3. Volume Types
   6. System Services
      1. Standard Kube-System Pods
      2. Third Party Add-Ons (Datadog, Helm)
      3. Developing from Scratch
   7. Ingress / Traffic Routing
      1. Defining Services ( ClusterIP / NodePort / LoadBalancer )
      2. Ingress Controllers / Rules
   8. Auto-Scaling / SRE
      1. Resources (Limits / Requests)
      2. Horizontal Pod Autoscaling
      3. Cluster Scaling
      4. Liveness / Readiness Probes
3. Pipelines
   1. Jenkins
   2. Containers as a Build Artifact / Promoting Builds
   3. Helm
   4. Integration Testing Micro-Services
