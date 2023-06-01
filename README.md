# Example of a HELM Chart for Confluent Service Mesh (CSM)
- mounts a secret as a volume with CSM confguration
- automatically creates services and mappings for all specified ports in range
- Usage example - adapt values.yaml to your needs, have your CSM configuration ready and do:

```bash
# Create secret from config
kubectl create secret generic \
    csm-service-properties \
    --from-file=csm.properties=./your-csm-config.properties

# Deploy HELM Chart
helm install csm-helm-test csm_service/ --values csm-service/values.yaml
```
