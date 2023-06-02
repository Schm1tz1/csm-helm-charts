# Helm Chart for Confluent Service Mesh (CSM)
This is an example HELM chart that deploys CSM as a common service (not a sidecar) in a fail-safe, replicated way on Kubernetes.

Covered by this example:
- Mounts a secret as a volume with CSM confguration
- Automatically creates services and mappings for all specified ports in range
- Kubernetes Probes 
- Service Load-Balancers
- Example for HPA with 80% pod CPU utilization threshold

Not covered by this example:
- CSM configuration itself
- Detailed ingress configuration

## Example Configurations provided

|Filename|Description|
|---|---|
|`values-example-LB.yaml`|Simple exanmple for a fail-safe setup with 2 CSM replicas and load-balancer|
|`values-example-LB-HPA.yaml`|Example with and load-balancer and autoscaling using HPA (horizontal pod autoscaler) that will spin up additional pods above 80% CPU utilization|

## How to use - example deployment / commands
Pich and adapt one values-*.yaml to your needs, mount your CSM configuration and you are ready to start !

Example:
```bash
# Create secret from config
kubectl create secret generic \
    csm-service-properties \
    --from-file=csm.properties=./your-csm-config.properties

# Deploy HELM Chart
helm install csm-helm-test csm_service/ --values csm-service/values.yaml

# Watch the events while the deployment starts
kubectl get events -A -L app.kubernetes.io/instance=csm-helm-test -w

# Once running, check the logs of CSM
kubectl logs -l app.kubernetes.io/instance=csm-helm-test --all-containers -f

# Perform Helm tests against the CSM and healthcheck service-ports
helm test csm-helm-test
```


