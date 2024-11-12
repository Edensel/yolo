kubectl config current-context
kubectl config get-contexts
kubectl config use-context <context-name>
kubectl config view
kubectl get nodes
kubectl apply -f manifests/mongo-pvc.yml
kubectl apply -f manifests/mongo-deployment.yml
kubectl apply -f manifests/backend-deployment.yml
kubectl apply -f manifests/frontend-deployment.yml
kubectl apply -f manifests/mongo-pvc.yml --validate=false
kubectl apply -f manifests/mongo-deployment.yml --validate=false
kubectl apply -f manifests/backend-deployment.yml --validate=false
kubectl apply -f manifests/frontend-deployment.yml --validate=false
minikube ip
