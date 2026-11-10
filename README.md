
# DIAGRAM ARSITEKTUR CLOUD SISTEM INFORMASI KAMPUS

## Deskripsi Proyek

Proyek ini menggambarkan rancangan infrastruktur cloud untuk Sistem Informasi Layanan Kampus. Sistem digunakan oleh mahasiswa, dosen, administrator, dan staf akademik.

Proyek ini terdiri dari tiga diagram utama:

1. Arsitektur Cloud Sistem Informasi Kampus.
2. Pipeline CI/CD (Continuous Integration / Continuous Deployment).
3. Alur Kerja Kolaborasi Tim Menggunakan GitHub.

---

# 1. DIAGRAM ARSITEKTUR CLOUD SISTEM INFORMASI KAMPUS

Diagram ini menggambarkan infrastruktur cloud yang mencakup pengguna, keamanan jaringan, server aplikasi, database, penyimpanan data, monitoring, dan layanan eksternal.

```mermaid
flowchart TD

    subgraph USERS["1. USER LAYER"]
        M["Mahasiswa"]
        D["Dosen"]
        ADM["Administrator"]
        STAFF["Staff Akademik"]
    end

    subgraph EDGE["2. SECURITY & EDGE LAYER"]
        DNS["Cloud DNS"]
        WAF["Web Application Firewall"]
        DDOS["DDoS Protection"]
        CDN["Content Delivery Network"]
        SSL["SSL / TLS Encryption"]
    end

    subgraph VPC["3. CLOUD INFRASTRUCTURE - VPC"]

        subgraph PUBLIC["PUBLIC SUBNET"]
            ALB["Application Load Balancer"]
            NAT["NAT Gateway"]
            BASTION["Bastion Host"]
        end

        subgraph PRIVATE["PRIVATE SUBNET"]
            WEB["Web Server"]
            APP["Application Server"]
            API["Backend API Gateway"]
            AUTH["Authentication Service"]
        end

        subgraph SERVICES["APPLICATION SERVICES"]
            AKADEMIK["Layanan Akademik"]
            NILAI["Sistem Informasi Nilai"]
            ABSEN["Sistem Absensi"]
            JADWAL["Manajemen Jadwal Kuliah"]
            PMB["Pendaftaran Mahasiswa Baru"]
        end

        subgraph DATABASE["DATABASE SUBNET"]
            DB[("Database Server")]
            CACHE[("Redis Cache")]
            REPLICA[("Database Replica")]
        end
    end

    subgraph STORAGE["4. DATA & STORAGE LAYER"]
        OBJECT["Cloud Object Storage"]
        FILES["File Storage"]
        BACKUP["Backup Storage"]
        ARCHIVE["Data Archive"]
    end

    subgraph SECURITY["5. SECURITY MANAGEMENT"]
        IAM["Identity & Access Management"]
        KEY["Key Management Service"]
        AUDIT["Audit Log"]
        SECRETS["Secrets Management"]
    end

    subgraph MONITOR["6. MONITORING & MANAGEMENT"]
        LOG["Cloud Logging"]
        METRIC["Cloud Monitoring"]
        ALERT["Alert Notification"]
        HEALTH["Health Check"]
    end

    subgraph EXTERNAL["7. EXTERNAL SERVICES"]
        EMAIL["Email Notification"]
        PAYMENT["Payment Gateway"]
        SMS["SMS / OTP Gateway"]
    end

    M --> DNS
    D --> DNS
    ADM --> DNS
    STAFF --> DNS

    DNS --> WAF
    WAF --> DDOS
    DDOS --> CDN
    CDN --> SSL
    SSL --> ALB

    ALB --> WEB
    WEB --> APP
    APP --> API
    API --> AUTH

    AUTH --> AKADEMIK
    AUTH --> NILAI
    AUTH --> ABSEN
    AUTH --> JADWAL
    AUTH --> PMB

    AKADEMIK --> DB
    NILAI --> DB
    ABSEN --> DB
    JADWAL --> DB
    PMB --> DB

    API --> CACHE
    DB --> REPLICA

    DB --> BACKUP
    AKADEMIK --> FILES
    FILES --> OBJECT
    OBJECT --> ARCHIVE

    AUTH -.-> IAM
    DB -.-> KEY
    APP -.-> SECRETS
    API -.-> AUDIT

    APP -.-> LOG
    DB -.-> LOG
    ALB -.-> HEALTH
    LOG --> METRIC
    HEALTH --> METRIC
    METRIC --> ALERT

    APP --> EMAIL
    APP --> PAYMENT
    AUTH --> SMS

    NAT --> APP
    BASTION -.-> APP

    style USERS fill:#DCEBFF,stroke:#2878C8,color:#123B68
    style EDGE fill:#D9F2FF,stroke:#1687B5,color:#07516D
    style VPC fill:#DDF5E5,stroke:#29965A,color:#14552F
    style PUBLIC fill:#ECF8EF,stroke:#54A66C,color:#14552F
    style PRIVATE fill:#E1F3E8,stroke:#29965A,color:#14552F
    style SERVICES fill:#C9EED6,stroke:#23834A,color:#14552F
    style DATABASE fill:#E9DFFF,stroke:#8756C5,color:#48277A
    style STORAGE fill:#FFF0D6,stroke:#D99A35,color:#754A0B
    style SECURITY fill:#FFE0E0,stroke:#D65A5A,color:#782727
    style MONITOR fill:#FFF0DF,stroke:#D68B36,color:#75430D
    style EXTERNAL fill:#E5E5FF,stroke:#6868C8,color:#33336F
```

