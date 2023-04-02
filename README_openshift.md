# anzo-unstructured-operator

## By Cambridge Semantics Inc

## Supported tags

* [latest](registry.connect.redhat.com/cambridgesemantics/unstructured-operator:latest)

## About Anzo Unstructured
Anzo Unstructured solution has two main parts:
### Microservice Leader
The Microservice Leader works in concert with the Anzo Agent and Anzo Unstructured to perform ingestion of unstructured data into RDF suitable for use in an Anzo Data Fabric. The Microservice Leader provides queuing and coordination functions for a cluster of Anzo Unstructured workers. Ingesting unstructured data with an Anzo Server requires the configuration and use of one or more Microservice Leader nodes.
### Anzo Unstructured Worker
The Anzo Unstructured Worker works in concert with the Microservice Leader and Anzo Agent to perform ingestion of unstructured data into RDF suitable for use in an Anzo Data Fabric. Anzo Unstructured supports extraction from plain text, HTML, PDF, Word, and Excel files, and creation of annotations based on regular expressions, a linked data knowledge base, or third-party annotators via a REST interface. Anzo Unstructured leverages Anzo's distributed microservice framework (via the Anzo Agent and Microservice Leader) to allow timely processing of large document sets. Use of Anzo Unstructured requires an appropriately licensed Anzo Server installation.

**Project Status:** stable

**Operator Version:** v1

## Prerequisites
* Kubernetes cluster, versions {1.24-1.16}
* Kubectl, versions {1.24-1.16}

## Setting up prerequisites

## Steps to deploy anzo-unstructured-operator

## Anzo Unstructured Docker Images Used for Deployment
When you deploy Anzo Unstructured using operator, following are the set of images used for actual deployments. We have given reference docker commands to download the latest releases for each of them below.

### Anzo Unstructured Operator

* To download latest release, please use:

   ```sh
   docker pull registry.connect.redhat.com/cambridgesemantics/unstructured-operator
   ```

### Anzo Microservices Leader

* To download latest release, please use:

   ```sh
   docker pull registry.connect.redhat.com/cambridgesemantics/anzo-microservices-leader
   ```

### Anzo Unstructured Worker

* To download latest release, please use:

   ```sh
   docker pull registry.connect.redhat.com/cambridgesemantics/anzo-unstructured-worker
   ```

```sh
# Create Namespace, mention namespace in metadata.name
$ kubectl create -f deploy/v1_namespace_default.yaml
# Setup Service Account
$ kubectl create -f deploy/default_v1_serviceaccount_unstructured-operator.yaml --namespace <namespace>
# Setup RBAC
$ kubectl create -f deploy/default_rbac.authorization.k8s.io_v1_role_unstructured-operator.yaml --namespace <namespace>
$ kubectl create -f deploy/default_rbac.authorization.k8s.io_v1_rolebinding_unstructured-operator.yaml --namespace <namespace>
$ kubectl create -f deploy/rbac.authorization.k8s.io_v1_clusterrole_unstructured-operator.yaml
$ kubectl create -f deploy/rbac.authorization.k8s.io_v1_clusterrolebinding_unstructured-operator.yaml
$ kubectl create -f deploy/policy_v1beta1_podsecuritypolicy_unstructured-privileged.yaml
# Setup the CRD
$ kubectl create -f deploy/crds/anzo-unstructured.clusters.cambridgesemantics.com_anzo-unstructureds_crd.yaml
# Deploy anzo-unstructured-operator
$ kubectl create -f deploy/default_apps_v1_deployment_unstructured-operator.yaml --namespace <namespace>
# Deploy AnzoUnstructured Custom Resource(CR)
$ kubectl apply -f deploy/crds/default_anzounstructured.clusters.cambridgesemantics.com_v1_anzounstructured_au01.yaml --namespace <namespace>
```

**NOTE** One needs to edit deploy/default_apps_v1_deployment_unstructured-operator.yaml, with right docker image details.

