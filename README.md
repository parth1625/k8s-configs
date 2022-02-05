# k8s

## Set up Minikube on Ubuntu:
    https://phoenixnap.com/kb/install-minikube-on-ubuntu

## Set up Minikube on MacOS:
    https://matthewpalmer.net/kubernetes-app-developer/articles/guide-install-kubernetes-mac.html

## Start minikube:
    minikube start

## SSH to minikube machone:
    minikube ssh

## Build Docker image:
    docker build -t <username>/<image-name>:<tag> .

## Push Docker image to Docker Hub:
    docker login
    docker push <username>/<image-name>:<tag>

## Get Nodes list:
    kubectl get nodes
    kubectl describe nodes <node-name>

## Get Pods list:
    kubectl get pods
    kubectl describe pods <pod-name>

## Using Labels/Selector to list Pods:
    kubectl get pods --show-labels
    kubectl get pods -l <name>=<value>
    kubectl get pods --selector <name>=<value>
    kubectl get pods --selector "<name> in (<value1>,<value2>)"
    kubectl get pods --selector "<name> notin (<value1>,<value2>)"

## Get Services list:
    kubectl get service
    kubectl get svc
    kubectl describe service <service-name>

## Get Deployments list:
    kubectl get deployment
    kubectl get deploy
    kubectl describe deployment <deployment-name>

## Create Pod/Service/Deployment/PVC using manifest file:
    kubectl create -f <filename.yml>

## Delete Pod/Service/Deployment/PVC from manifest file:
    kubectl delete -f <filename.yml>

## Delete all resources:
    kubectl delete all --all

## Expose Deployment to specific port:
    kubectl expose deployment <deployment-name> --port 80 --type=NodePort
    kubectl expose deployment <deployment-name> --port 80 --type=ClusterIP
    kubectl expose deployment <deployment-name> --port 80 --type=LoadBalancer

## Perform Roll Update in Deployment:
    kubectl set image deployment <deployment-name> <container-name>=<image-name>:<tag>
    kubectl set image deployment <deployment-name> <container-name>=<image-name>:<tag> --record

## Minikube url:
    minikube ip
    minikube service <service-name>
    minikube service <service-name> --url

## Install Helm:
**Ubuntu**:
    curl https://baltocdn.com/helm/signing.asc | sudo apt-key add -
    sudo apt-get install apt-transport-https --yes
    echo "deb https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
    sudo apt-get update
    sudo apt-get install helm

**MacOS**:
    brew install helm

## Helm commands:
    helm list
    helm list --all-namespaces
    helm ls
    helm get manifest code-crow-client-prod
    helm get manifest code-crow-server-prod
    helm delete <deployment name> --namespace <namespace_name>

## Cretea K8S secrets:
    kubectl create secret generic <name> --from-literal=<key>='<value>'

## Create Docker registry secret:
    kubectl create secret docker-registry docker-reg --docker-server=https://index.docker.io/v1/  --docker-username=<DOCKER_USERNAME>  --docker-password=<DOCKER_PASSWORD>

## Horizontal Auto Scale in minikube:
    minikube addons list
    minikube addons enable metrics-server
    kubectl get pod,svc -n kube-system
    kubectl autoscale deployment <DEPLOYMENT_NAME> --cpu-percent=50 --min=1 --max=10
    kubectl get hpa

## Install Traefik Ingress Controller via Helm:
    helm repo add traefik https://helm.traefik.io/traefik
    helm repo update
    kubectl create namespace traefik
    helm install traefik traefik/traefik -n traefik
    kubectl port-forward $(kubectl get pods --selector "app.kubernetes.io/name=traefik" --output=name) 9000:9000

## Install Nginx Ingress COntroller via Helm:
    helm upgrade --install ingress-nginx ingress-nginx --repo https://kubernetes.github.io/ingress-nginx --namespace ingress-nginx --create-namespace
    kubectl --namespace ingress-nginx get services -o wide -w ingress-nginx-controller
    kubectl --namespace ingress-nginx get services

## Configure cert manager for Nginx Ingress:
    kubectl apply --validate=false -f https://github.com/jetstack/cert-manager/releases/download/v1.0.1/cert-manager.yaml             (Kubernetes version 1.16+)

    kubectl apply --validate=false -f https://github.com/jetstack/cert-manager/releases/download/v1.0.1/cert-manager-legacy.yaml      (Kubernetes <1.16 version)

## Kubernetes Nginx Ingress Controller LetsEncrypt:
    kubectl apply -f letsencrypt-issuer.yml

## Creating Nginx Ingress Let’s Encrypt TLS Certificate:
    kubectl apply -f letsencrypt-cert.yml
    kubectl get certificates <CERTIFICATE_NAME>
    kubectl get secrets <SECRET_NAME>

## Point Nginx Ingress Let’s Encrypt Certificate in Nginx Ingress:
    kubectl edit ingress nginx-ingress

#### Edit the following line in ingress file:-

    cert-manager.io/cluster-issuer: letsencrypt-prod
    tls:
    - hosts:
        - <DOMAIN_NAME>
        secretName: <SECRET_NAME>

## AWS EKS Commands:
    aws eks list-clusters
    aws eks list-nodegroups --cluster-name <CLUSTER_NAME>
    aws eks --region <REGION_NAME> update-kubeconfig --name stag-crow-k8s-cluster  --kubeconfig  /var/www/kubernetes/stag-crow-k8s-cluster.yml 
    aws eks --region <REGION_NAME> update-kubeconfig --name <CLUSTER_NAME>  --kubeconfig <YAML_FILENAME>
    kubectl get pods --kubeconfig <YAML_FILENAME>
    aws eks update-kubeconfig --name <CLUSTER_NAME> --region <REGION_NAME>
    kubectl config get-contexts
    kubectl get namespace
    kubectl delete ns <NAMEPSACE_NAME>