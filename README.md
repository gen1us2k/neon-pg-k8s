# neon-pg-k8s
Running Neon Serverless postgres in k8s

## Cloning repo

```
git clone git@github.com:gen1us2k/neon-pg-k8s
cd neon-pg-k8s
```

## Spinning up minikube cluster

You can spin-up kind/minikube/k3d or any other cluster to check this out. I will use `minikube` for this showcase

```
minikube start --nodes=3
```

## Running storage broker

```
kubectl apply -f storagebroker.yml
```

Wait until a pod will be up and running. It creates deployment and service

## Running safekeepers
The example uses statefulset for safekeepers (I'm not sure that we need it but they group together using Paxos)

```
kubectl apply -f safekeepers.yaml
```

it will run 3 safekeeper pods and create a service

## Running pageserver

```
kubectl apply -f pageserver.yml
```

It creates deployment for pageserver and a service

## Running minikube tunnel

To make it easier to connect to compute nodes (we will deploy them later) you need to run 

```
minikube tunnel
```
because compute service uses `LoadBalancer` expose type

## Running compute nodes

The example deploys 5 nodes (Autoscaling and scaling to zero in the roadmap)

```
kubectl apply -f compute.yml
```

## Connecting to the pg cluster

from your local machine you can use psql to connect to the cluster
```
psql -h 127.0.0.1  -U cloud_admin postgres
```
