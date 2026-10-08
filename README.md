# Student Task Manager

A simple web application built for a DevOps mini project demonstrating:

**Web Application → Docker → Kubernetes → Public Web Application**

## Live Application

The Student Task Manager is available online at:

**https://student-task-2026.netlify.app/**

The application allows users to manage student tasks through a simple web interface.

> **Note:** The Netlify URL is the current public web URL for the application. The Kubernetes deployment is used to demonstrate containerization and orchestration as part of the DevOps project.

---

##  What the Application Does

Student Task Manager is a simple task management application that lets you:

- View task statistics
  - Total tasks
  - Completed tasks
  - Pending tasks
- Add new tasks with a title and description
- Mark tasks as completed or pending
- Delete tasks
- Search tasks by title
- Start with 5 sample tasks related to a DevOps assignment

---

##  Tech Stack

- **Frontend:** React + TypeScript
- **Build Tool:** Vite
- **Styling:** Tailwind CSS
- **Icons:** Lucide React
- **Containerization:** Docker
- **Web Server:** Nginx
- **Container Orchestration:** Kubernetes
- **Local Kubernetes Cluster:** Minikube
- **Public Web Hosting:** Netlify

---

##  How to Install Dependencies

Clone or download the project and navigate to the project directory:

bash
npm install


---

##  How to Run the Application Locally

Start the Vite development server:

bash
npm run dev


Vite will display the local URL in the terminal.

To create a production build:

bash
npm run build

To preview the production build:

bash
npm run preview


---

##  Docker

### Build the Docker Image

Build the production Docker image:

bash
docker build -t student-task-manager:1.0 .


### Run the Docker Container

Run the container:

bash
docker run -d -p 8080:80 --name student-task-manager student-task-manager:1.0


The application can then be accessed at:


http://localhost:8080


### Stop the Container

bash
docker stop student-task-manager


### Remove the Container

bash
docker rm student-task-manager


---

#  Kubernetes Deployment

The application can be deployed to a Kubernetes cluster using Minikube.

The Kubernetes manifests are located in the `kubernetes/` directory.

| File | Purpose |
|---|---|
| `deployment.yaml` | Deploys 2 replicas of the application |
| `service.yaml` | Exposes the application using a NodePort service |

## Kubernetes Deployment Details

- **Replicas:** 2
- **Container Port:** 80
- **Service Type:** NodePort
- **NodePort:** 30080
- **Target Port:** 80
- **Web Server:** Nginx
- **Readiness Probe:** HTTP check on `/`
- **Liveness Probe:** HTTP check on `/`

Two replicas are used to demonstrate Kubernetes workload replication and availability.

---

##  Deploy Using Minikube

### 1. Start Minikube

bash
minikube start --driver=docker


### 2. Verify the Kubernetes Node

bash
kubectl get nodes


The Minikube node should show:


STATUS
Ready


### 3. Load the Docker Image into Minikube

If the image has already been built locally:

bash
minikube image load student-task-manager:1.0


Verify that the image is available:

bash
minikube image ls


---

### 4. Apply the Kubernetes Deployment

bash
kubectl apply -f kubernetes/deployment.yaml


### 5. Apply the Kubernetes Service

bash
kubectl apply -f kubernetes/service.yaml


---

##  Verify the Kubernetes Deployment

Check the deployment:

bash
kubectl get deployment

Expected result:


NAME                   READY
student-task-manager   2/2


Check the pods:

bash
kubectl get pods


The two application pods should have:


READY   STATUS
1/1     Running
1/1     Running


Check the service:

bash
kubectl get services


The service should show:


80:30080/TCP


---

## Access the Kubernetes Application

Because Minikube is running locally, use the following command to obtain the accessible Minikube URL:

bash
minikube service student-task-manager-service --url


This may return a local URL such as:


http://127.0.0.1:53100


Open the returned URL in your browser.

### Temporary Public Access

For demonstration purposes, the local Kubernetes service can be exposed through a tunneling service.

For example, using Cloudflare Tunnel:

bash
cloudflared tunnel --url http://127.0.0.1:53100


This generates a temporary public HTTPS URL that can be shared for demonstration purposes.

> The temporary tunnel works only while Minikube and the tunnel are running.

---

##  Application Architecture


                 User
                  │
                  ▼
        Student Task Manager
                  │
                  ▼
             Docker Image
                  │
                  ▼
          Kubernetes Deployment
                  │
          ┌───────┴───────┐
          ▼               ▼
       Pod 1             Pod 2
          │               │
          └───────┬───────┘
                  ▼
       Kubernetes NodePort
          Port 30080
                  │
                  ▼
             Web Browser


##  Project Structure


student-task-manager/
├── src/
│   ├── App.tsx
│   ├── main.tsx
│   ├── index.css
│   ├── types.ts
│   └── storage.ts
│
├── kubernetes/
│   ├── deployment.yaml
│   └── service.yaml
│
├── Dockerfile
├── .dockerignore
├── package.json
└── vite.config.ts
```

---

##  Health Checks

The application serves the main page at `/` and returns HTTP 200.

Kubernetes uses HTTP health checks to monitor the application.

### Readiness Probe

The readiness probe determines whether a pod is ready to receive traffic.

### Liveness Probe

The liveness probe determines whether the application is still running correctly.

These probes help Kubernetes manage unhealthy containers automatically.

---

##  Clean Up

Remove the Kubernetes resources:

bash
kubectl delete -f kubernetes/


Stop Minikube:

bash
minikube stop


##  Project Objective

The objective of this project is to demonstrate a basic DevOps workflow:

text
Develop Web Application
          ↓
      Build with Vite
          ↓
   Create Docker Image
          ↓
   Run Docker Container
          ↓
 Deploy Using Kubernetes
          ↓
     Create 2 Replicas
          ↓
 Expose Using Kubernetes Service
          ↓
      Access Application


The project demonstrates fundamental concepts of:

- Web application development
- Docker containerization
- Kubernetes deployments
- Kubernetes pods
- Kubernetes services
- Application replication
- Health checks
- DevOps deployment workflow



##  Live Application

**Student Task Manager:**  
https://student-task-2026.netlify.app/

