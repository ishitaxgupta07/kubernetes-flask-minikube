# Flask App on Kubernetes using Minikube

A simple Flask web application deployed locally on Kubernetes using Minikube, Docker, and Kubernetes YAML manifests.

## Tech Stack

- Python 3.12
- Flask 3.1.2
- Docker
- Kubernetes
- Minikube
- kubectl

## Project Structure

```text
kubernetes-flask-minikube/
├── app.py
├── requirements.txt
├── Dockerfile
├── k8s/
│   ├── flask-deployment.yaml
│   └── flask-service.yaml
└── README.md
```

## Application

The Flask application exposes a GET endpoint at `/` on port `15000` and returns:

```text
Hello from Flask on Kubernetes!
```

## Run Locally with Minikube

### 1. Start Minikube

```powershell
minikube start --driver=docker
```

### 2. Build the Docker image inside Minikube

```powershell
minikube image build -t flask-app:latest .
```

### 3. Deploy the application

```powershell
kubectl apply -f .\k8s\flask-deployment.yaml
kubectl apply -f .\k8s\flask-service.yaml
```

### 4. Verify the deployment

```powershell
kubectl rollout status deployment/flask-app
kubectl get pods -l app=flask-app
kubectl get services
```

### 5. Access the application

```powershell
minikube service flask-app-service --url
```

Keep the terminal running while using the URL provided. In another PowerShell window, test it with:

```powershell
curl.exe http://127.0.0.1:<PORT>
```

Replace `<PORT>` with the port in the URL returned by Minikube.

Expected response:

```text
Hello from Flask on Kubernetes!
```

## Kubernetes Resources

- **Deployment:** Manages the Flask application pod.
- **Service:** Exposes the application through a NodePort service.
- **Container port:** `15000`
- **NodePort:** `30081`
- **Image:** `flask-app:latest`

## Cleanup

To remove the deployed resources:

```powershell
kubectl delete -f .\k8s\flask-service.yaml
kubectl delete -f .\k8s\flask-deployment.yaml
```

To stop Minikube:

```powershell
minikube stop
```
