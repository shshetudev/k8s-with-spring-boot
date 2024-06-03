### Steps/Commands to follow

- **Install kubernetes support in mac/linux/windows:**
<https://kubernetes.io/docs/tasks/tools/install-kubectl-macos/>
- **Configure minikube:**
  - **Install minikube:** <https://formulae.brew.sh/formula/minikube>
  - **Start the minikube:** `minikube start`
  -
- **Clean build the jar:** `./gradlew clean build`
- **Build the docker image:** `docker build -t spring-boot-k8s-app:latest .`
- **Push the docker image to a registry:**

```docker
docker tag spring-boot-k8s-app:lastest <dockerhub-username>/spring-boot-k8s-app:latest
docker push <dockerhub-username>/spring-boot-k8s-app:latest
```

- Create a kubernetes deployment(`deployment.yaml`) -> Create a K8s service(`service.yaml`)
- **Deploy to K8s:**

```docker
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

#### Add monitoring

- Access the application: `kubectl get svc spring-boot-k8s-app`
- Install helm for adding **Prometheus[Monitoring] and fluentd[Application logs]**: `brew install helm`
- Setting Up Monitoring with Prometheus:

```docker
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm install prometheus prometheus-community/prometheus
```

- ConfigMaps:
  - Apply the config map: `kubectl apply -f prometheus-configmap.yaml`
  - Get the config maps: `kubectl get configmaps`
  - Describe any config map: `kubectl describe configmap prometheus-config`
  
- **Create a prometheus deployment file and apply it:** `kubectl apply -f prometheus-deployment.yaml`
- **Access the Prometheus Dashboard:** `kubectl port-forward deploy/prometheus-server 9090`

### Add logging suport

- Add fluent:

```docker
helm repo add fluent https://fluent.github.io/helm-charts
helm install my-fluentd fluent/fluentd
```

### Configure Fluentd to forward logs to an external log management system like Elasticsearch or a simple storage service like Amazon S3

### Set Resource Requests and Limits

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-spring-boot-app
spec:
  replicas: 3
  template:
    spec:
      containers:
        - name: my-spring-boot-app
          image: <your-dockerhub-username>/my-spring-boot-app:latest
          resources:
            requests:
              cpu: 100m
              memory: 256Mi
            limits:
              cpu: 200m
              memory: 512Mi
```
