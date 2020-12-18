# unstructured-operator

## By Cambridge Semantics Inc.

## Prerequisites

* Red Hat Openshift Container Platform on Kubernetes, version >= 4.3
* Kubectl, versions {1.18-1.14}
* Anzo Unstructured Operator Subscription

## Optional Prerequisites
### Create Project(Namespace) if required
<img src="images/projects_screen.png" width="67%">
<img src="images/create-openshift-project.png" width="67%">

## Steps to deploy Anzo Unstructured Operator

#### Login to OpenShift Console:

#### Operators --> OperatorHub --> Search Unstructured Operator --> Install
<img src="images/kubeadmin-operatorhub-unstructured-search.png" width="67%">

#### Select Namespace where you want to deploy the operator and click on Install
<img src="images/kubeadmin-unstructured-operator-installdialog.png" width="67%">

#### And check that operator installation is successful

#### Operators --> Installed Operators --> Unstructured Operator --> Create Instance
<img src="images/kubeadmin-create-unstructured.png" width="67%">

#### Use either Form View or YAML View to set required properties, use **Anzo Unstructured CustomResource(CR) Specification** for definitions
<img src="images/kubeadmin-install-unstructured-form.png" width="67%">
<img src="images/kubeadmin-install-unstructured-yaml.png" width="67%">

## Anzo Unstructured CustomResource(CR) Specification

The following table lists the configurable parameters for Anzo Unstructured and their default values.(CR API Version: v1beta1)

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
