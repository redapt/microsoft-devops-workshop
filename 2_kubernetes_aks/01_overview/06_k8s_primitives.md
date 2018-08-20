# Kubernetes Primitives

If the previous pieces were the brain and nervous system, the primitives would be the language you use to communicate desired state. 

**Note:** Not every object or primitive is listed here, those that are included were picked as being the most useful to daily and weekly operations of an app team using K8s. These are also referred to as Kubernetes "Objects" or "API Primitives"

#### Cluster
A cluster is a set of machines (physical or virtual) on which your applications are managed and run. All machines are managed as a cluster (or set of clusters, depending on the topology used).

#### Nodes (minions) 
You can think of these as "container clients". These are the individual hosts (physical or virtual) that Docker is installed on and hosts the various containers within your managed cluster.
Each node will run etcd (a key-pair management and communication service, used by Kubernetes for exchanging messages and reporting on cluster status) as well as the Kubernetes Proxy.

#### Pods 
A pod consists of one or more containers. Those containers are guaranteed (by the cluster controller) to be located on the same host machine (aka "co-located") in order to facilitate sharing of resources. For an example, it makes sense to have database processes and data containers as close as possible. In fact, they really should be in the same pod.
Pods "work together", as in a multi-tiered application configuration. Each set of pods that define and implement a service (e.g., MySQL or Apache) are defined by the label selector (see below).
Pods are assigned unique IPs within each cluster. These allow an application to use ports without having to worry about conflicting port utilization.
Pods can contain definitions of disk volumes or shares, and then provide access from those to all the members (containers) within the pod.
Pod management is done through the API or delegated to a controller.

A Pod is the smallest and simplest Kubernetes object. It is the unit of deployment in Kubernetes, which represents a single instance of the application. A Pod is a logical collection of one or more containers, which:

1. are scheduled together on the same host;
2. share the same network namespace; and
3. mount the same external storage (Volumes).

Pods are ephemeral in nature, and they do not have the capability to self-heal by themselves. That is why we use them with controllers, which can handle a Pod's replication, fault tolerance, self-heal, etc. Examples of controllers are Deployments, ReplicaSets, ReplicationControllers, DaemonSets, etc. We attach the Pod's specification to other objects using Pod Templates.

#### Controllers 
These are used in the management of your cluster. Controllers are the mechanism by which your desired configuration state is enforced.
Controllers manage a set of pods and, depending on the desired configuration state, may engage other controllers to handle replication and scaling. It is also responsible for replacing any container in a pod that fails (based on the desired state of the cluster). Deployment, Replication Controllers, Replica Sets, Daemon Sets, and Jobs are all different types of Kubernetes controllers. 

#### Replication Controllers (RC) 
An abstraction used to manage pod lifecycles. One of the key uses of RCs is to maintain a certain number of running Pods (e.g., for scaling or ensuring that at least one Pod is running at all times, etc.). It is considered a "best practice" to use RCs to define Pod lifecycles, rather than creating Pods directly. Each pod created will match the name of the replication controller with a randomly generated suffix to identify which instance it is.

#### Deployments
Generally, when you are rolling out a new version of your application, you would do so through a primitive called a Deployment. A deployment is effectively a set of ReplicationControllers. The abstraction enables simple rolling updates by creating a new ReplicationController and simultaneous scaling it up and the old one down, per policies defined in the deployment, including readiness and liveness checks. Each RC is named after the deployment with a randomly generated suffix to identify which instance it is. This name propagates to the underlying pods.

#### Horizontal Pod Autoscaler (HPA)
You can defined an HPA that triggers ReplicationControllers to scale up, when CPU thresholds are reached within a pod. The latest version of Kubernetes requires that the K8s Metrics Server be installed on your cluster. With resource constraints defined properly within the pod definition, automatic or manual scale up can trigger an "Unschedulable" event. There is an optional cluster-autoscaler which ties into the major cloud platforms, to increase node pool size if this event is thrown.

#### Replica Sets (RS)
They also monitor and ensure the required number of Pods are running, replacing Pods that die. The replica sets controller follows different rules than the standard replication controller. When it brings up new instances, each is appended with a number, indicating the order it was created. When the number of pods are reduced it destroys the newest instances first, generally scale down is more gated and throttled for these sets as well.
Due to the number appending to the pod names, you are also given predictable host names, which makes certain things easier as well.

#### Daemon Sets (DS)
These are defined when you have Pods that need to be run on every node in your cluster, or those which meet certain criteria. The controller kicks off pod creation whenever a new node is introduced to the cluster. This is typically used for system wide tools like log forwarders, or a metrics collectors. For example tools like DataDog can be installed via DaemonSet, with your keys as part of the definition.

#### Jobs
The jobs controller functions similarly to cron jobs that you may be familiar with. Each job encapsulates a pod definition that runs, to complete some task, and then ends. These jobs can be scheduled to run only at creation time or periodically, on an interval. The most common things that users are using jobs for are database migration and database backup. Jobs like these can be kicked off simultaneously with, or prior to rolling out a new deployment.

#### Labels 
Clients can attach key-value pairs to any object in the system (e.g., Pods or Nodes). These become the labels that identify them in the configuration and management of them. The key-value pairs can be used to filter, organize, and perform mass operations on a set of resources.

#### Selectors 
Label Selectors represent queries that are made against those labels. They resolve to the corresponding matching objects. A Selector expression matches labels to filter certain resources. For example, you may want to search for all pods that belong to a certain service, or find all containers that have a specific tier Label value as "database". Labels and Selectors are inherently two sides of the same coin. You can use Labels to classify resources and use Selectors to find them and use them for certain actions.
These two items are the primary way that grouping is done in Kubernetes and determine which components that a given operation applies to when indicated.

#### Services 
A Service is an abstraction on top of Pods, which provides a single IP address and DNS name by which the Pods can be accessed. This load balancing configuration is much easier to manage and helps scale Pods seamlessly.
Kubernetes can then provide service discovery and handle routing with the static IP for each pod as well as load balancing (round-robin based) connections to that service among the pods that match the label selector indicated.

There are 3 types of Services, each including all of their primitive counterparts building up to `LoadBalancer`.

1. ClusterIP - This results in a local ip that can be reached by other pods within the cluster only.
2. NodePort - Includes a ClusterIP, but also creates an iptable rule on each node, at a designated port per service with this type, to allow external traffic.
3. LoadBalancer - Typically used with CloudProviders, this provides a ClusterIP, a NodePort, and additionally creates a cloud loadbalancer attached to your nodes on that NodePort.

#### Ingress Controller
It's a common pattern to expose an add-on Ingress Controller via a specific NodePort, and define internal Ingress rules to route traffic to your pods from there based on hostname, path, etc. A proper kubernetes ingress controller should be able to consume ingress resources, and adjust its routing policy accordingly. The 2 most popular open-source solutions are nginx and haproxy. Each of these have community supported images that include control loops, to dynamically adjust their configuration based on ingress resources in Kubernetes.

#### Ingress
The ingress resource does not do much without one of the optional ingress controllers deployed. The ingress resource is the set of rules which define hostname to service name maps, so that all your cluster traffic can flow through the same external ip, and get routed appropriately. You also have options to perform TLS termination using Kubernetes Secrets.

## Secrets

## Configmaps

#### Volumes 
A Volume is a directory with data, which is accessible to a container. The volume co-terminates with the Pods that encloses it.

#### Namespace 
A Namespace provides additional qualification to a resource name. This is especially helpful when multiple teams/projects are using the same cluster and there is a potential for name collision. You can think of a Namespace as a virtual wall between multiple clusters.