# flux-demo
## Ran the given commands:
 mkdir -p clusters/my-cluster/flux-system
touch clusters/my-cluster/flux-system/gotk-components.yaml \
    clusters/my-cluster/flux-system/gotk-sync.yaml \
    clusters/my-cluster/flux-system/kustomization.yaml
## Update the kustomization.yaml 

apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
resources:
- gotk-components.yaml
- gotk-sync.yaml

## Ran the given command:      
git add -A && git commit -m "init flux" && git push
## Run the bootstrap for clusters/my-cluster:
 flux bootstrap github \
  --token-auth \
  --owner=bansodashish2511 \
  --repository=flux-demo \
  --branch=main \
  --path=clusters/my-cluster \
  --personal
  To make further amendments, pull the changes locally, edit the kustomization.yaml file, push the changes upstream and rerun bootstrap.
