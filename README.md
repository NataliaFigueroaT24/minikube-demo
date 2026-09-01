# minikube-demo

# Setup
brew install minikube kubectl
minikube start --driver=docker --cpus=4 --memory=6g

# Desplegar
kubectl apply -f .
kubectl rollout status deployment/web
minikube service web --url

# ECR
kubectl create secret docker-registry ecr-creds \
  --docker-server="$REGISTRY" --docker-username=AWS \
  --docker-password="$(aws ecr get-login-password --region $AWS_REGION)"

# Iterar rápido sin registry
eval $(minikube docker-env) && docker build -t pyapp:dev .

# Depurar
kubectl describe pod -l app=web
kubectl logs -f -l app=web
kubectl get endpoints

# Limpiar
minikube delete