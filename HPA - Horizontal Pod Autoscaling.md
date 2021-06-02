# Introduction
Many workloads have a dynamic nature that varies over time and makes it difficult to have a fixed configuration for incoming workloads. But cloud native technologies like Kubernetes allow you to build applications that adapt to changing loads. Kubernetes Autoscaling allows us to define a variable capacity for an application, which is not fixed, but rather guarantees enough capacity to handle a different load. The most direct approach to achieving such behavior is by using a Horizontal Pod Autoscaler (HPA) to horizontally scale the number of pods in the architecture.



## How to work? 
Horizontal Pod Autoscaling (HPA) scales the number of pod replicas. Most of their implementations use CPU and memory as factors in deciding whether to increase or decrease the number of replicas.


**Autoscaling**
A method used in cloud computing, whereby the amount of computational resources, generally measured in terms of the number of active servers, is automatically scaled based on the required load.

**Pod**
Group of one or more containers, with shared storage / network, and a specification on how to run the containers.

<p align="center">
<img height="400" src="https://user-images.githubusercontent.com/13514156/120510340-f2ba4c80-c38e-11eb-9b8c-31bd9930331a.png">
</p>

<p align="center">
<img height="400" src="https://user-images.githubusercontent.com/13514156/120510345-f352e300-c38e-11eb-8597-a02a71afcb7c.png">
</p>

<p align="center">
<img height="400" src="https://user-images.githubusercontent.com/13514156/120510347-f352e300-c38e-11eb-99ff-ff09ac7cb8ba.png">
</p>

The HPA is implemented as a control loop. Periodically check the CPU utilization of the pods you are targeting. (The HPA period is controlled by the controller prompt: horizontal-pod-autoscaler-sync period. The default is 30 seconds). Then, it compares the arithmetic mean of the CPU utilization of the pods with the goal of adjusting the number of replicas if necessary.

## Use CPU:

The current CPU usage of a pod divided by the sum of CPU requested by the pod's containers.

The autoscaler accesses the corresponding replication controller or deployment by sub-resource of Scale. Scale is an interface that allows you to dynamically set the number of replicas and know their current state.

- HPA continuously verifies the values ​​of the metrics that were configured, by default it is in 30sec intervals. This can be configured via the controller driver prompt -horizontal-pod-autoscaler-sync period
- HPA tries to increase the number of pods if the specified threshold is met. Deployment Controller would create the number of pods needed
- HPA updates the number of replicas within the deployment or replication controller
- HPA waits 3 minutes after the last scaling events to allow the metrics to stabilize.

Deployment yaml
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: #{service}#-deployment
  namespace: #{namespace}#
spec:
  replicas: #{replicas}#
  selector:
    matchLabels:
      pod: #{service}#-pod
  template:
    spec:
      containers:
          resources:
            requests:
                cpu: #{requests-cpu}#
                memory: #{requests-memory}#
            limits:
                cpu: #{requests-cpu-max}#
                memory: #{requests-memory-max}#
```


**Requests**
  - **cpu**: Amount of CPU necessary for the correct operation of the pod, this is defined by doing tests with the following command, while doing stress tests:
    watch -n 1 kubectl top pod id
  - **memory**: Amount of memory necessary for the correct operation of the pod

**Limits**
  - **cpu**: Maximum amount of CPU that the pod can consume, it can affect the correct operation of the HPA.
  - **memory**: Maximum amount of memory that the pod can consume.

HPA yaml
```yaml
apiVersion: autoscaling/v1
kind: HorizontalPodAutoscaler
metadata:
  name: #{service}#-hpa
  namespace: #{namespace}#
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: #{service}#-deployment
    minReplicas: #{replicas}#
    maxReplicas: #{max-replicas}#
    targetCPUUtilizationPercentage: #{target-CPU}#
```

- **minReplicas**: At least 3 replicas must be defined to ensure the high availability of resources

- **maxReplicas**: This is defined according to business requirements.

- **targetCPUUtilizationPercentage**: The CPU percentage must be defined according to the needs of each project. Note that for this value the limit is not 100%, that is, there may be cases where a value of 150% makes sense.
