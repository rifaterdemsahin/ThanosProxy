Bitnami provides Helm charts to deploy applications easily on Kubernetes. Below are the steps and scripts to install **Thanos**, **Prometheus**, or related components using Bitnami's Helm charts.

---

### **1. Prerequisites**
Ensure you have the following set up:
- A Kubernetes cluster (e.g., Minikube, GKE, EKS, or AKS).
- `kubectl` installed and configured to communicate with your cluster.
- Helm installed (`v3+` recommended).

---

### **2. Add Bitnami Helm Repository**

Add the Bitnami repository to Helm:
```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
```

@rifaterdemsahin ➜ /workspaces/ThanosProxy (main) $ helm repo add bitnami https://charts.bitnami.com/bitnami
"bitnami" has been added to your repositories


---

### **3. Install Prometheus with Bitnami Helm Chart**

Install Prometheus to provide metrics data:
```bash
helm install prometheus bitnami/prometheus
```

To list all installed Helm charts:
```bash
helm list
```
@rifaterdemsahin ➜ /workspaces/ThanosProxy (main) $ helm list
NAME            NAMESPACE       REVISION        UPDATED                                 STATUS  CHART                    APP VERSION
prometheus      default         1               2024-11-20 13:38:27.754948366 +0000 UTC deployedprometheus-1.3.28        2.55.1     

Customize values if needed:
```bash
helm install prometheus bitnami/prometheus \
  --set server.service.type=LoadBalancer \
  --set alertmanager.enabled=false \
  --set pushgateway.enabled=false
```

---

### **4. Install Thanos with Bitnami Helm Chart**

Deploy Thanos to extend Prometheus functionality:
```bash
helm install thanos bitnami/thanos
```

You can customize the deployment using `--set` flags or a values file. For example:
```bash
helm install thanos bitnami/thanos \
  --set query.enabled=true \
  --set storegateway.enabled=true \
  --set bucketConfig.secretName=thanos-bucket-config \
  --set bucketConfig.secretKey=bucket.yaml
```

#### Bucket Configuration:
If you're connecting to an object storage service, create a secret with your bucket configuration:
```yaml
type: S3
config:
  bucket: "your-bucket-name"
  endpoint: "s3.amazonaws.com"
  accessKey: "your-access-key"
  secretKey: "your-secret-key"
```

Create the secret in Kubernetes:
```bash
kubectl create secret generic thanos-bucket-config \
  --from-file=bucket.yaml
```

---

### **5. Install a Query Frontend**

If you want a **Thanos Query Frontend**, ensure the query frontend is enabled in the Thanos chart:
```bash
helm install thanos bitnami/thanos \
  --set queryFrontend.enabled=true \
  --set queryFrontend.replicaCount=2 \
  --set queryFrontend.downstream.url=http://thanos-query:9090
```

---

### **6. Install Storage Gateway**

To set up a **Storage Gateway**, ensure the `storegateway` component is enabled:
```bash
helm install thanos-storegateway bitnami/thanos \
  --set storegateway.enabled=true \
  --set bucketConfig.secretName=thanos-bucket-config
```

---

### **7. Install Ruleset Manager (Thanos Ruler)**

Deploy a Thanos Ruler instance to manage alerts and rules:
```bash
helm install thanos-ruler bitnami/thanos \
  --set ruler.enabled=true \
  --set ruler.alertmanagers.url=http://prometheus-alertmanager.default.svc.cluster.local:9093
```

---

### **8. Verify Installation**

Check the status of your Helm releases:
```bash
helm list
```

List all running pods to ensure components are up:
```bash
kubectl get pods
```

---

### **9. Access Services**

To access the Prometheus or Thanos UIs:
- Forward the service port:
  ```bash
  kubectl port-forward svc/prometheus-server 9090:9090
  kubectl port-forward svc/thanos-query 9091:9090
  ```
- Open your browser and navigate to `http://localhost:9090` or `http://localhost:9091`.

---

### **10. Clean Up**

To uninstall any of the releases:
```bash
helm uninstall <release-name>
```

For example:
```bash
helm uninstall prometheus
helm uninstall thanos
```

---

Let me know if you need detailed `values.yaml` configurations or more examples!