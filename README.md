flowchart LR
    %% Styles & Colors
    classDef devStyle fill:#2b6cb0,color:#fff,stroke:#1a365d,stroke-width:2px;
    classDef buildStyle fill:#3182ce,color:#fff,stroke:#2b6cb0,stroke-width:2px;
    classDef testStyle fill:#805ad5,color:#fff,stroke:#553c9a,stroke-width:2px;
    classDef regStyle fill:#4a5568,color:#fff,stroke:#2d3748,stroke-width:2px;
    classDef deployStyle fill:#38a169,color:#fff,stroke:#276749,stroke-width:2px;
    classDef monStyle fill:#dd6b20,color:#fff,stroke:#9c4221,stroke-width:2px;

    %% Nodes
    A["1. Push Code<br/>(Git / GitHub)"]:::devStyle
    B["2. Build Image<br/>(GitHub Actions / Docker)"]:::buildStyle
    C["3. Automated Test & Scan<br/>(Jest / SonarQube / Trivy)"]:::testStyle
    D["4. Artifact Storage<br/>(Docker Hub / AWS ECR)"]:::regStyle
    E["5. Continuous Deploy<br/>(ArgoCD / Kubernetes)"]:::deployStyle
    F["6. Monitoring & Alert<br/>(Prometheus / Grafana)"]:::monStyle

    %% Connections
    A -->|"Webhook Trigger"| B
    B -->|"Build Success"| C
    C -->|"Tests Passed"| D
    D -->|"GitOps Sync"| E
    E -->|"Metrics & Logs"| F
