# Graph Studio Unstructured Operator (anzo-unstructured-operator)
## By Altair Engineering Inc.
## Supported tags

| Release | Tags                                       |
| :---:   | :---                                       |
|  3.1.0   |  3.1.0, 3.1.0-{{ build }}  |
|  2.0.7   |  2.0.7, 2.0.7-{{ build }}  |
|  2.0.6   |  2.0.6, 2.0.6-{{ build }}  |
|  2.0.5   |  2.0.5, 2.0.5-{{ build }}  |
|  2.0.3   |  2.0.3, 2.0.3-{{ build }}  |
|  2.0.2   |  2.0.2, 2.0.2-{{ build }}  |
|  2.0.1   |  2.0.1, 2.0.1-{{ build }}  |
|  2.0.0   |  2.0.0, 2.0.0-{{ build }}  |
|  1.1.0   |  1.1.0, 1.1.0-{{ build }}  |
## About Graph Studio Unstructured
Graph Studio Unstructured solution has two main parts:
### Microservice Leader
The Microservice Leader works in concert with the Graph Studio Agent and Graph Studio Unstructured to perform ingestion of unstructured data into RDF suitable for use in an Graph Studio Data Fabric. The Microservice Leader provides queuing and coordination functions for a cluster of Graph Studio Unstructured workers. Ingesting unstructured data with an Graph Studio Server requires the configuration and use of one or more Microservice Leader nodes.
### Graph Studio Unstructured Worker
The Graph Studio Unstructured Worker works in concert with the Microservice Leader and Graph Studio Agent to perform ingestion of unstructured data into RDF suitable for use in an Graph Studio Data Fabric. Graph Studio Unstructured supports extraction from plain text, HTML, PDF, Word, and Excel files, and creation of annotations based on regular expressions, a linked data knowledge base, or third-party annotators via a REST interface. Graph Studio Unstructured leverages Graph Studio's distributed microservice framework (via the Graph Studio Agent and Microservice Leader) to allow timely processing of large document sets. Use of Graph Studio Unstructured requires an appropriately licensed Graph Studio Server installation.

**Project Status** stable

**Operator Version** v1
## Prerequisites
* Kubernetes cluster, versions {1.31-1.25}
* Kubectl, versions {1.31-1.25}

## Setting up prerequisites

## Steps to deploy anzo-unstructured-operator

**NOTE**: You must get following docker images for deployment

### Graph Studio Unstructured Operator

* Path of private GCR repository: ```https://hub.docker.com/r/cambridgesemantics/unstructured-operator```

* Command to download the latest release, please use:

    ```sh
   docker pull cambridgesemantics/unstructured-operator
   ```

### Graph Studio Unstructured Leader

* Path of private GCR repository:: ```https://hub.docker.com/r/cambridgesemantics/anzo-microservices-leader```

* Command to download the latest release, please use:

    ```sh
   docker pull cambridgesemantics/anzo-microservices-leader
   ```
### Graph Studio Unstructured Workers

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

