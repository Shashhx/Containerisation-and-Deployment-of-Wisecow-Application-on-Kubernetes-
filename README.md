# Containerisation-and-Deployment-of-Wisecow-Application-on-Kubernetes-

1. Dockerization:
Clone the repository git clone https://github.com/nyrahul/wisecow

2. Build the Docker image:

````
docker build -t wisecow-app .
````

3. Test the Docker image locally:
````
docker run -p 8080:80 wisecow-app
````
4. Apply the manifest files:
````
kubectl apply -f wisecow-deployment.yaml
kubectl apply -f wisecow-service.yaml
````

# Continuous Integration and Deployment (CI/CD)
5. GitHub Actions Workflow:
- Create: `github/workflows/ci-cd.yml`:

```ruby
name: CI/CD Pipeline

on:
  push:
    branches:
      - main

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout code
      uses: actions/checkout@v2
    - name: Login to Container Registry
      uses: docker/login-action@v1
      with:
        username: ${{ secrets.REGISTRY_USERNAME }}
        password: ${{ secrets.REGISTRY_PASSWORD }}
    - name: Build Docker image
      run: docker build -t <your-container-registry>/wisecow-app:latest .
    - name: Push Docker image
      run: docker push <your-container-registry>/wisecow-app:latest
    - name: Deploy to Kubernetes
      run: kubectl apply -f wisecow-deployment.yaml
      env:
        KUBECONFIG: ${{ secrets.KUBE_CONFIG_DATA }}
```

# TLS Implementation
6. Generate TLS Certificates:
   - Use tools like Let's Encrypt to generate certificates.
7. Configure TLS in Kubernetes Ingress:
   - Create an Ingress resource for TLS termination.
   - Update `wisecow-service.yaml` to use NodePort or ClusterIP type.
   - Configure Ingress rules for secure communication.
