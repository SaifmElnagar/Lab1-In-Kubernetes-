

---

# Kubernetes Resource Requests and Limits

## Introduction
Kubernetes uses **requests** and **limits** to manage CPU and memory resources for containers. This ensures efficient utilization of node resources, and guarantees that containers receive the resources they need while preventing resource overconsumption.

## Requests
Requests specify the **minimum** CPU and memory a container requires. They are used by the Kubernetes scheduler to determine where a pod can be placed.
```yaml
resources:
  requests:
    cpu: "250m"
    memory: "64Mi"
```

### Key Points:
- **CPU Request**: Minimum CPU in millicores (`250m` = 0.25 CPU).
- **Memory Request**: Minimum memory in megabytes (`64Mi`).

## Limits
Limits define the **maximum** CPU and memory a container can consume. If the container exceeds the CPU limit, it will be throttled. If it exceeds the memory limit, it may be terminated.
```yaml
resources:
  limits:
    cpu: "500m"
    memory: "128Mi"
```

### Key Points:
- **CPU Limit**: Maximum CPU in millicores (`500m` = 0.5 CPU).
- **Memory Limit**: Maximum memory in megabytes (`128Mi`).

## Requests vs. Limits
| **Parameter**  | **Requests**                   | **Limits**                      |
|----------------|--------------------------------|---------------------------------|
| **Purpose**    | Minimum guaranteed resources   | Maximum allowed resources       |
| **Usage**      | Used for scheduling            | Enforced during runtime         |
| **Effect**     | Ensures container placement    | Throttling (CPU) or termination (memory) when exceeded |

## Example Pod Specification
Here’s an example of a pod with requests and limits defined:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: my-container
    image: nginx
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "128Mi"
        cpu: "500m"
```

## Resource Overcommitment
In Kubernetes, you can **overcommit** resources by requesting fewer resources than the limit. However, this can lead to performance issues if the container reaches its limit.

## Resource Quotas
Resource quotas can be used to limit resource consumption across a namespace:
```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: namespace-quota
  namespace: my-namespace
spec:
  hard:
    requests.cpu: "2"
    requests.memory: "1Gi"
    limits.cpu: "4"
    limits.memory: "2Gi"
```

## Conclusion
Defining resource requests and limits helps balance resource usage across nodes, ensures containers get the necessary resources, and prevents overconsumption that can lead to system instability.

--- 


