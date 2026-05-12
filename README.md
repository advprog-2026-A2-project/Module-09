# Module-09


### 1. Current Architecture Diagrams

**Context Diagram**
```mermaid
C4Context
  title System Context Diagram - Current Architecture
  Person(student, "Student", "Pengguna aplikasi E-Learning")
  System(platform, "E-Learning Platform", "Sistem Monolithic untuk Auth, Bacaan, Kuis, dan Forum")
  
  Rel(student, platform, "Menggunakan", "HTTPS")
```

**Container Diagram**
```mermaid
C4Container
  title Container Diagram - Current Architecture (Monolith)
  Person(student, "Student", "Pengguna E-Learning")
  System_Boundary(c1, "E-Learning Platform") {
      Container(web_app, "Web Application", "Java Spring Boot / Django", "Menangani seluruh logika bisnis: Auth, Bacaan, Kuis, Forum dalam satu aplikasi")
      ContainerDb(db, "Shared Database", "PostgreSQL", "Single database untuk menyimpan semua data modul")
  }
  
  Rel(student, web_app, "Mengakses sistem", "HTTPS")
  Rel(web_app, db, "Membaca & Menyimpan data", "JDBC/ORM")
```

**Deployment Diagram**
```mermaid
C4Deployment
  title Deployment Diagram - Current Architecture
  Deployment_Node(client, "User Device", "Browser") {
      Container(ui, "Web UI", "HTML/CSS/JS", "Antarmuka pengguna")
  }
  Deployment_Node(server, "App Server", "Ubuntu/VM") {
      Container(app, "Monolithic Application", "Java/Python", "Menjalankan semua modul")
  }
  Deployment_Node(db_server, "Database Server", "Ubuntu/VM") {
      ContainerDb(db, "PostgreSQL", "Relational Database", "H2/Postgres Single Instance")
  }
  
  Rel(ui, app, "Request API", "HTTPS")
  Rel(app, db, "Query Data", "TCP/IP")
```

1. The current architecture of the group: Monolithic
2. The future architecture of the group : Microservice
3.  Explanation of risk storming of the group:
## Critical Risks at Scale (1M DAU)

### Risk : Database Scalability Bottleneck 

**Scenario:** Peak hours with 100K concurrent users

#### Current Architecture
- Single H2/PostgreSQL database instance
- All modules (auth, bacaan, kuis, forum) share one database
- No read replicas or sharding strategy
- Auto-increment primary keys create hotspots

#### Risk Statement
> "If user adoption reaches 1M DAU, database becomes the primary bottleneck, causing 5-10 second query latencies and 30-40% service unavailability during peak hours."

#### Manifestations
1. **Connection Pool Exhaustion**
2. **Lock Contention**
3. **Query Performance Degradation**

Alasan memakai risk storming:
1. Identifikasi Risiko Awal dalam Development
   Diperlukan cara cepat untuk mengidentifikasi potensi masalah sebelum mereka menjadi kritis
   Risk Storming memungkinkan tim untuk berdiskusi dan mengidentifikasi risiko secara kolaboratif.

2. Fokus pada Arsitektur
   Risk Storming khusus dirancang untuk menganalisis risiko yang terkait dengan keputusan arsitektur, sehingga kami dapat mengevaluasi keputusan arsitektur tersebut.

3. Identifikasi Risiko Spesifik
   Risk Storming membantu mengidentifikasi risiko di area-area penting.

4. Scalability & Maintainability
   Proyek akan terus berkembang dengan fitur baru, dan
   Risk Storming membntu memahami trade-offs dari keputusan desain saat ini, sehingga
   memandu refctoring dan architectural improvements di masa depan.

