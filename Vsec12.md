# Experiment 12 — CI/CD Pipeline Using Jenkins, Docker and Kubernetes

## 1. Aim

To set up a CI/CD pipeline using Jenkins, Docker and Kubernetes to automate the development-to-deployment process for an AI model.

## 2. Exact assignment requirement

The assignment states:

> Set up a CI/CD pipeline with Jenkins, Docker, and Kubernetes to automate the development-to deployment process for your AI model.

This guide implements that requirement using:
- Jenkins for pipeline automation.
- Docker for packaging the AI application into an image.
- Docker Hub as the image registry.
- Kubernetes on Minikube for deployment.
- GitHub as the source-code repository.

The assignment does not prescribe a particular model, programming language, registry, or trigger. The Flask + scikit-learn sentiment demo, Docker Hub, GitHub, and Poll SCM trigger are implementation choices for this practical.

## 3. What is CI/CD?

**CI (Continuous Integration):** Developers frequently push code changes to a shared repository. The pipeline automatically checks out the latest code and performs build steps.

**CD (Continuous Delivery/Deployment):** After the build succeeds, the pipeline packages and delivers the application. In this practical, Jenkins pushes a Docker image to Docker Hub and updates the Kubernetes Deployment.

- Continuous Delivery means changes are prepared for release, often with a manual approval before production.
- Continuous Deployment means every qualifying change is deployed automatically.
- This practical uses automatic deployment to a local Minikube learning cluster.

## 4. Architecture / workflow

Developer changes code
        |
        v
GitHub repository
        |
        | Jenkins detects a new commit (Poll SCM)
        v
Jenkins Pipeline
        |
        +--> Checkout source code
        |
        +--> Build Docker image
        |
        +--> Login to Docker Hub
        |
        +--> Push versioned image
        |
        +--> Update Kubernetes Deployment
        |
        v
Minikube Kubernetes cluster
        |
        v
Pods run the updated AI application

## 5. Important concepts

| Term | Meaning |
|---|---|
| CI/CD | Automated process to integrate, build, test and deliver software |
| Jenkins | Automation server that runs pipeline stages |
| Pipeline | Ordered stages that perform the CI/CD workflow |
| Jenkinsfile | Text file defining the pipeline |
| Docker | Builds and runs container images |
| Docker image | Packaged application and its dependencies |
| Container | Running instance of an image |
| Docker Hub | Registry for storing and sharing Docker images |
| Kubernetes | Orchestrates containerized applications |
| Minikube | Local Kubernetes cluster for learning/testing |
| Deployment | Kubernetes resource that manages application Pods |
| Pod | Smallest deployable Kubernetes unit |
| Poll SCM | Jenkins periodically checks source control for changes |
| Webhook | Git hosting service notifies Jenkins when an event occurs |

## 6. Prerequisites

This guide assumes Windows with:
1. Docker Desktop installed and running (Linux containers).
2. Minikube installed and running.
3. kubectl installed.
4. Jenkins installed and running at `http://localhost:8080`.
5. Git installed.
6. A GitHub account and repository.
7. A Docker Hub account.
8. A Jenkins installation that can run Windows `bat` commands and access Docker CLI, kubectl, and the same Minikube kubeconfig.

**Important Jenkins-on-Windows note:** Jenkins commands run under the Windows account/service that runs Jenkins, not necessarily the account used to open your desktop. That Jenkins account must have Docker and kubectl in PATH and access to the Minikube Kubernetes context. If `kubectl` works in your personal PowerShell but fails in Jenkins, configure the Jenkins service account/PATH/kubeconfig; do not assume Jenkins automatically uses your interactive user's settings.

Check from your normal PowerShell:
```powershell
docker --version
kubectl version --client
minikube version
git --version
```

## 7. Project files

Create a folder named `Experiment12-CICD` and use this structure:

```text
Experiment12-CICD/
│
├── app.py
├── requirements.txt
├── Dockerfile
├── .dockerignore
├── Jenkinsfile
│
└── k8s/
    ├── deployment.yaml
    └── service.yaml
```

The project ZIP supplied with this README contains these files.

## 8. Step 1 — Prepare the AI application

The supplied `app.py` is a small sentiment classification API using scikit-learn and Flask.

Endpoints:
- `GET /` — application information.
- `GET /health` — health status.
- `POST /predict` — predicts positive or negative sentiment from JSON text.

Example request:
```json
{"text":"I love this product"}
```

Example response:
```json
{"text":"I love this product","sentiment":"positive"}
```

This tiny sample model is only a deployment demonstration; it is not a validated production model.

## 9. Step 2 — Create the GitHub repository and upload project

