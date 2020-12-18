# anzo-unstructured-operator
## By Cambridge Semantics Inc
## Supported tags
## About Anzo Unstructured
Anzo Unstructured solution has two main parts:
### Microservice Leader
The Microservice Leader works in concert with the Anzo Agent and Anzo Unstructured to perform ingestion of unstructured data into RDF suitable for use in an Anzo Data Fabric. The Microservice Leader provides queuing and coordination functions for a cluster of Anzo Unstructured workers. Ingesting unstructured data with an Anzo Server requires the configuration and use of one or more Microservice Leader nodes.
### Anzo Unstructured Worker
The Anzo Unstructured Worker works in concert with the Microservice Leader and Anzo Agent to perform ingestion of unstructured data into RDF suitable for use in an Anzo Data Fabric. Anzo Unstructured supports extraction from plain text, HTML, PDF, Word, and Excel files, and creation of annotations based on regular expressions, a linked data knowledge base, or third-party annotators via a REST interface. Anzo Unstructured leverages Anzo's distributed microservice framework (via the Anzo Agent and Microservice Leader) to allow timely processing of large document sets. Use of Anzo Unstructured requires an appropriately licensed Anzo Server installation.

**Project Status** stable

**Operator Version** v1beta1
## Prerequisites
* Kubernetes cluster, versions {1.18-1.14}
* Kubectl, versions {1.18-1.14}
## Setting up prerequisites
## Steps to deploy anzo-unstructured-operator

**NOTE**: You must get following docker images for deployment
### AnzoUnstructured Operator
* Path of private GCR repository: ```https://hub.docker.com/r/cambridgesemantics/unstructured-operator```
* Command to download the latest release, please use:
    ```sh
   docker pull cambridgesemantics/unstructured-operator
   ```
### AnzoUnstructured Leader
* Path of private GCR repository:: ```https://hub.docker.com/r/cambridgesemantics/anzo-microservices-leader```
* Command to download the latest release, please use:
    ```sh
   docker pull cambridgesemantics/anzo-microservices-leader
   ```
### AnzoUnstructured Workers
* Path of private GCR repository:: ```https://hub.docker.com/r/cambridgesemantics/anzo-unstructured-worker```
* Command to download the latest release, please use:
   ```sh
   docker pull cambridgesemantics/anzo-unstructured-worker
   ```

## Connect to Kubernetes cluster
You can connect to Kubernetes cluster hosted in public cloud service providers such as AWS, Google, Microsoft Azure, after adding/updating ```${HOME}/.kube/config``` file on workstation where ```kubectl``` is installed.
* For AWS, install ```aws-cli``` and use:
 ```aws eks --region <region-name> update-kubeconfig --name <EKS-cluster-name>```
* For Google, use:
 ```gcloud container clusters get-credentials <GKE-cluster-name> --zone <zone-name> --project <project-name> --region <region-name>```
   To install gcloud, please refer to ```https://cloud.google.com/sdk/install```
* For Microsoft Azure, use:
 ```az aks get-credentials --resource-group <resource-group-name> --name <AKS-cluster-name>```
   To install azure cli, please refer to ```https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest```

## RBAC Prerequisites
For deploying AnzoUnstructured operator and clusters managed by it, user needs to enable RBAC and configure kubernetes objects mentioned below.
```sh
# Create Namespace
$ kubectl create -f deploy/namespace.yaml
# Setup Service Account
$ kubectl create -f deploy/service_account.yaml --namespace <namespace>
# Setup RBAC
$ kubectl create -f deploy/role.yaml --namespace <namespace>
$ kubectl create -f deploy/role_binding.yaml --namespace <namespace>
$ kubectl create -f deploy/cluster_role.yaml
$ kubectl create -f deploy/cluster_role_binding.yaml
$ kubectl create -f deploy/psp.yaml
# Setup the CRD
$ kubectl create -f deploy/crds/anzo-unstructured.clusters.cambridgesemantics.com_anzo-unstructureds_crd.yaml
# Deploy anzo-unstructured-operator
$ kubectl create -f deploy/operator.yaml --namespace <namespace>
# Deploy AnzoUnstructured Custom Resource(CR), i.e. AnzoUnstructured cluster deployment
$ kubectl apply -f deploy/crds/anzo-unstructured_v1_anzo-unstructured_cr.yaml --namespace <namespace>
```
**NOTE** One needs to edit deploy/operator.yaml, deploy/crds/anzo-unstructured_v1beta1_anzo-unstructured_cr.yaml with right docker image details.

