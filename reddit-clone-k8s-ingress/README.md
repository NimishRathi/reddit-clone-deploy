# Reddit Clone App on Kubernetes with Ingress
This project demonstrates how to deploy a Reddit clone app on Kubernetes  and expose it to the world 

## Installation
Follow these steps to install and run the Reddit clone app on your local machine:

1) Clone this repository to your local machine: `git clone
2) https://github.com/NimishRathi/reddit-clone-deploy.git
3) Navigate to the project directory: `cd reddit-clone-k8s-ingress`
4) Build the Docker image for the Reddit clone app: `docker build -t reddit-clone-app .`
5) Deploy the app to Kubernetes: `kubectl apply -f deployment.yaml`
1) Deploy the Service for deployment to Kubernetes: `kubectl apply -f service.yaml`` or
6) Expose the app as a Kubernetes service: `kubectl expose deployment reddit-deployment --type=NodePort --port=3000`

## Step 1: Clone the source code
The first step is to clone the source code for the app. You can do this by using this command 
git clone https://github.com/NimishRathi/reddit-clone-deploy.git

## Step 2: Containerize the Application using Docker
Write a Dockerfile with the following code:


FROM node:19-alpine3.15

WORKDIR /reddit-clone

COPY . /reddit-clone

RUN npm install 

EXPOSE 3000

CMD ["npm","run","dev"]


## Step 3) Building Docker-Image
cd reddit-clone-deploy 
Now it's time to build Docker Image from this Dockerfile. docker build .  
use this command to build a docker image.

Step 4) Push the Image To DockerHub
Now push this Docker Image to DockerHub so our Deployment file can pull this image & run the app in Kubernetes pods.

# First login to your DockerHub account using Command i.e
docker login 
and give your username & password.

# tag the image using command 
docker  tag <image-name> <dockerhub-username>/<repo-name>:tag  

# Then use 
docker push <DockerHub_Usernam>/<tagged-Imagename>:<actual-image-name> for pushing to the DockerHub.
You can use an existing docker image i.e  for deployment.
nimishrathi12/clone-of-redit


## Step 5) Write a Kubernetes Manifest File
Write Deployment.yml file

apiVersion: apps/v1

kind: Deployment

metadata:

  name: reddit-clone-deployment
  
  labels:
  
    app: reddit-clone
    
spec:

  replicas: 2
  
  selector:
  
    matchLabels:
    
      app: reddit-clone
      
  template:
  
    metadata:
    
      labels:
      
      app: reddit-clone
      
    spec:
    
      containers:
      
      - name: reddit-clone
      
        image: nimishrathi12/clone-of-redit
        
        ports:
        
        - containerPort: 3000

##        Write Service.yml file
        apiVersion: v1
        
kind: Service

metadata:

  name: reddit-clone-service
  
  labels:
  
    app: reddit-clone
spec:

  type: NodePort
  
  ports:
  
  - port: 3000
    
    targetPort: 3000
    
    nodePort: 31000
    
  selector:
  
    app: reddit-clone

  ##  Expose the app


# you can also see the deployed application on Ec2_ip:3000
# Make sure you open the 3000 port in a security group of your Ec2 Instance.




