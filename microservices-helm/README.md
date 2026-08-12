# microservices Helm chart

This chart is derived from the provided deployment-service.yaml and keeps the same
images, service names, ports, environment variables, probes, and resource requests/limits.

## Install

helm upgrade --install microservices ./microservices-helm \
  -n microservices --create-namespace

## Validate before installing

helm lint ./microservices-helm
helm template microservices ./microservices-helm -n microservices
helm upgrade --install microservices ./microservices-helm \
  -n microservices --create-namespace --dry-run

## Check

kubectl get pods -n microservices
kubectl get svc -n microservices

## Frontend

The chart preserves:
- frontend Service: NodePort, port 80 -> targetPort 8080
- frontend-external Service: LoadBalancer, port 80 -> targetPort 8080

The NodePort is intentionally not hard-coded, matching the source manifest.
Kubernetes will allocate it.

## Argo CD

After pushing this chart to Git, create an Argo CD Application whose source.path
points to the directory containing this chart. Example:

spec:
  source:
    repoURL: https://github.com/YOUR_USER/YOUR_REPO.git
    targetRevision: main
    path: helm/microservices
    helm:
      releaseName: microservices
  destination:
    server: https://kubernetes.default.svc
    namespace: microservices
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
