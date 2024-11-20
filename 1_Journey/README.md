# All the steps in the journey 🚀

1. Step one: Pair with team members 🤝
2. Get the code 💻
3. Run the code 🏃‍♂️

    - Start Minikube
    - Check Minikube status
    - Install Helm
    - Add Helm repo
    - Update Helm repo
    - Add Prometheus (Collect metrics from instance or the pod)
    -- Install with yalm config
    -- Install with parameters at start
    - Set up the ingress
    - Expose remote write (do it from Prometheus with config)
    - Configure the receiver for Thanos
    - Set the frontend query
    - Set Alertmanager
    - Set the rules

```mermaid
graph TD
    A[Step 1: Pair with team members] --> B[Step 2: Get the code]
    B --> C[Step 3: Run the code]
    C --> D[Start Minikube]
    D --> E[Check Minikube status]
    E --> F[Install Helm]
    F --> G[Add Helm repo]
    G --> H[Update Helm repo]
    H --> I[Add Prometheus]
    I --> J[Set up the ingress]
    J --> K[Expose remote write]
    K --> L[Configure the receiver for Thanos]
    L --> M[Set the frontend query]
    M --> N[Set Alertmanager]
    N --> O[Set the rules]
```


prompts:
- Convert or update the mermaid chart
- Add or update emojis
- Fix syntax and grammar