### Penjelasan

- User Layer: Pengguna sistem kampus.
- Security & Edge Layer: Perlindungan dan pengamanan akses.
- Cloud Infrastructure: Tempat server dan aplikasi berjalan.
- Data & Storage: Penyimpanan database dan file.
- Security Management: Pengelolaan identitas dan keamanan.
- Monitoring: Pemantauan kondisi sistem.
- External Services: Integrasi email, pembayaran, dan OTP.

---

# 2. DIAGRAM PIPELINE CI/CD

Pipeline CI/CD adalah proses otomatis untuk mengintegrasikan, menguji, dan melakukan deployment aplikasi.

```mermaid
flowchart TD

    A["Developer<br/>Push Code"]
    B["GitHub Repository"]
    C["GitHub Actions<br/>Build Image"]
    D["Automated Testing<br/>Jest / SonarQube"]
    E["Security Scan<br/>Trivy"]
    F["Container Registry<br/>Docker Hub / AWS ECR"]
    G["Deployment<br/>Argo CD / Kubernetes"]
    H["Application Running"]
    I["Monitoring<br/>Prometheus / Grafana"]

    A --> B
    B --> C
    C --> D
    D --> E
    E --> F
    F --> G
    G --> H
    H --> I

    D -.->|Test Failed| J["Fix Code"]
    E -.->|Security Issue| J
    J --> A

    style A fill:#DCEBFF,stroke:#2878C8,color:#123B68
    style B fill:#DCEBFF,stroke:#2878C8,color:#123B68
    style C fill:#D9F2FF,stroke:#1687B5,color:#07516D
    style D fill:#E9DFFF,stroke:#8756C5,color:#48277A
    style E fill:#FFE0E0,stroke:#D65A5A,color:#782727
    style F fill:#FFF0D6,stroke:#D99A35,color:#754A0B
    style G fill:#DDF5E5,stroke:#29965A,color:#14552F
    style H fill:#DDF5E5,stroke:#29965A,color:#14552F
    style I fill:#FFF0DF,stroke:#D68B36,color:#75430D
    style J fill:#FFE0E0,stroke:#D65A5A,color:#782727
```

### Penjelasan

1. Developer melakukan Push Code ke GitHub.
2. GitHub menyimpan kode dalam repository.
3. GitHub Actions menjalankan proses Build.
4. Automated Testing memeriksa kualitas aplikasi.
5. Security Scan memeriksa keamanan kode.
6. Container Registry menyimpan Docker Image.
7. Argo CD dan Kubernetes melakukan deployment.
8. Aplikasi berjalan dan dapat digunakan.
9. Prometheus dan Grafana melakukan monitoring.

