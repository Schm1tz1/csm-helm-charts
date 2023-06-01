# Example of a HELM Chart for Confluent Service Mesh (CSM)
This is an example HELM chart that deploys CSM as a common service (not a sidecar) in a fail-safe, replicated way on Kubernetes.

Covered by this example:
- mounts a secret as a volume with CSM confguration
- automatically creates services and mappings for all specified ports in range
- Usage example - adapt values.yaml to your needs, have your CSM configuration ready and do:

Not covered by this example:
- CSM configuration itself
- Ingress configuration

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
```
