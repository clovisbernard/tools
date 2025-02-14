curl -sSL -o argocd https://github.com/argoproj/argo-cd/releases/latest/download/argocd-darwin-amd64
chmod +x argocd
sudo mv argocd /usr/local/bin/
argocd version
kubectl port-forward svc/argocd-server -n argocd 8080:443
argocd login localhost:8080
kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 -d
argocd login localhost:8080 --username admin --password knJtU



argocd app create app \
--repo https://github.com/clovisbernard/tools.git \
--path k8s/appli/001-NodePort \
--dest-namespace app \
--dest-server https://kubernetes.default.svc