Jika pengujian atau pemindaian keamanan gagal, developer memperbaiki kode dan mengulang proses.

---

# 3. DIAGRAM ALUR KERJA KOLABORASI TIM GITHUB

Diagram ini menggambarkan proses kerja sama beberapa anggota tim dalam mengembangkan proyek menggunakan GitHub.

```mermaid
flowchart TD

    A["Project Manager<br/>Membuat Repository"]
    B["Menambahkan Anggota Tim<br/>Collaborators"]
    C["Membuat Branch Development"]

    subgraph TEAM["TEAM DEVELOPMENT"]
        D["Developer 1<br/>Mengerjakan Fitur 1"]
        E["Developer 2<br/>Mengerjakan Fitur 2"]
        F["Developer 3<br/>Mengerjakan Fitur 3"]
    end

    G["Commit & Push Code"]
    H["Create Pull Request"]
    I["Code Review"]
    J{"Kode Disetujui?"}
    K["Revisi Kode"]
    L["Merge ke Main Branch"]
    M["Testing & Integration"]
    N["Release Project"]
    O["Dokumentasi README.md"]

    A --> B
    B --> C

    C --> D
    C --> E
    C --> F

    D --> G
    E --> G
    F --> G

    G --> H
    H --> I
    I --> J

    J -->|Tidak| K
    K --> G

    J -->|Ya| L
    L --> M
    M --> N
    N --> O

    style A fill:#DCEBFF,stroke:#2878C8,color:#123B68
    style B fill:#D9F2FF,stroke:#1687B5,color:#07516D
    style C fill:#E9DFFF,stroke:#8756C5,color:#48277A
    style TEAM fill:#ECF8EF,stroke:#54A66C,color:#14552F
    style D fill:#DDF5E5,stroke:#29965A,color:#14552F
    style E fill:#DDF5E5,stroke:#29965A,color:#14552F
    style F fill:#DDF5E5,stroke:#29965A,color:#14552F
    style G fill:#FFF0D6,stroke:#D99A35,color:#754A0B
    style H fill:#D9F2FF,stroke:#1687B5,color:#07516D
    style I fill:#E9DFFF,stroke:#8756C5,color:#48277A
    style J fill:#FFF0DF,stroke:#D68B36,color:#75430D
    style K fill:#FFE0E0,stroke:#D65A5A,color:#782727
    style L fill:#DDF5E5,stroke:#29965A,color:#14552F
    style M fill:#D9F2FF,stroke:#1687B5,color:#07516D
    style N fill:#DDF5E5,stroke:#29965A,color:#14552F
    style O fill:#FFF0D6,stroke:#D99A35,color:#754A0B
```

### Penjelasan

1. Project Manager membuat repository GitHub.
2. Anggota tim ditambahkan sebagai collaborator.
3. Tim membuat branch untuk pengembangan.
4. Setiap developer mengerjakan fitur masing-masing.
5. Developer melakukan Commit dan Push Code.
6. Developer membuat Pull Request.
7. Anggota tim melakukan Code Review.
8. Jika ada kesalahan, kode diperbaiki.
9. Jika disetujui, kode di-merge ke Main Branch.
10. Tim melakukan testing, integrasi, dan release.
11. Dokumentasi proyek dilengkapi dalam README.md.

---

# 4. KESIMPULAN

Berdasarkan ketiga diagram tersebut, dapat disimpulkan bahwa sistem informasi layanan kampus membutuhkan infrastruktur cloud yang aman, terintegrasi, dan dapat dipantau.

Pipeline CI/CD membantu proses pengembangan dan deployment aplikasi menjadi lebih otomatis dan efisien.

Sementara itu, kolaborasi tim menggunakan GitHub membantu setiap anggota bekerja secara terstruktur melalui branch, commit, pull request, code review, dan merge.

Ketiga diagram tersebut saling mendukung dalam membangun dan mengelola Sistem Informasi Layanan Kampus berbasis Cloud Computing.
