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

  ## Ran the given commands:
    mkdir charts
    cd charts
    helm create nginx

 ##  Modify the nginx/values.yaml file
     ingress:
        enabled: true # changed
        className: "nginx" # changed
        annotations: {}
        # kubernetes.io/ingress.class: nginx
        # kubernetes.io/tls-acme: "true"
        hosts:
            - host: "" # changed
            paths:
            - path: /
            pathType: ImplementationSpecific


## Create a Helm Release resource file in clusters/my-clusters path. The filename could be nginx-helm-release.yaml. The contents are as folows:

    apiVersion: helm.toolkit.fluxcd.io/v2beta1
    kind: HelmRelease
    metadata:
    name: nginx
    namespace: default
    spec:
    interval: 1m
    chart:
        spec:
        chart: ./charts/nginx
        sourceRef:
            kind: GitRepository
            name: flux-system
            namespace: flux-system
        interval: 1m  

## Force Flux CD to do a reconciliation by running the following command:

    flux reconcile kustomization flux-system --with-source

## Observe the Helm Release resource and the pods:
    kubectl get helmrelease
    kubectl get pods
