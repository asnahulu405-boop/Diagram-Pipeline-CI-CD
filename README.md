
flowchart LR
    A["1. Push Code<br/>Git / GitHub"]
    B["2. Build Image<br/>GitHub Actions / Docker"]
    C["3. Test & Scan<br/>Jest / SonarQube / Trivy"]
    D["4. Artifact Storage<br/>Docker Hub / AWS ECR"]
    E["5. Continuous Deploy<br/>Argo CD / Kubernetes"]
    F["6. Monitoring & Alert<br/>Prometheus / Grafana"]

    A -->|Webhook Trigger| B
    B -->|Build Success| C
    C -->|Tests Passed| D
    D -->|GitOps Sync| E
    E -->|Metrics & Logs| F

    classDef dev fill:#2b6cb0,color:#fff,stroke:#1a365d,stroke-width:2px
    classDef build fill:#3182ce,color:#fff,stroke:#2b6cb0,stroke-width:2px
    classDef test fill:#805ad5,color:#fff,stroke:#553c9a,stroke-width:2px
    classDef storage fill:#4a5568,color:#fff,stroke:#2d3748,stroke-width:2px
    classDef deploy fill:#38a169,color:#fff,stroke:#276749,stroke-width:2px
    classDef monitor fill:#d69e2e,color:#fff,stroke:#9c4221,stroke-width:2px

    class A dev
    class B build
    class C test
    class D storage
    class E deploy
    class F monitor
