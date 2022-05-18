# NXTP Router helm

Production-ready helm for NXTP routers.

*This example helm deployment assumes that the cluster already has a monitoring stack installed

## Router Setup Using helm

### Requirements

- [ Kubernetes Cluster ](https://kubernetes.io/) version 1.18 or above
- [ Helm ](https://helm.sh/docs/intro/install/) version 3

### Run helm deployment

1. Clone repo

```
cd ~
git clone https://github.com/connext/nxtp-router-helm.git
```


2.  Edit `values.yaml` file.

Under config fill the config.json needed for the router:

```sh
config: |-
      {
        "network":"testnet",
        "logLevel": "debug",
        "web3SignerUrl": "http://0.0.0.0:9000",
        "sequencerUrl": "https://sequencer.testnet.staging.connext.ninja",
        "redis": {
          "host": "redis-master",
          "port": 6379
        },
        "server": {
          "adminToken": "blablabla",
          "port": 8080
        },
        "chains": {
          "2000": {
            "providers": ["https://rinkeby.infura.io/v3/38f8f85747014e87b48035d84398a97c"],
            "assets": [
              {
                "name": "TEST",
                "address": "0x3CF0A8545bF9a768d019baea96EC19573FFE0665"
              }
            ]
          },
          "3000": {
            "providers": ["https://kovan.infura.io/v3/38f8f85747014e87b48035d84398a97c"],
            "assets": [
              {
                "name": "TEST",
                "address": "0x3CF0A8545bF9a768d019baea96EC19573FFE0665"
              }
            ]
          }
        }
      } 
```
*This is just an example for the configuration file of the router

See [Connext docs](https://docs.connext.network/Routers/configuration) for configuration description.

** To configure web3signer just add in values.yaml the private key of the wallet you want to use
```
web3signer: <signer>
```

3. Inside the cloned repository directory, run:

```
helm install . --generate-name -n <namespace>
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

4. Check if the deployment was successful

```
kubectl get pods
```

5. Check the logs.

```
kubectl logs <router-pod>
```


6. Stop and delete the deployment.

```
helm uninstall <RELEASE_NAME> -n <namespace>
```

The output should look like below:
```
release "chart-<random number>" uninstalled
```

## Other Tasks


### Update Version

1. Modify `values.yaml` to change `tag: v0.1.34` to the lastest image version
2. Check your chart name

```
helm list -n <namespace>
```

3. Update helm deployment using the following commands:

```
helm upgrade <chart-name> ./ -f values.yaml -n <namespace>
```
