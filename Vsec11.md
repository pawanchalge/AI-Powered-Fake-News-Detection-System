# Experiment 11 — Deploy and Scale a Dockerized AI Application Using Minikube

## Aim
Set up a local Kubernetes cluster with Minikube, deploy a Dockerized AI application, and scale the application.

## What the assignment requires
The assignment's exact requirement is: "Set up a local Kubernetes cluster with Mini Kube to deploy and scale your Dockized AI application."

This example uses a small Flask + scikit-learn sentiment-classification demo. The dataset is deliberately tiny and is for demonstrating deployment only, not for real-world accuracy.

## Concepts
- Docker image: packaged application and dependencies.
- Kubernetes: orchestrates containerized applications.
- Minikube: runs a local Kubernetes cluster.
- kubectl: command-line client for Kubernetes.
- Cluster: Kubernetes environment.
- Node: machine in the cluster.
- Pod: smallest deployable Kubernetes unit; runs one or more containers.
- Deployment: maintains the desired number of Pods.
- Replica: an instance of a Pod managed by a Deployment.
- Service: stable network access to matching Pods.
- Scaling: changing the number of running replicas.

## Prerequisites (Windows)
1. Install Docker Desktop and start it. For this guide, use the Docker Engine with Linux containers.
2. Install Minikube.
3. Install kubectl.
4. Open PowerShell or Command Prompt.
5. Check:
   ```powershell
   docker --version
   kubectl version --client
   minikube version
   ```
   Also run `docker ps` to confirm Docker is running.

## Step 1 — Start Minikube
Use Docker Desktop as the Minikube driver:
```powershell
minikube start --driver=docker
```
Check the cluster:
```powershell
minikube status
kubectl cluster-info
kubectl get nodes
```
Expected: Minikube is running and one node is listed as `Ready`.

## Step 2 — Build the Docker image inside Minikube
Open PowerShell in this project folder (the folder containing the Dockerfile):
```powershell
minikube image build -t ai-app:v1 .
```
This builds the image in Minikube's image store, so Kubernetes can use it locally.
Verify:
```powershell
minikube image ls
```
Look for `ai-app:v1`.

## Step 3 — Deploy the application
Apply the Kubernetes Deployment:
```powershell
kubectl apply -f deployment.yaml
```
Create the Service:
```powershell
kubectl apply -f service.yaml
```
Check resources:
```powershell
kubectl get deployments
kubectl get pods
kubectl get services
```
Wait until the Pod is `Running` and `READY` shows `1/1`. If it is still starting:
```powershell
kubectl rollout status deployment/ai-app
```

## Step 4 — Test the application
Open the service in a browser:
```powershell
minikube service ai-app-service
```
This opens the service URL. The home endpoint should return a JSON message.
For a health check, add `/health` to the URL.
For prediction, send a POST request to `/predict` with JSON:
```json
{"text":"I love this product"}
```
Example using PowerShell (replace URL with the URL printed by `minikube service ai-app-service --url`):
```powershell
$url = minikube service ai-app-service --url
Invoke-RestMethod -Method Post -Uri "$url/predict" -ContentType "application/json" -Body '{"text":"I love this product"}'
```

## Step 5 — Scale the application
Scale from one Pod to three:
```powershell
kubectl scale deployment ai-app --replicas=3
```
Verify:
```powershell
kubectl get deployments
kubectl get pods
```
Expected: Deployment shows `3/3` ready replicas and three Pods are listed (allow a little time for startup).
Scale back to one:
```powershell
kubectl scale deployment ai-app --replicas=1
```
Verify again:
```powershell
kubectl get pods
```

## Step 6 — Inspect and troubleshoot
Useful commands:
```powershell
kubectl describe deployment ai-app
kubectl describe pod <pod-name>
kubectl logs <pod-name>
kubectl get events --sort-by=.metadata.creationTimestamp
kubectl get all
minikube dashboard
```
Replace `<pod-name>` with an actual Pod name from `kubectl get pods`.

Common problems:
- `minikube start` fails: ensure Docker Desktop is open and running; then retry `minikube start --driver=docker`.
- ImagePullBackOff or ErrImageNeverPull: ensure the image was built inside Minikube using `minikube image build -t ai-app:v1 .`; check exact image name and `imagePullPolicy: Never`.
- Pod not Ready: inspect `kubectl describe pod <pod-name>` and `kubectl logs <pod-name>`.
- Service not opening: check `kubectl get svc`; try `minikube service ai-app-service --url`.
- Port/driver issues: run `minikube status` and `kubectl get nodes`.

## Step 7 — Clean up
Remove the Kubernetes resources:
```powershell
kubectl delete -f service.yaml
kubectl delete -f deployment.yaml
```
Stop Minikube:
```powershell
minikube stop
```
Optional: delete the local cluster:
```powershell
minikube delete
```

## Result
A local Kubernetes cluster was started using Minikube. The Dockerized AI demo was deployed using a Kubernetes Deployment, exposed through a Service, and successfully scaled from one replica to three and back to one.

## Viva questions
1. What is Kubernetes? — An open-source platform for automating deployment, scaling, and management of containerized applications.
2. What is Minikube? — A tool that runs a local Kubernetes cluster, commonly for learning and development.
3. What is kubectl? — The command-line tool used to communicate with a Kubernetes cluster.
4. What is a Pod? — The smallest deployable unit in Kubernetes; it can contain one or more containers.
5. What is a Deployment? — A Kubernetes resource that manages Pods and maintains the desired replica count.
6. What is a replica? — Another running instance of the application Pod.
7. What is a Service? — A stable way to expose a set of Pods over the network.
8. What is scaling? — Increasing or decreasing the number of application replicas.
9. What does `kubectl get pods` do? — Lists Pods in the current namespace.
10. What does `kubectl scale deployment ai-app --replicas=3` do? — Sets the desired number of Pods for the Deployment to three.
11. Why build the image inside Minikube? — So the local Kubernetes node can access the image without pulling it from a remote registry.
12. What is `imagePullPolicy: Never`? — It tells Kubernetes not to try pulling the image; it must already exist locally.
13. What is NodePort? — A Service type that exposes an application on a port on the cluster node.
14. What is the difference between Docker and Kubernetes? — Docker builds and runs containers; Kubernetes orchestrates containerized workloads across a cluster.
15. Why use Minikube? — It provides a local environment to learn and test Kubernetes without a cloud cluster.

## Screenshots to capture during your own practical
1. `docker --version`, `minikube version`, `kubectl version --client`.
2. Successful `minikube start --driver=docker`.
3. `kubectl get nodes` showing `Ready`.
4. Successful `minikube image build`.
5. `kubectl get deployments`, `kubectl get pods`, and `kubectl get services`.
6. Browser showing the application home JSON.
7. Prediction request and response.
8. `kubectl get pods` showing three replicas after scaling.
9. `kubectl get pods` showing one replica after scaling down.

Capture your own terminal/browser screenshots. Do not present reference images as proof that your machine ran the experiment.
