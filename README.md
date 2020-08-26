## K8S CODE SNIPPETS AND ARGO DEPLOY RECIPE

# KUBECTL COMMON COMMANDS
1. kubectl get 
1. kubectl describe  
1. kubectl logs 
1. kubectl exec

---
# POD ACCESS
* Environment vars setup `export POD_NAME=$(kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}')`
* Validating vars `echo Name of the Pod: $POD_NAME`
* Checking API `curl http://localhost:8001/api/v1/namespaces/default/pods/$POD_NAME/proxy/`
* Executing inside the pod: `kubectl exec -ti $POD_NAME bash`

----

# KUBECTL ARGO ROLLOUT INSTALLATION
* `kubectl create namespace argo-rollouts`
* `kubectl apply -n argo-rollouts -f https://raw.githubusercontent.com/argoproj/argo-rollouts/stable/manifests/install.yaml`


-------------
# KUBECTL ARGO BLUE-GREEN DEPLOY
* sample app: `argocd app create --name blue-green --repo https://github.com/argoproj/argocd-example-apps --dest-server https://kubernetes.default.svc --dest-namespace default --path blue-green` 
* `argocd app sync blue-green`
* trigger: `argocd app set blue-green -p image.tag=0.2 && argocd app sync blue-green`
* rollout: `argocd app patch-resource blue-green --kind Rollout --resource-name blue-green-helm-guestbook --patch '{ "status": { "verifyingPreview": false } }' --patch-type 'application/merge-patch+json'`

# References
1. [Argo Rollout Installation](https://github.com/argoproj/argo-rollouts#installation)
1. [Argo CD Blue-green Sample](https://github.com/argoproj/argocd-example-apps/tree/master/blue-green)
