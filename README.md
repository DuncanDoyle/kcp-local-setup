# KCP Setup

```export CLUSTERS_DIR=.```

```cd clusters/kind```

``` ./createKindClusters.sh```

# kcp
git clone kcp git@github.com:kcp-dev/kcp.git

# build all binary
``` make build-all```
# Start kcp
```./kcp start```

 ```export KUBECONFIG=.kcp/admin.kubeconfig```
Get workspaces
```kubectl get workspaces```
Create workspace us-east
``` kubectl kcp ws create wk-us-east ```
Create workspace us-west
```kubectl kcp ws create wk-us-west ```

# Select workspace us-east
 kubectl kcp ws wk-us-east

 # 
 ```kubectl kcp ws current```

# register the physical cluster
```
kubectl kcp workload sync us-east \
                --syncer-image ghcr.io/kcp-dev/kcp/syncer:v0.8.0 \
                --resources deployments.apps,pods,services,poddisruptionbudgets.policy,ingresses.networking.k8s.io,networkpolicies.networking.k8s.io \
                --output-file sync-us-east.yaml
```

```export KUBECONFIG=kcp-setup/clusters/kind/us-east1.kubeconfig```
``` kubectl create -f kcp/bin/sync-us-east.yaml ```

# kcp terminal check the sync target
```kubectl get synctargets.workload.kcp.dev ```

# Switch back to root workspace
```kubectl kcp ws root```
# Select the west workspace
``` kubectl kcp ws wk-us-west ```

# Select workspace us-west
 kubectl kcp ws wk-us-west
# register the physical cluster
```
kubectl kcp workload sync us-west \
                --syncer-image ghcr.io/kcp-dev/kcp/syncer:v0.8.0 \
                --resources deployments.apps,pods,services,poddisruptionbudgets.policy,ingresses.networking.k8s.io,networkpolicies.networking.k8s.io \
                --output-file sync-us-west.yaml
```
# Export kube config
```export KUBECONFIG=kcp-setup/clusters/kind/us-west1.kubeconfig ```
# Register the physical cluster in workspace wk-us-west
``` kubectl apply -f kcp/bin/sync-us-west.yaml ```
# Check
 ```kubectl get synctargets.workload.kcp.dev```