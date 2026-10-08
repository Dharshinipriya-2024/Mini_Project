# Student Task Manager

A simple web application built for a DevOps mini project demonstrating **Web Application → Docker → Kubernetes**.

## What the Application Does

Student Task Manager is a simple task management app that lets you:

- View task statistics (total, completed, pending)
- Add new tasks with a title and description
- Mark tasks as completed or pending
- Delete tasks
- Search tasks by title

The app comes pre-loaded with 5 sample tasks related to a DevOps assignment.

## Tech Stack

- **Frontend:** React + TypeScript
- **Build Tool:** Vite
- **Styling:** Tailwind CSS
- **Icons:** Lucide React

## How to Install Dependencies

```bash
npm install
```

## How to Run It Locally

```bash
npm run dev
```

The app will be available at *student-task-2026.netlify.app*

To create a production build:

```bash
npm run build
npm run preview
```

## How to Build the Docker Image

```bash
docker build -t student-task-manager:1.0 .
```

## How to Run the Docker Container

```bash
docker run -p 5000:5000 student-task-manager:1.0
```

The app will be available at **http://localhost:5000**

## Kubernetes Deployment

The application can be deployed to a Kubernetes cluster (e.g., Minikube).

### Configuration

The Kubernetes manifests are in the `kubernetes/` directory:

| File | Purpose |
|------|---------|
| `deployment.yaml` | Deploys 2 replicas of the application |
| `service.yaml` | Exposes the app on NodePort 30001 |

### Deployment Details

- **Replicas:** 2 (for high availability)
- **Container Port:** 5000
- **Service Type:** NodePort (accessible on port 30001)
- **Liveness Probe:** Checks `http://localhost:5000/` every 30 seconds
- **Readiness Probe:** Checks `http://localhost:5000/` every 10 seconds
- **Resource Limits:** 250m CPU / 256Mi memory per pod

### Deploy Steps (Minikube)

1. Start Minikube:

```bash
minikube start
```

2. Point your local Docker to Minikube's daemon:

```bash
eval $(minikube docker-env)
```

3. Build the image inside Minikube:

```bash
docker build -t student-task-manager:1.0 .
```

4. Apply the Kubernetes manifests:

```bash
kubectl apply -f kubernetes/
```

5. Check the deployment:

```bash
kubectl get pods
kubectl get services
```

6. Access the app:

```bash
minikube service student-task-manager
```

Or open **http://<minikube-ip>:30001** in your browser.

### Clean Up

```bash
kubectl delete -f kubernetes/
minikube stop
```

## Project Structure

```
student-task-manager/
├── src/
│   ├── App.tsx          # Main application component
│   ├── main.tsx         # React entry point
│   ├── index.css        # Global styles
│   ├── types.ts         # Task type definition
│   └── storage.ts       # Local storage + sample data
├── kubernetes/
│   ├── deployment.yaml  # Kubernetes Deployment (2 replicas)
│   └── service.yaml     # Kubernetes Service (NodePort)
├── Dockerfile           # Multi-stage Docker build
├── .dockerignore        # Docker ignore file
├── package.json         # Dependencies and scripts
└── vite.config.ts       # Vite configuration (port 5000)
```

## Health Check

The app serves a static page at `/` which returns HTTP 200, making it suitable for Kubernetes liveness and readiness probes. The `serve` package used in the Docker container responds to all requests on port 5000.
