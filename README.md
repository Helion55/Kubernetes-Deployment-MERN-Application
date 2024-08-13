# Deploying The Application 

![Diagram](https://github.com/Helion55/Kubernetes-Deployment-MERN-Application/blob/main/Kubernetes-Deployment-MERN-Application-Diagram.jpg?raw=true)

1. **Creating Dockerfiles**
   - Backend application have its own Dockerfile in backend directory.
   - Use command " docker build -t image-name . "
   - Frontend application also have its own Dockerfile in frontend directory.
   - Use command " docker build -t image-name . "

3. **Running Containers With Docker-Compose**
   - Use command " docker-compose up " to to create (if images are not created) and run all the containers, a network will connect all of them automatically.

4. **Running on Kuberntes Cluster**
   - As docker images are build and pushed to Dockerhub Kubernetes Deployment files will pull them.
   - Setup your Kubernetes Cluster (for locally use Minikube) 
   - Use command " kubectl apply -f database.yaml "
   - Use command " kubectl apply -f backend.yaml "
   - Use command " kubectl apply -f frontend.yaml "
   - Use command " kubectl apply -f frontservice.yaml "
   - database.yaml backend.yaml frontend.yaml have both Deployment and Service components.
   - To get expose the website in minikube Nodeport is used (if Domain-name is present then LoadBalancer type is used) and frontservice.yaml is the NodePort type service so it is created separately for convenience.
   - minikube will assign Ip address to this service , use "minikube service service-name --url" to get the address
   - Open your browser and go to that ip with port 30005, Your Website is ready....!