1. Sign in to GitHub.
2. Create a repository, for example `Experiment12-CICD`.
3. Keep it public for the simplest classroom setup, or configure credentials if private.
4. Open PowerShell in the project folder.
5. Run:

```powershell
git init
git add .
git commit -m "Initial AI application and CI CD pipeline"
git branch -M main
git remote add origin https://github.com/YOUR_GITHUB_USERNAME/Experiment12-CICD.git
git push -u origin main
```

Replace `YOUR_GITHUB_USERNAME` with your actual GitHub username.

Verify that GitHub shows `app.py`, `Dockerfile`, `requirements.txt`, `Jenkinsfile`, and the `k8s` folder.

## 10. Step 3 — Build and test Docker image manually once

Replace `YOUR_DOCKERHUB_USERNAME` with your Docker Hub username:

```powershell
docker build -t YOUR_DOCKERHUB_USERNAME/ai-sentiment-app:initial .
docker run -d --name ai-app-test -p 5000:5000 YOUR_DOCKERHUB_USERNAME/ai-sentiment-app:initial
```

Test:
```powershell
curl http://localhost:5000/health
```

Stop and remove the test container:
```powershell
docker stop ai-app-test
docker rm ai-app-test
```

Log in to Docker Hub and create a repository named `ai-sentiment-app`. For this basic guide, make the repository public so Minikube can pull the image without an imagePullSecret.

## 11. Step 4 — Start Minikube and create initial Kubernetes resources

Start cluster:
```powershell
minikube start --driver=docker
kubectl get nodes
```

Update `k8s/deployment.yaml` by replacing `YOUR_DOCKERHUB_USERNAME` with your Docker Hub username.

Push the initial image to Docker Hub:
```powershell
docker login
docker push YOUR_DOCKERHUB_USERNAME/ai-sentiment-app:initial
```

Apply the Deployment and Service:
```powershell
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
```

Check:
```powershell
kubectl get deployments
kubectl get pods
kubectl get services
kubectl rollout status deployment/ai-app
```

Open the app:
```powershell
minikube service ai-app-service
```

The initial Deployment must exist before the Jenkins pipeline can update its container image.

## 12. Step 5 — Configure Jenkins

Open:
```text
http://localhost:8080
```

In Jenkins:
1. Complete the initial unlock/setup if this is the first launch.
2. Install suggested plugins if prompted.
3. Open **Manage Jenkins → Plugins**.
4. Ensure these plugins are installed:
   - Git plugin
   - Pipeline plugin
   - Credentials Binding plugin (often installed as a dependency or available to install)
5. Restart Jenkins if the plugin installer requests it.

The assignment names Git, GitHub and Pipeline plugins. The Pipeline also uses Credentials Binding to access Docker Hub credentials securely. A GitHub plugin is useful for GitHub integrations, but Poll SCM can work without a GitHub webhook.

## 13. Step 6 — Add Docker Hub credentials to Jenkins

Do not write your Docker Hub password directly into the Jenkinsfile.

1. Open **Manage Jenkins → Credentials**.
2. Choose the appropriate global/system domain.
3. Click **Add Credentials**.
4. Kind: **Username with password**.
5. Username: your Docker Hub username.
6. Password: your Docker Hub password or access token.
7. ID: exactly `dockerhub-creds`.
8. Save.

Use a Docker Hub access token where available rather than your account password. Never commit the token to GitHub or share it in screenshots.

## 14. Step 7 — Edit the Jenkinsfile

Open `Jenkinsfile`.

Replace:
```groovy
DOCKER_IMAGE = "YOUR_DOCKERHUB_USERNAME/ai-sentiment-app"
```

with your actual Docker Hub image name, for example:
```groovy
DOCKER_IMAGE = "mydockername/ai-sentiment-app"
```

The supplied Jenkinsfile contains these stages:
1. Checkout — gets source code from GitHub.
2. Build Docker Image — builds a new image tagged with the Jenkins build number.
3. Login and Push to Docker Hub — authenticates using Jenkins credentials and pushes the image.
4. Deploy to Kubernetes — changes the Deployment image and waits for rollout.
5. Verify Deployment — lists Kubernetes Deployments, Pods and Services.

Commit and push the updated Jenkinsfile:
```powershell
git add Jenkinsfile
git commit -m "Configure Jenkins pipeline"
git push
```

## 15. Step 8 — Create a Jenkins Pipeline job