## Steps to delete AnzoUnstructured CR and anzo-unstructured-operator
```sh
# Delete AnzoUnstructured CR
kubectl delete -f deploy/crds/anzo-unstructured_v1_anzo-unstructured_cr.yaml --namespace <namespace>
# Delete anzo-unstructured-operator
kubectl delete -f deploy/operator.yaml --namespace <namespace>
# Delete RBAC
kubectl delete -f deploy/role.yaml --namespace <namespace>
kubectl delete -f deploy/role_binding.yaml --namespace <namespace>
kubectl delete -f deploy/cluster_role.yaml
kubectl delete -f deploy/cluster_role_binding.yaml
kubectl delete -f deploy/psp.yaml
# Delete Service Account
kubectl delete -f deploy/service_account.yaml --namespace <namespace>
# Delete CRD
kubectl delete -f deploy/crds/anzo-unstructured.clusters.cambridgesemantics.com_anzo-unstructureds_crd.yaml
```
## AnzoUnstructured CustomResource(CR) Specification
The following table lists the configurable parameters for AnzoUnstructured and their default values.(CR API Version: v1beta1)
| Parameter | Description | Default |
|-----------|-------------|---------|
| `metadata.name` | Name of CR | au01 |
| `metadata.namespace` | Namespace of CR | |
| `metadata.labels` | Dictionary of (key: val) as labels of CR | |
| `spec.serviceAccountName` | Name of service account for deploying CR | unstructured-operator |
| `spec.volumes` | List of persistent volumes for AnzoUnstructured | commented, please uncomment to add value |
| `spec.volumes.[i].name` | Name for persistent volume | |
| `spec.volumes.[i].mountPath` | Path where persistent volume should be mounted inside container | |
| `spec.volumes.[i].pv` | Attributes to configure persistent volume, of type v1.PersistentVolume | |
| `spec.volumes.[i].pvc` | Attributes to configure persistent volume claim, of type v1.PersistentVolumeClaim | |
| `spec.anzoMicroservicesLeader.image.registry` | Docker image registry for AnzoUnstructured leader image | us.gcr.io/cloud-sandbox-1162 |
| `spec.anzoMicroservicesLeader.image.name` | Docker image name for AnzoUnstructured leader image | cambridgesemantics/anzo-microservices-leader |
| `spec.anzoMicroservicesLeader.image.tag` | Docker image tag for AnzoUnstructured leader image | latest |
| `spec.anzoMicroservicesLeader.image.pullPolicy` | Docker image pull policy for AnzoUnstructured leader image | IfNotPresent |
| `spec.anzoMicroservicesLeader.image.pullSecrets` | Docker image pull secrets for AnzoUnstructured leader image | |
| `spec.anzoMicroservicesLeader.size` | No of AnzoUnstructured leader cluster pods | 1 |
| `spec.anzoMicroservicesLeader.resources.requests.cpu` | Resource request for AnzoUnstructured leader container, number of CPUs | 4000m |
| `spec.anzoMicroservicesLeader.resources.requests.memory` | Resource request for AnzoUnstructured leader container, memory size in MB | 4194M |
| `spec.anzoMicroservicesLeader.resources.limits.cpu` | Resource limit for AnzoUnstructured leader container, number of CPUs | 16000m |
| `spec.anzoMicroservicesLeader.resources.limits.memory` | Resource limit for AnzoUnstructured leader container, memory size in MB | 50331M |
| `spec.anzoMicroservicesLeader.tolerations` | AnzoUnstructured leader pod tolerations | |
| `spec.anzoMicroservicesLeader.affinity` | AnzoUnstructured leader pod node affinity | |
| `spec.anzoMicroservicesLeader.bootProperties` | AnzoUnstructured leader specific boot properties | |
| `spec.anzoUnstructuredWorker.image.registry` | Docker image registry for AnzoUnstructured worker image | docker.io |
| `spec.anzoUnstructuredWorker.image.name` | Docker image name for AnzoUnstructured worker image | cambridgesemantics/anzo-unstructured-worker |
| `spec.anzoUnstructuredWorker.image.tag` | Docker image tag for AnzoUnstructured worker image | latest |
| `spec.anzoUnstructuredWorker.image.pullPolicy` | Docker image pull policy for AnzoUnstructured worker image | IfNotPresent |
| `spec.anzoUnstructuredWorker.image.pullSecrets` | Docker image pull secrets for AnzoUnstructured worker image | |
| `spec.anzoUnstructuredWorker.size` | No of AnzoUnstructured worker cluster pods | 1 |
| `spec.anzoUnstructuredWorker.resources.requests.cpu` | Resource request for AnzoUnstructured worker container, number of CPUs | 4000m |
| `spec.anzoUnstructuredWorker.resources.requests.memory` | Resource request for AnzoUnstructured worker container, memory size in MB | 4194M |
| `spec.anzoUnstructuredWorker.resources.limits.cpu` | Resource limit for AnzoUnstructured worker container, number of CPUs | 16000m |
| `spec.anzoUnstructuredWorker.resources.limits.memory` | Resource limit for AnzoUnstructured worker container, memory size in MB | 50331M |
| `spec.anzoUnstructuredWorker.tolerations` | AnzoUnstructured worker pod tolerations | commented, please uncomment to add value |
| `spec.anzoUnstructuredWorker.affinity` | AnzoUnstructured worker pod node affinity | |
| `spec.anzoUnstructuredWorker.bootProperties` | AnzoUnstructured worker specific boot properties | |
| `spec.bootProperties` | Boot properties i.e. the environment variables for AnzoUnstructured CR | commented, please uncomment to add value |


## References
```https://docs.cambridgesemantics.com/```