For deploying Graph Studio Unstructured operator and clusters managed by it, user needs to enable RBAC and configure kubernetes objects mentioned below.

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
# Setup the CRD
$ kubectl create -f deploy/crds/anzo-unstructured.clusters.cambridgesemantics.com_anzo-unstructureds_crd.yaml
# Deploy anzo-unstructured-operator
$ kubectl create -f deploy/default_apps_v1_deployment_unstructured-operator.yaml --namespace <namespace>
# Deploy Graph Studio Unstructured Custom Resource(CR), i.e. Graph Studio Unstructured cluster deployment (#1)
# NOTE: This CR is designed for images in Red Hat registry. Skip this step to deploy CR for different environment.
$ kubectl apply -f deploy/crds/default_anzounstructured.clusters.cambridgesemantics.com_v1_anzounstructured_au01.yaml --namespace <namespace>
# Deploy Graph Studio Unstructured Custom Resource(CR) (#2)
$ kubectl apply -f config/samples/anzounstructured_v1_anzounstructured.yaml --namespace <namespace>
```
**NOTE** One needs to edit deploy/default_apps_v1_deployment_unstructured-operator.yaml, with right docker image details.

## Steps to delete Graph Studio Unstructured CR and anzo-unstructured-operator
```sh
# Delete Graph Studio Unstructured CR (#1)
kubectl delete -f deploy/crds/default_anzounstructured.clusters.cambridgesemantics.com_v1_anzounstructured_au01.yaml --namespace <namespace>
# Delete Graph Studio Unstructured CR (#2)
kubectl delete -f config/samples/anzounstructured_v1_anzounstructured.yaml --namespace <namespace>
# Delete anzo-unstructured-operator
kubectl delete -f deploy/default_apps_v1_deployment_unstructured-operator.yaml --namespace <namespace>
# Delete RBAC
kubectl delete -f deploy/default_rbac.authorization.k8s.io_v1_role_unstructured-operator.yaml --namespace <namespace>
kubectl delete -f deploy/default_rbac.authorization.k8s.io_v1_rolebinding_unstructured-operator.yaml --namespace <namespace>
kubectl delete -f deploy/rbac.authorization.k8s.io_v1_clusterrole_unstructured-operator.yaml
kubectl delete -f deploy/rbac.authorization.k8s.io_v1_clusterrolebinding_unstructured-operator.yaml
# Delete Service Account
kubectl delete -f deploy/default_v1_serviceaccount_unstructured-operator.yaml --namespace <namespace>
# Delete CRD
kubectl delete -f deploy/crds/anzo-unstructured.clusters.cambridgesemantics.com_anzo-unstructureds_crd.yaml
```
## Graph Studio Unstructured CustomResource(CR) Specification
The following table lists the configurable parameters for Graph Studio Unstructured and their default values.(CR API Version: v2)
| Parameter | Description | Default |
|-----------|-------------|---------|
| `metadata.name` | Name of CR | au01 |
| `metadata.namespace` | Namespace of CR | |
| `metadata.labels` | Dictionary of (key: val) as labels of CR | |
| `spec.volumes` | List of persistent volumes for Graph Studio Unstructured | commented, please uncomment to add value |
| `spec.volumes.[i].name` | Name for persistent volume | |
| `spec.volumes.[i].mountPath` | Path where persistent volume should be mounted inside container | |
| `spec.volumes.[i].pv` | Attributes to configure persistent volume, of type v1.PersistentVolume | |
| `spec.volumes.[i].pvc` | Attributes to configure persistent volume claim, of type v1.PersistentVolumeClaim | |
| `spec.msLeader.nodeConfig.spec` | Configuration specification for Graph Studio Unstructured Leader pods | |
| `spec.msLeader.nodeConfig.spec.replicas` | Number of pods for Graph Studio Unstructured Leader | 1 |
| `spec.msLeader.nodeConfig.spec.serviceName` | Name of headless service for Graph Studio Unstructured | au-<metadata.name>-ms |
| `spec.msLeader.nodeConfig.spec.template.spec.serviceAccountName` | Service account name for pods | unstructured-operator |
| `spec.msLeader.nodeConfig.spec.template.spec.containers.x.Name` | Name of Graph Studio Unstructured Leader container | ms |
| `spec.msLeader.jvmMemory` | Graph Studio Unstructured leader JVM memory | |
| `spec.msLeader.bootProperties` | Graph Studio Unstructured leader specific boot properties | |
| `spec.auWorker.nodeConfig.spec` | Configuration specification for Graph Studio Unstructured Worker pods | |
| `spec.auWorker.nodeConfig.spec.replicas` | Number of pods for Graph Studio Unstructured Worker | 1 |
| `spec.auWorker.nodeConfig.spec.serviceName` | Name of headless service for Graph Studio Unstructured | au-<metadata.name>-w |
| `spec.auWorker.nodeConfig.spec.template.spec.serviceAccountName` | Service account name for pods | unstructured-operator |
| `spec.auWorker.nodeConfig.spec.template.spec.containers.x.Name` | Name of Graph Studio Unstructured Worker container | w |
| `spec.auWorker.bootProperties` | Graph Studio Unstructured worker specific boot properties | |
| `spec.auWorker.jvmMemory` | Graph Studio Unstructured worker JVM memory | |
| `spec.bootProperties` | Boot properties i.e. the environment variables for Graph Studio Unstructured CR | commented, please uncomment to add value |


## References
```https://docs.cambridgesemantics.com/```