1. Jenkins dashboard → **New Item**.
2. Enter name: `AI-CICD-Pipeline`.
3. Select **Pipeline**.
4. Click **OK**.
5. Under General, optionally add a description.
6. Under **Build Triggers**, select **Poll SCM**.
7. Enter:
   ```text
   H/2 * * * *
   ```
   This asks Jenkins to check the repository approximately every two minutes. Jenkins uses a hashed schedule; it is not an exact promise of a build at every fixed two-minute boundary.
8. Under **Pipeline**:
   - Definition: `Pipeline script from SCM`.
   - SCM: `Git`.
   - Repository URL: `https://github.com/YOUR_GITHUB_USERNAME/Experiment12-CICD.git`.
   - Credentials: add/select credentials if the repository is private.
   - Branch: `*/main`.
   - Script Path: `Jenkinsfile`.
9. Click **Save**.

## 16. Step 9 — Run and verify the pipeline

1. Open the job `AI-CICD-Pipeline`.
2. Click **Build Now** for the first run.
3. Open the build number.
4. Select **Console Output**.
5. Verify that the stages complete successfully:
   - Checkout
   - Build Docker Image
   - Login and Push to Docker Hub
   - Deploy to Kubernetes
   - Verify Deployment

The first build can be run manually. After that, push a new code change to GitHub and wait for Poll SCM to detect it.

## 17. Step 10 — Demonstrate automatic deployment

Make a small harmless change in `app.py`, such as changing the home endpoint's message.

Then:
```powershell
git add app.py
git commit -m "Update AI application message"
git push
```

Wait for Jenkins to poll the repository. Open the new build's Console Output.

After successful pipeline completion, verify:
```powershell
kubectl get deployments
kubectl get pods
kubectl describe deployment ai-app
kubectl rollout history deployment/ai-app
```

The Deployment should now reference the new image tag matching the Jenkins build number.

To access the app:
```powershell
minikube service ai-app-service
```

Test the health endpoint in the browser by adding `/health` to the service URL.

## 18. How the Jenkinsfile works

- `pipeline {}` — defines a Declarative Pipeline.
- `agent any` — runs on any available Jenkins agent.
- `environment {}` — defines environment variables used by stages.
- `triggers { pollSCM(...) }` — checks source control for changes.
- `stages {}` — groups pipeline steps.
- `checkout scm` — checks out the configured repository.
- `docker build` — builds the Docker image.
- `withCredentials` — temporarily makes stored credentials available to the build step.
- `docker push` — uploads the image to Docker Hub.
- `kubectl set image` — updates the image used by the Kubernetes Deployment.
- `kubectl rollout status` — waits for the rollout to complete.
- `post` — runs success/failure actions after the pipeline.

## 19. Important troubleshooting

### A. Jenkins says `docker is not recognized`
Docker CLI is not in the PATH available to the Jenkins process. Install Docker Desktop/CLI correctly and configure PATH for the Jenkins service account. Restart Jenkins after changing system environment variables.

### B. Jenkins says `kubectl is not recognized`
Install kubectl and configure PATH for the Jenkins service account. Test `kubectl version --client` from the same Windows account that runs Jenkins.

### C. Jenkins cannot connect to Kubernetes
The Jenkins service account may not have the same kubeconfig or Minikube context as your interactive account. Configure access securely for the Jenkins service account and verify:
```powershell
kubectl config current-context
kubectl get nodes
```
Run these under the Jenkins execution account as well.

### D. Docker Hub login or push fails
Check the Jenkins credential ID is exactly `dockerhub-creds`, the username/token is valid, and the image name includes your Docker Hub username and repository.

### E. Kubernetes reports `ImagePullBackOff`
Check that the image exists in Docker Hub, the repository is public or has a valid pull secret, and the image tag in the Deployment is correct:
```powershell
kubectl describe pod <pod-name>
```

### F. Deployment not found
Create the initial Deployment before running the pipeline:
```powershell
kubectl apply -f k8s/deployment.yaml
```

### G. Rollout fails
Check:
```powershell
kubectl get pods
kubectl describe deployment ai-app
kubectl logs <pod-name>
```

### H. Pipeline does not trigger after Git push
Check Poll SCM is enabled, the repository URL and branch are correct, and Jenkins can access GitHub. You can click **Build Now** to test the pipeline manually.

### I. Jenkins pipeline fails on `bat`
The supplied Jenkinsfile is for a Windows Jenkins agent. A Linux Jenkins agent needs shell `sh` steps and an appropriately configured Docker/Kubernetes environment; do not mix Windows `bat` syntax with Linux agents.

## 20. Screenshots to capture for practical record

