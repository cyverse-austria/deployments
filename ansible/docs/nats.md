# NATS installation

To install NATS within the cluster using default values, run the following, substituting the path to the KUBECONFIG file as appropriate:

```bash
export KUBECONFIG=~/.kube/admin.conf

ansible-playbook install.yml
```

# Dowwnloading certa and creds

The best way to get an accurate version of the services.creds and TLS files are to grab them directly from the secrets and ConfigMaps used by the services. The `nats-box` pod does not keep those files around across restarts, so you may get invalid files or be fooled into thinking that the cluster hasn't been initialized if that pod has restarted at some point.

In most cases, the client and server certificate authorities will be the same, but we're showing how to download them both for completeness.

Make sure that your KUBECONFIG environment variable is set to the correct kubeconfig file. Replace the namespace as needed in each case.

Make sure you have the following tools installed:

- `kubectl`
- `jq`
- `base64`

## NATS client TLS files

On Linux:

```bash
kubectl -n qa get secrets nats-client-tls -o json | jq -r '.data["ca.crt"]' | base64 -d > nats-client-ca.crt

kubectl -n qa get secrets nats-client-tls -o json | jq -r '.data["tls.crt"]' | base64 -d > nats-tls-client.crt

kubectl -n qa get secrets nats-client-tls -o json | jq -r '.data["tls.key"]' | base64 -d > nats-tls-client.key
```

On MacOS:

```bash
kubectl -n qa get secrets nats-client-tls -o json | jq -r '.data["ca.crt"]' | base64 -D > nats-client-ca.crt

kubectl -n qa get secrets nats-client-tls -o json | jq -r '.data["tls.crt"]' | base64 -D > nats-tls-client.crt

kubectl -n qa get secrets nats-client-tls -o json | jq -r '.data["tls.key"]' | base64 -D > nats-tls-client.key
```

## NATS server TLS files

On Linux:

```bash
kubectl -n qa get secrets nats-server-tls -o json | jq -r '.data["ca.crt"]' | base64 -d > nats-server-ca.crt

kubectl -n qa get secrets nats-server-tls -o json | jq -r '.data["tls.crt"]' | base64 -d > nats-tls-server.crt

kubectl -n qa get secrets nats-server-tls -o json | jq -r '.data["tls.key"]' | base64 -d > nats-tls-server.key

```

On MacOS:

```bash
kubectl -n qa get secrets nats-server-tls -o json | jq -r '.data["ca.crt"]' | base64 -D > nats-server-ca.crt

kubectl -n qa get secrets nats-server-tls -o json | jq -r '.data["tls.crt"]' | base64 -D > nats-tls-server.crt

kubectl -n qa get secrets nats-server-tls -o json | jq -r '.data["tls.key"]' | base64 -D > nats-tls-server.key

```
