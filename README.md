# Kubernetes Deployment of a MERN stack Hotel-Booking application 

![Diagram](https://github.com/Helion55/Kubernetes-Deployment-MERN-Application/blob/main/Mern-diagram.jpg?raw=true)

## Project Overview
A fullstack Hotel Booking application made using MongoDB, ExpressJS, ReactJS and Node (MERN), is deployed on kubernetes cluster after creating Dockerfiles and testing them locally. MongoDB is running as deployment on the cluster, backend is connected with it via 
MongoDB connection string and it is running as a ClusterIP service. The frontend is using environment variable to connect with backend and it is running as a NodePort service to access it on a specific port in the cluster.

## Tech Stack
- MongoDB
- Node (ExpressJS, ReactJs)
- Docker
- Kubernetes

## Steps to Deploy
1. Creating Dockerfiles
2. Testing them with Docker-Compose
3. Running on Kubernetes Cluster

### 1. Creating Dockerfiles
Backend and Forntend applications both have their Dockerfiles on their directory. Dockerfiles are written to follow these few steps...
- Pulling the Base Image
- Creating a workdirectory
- Coping the current directory files into container's working directory
- Installing Dependencies
- Running the Application
- Exposing the Port

Using the command
```bash
docker build -t image-name .
```
inside each directory to build the images.
   
### 2. Testing them with Docker-Compose
Docker compose will pull the MongoDB image, run the frontend and backend image and will create a Docker Network to connect them.

```Dockerfile
version: "3.8"
services:
 mongo:
  image: mongo
  container_name: database
  volumes:
   - ./data/:/data/db
  restart: unless-stopped

 backend:
  build: ./backend
  container_name: backend
  ports:
   - "7000:7000"
  environment:
   MONGODB_CONNECTION_STRING: "mongodb://mongo:27017" 
   FRONTEND_URL: http://frontend:3000
  depends_on:
   - mongo

 frontend:
  build: ./frontend
  container_name: frontend
  ports:
   - "3000:3000"
   - "5173:5173"
  environment:
   VITE_API_BASE_URL: http://backend:7000
  depends_on:
   - backend
```
Running this Docker Compose file, Using the Command...
```bash
docker compose up
```

### 3. Running on Kubernetes Cluster
The Kubernetes folder is containing all the manifest files for database, backend and frontend. a separate NodePort service file is created to access the application for users in port 5173 and another ClusterIp service is
created to connect with the backend in port 3000. To get expose the website in minikube Nodeport is used, if Domain-name is present then LoadBalancer type can be used.
Applying the files using command...
```bash
kubectl apply -f Kubernetes<FOLDER-NAME>
```
In Local Deployment setup with Minikube, it will assign Ip address to this service 
```bash
minikube ip
```
on executing this command.
Now Opening browser and going to that ip with port 30005, the application is ready....🚀

