# Database

Relational database using [MySQL](https://www.mysql.com/). Kubernetes deployment based on the [official guide](https://kubernetes.io/docs/tasks/run-application/run-single-instance-stateful-application/).

Runs on a single node, but can be [replicated](https://kubernetes.io/docs/tasks/run-application/run-replicated-stateful-application/) using StatefulSets. This will be easier to do locally once Kubernetes supports dynamic provisioning of volumes on local setups. 

## Getting started

### Prerequisites

* A Kubernetes cluster
* kubectl

### Deploy MySQL

Create a persistent volume for data:

```
kubectl apply -f mysql-pv.yaml
```

Store credentials as Secret:

```
kubectl create secret generic mysql-pass --from-literal=password=YOUR_PASSWORD
```

Deploy MySQL:

```
kubectl apply -f mysql-deployment.yaml
```

Assert that the pod is running:

```
kubectl get pods -l app=mysql
```

### Accessing MySQL

The preceding YAML file creates a service that allows other Pods in the cluster to access the database. The Service option ```clusterIP: None``` lets the Service DNS name resolve directly to the Pod’s IP address. This is optimal when you have only one Pod behind a Service and you don’t intend to increase the number of Pods.

### Deploying phpMyAdmin

Execute the command:

```
kubectl apply -f phpmyadmin-deployment.yaml
```

phpMyAdmin can now be accessed on ```[host]/phpmyadmin```
