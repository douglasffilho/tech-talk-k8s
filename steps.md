ref: https://kubernetes.io/pt-br/docs/concepts/_print/

Como era a virtualização?
Baremetal com port forward, VM (replicação de custo em recursos compartilhados), Containers
(economia de recursos, separação de amibientes por grupos)

Pq usar kubernetes?
service discovery, load balancing, orquestração de storage, automação de deploy e releases,
controle de disponibilidade por recursos, uso de recursos controlado, autocorreção, gerenciamento
de segredos, não limita o que será executado no container, não opina sobre codigo fonte ou
processo de CI/CD, não fornece serviços de middleware a nivel de aplicação, não opina sobre
soluções de logging, monitoramento ou alertas, api declarativa

Não substitui docker, apenas cria uma maneira de lidar com containers (não necessariamente
containers docker, mas, OCI)

Criando Cluster k8s:
  - name: Install kubectl
    command: https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#install-using-native-package-management

  - name: Install K3D tools
    command: https://k3d.io/v5.3.0/#installation

  - name: Create a default loadbalanced k8s cluster "duvida - cada aplicação tem um cluster para si?"
    command: k3d cluster create newcluster && kubectl cluster-info

  - name: Create a pod from pod.yml
    command: kubectl create -f pod.yml && watch -n 1 kubectl get pods

  - name: Create a route to the pod
    command: kubectl port-forward pod/web-project 8080:80

  - name: Delete pod
    command: kubectl delete -f pod.yml

  - name: Create a replicaset from replicaset.yml
    command: kubectl apply -f replicaset.yml && watch -n 1 kubectl get pods

  - name: read replicaset status
    command: watch -n 1 kubectl get replicaset

  - name: change replicaset replicas to 2 and apply
    command: kubectl apply -f replicaset.yml && watch -n 1 kubectl get replicaset

  - name: remove replicas
    command: kubectl delete -f replicaset.yml && watch -n 1 kubectl get replicaset

  - name: create deployment from deployment file without service
    command: kubectl apply -f deployment.yml && watch -n 1 kubectl get deployment

  - name: delete deployments
    command: kubectl delete -f deployment.yml

  - name: Delete current cluster
    command: k3d cluster delete newcluster

  - name: Create a greater load balanced k8s cluster
    command: k3d cluster create newcluster --servers 1 --agents 1 -p "8080:30000@loadbalancer"

  - name: create deployment from deployment file with service
    command: kubectl apply -f deployment-with-service.yml && watch -n 1 kubectl get deployment

  - name: change deployment to have 10 replicas
    command: kubectl apply -f deployment-with-service.yml && watch -n 1 kubectl get deployment

  - name: read all endpoints
    command: kubectl get endpoints

  - name: read web project endpoints
    command: kubectl get endpoints web-project-service

  - name: change deployment to blue image to see blue green evolution
    command: kubectl apply -f deployment-with-service.yml && watch -n 1 kubectl get deployment

  - name: rollout last changes
    command: kubectl rollout undo deployment web-project-deployment && watch -n 1 kubectl get deployment

  - name: delete deployments
    command: kubectl delete -f deployment-with-service.yml

  - name: create canary deployment with less replicas
    command: kubectl apply -f deployment-canary.yml && watch -n 1 kubectl get deployment

  - name: create canary deployment with more blue replicas and less green replicas
    command: kubectl apply -f deployment-canary.yml && watch -n 1 kubectl get deployment
