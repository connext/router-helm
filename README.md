# Amarok Router helm

Production-ready helm for Amarok routers.

*This example helm deployment assumes that the cluster already has a monitoring stack installed

This helm contains:
- Redis
- Web3signer
- Router executer
- Router publisher
- Router subscriber

## Router Setup Using helm
### Requirements

- [ Kubernetes Cluster ](https://kubernetes.io/) version 1.18 or above
- [ Helm ](https://helm.sh/docs/intro/install/) version 3
- [ Rabbitmq ](https://www.rabbitmq.com/) (we used this [helm](https://bitnami.com/stack/rabbitmq/helm) to install on our cluster)

### Run helm deployment

1. Clone repo

```
cd ~
git clone https://github.com/connext/nxtp-router-helm.git
```


2.  Edit `values-mainnet.yaml` or `values-testnet.yaml` file.

- Update the config with the proper rabbitmq endpoint.
- Remove or add networks which are you planning to support

3. Edit  `values.yaml` file.

- ```web3signer: <signer>``` change the ```<signer>``` with the private key of the signer you are planning to use

4. Inside the cloned repository directory, run:

If you are planning to run the router on testnet please use ```values-testnet.yaml```:
```
helm install . --generate-name  --values=values-testnet.yaml,values.yaml -n <namespace>
```
If you are planning to run the router on mainnet please use ```values-mainnet.yaml```:
```
helm install . --generate-name  --values=values-mainnet.yaml,values.yaml -n <namespace>
```

The output should look like below:
```
NAME: chart-<random number>
LAST DEPLOYED: <today's date>
NAMESPACE: <namespace>
STATUS: deployed
REVISION: 1
TEST SUITE: None
```

5. Check if the deployment was successful

```
kubectl get pods
```

6. Check the logs.

```
kubectl logs <router-pod>
```


7. Stop and delete the deployment.

```
helm uninstall <RELEASE_NAME> -n <namespace>
```

The output should look like below:
```
release "chart-<random number>" uninstalled
```

## Other Tasks


### Update Version

1. Modify `values.yaml` to change `tag: sha-20b6014` to the lastest image version
2. Check your chart name

```
helm list -n <namespace>
```

3. Update helm deployment using the following commands:

```
helm upgrade <chart-name> ./ -f values.yaml -n <namespace>
```