## Steps to delete AnzoUnstructured CR and anzo-unstructured-operator
```sh
# Delete AnzoUnstructured CR
kubectl delete -f deploy/crds/default_anzounstructured.clusters.cambridgesemantics.com_v1_anzounstructured_au01.yaml --namespace <namespace>
# Delete anzo-unstructured-operator
kubectl delete -f deploy/default_apps_v1_deployment_unstructured-operator.yaml --namespace <namespace>
# Delete RBAC
kubectl delete -f deploy/default_rbac.authorization.k8s.io_v1_role_unstructured-operator.yaml --namespace <namespace>
kubectl delete -f deploy/default_rbac.authorization.k8s.io_v1_rolebinding_unstructured-operator.yaml --namespace <namespace>
kubectl delete -f deploy/rbac.authorization.k8s.io_v1_clusterrole_unstructured-operator.yaml
kubectl delete -f deploy/rbac.authorization.k8s.io_v1_clusterrolebinding_unstructured-operator.yaml
kubectl delete -f deploy/policy_v1beta1_podsecuritypolicy_unstructured-privileged.yaml
# Delete Service Account
kubectl delete -f deploy/default_v1_serviceaccount_unstructured-operator.yaml --namespace <namespace>
# Delete CRD
kubectl delete -f deploy/crds/anzo-unstructured.clusters.cambridgesemantics.com_anzo-unstructureds_crd.yaml
```

## AnzoUnstructured CustomResource(CR) Specification
The following table lists the configurable parameters for AnzoUnstructured and their default values.(CR API Version: v1)
| Parameter | Description | Default |
|-----------|-------------|---------|
| `metadata.name` | Name of CR | au01 |
| `metadata.namespace` | Namespace of CR | |
| `metadata.labels` | Dictionary of (key: val) as labels of CR | |
| `spec.volumes` | List of persistent volumes for AnzoUnstructured | commented, please uncomment to add value |
| `spec.volumes.[i].name` | Name for persistent volume | |
| `spec.volumes.[i].mountPath` | Path where persistent volume should be mounted inside container | |
| `spec.volumes.[i].pv` | Attributes to configure persistent volume, of type v1.PersistentVolume | |
| `spec.volumes.[i].pvc` | Attributes to configure persistent volume claim, of type v1.PersistentVolumeClaim | |
| `spec.msLeader.nodeConfig.spec` | Configuration specification for AnzoUnstructured Leader pods | |
| `spec.msLeader.nodeConfig.spec.replicas` | Number of pods for AnzoUnstructured Leader | 1 |
| `spec.msLeader.nodeConfig.spec.serviceName` | Name of headless service for AnzoUnstructured | au-<metadata.name>-ms |
| `spec.msLeader.nodeConfig.spec.template.spec.serviceAccountName` | Service account name for pods | unstructured-operator |
| `spec.msLeader.nodeConfig.spec.template.spec.containers.x.Name` | Name of AnzoUnstructured Leader container | ms |
| `spec.msLeader.jvmMemory` | AnzoUnstructured leader JVM memory | |
| `spec.msLeader.bootProperties` | AnzoUnstructured leader specific boot properties | |
| `spec.auWorker.nodeConfig.spec` | Configuration specification for AnzoUnstructured Worker pods | |
| `spec.auWorker.nodeConfig.spec.replicas` | Number of pods for AnzoUnstructured Worker | 1 |
| `spec.auWorker.nodeConfig.spec.serviceName` | Name of headless service for AnzoUnstructured | au-<metadata.name>-w |
| `spec.auWorker.nodeConfig.spec.template.spec.serviceAccountName` | Service account name for pods | unstructured-operator |
| `spec.auWorker.nodeConfig.spec.template.spec.containers.x.Name` | Name of AnzoUnstructured Worker container | w |
| `spec.auWorker.bootProperties` | AnzoUnstructured worker specific boot properties | |
| `spec.auWorker.jvmMemory` | AnzoUnstructured worker JVM memory | |
| `spec.bootProperties` | Boot properties i.e. the environment variables for AnzoUnstructured CR | commented, please uncomment to add value |


## References
```https://docs.cambridgesemantics.com/```
