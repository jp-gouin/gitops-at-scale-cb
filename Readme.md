# GitOps at scale for cluster bootstraping

This repository host the example mentioning in the article [GitOps at scale - clusters bootstraping](https://medium.com/@jp-gouin/gitops-at-scale-clusters-bootstrapping-f36695d4340d)

## Cluster deployment
Under `kind/` you'll find 4 clusters definition
1. bootstrap cluster where to install ArgoCD and applicationSet
2. staging cluster
3. qa cluster
4. prod cluster

Each kind cluster should expose the `kubeAPI` for make sure to update `apiServerAddress` in each file

## ArgoCD installation
Install ArgoCD in the bootstrap cluster following the [documentation](https://argo-cd.readthedocs.io/en/stable/getting_started/)

Register all clusters into ArgoCD using `argocd` cli : 

```sh
argocd cluster add kind-gitops-staging
argocd cluster add kind-gitops-qa
argocd cluster add kind-gitops-prod
```

## Application deployment
On the bootstrap cluster : 

Edit the `infra-application-set.yaml` file , update the cluster list with the IP of your kind clusters

Apply the `ApplicationSet`
```sh
kubectl apply -f infra-application-set.yaml
```

## Forking this repo
I recommend to fork this repo in your playground. 

You'll have to edit all application to target your git repo : 
- `envs/staging/staging-apps/template/application-cert-manager.yaml`
- `envs/staging/staging-apps/template/application-nginx-ingress-controller.yaml`
- `envs/prod/prod-apps/template/application-cert-manager.yaml`
- `envs/prod/prod-apps/template/application-nginx-ingress-controller.yaml`
- `envs/qa/qa-apps/template/application-cert-manager.yaml`
- `envs/qa/qa-apps/template/application-nginx-ingress-controller.yaml`

And the `infra-application-set.yaml` file