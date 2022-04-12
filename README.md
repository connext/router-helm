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
        "adminToken": "",
        "chainConfig": {
          "100": {
          "providers": [
            "https://rpc.xdaichain.com/",
            "https://xdai.poanetwork.dev/",
            "https://dai.poa.network/"
          ]
          },
        "2001": {
            "providers": [
              "https://rpc.c1.milkomeda.com:8545"],
            "minGas": "10000000000000"
          },
        "1": {
          "providers": [
            "https://cloudflare-eth.com"
          ]
        }
        },
        "logLevel": "info",
        "network": "mainnet",
        ## Fill mnemonic operator
        "mnemonic": "",
        ## Fill the router contract address
        "routerContractAddress": "",
        "swapPools": [
            {
            "name": "USDT",
            "assets": [
                {
              "chainId": 100,
              "assetId": "0x4ECaBa5870353805a9F068101A40E0f32ed605C6"
              },
              {
                "chainId": 2001,
                "assetId": "0xab58da63dfdd6b97eaab3c94165ef6f43d951fb2"
              }
            ]
            },   {
                "name": "USDC",
                "assets": [
                {
                  "chainId": 100,
                  "assetId": "0xDDAfbb505ad214D7b80b1f830fcCc89B60fb7A83"
                  },
                  {
                  "chainId": 2001,
                  "assetId": "0x5a955fddf055f2de3281d99718f5f1531744b102"
                  }
                ]
              }
            ]
        }  
```
*This is just an example for the configuration file of the router

See [Connext docs](https://docs.connext.network/Routers/Reference/configuration/) for configuration description.

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