Capture screenshots from your own system:
1. GitHub repository showing project files.
2. Docker image successfully built.
3. Docker Hub repository showing the pushed image tag.
4. Minikube cluster running and `kubectl get nodes`.
5. Kubernetes Deployment and Service created.
6. Jenkins dashboard showing the Pipeline job.
7. Pipeline configuration showing Pipeline script from SCM.
8. Jenkins Console Output showing successful stages.
9. Jenkins build history showing a new build after a Git push.
10. `kubectl get deployments` and `kubectl get pods` after deployment.
11. Browser showing the running application or `/health` response.

Do not use unrelated tutorial screenshots as proof of your own practical execution.

## 21. Result

A CI/CD pipeline was configured using GitHub, Jenkins, Docker, Docker Hub and Kubernetes on Minikube. Jenkins checks out source code, builds a Docker image, pushes the versioned image to Docker Hub, updates the Kubernetes Deployment and verifies the rollout.

## 22. Viva questions and answers

1. **What is CI?**  
   Continuous Integration is the practice of frequently integrating code changes into a shared repository and automatically building/checking them.

2. **What is CD?**  
   Continuous Delivery or Continuous Deployment automates preparing or releasing application changes after integration.

3. **What is Jenkins?**  
   Jenkins is an automation server used to run build, test and deployment pipelines.

4. **What is a Jenkins Pipeline?**  
   A sequence of automated stages that performs tasks such as checkout, build and deployment.

5. **What is a Jenkinsfile?**  
   A text file stored with the project source code that defines the pipeline.

6. **What is Docker?**  
   A platform for building and running containerized applications.

7. **What is a Docker image?**  
   A packaged, read-only template used to create containers.

8. **What is Docker Hub?**  
   An online registry for storing and sharing Docker images.

9. **Why is Docker used in CI/CD?**  
   It packages the application and its dependencies consistently for deployment.

10. **What is Kubernetes?**  
    A platform for orchestrating containerized applications.

11. **What is Minikube?**  
    A tool for running a local Kubernetes cluster.

12. **What is a Pod?**  
    The smallest deployable unit in Kubernetes; it contains one or more containers.

13. **What is a Deployment?**  
    A Kubernetes resource that manages Pods and their desired number of replicas.

14. **What is Poll SCM?**  
    A Jenkins trigger that periodically checks the source repository for changes.

15. **What is a webhook?**  
    An event notification sent by a service such as GitHub to Jenkins when an event occurs.

16. **What is the purpose of `docker build`?**  
    It creates a Docker image from a Dockerfile and build context.

17. **What is the purpose of `docker push`?**  
    It uploads an image to a container registry.

18. **What does `kubectl set image` do?**  
    It updates the container image in a Kubernetes resource such as a Deployment.

19. **What does `kubectl rollout status` do?**  
    It reports whether the Deployment rollout has completed successfully.

20. **Why use build numbers as image tags?**  
    They provide distinct image versions for different pipeline runs and make releases easier to trace.

21. **What happens if a pipeline stage fails?**  
    Later normal stages are skipped, the build is marked failed, and the console output helps identify the problem.

22. **What is the difference between Continuous Delivery and Continuous Deployment?**  
    Continuous Delivery keeps a release ready for approval; Continuous Deployment automatically deploys qualifying changes.

23. **Why store credentials in Jenkins Credentials?**  
    To avoid hardcoding sensitive passwords or tokens in source code.

24. **What is the role of GitHub in this experiment?**  
    It stores the source code and provides the change history that Jenkins monitors.

25. **Explain the complete CI/CD flow.**  
    Developer pushes code to GitHub; Jenkins checks out the code, builds a Docker image, pushes it to Docker Hub, updates Kubernetes, and verifies the rollout.

## 23. Final command revision sheet

```powershell
# Check tools
docker --version
kubectl version --client
minikube version
git --version

# Start local Kubernetes
minikube start --driver=docker
kubectl get nodes

# Build and test Docker
docker build -t YOUR_DOCKERHUB_USERNAME/ai-sentiment-app:initial .
docker run -d --name ai-app-test -p 5000:5000 YOUR_DOCKERHUB_USERNAME/ai-sentiment-app:initial
docker stop ai-app-test
docker rm ai-app-test

# Push initial image
docker login
docker push YOUR_DOCKERHUB_USERNAME/ai-sentiment-app:initial

# Deploy to Kubernetes
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
kubectl get deployments
kubectl get pods
kubectl get services

# Verify deployment
kubectl rollout status deployment/ai-app
minikube service ai-app-service

# After pushing a code change, verify rollout
kubectl get pods
kubectl rollout history deployment/ai-app
```

**Note:** The Jenkins pipeline automates the image build, image push, and subsequent Kubernetes update. The initial Kubernetes Deployment and initial image must be created once before the first pipeline run.
