# Database

Relational database using [Postgres](https://www.postgresql.org/).

Runs on a single node, but can be replicated using [StatefulSets](https://kubernetes.io/docs/tasks/run-application/run-replicated-stateful-application/). This will be easier to do locally once Kubernetes supports dynamic provisioning of volumes on local setups. 

## Getting started

### Prerequisites

* A Kubernetes cluster with kubectl access

### Deploy Postgres

Create a persistent volume for data:

```
kubectl apply -f postgres-pvc.yaml
```

Store credentials as Secret:

```
kubectl create secret generic db-secret --from-literal=username=postgres --from-literal=password=<YOUR PASSWORD>
```

Deploy Postgres:

```
kubectl apply -f postgres-deployment.yaml
```

Assert that the pod is running:

```
kubectl get pods -l app=postgres
```

### Accessing Postgres

The preceding YAML file creates a service that allows other Pods in the cluster to access the database. The Service option ```clusterIP: None``` lets the Service DNS name resolve directly to the Pod’s IP address. This is optimal when you have only one Pod behind a Service and you don’t intend to increase the number of Pods.

### Deploying Adminer

[Adminer](https://www.adminer.org/) is a lightweight tool for managing content in Postgres databases. Deploy by executing the command:

```
kubectl apply -f adminer-deployment.yaml
```
