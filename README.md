# ADYPU-CS-AI-OPS
Repo for AIOPS solutions

#### Using Prometheus with Kubernetes

#### Introduction
This is basic Prometheus Queries (PromQL) and demonstrates how the Kubernetes architecture may be interrogated. It also introduces a simple means of stressing a cluster and demonstrates how those techniques affect the metrics being stored in the time series database.

#### Solution

1. Log in to the master node:

``
ssh cloud_user@[Public IP here]
``

2. List the home directory contents:

``
 ls -l
``

3. Verify that stress-test.yaml is present.


4. Access Prometheus from your browser:

``
 http://[Master Node Public IP address]:9090
``

Status > Targets will show you more information on the targets in the cluster.


5. You may also access the cAdvisor dashboard at the following address:

``
 http://[Worker Node Public IP Address]:8080
``

#### Use the provided PromQL queries to interrogate your cluster

###### The following are the suggested PromQL queries you may perform.

###### To measure CPU utilization:

``
node_cpu_seconds_total
``

``
irate(node_cpu_seconds_total{job="node"}[5m])
``

``
avg(irate(node_cpu_seconds_total{job="node"}[5m])) by (instance)
``

``
avg(irate(node_cpu_seconds_total{job="node",mode="idle"}[5m])) by (instance) * 100
``

``
100 - avg(irate(node_cpu_seconds_total{job="node",mode="idle"}[5m])) by (instance) * 100
``

###### To measure memory, use:

``
(node_memory_MemTotal_bytes - (node_memory_MemFree_bytes + node_memory_Cached_bytes + node_memory_Buffers_bytes)) / node_memory_MemTotal_bytes * 100
``

###### Deploy the stress test and vary its replicas to examine changes

###### In the terminal emulator session that is established to the Master Node:

###### Deploy the Stress-Test Deployment:

``
 kubectl create -f stress-test.yaml
``

###### Interrogate the number of replicas deployed:

``
 kubectl get deployments
``

###### Interrogate the pods running:

``
 kubectl get pods
``

Refresh your Prometheus graphs and scale the deployment up and down to vary metrics
Use refresh on your browser to see the time series metrics change over time.

###### Use the following command to increase and decrease the number of replicas running in the stress-test deployment.

``
 kubectl scale deployment.v1.apps/stress-test --replicas=[from 1 to 50 here]
``
