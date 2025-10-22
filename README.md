
# 🧩 **Microservices Core Tech Stack**

| **Category**                           | **Technology / Tool**                                           | **Description / Purpose**                                                                                                                              |
| -------------------------------------- | --------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **1️⃣ API Gateway**                    | **Spring Cloud Gateway (SCG)**                                  | Acts as a single entry point for all client requests. Handles routing, authentication, rate limiting, and monitoring across multiple microservices.  <a href="https://github.com/mohammad2java/spring-cloud-gateway-poc" target="_blank">🔗 Spring Cloud Gateway POC »</a> |

| **2️⃣ Service Discovery**              | **Netflix Eureka Server**                                       | Keeps track of all running microservice instances. Enables dynamic registration and lookup, eliminating hardcoded service URLs.                        |
| **3️⃣ Centralized Configuration**      | **Spring Cloud Config Server**                                  | Stores configuration files for all microservices in one central place (often a Git repo). Allows runtime refresh of configuration without redeploying. |
| **4️⃣ Inter-Service Communication**    | **Spring WebFlux / RESTTemplate / WebClient**                   | Used by microservices to call other microservices over HTTP (REST). WebClient is reactive and preferred for non-blocking calls.                        |
| **5️⃣ Circuit Breaker & Resilience**   | **Resilience4j / Spring Cloud CircuitBreaker**                  | Prevents cascading failures by handling timeouts, retries, rate limits, and fallback logic for dependent service failures.                             |
| **6️⃣ Load Balancing**                 | **Spring Cloud LoadBalancer**                                   | Distributes traffic among multiple instances of a microservice registered in Eureka. Replaces Ribbon (deprecated).                                     |
| **7️⃣ API Documentation**              | **Swagger / OpenAPI / SpringDoc**                               | Auto-generates REST API documentation with an interactive UI (Swagger UI). Useful for developers and QA testing.                                       |
| **8️⃣ Authentication & Authorization** | **Spring Security + OAuth2 / Keycloak / JWT**                   | Handles secure access to APIs. Implements token-based authentication and role-based authorization.                                                     |
| **9️⃣ Data Access Layer**              | **Spring Data JPA / Hibernate / Spring Data MongoDB**           | Simplifies database access and ORM for relational or NoSQL databases.                                                                                  |
| **🔟 Database per Service**            | **PostgreSQL / MySQL / MongoDB**                                | Each microservice has its own isolated database to ensure loose coupling and autonomy (polyglot persistence).                                          |
| **11️⃣ Messaging / Event Bus**         | **Apache Kafka / RabbitMQ / AWS SQS**                           | Enables asynchronous communication and event-driven architecture between microservices. Ideal for decoupled, scalable systems.                         |
| **12️⃣ Caching**                       | **Redis / Caffeine / Hazelcast**                                | Reduces latency and database load by storing frequently accessed data in memory.                                                                       |
| **13️⃣ File / Object Storage**         | **AWS S3 / MinIO**                                              | Used for file uploads, static content, backups, and document storage.                                                                                  |
| **14️⃣ Distributed Tracing**           | **Spring Cloud Sleuth + Zipkin / Jaeger**                       | Tracks request flow across multiple microservices for debugging and performance analysis.                                                              |
| **15️⃣ Logging**                       | **Logback / ELK (Elasticsearch, Logstash, Kibana) / Coralogix** | Centralized log aggregation, visualization, and analysis. Helps identify production issues quickly.                                                    |
| **16️⃣ Metrics & Monitoring**          | **Micrometer + Prometheus + Grafana**                           | Captures application performance metrics, latency, and JVM stats. Displays real-time dashboards for observability.                                     |
| **17️⃣ Testing**                       | **JUnit5 / Mockito / WireMock / Testcontainers**                | Supports unit, integration, and contract testing for each microservice, ensuring code reliability.                                                     |
| **18️⃣ Build & Dependency Management** | **Maven / Gradle**                                              | Builds microservice artifacts (JARs), manages dependencies, and handles versioning.                                                                    |
| **19️⃣ Externalized Config / Secrets** | **Spring Cloud Vault / AWS Secrets Manager / K8s Secrets**      | Stores sensitive data (passwords, API keys) securely outside of source code.                                                                           |
| **20️⃣ Feature Management**            | **Spring Cloud Config + Unleash / LaunchDarkly**                | Enables gradual rollout of features and configuration toggles without redeployment.                                                                    |

---

### 🧠 **Key Principles**

* **Each service** should be **independent, stateless**, and **own its own data**.
* **API Gateway (SCG)** acts as the centralized routing and security layer.
* **Eureka + Config Server** form the core **Spring Cloud Service Mesh**.
* Use **Resilience4j + Sleuth + Micrometer** for observability and fault tolerance.
* All services should follow **12-Factor App** guidelines.


# 🧭 **The 12-Factor App Principles (Simplified for Microservices)**

| **#**                       | **Factor Name**                                                         | **Simple Explanation**                                                                                             | **How It Applies to Microservices**                                                  |
| --------------------------- | ----------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------ |
| **1️⃣ Codebase**            | **One codebase per app, tracked in version control (like Git)**         | Each microservice should have its own repository or folder in Git. No shared code between unrelated services.      | Example: Each service (Order, Payment, User) has its own Git repo.                   |
| **2️⃣ Dependencies**        | **Explicitly declare all dependencies**                                 | Use tools like Maven or Gradle to list every library your service needs — don’t rely on system-wide packages.      | Example: Add dependencies in `pom.xml` instead of installing manually on the server. |
| **3️⃣ Config**              | **Store config separately from code**                                   | Keep environment-specific settings (DB URLs, keys, secrets) outside the source code.                               | Example: Use Spring Cloud Config Server, or environment variables in EKS.            |
| **4️⃣ Backing Services**    | **Treat attached services (DB, queues, etc.) as replaceable resources** | Your service should not depend on a specific database or messaging instance — only on its connection details.      | Example: Switch from MySQL to PostgreSQL without code change.                        |
| **5️⃣ Build, Release, Run** | **Separate build and run stages**                                       | Build your app once (JAR/Docker image), then deploy the same artifact to different environments (dev, test, prod). | Example: CI/CD builds once, deploys the same image everywhere.                       |
| **6️⃣ Processes**           | **Run the app as one or more stateless processes**                      | Services should not keep session or user data in memory; store it in Redis or DB.                                  | Example: Any SCG pod can handle any request — no sticky sessions.                    |
| **7️⃣ Port Binding**        | **Export services via ports**                                           | Each service should run independently and expose its API via its own port.                                         | Example: Spring Boot apps listen on port 8080 or 9090.                               |
| **8️⃣ Concurrency**         | **Scale by adding more instances, not bigger ones**                     | Run multiple instances (pods or EC2s) instead of increasing memory/CPU of a single one.                            | Example: Kubernetes scales from 3 → 6 pods based on traffic.                         |
| **9️⃣ Disposability**       | **Start fast, stop gracefully**                                         | Services should start quickly and handle shutdowns cleanly (finish ongoing requests, release resources).           | Example: Spring Boot gracefully shuts down when the pod is terminated.               |
| **🔟 Dev/Prod Parity**      | **Keep development, staging, and production as similar as possible**    | Use the same configurations, dependencies, and tools in all environments.                                          | Example: Use Docker locally, same image runs in EKS.                                 |
| **11️⃣ Logs**               | **Treat logs as event streams**                                         | Write logs to standard output (console). Let tools like ELK or CloudWatch collect them.                            | Example: `stdout` → FluentBit → Elasticsearch → Kibana.                              |
| **12️⃣ Admin Processes**    | **Run admin/maintenance tasks as one-off processes**                    | Don’t mix admin scripts with app runtime — run them separately when needed.                                        | Example: Run DB migrations using a separate job or container, not inside the app.    |

---

### ✅ **Summary**

> The 12-Factor principles help microservices stay **portable, scalable, cloud-ready, and easy to maintain**.
> They ensure every service can be **deployed anywhere, scaled anytime, and updated safely.**

---





# **Spring Cloud Gateway High Availability Architecture — EC2 Deployment**

## **Overview**

This document describes a **highly available, EC2-based deployment** of **Spring Cloud Gateway (SCG)** in production. The architecture leverages:

* **AWS Application Load Balancer (ALB)**
* **Target Groups**
* **Spring Cloud Gateway nodes**
* **Service Discovery (Eureka / Consul)**
* **Backend microservices**
* Optional supporting services (Redis, Config Server)

---

## **Architecture Diagram (EC2)**

```
          Internet Clients
                  │
                  ▼
       ┌─────────────────────────┐
       │ AWS Application Load    │
       │ Balancer (ALB, Public) │
       └─────────────┬──────────┘
                     │
            ┌────────┴────────┐
            │  ALB Target Groups │
            │------------------│
            │ SCG-Target-Group │
            │ (SCG EC2 nodes)  │
            └────────┬────────┘
                     │
         ┌───────────┴───────────┐
         │                       │
   ┌────────────┐          ┌────────────┐
   │ SCG Node A │          │ SCG Node B │
   │ (EC2)      │          │ (EC2)      │
   │ AZ-1a      │          │ AZ-1b      │
   └─────┬──────┘          └─────┬──────┘
         │                       │
         ▼                       ▼
   ┌───────────────────────────────┐
   │ Eureka / Discovery Servers    │
   │ 2-3 EC2 nodes across AZs      │
   └─────────────┬─────────────────┘
                 │
                 ▼
        Backend Microservices
       (EC2 / ECS / RDS / Redis)
```

---

## **Components**

### 1. **AWS Application Load Balancer (ALB)**

* **Public-facing entry point** for client requests.
* Handles:

  * **SSL/TLS termination** via AWS ACM certificates
  * **Health checks** for SCG nodes
  * Multi-AZ traffic routing
* Forward traffic to **Target Groups** containing SCG nodes.

---

### 2. **ALB Target Groups**

* **Groups of EC2 instances** registered as backend targets for ALB.
* Example:

  * `SCG-Target-Group` → SCG EC2 nodes
* Responsibilities:

  * Load balancing requests across SCG nodes
  * Health checks and removal of unhealthy nodes
* Can create **multiple target groups** if exposing other services externally.

---

### 3. **Spring Cloud Gateway (SCG) Nodes**

* Deployed on **EC2 instances** (recommended: Auto Scaling Group for HA)
* **Stateless** pods, scale horizontally.
* Connect to **Eureka/Consul** for dynamic routing.
* Receives traffic from ALB target group.
* Health checks via `/actuator/health`.

---

### 4. **Discovery Server (Eureka / Consul)**

* Deployed on **2–3 EC2 nodes**, multi-AZ.
* Maintains registry of SCG and microservices.
* Provides:

  * Service discovery
  * Health tracking
  * Dynamic routing for SCG

---

### 5. **Backend Microservices**

* Deployed on EC2 / ECS / RDS
* Registered with **Eureka/Consul**
* SCG routes traffic based on **service ID** (e.g., `lb://order-service/orders`)
* Optional supporting services: **Redis**, **Config Server**

---

## **Traffic Flow**

```
Client
  │
  ▼
AWS ALB (Public)
  │
  ▼
ALB Target Group (SCG nodes)
  │
  ▼
SCG EC2 Instances (Stateless)
  │
  ▼
Discovery Server (Eureka/Consul)
  │
  ▼
Backend Microservices
```

---

## **High Availability Features**

* **ALB:** Multi-AZ, health checks, SSL termination
* **Target Groups:** Auto-remove unhealthy EC2 nodes
* **SCG:** Multiple EC2 nodes, stateless, Auto Scaling
* **Eureka:** 2–3 EC2 nodes, multi-AZ
* **Microservices:** Multiple replicas
* **Redis / Config Server:** Optional, HA via cluster
* **Monitoring:** CloudWatch, Prometheus/Grafana
* **Auto-scaling:** EC2 Auto Scaling Group for SCG and microservices
* **Graceful shutdown:** Prevent request loss

---

## **Best Practices**

* Deploy **SCG and Eureka nodes across multiple AZs**.
* Keep **SCG stateless**; store sessions in Redis if needed.
* Use **health checks** on `/actuator/health`.
* Offload SSL termination to ALB using **AWS ACM**.
* Monitor traffic, logs, and metrics via CloudWatch or Prometheus/Grafana.
* Tag ALB and Target Groups for clarity and cost management.

---

✅ **Conclusion:**
The EC2-based SCG architecture provides **high availability, fault tolerance, and scalability** using ALB with target groups, multiple SCG nodes, and discovery server integration. Suitable for teams using **traditional VM-based deployments** or hybrid environments.

---

# 🏗️ **Spring Cloud Gateway High Availability Architecture – EKS Based**

## **1️⃣ Overview**

In this setup, Spring Cloud Gateway (SCG) is **deployed as a container inside an Amazon EKS cluster**.
Traffic from the internet is routed through an **AWS Application Load Balancer (ALB)**, which is **automatically managed** by the **AWS Load Balancer Controller** in the EKS environment.

---

## **2️⃣ Architecture Diagram**

```
                    Internet Clients
                            │
                            ▼
                 ┌────────────────────────────┐
                 │ AWS Application Load       │
                 │ Balancer (Public ALB)      │
                 │ (Managed by AWS LB Ctrlr)  │
                 └─────────────┬──────────────┘
                               │
                     (Ingress Resource)
                               │
                               ▼
                ┌──────────────────────────┐
                │   Kubernetes Service      │
                │ (Type: ClusterIP)        │
                └─────────────┬────────────┘
                              │
                     Routes Traffic via Kube Proxy
                              │
                              ▼
             ┌────────────────────────────────────────┐
             │ Spring Cloud Gateway Pods (Replicas)    │
             │   - scg-deployment-1                    │
             │   - scg-deployment-2                    │
             │   - scg-deployment-3                    │
             └────────────────────────────────────────┘
                              │
                              ▼
               ┌────────────────────────────┐
               │  Eureka / Discovery Pods    │
               │  (2–3 replicas across AZs) │
               └────────────┬───────────────┘
                            │
                            ▼
                 Backend Microservices
               (K8s Services or External APIs)
```

---

## **3️⃣ Components Explanation**

### **🔹 AWS Load Balancer Controller**

* A **Kubernetes controller** running inside the EKS cluster.
* Watches **Ingress** resources.
* Automatically **creates and configures ALBs** (Application Load Balancers) in AWS.
* Manages **Target Groups** behind the ALB mapped to Kubernetes Services.
* Handles **SSL termination** and **health checks**.

### **🔹 Ingress Resource**

* Defines **external access rules** (hostname/path-based routing).
* Maps incoming requests from ALB to internal Kubernetes Services.
* Example:

  ```yaml
  apiVersion: networking.k8s.io/v1
  kind: Ingress
  metadata:
    name: scg-ingress
    annotations:
      kubernetes.io/ingress.class: alb
      alb.ingress.kubernetes.io/scheme: internet-facing
      alb.ingress.kubernetes.io/target-type: ip
      alb.ingress.kubernetes.io/listen-ports: '[{"HTTPS":443}]'
      alb.ingress.kubernetes.io/certificate-arn: <SSL-CERT-ARN>
  spec:
    rules:
      - host: api.mycompany.com
        http:
          paths:
            - path: /
              pathType: Prefix
              backend:
                service:
                  name: scg-service
                  port:
                    number: 8080
  ```

---

### **🔹 Kubernetes Service**

* A **ClusterIP Service** that exposes the SCG deployment to the cluster.
* The **Ingress** routes traffic to this service.
* Example:

  ```yaml
  apiVersion: v1
  kind: Service
  metadata:
    name: scg-service
  spec:
    selector:
      app: spring-cloud-gateway
    ports:
      - port: 8080
        targetPort: 8080
  ```

---

### **🔹 SCG Deployment**

* A **Kubernetes Deployment** ensures multiple replicas (pods) of SCG.
* Achieves **horizontal scaling** and **high availability**.
* Example:

  ```yaml
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: scg-deployment
  spec:
    replicas: 3
    selector:
      matchLabels:
        app: spring-cloud-gateway
    template:
      metadata:
        labels:
          app: spring-cloud-gateway
      spec:
        containers:
          - name: scg
            image: myrepo/spring-cloud-gateway:latest
            ports:
              - containerPort: 8080
  ```

---

## **4️⃣ Key Features of This Setup**

✅ **Fully Managed ALB** (no manual creation needed).
✅ **SSL termination at ALB** — offloads SSL from SCG.
✅ **Automatic health checks** via AWS LB Controller.
✅ **Eureka discovery** ensures dynamic service resolution.
✅ **Horizontal Pod Autoscaler (HPA)** can scale SCG automatically.
✅ **Multi-AZ High Availability** — pods and ALB target groups span multiple Availability Zones.

---

## **5️⃣ Traffic Flow Summary**

```
Client → ALB (HTTPS)
       → Ingress Resource
       → scg-service (ClusterIP)
       → SCG Pods
       → Eureka Discovery Service
       → Backend Microservices
```

---

## **6️⃣ High Availability Highlights**

* **ALB** auto-distributes across AZs.
* **Kubernetes scheduler** ensures SCG and Eureka pods are placed in different AZs.
* **Pod health checks** + **ALB target health checks** = full fault tolerance.
* **Stateless SCG design** ensures no session dependency.

---

Excellent 👍 — here’s the **comparison summary table** that bridges both documents, showing the **key differences between EC2-based and EKS-based Spring Cloud Gateway (SCG) deployments**.

You can add this at the end of both READMEs or keep it as a standalone reference page.

---

# ⚖️ **Spring Cloud Gateway — EC2 vs EKS Production Architecture Comparison**

| **Aspect**                    | **EC2-Based Deployment**                                       | **EKS-Based Deployment**                                                   |
| ----------------------------- | -------------------------------------------------------------- | -------------------------------------------------------------------------- |
| **Infrastructure Type**       | Virtual Machines (EC2 Instances)                               | Kubernetes-managed containers (Pods)                                       |
| **Deployment Unit**           | EC2 instances (manually provisioned or via Auto Scaling Group) | Pods controlled by a Kubernetes Deployment                                 |
| **Load Balancer**             | AWS Application Load Balancer (ALB) manually configured        | AWS ALB automatically created via Ingress and AWS Load Balancer Controller |
| **Target Groups**             | Required — ALB routes to EC2 Target Groups                     | Automatically managed — ALB routes to Pod IPs via Service/Ingress          |
| **Scaling**                   | EC2 Auto Scaling Group (ASG)                                   | Horizontal Pod Autoscaler (HPA)                                            |
| **Traffic Routing**           | ALB → Target Group → EC2 (SCG) → Eureka → Backend              | ALB → Ingress → Service → SCG Pods → Eureka → Backend                      |
| **SSL Termination**           | At ALB (manual setup)                                          | At ALB (managed via Ingress annotations)                                   |
| **Discovery Server**          | Eureka servers deployed on EC2                                 | Eureka servers deployed as Kubernetes Pods                                 |
| **Health Checks**             | ALB performs health checks on EC2 instances                    | ALB + Kubernetes perform health checks on Pods                             |
| **High Availability (HA)**    | Multi-AZ EC2 instances + Target Groups                         | Multi-AZ Pods + Kubernetes self-healing + Managed ALB                      |
| **Configuration Management**  | Manually configured (user data, systemd, scripts)              | Declarative YAML manifests (Deployments, Services, Ingress)                |
| **Monitoring & Logging**      | CloudWatch, EC2 metrics, custom agents                         | CloudWatch + Prometheus + FluentBit/CloudWatch Logs                        |
| **Infrastructure Automation** | Terraform / CloudFormation                                     | Helm / Kustomize / Terraform (for EKS)                                     |
| **Resilience**                | Instance restart via ASG                                       | Pod rescheduling by Kubernetes                                             |
| **Cost Optimization**         | Pay per EC2 instance                                           | Pay per Pod usage (on shared worker nodes)                                 |
| **Best For**                  | Simpler setups or legacy environments                          | Modern, containerized, and scalable architectures                          |

---

### ✅ **Key Takeaways**

* **EKS-based setup** is **more dynamic, scalable, and automated** — best for microservice-heavy environments.
* **EC2-based setup** offers **more control** and is easier for smaller or transitional deployments.
* Both architectures can achieve **99.9% uptime** with proper multi-AZ configuration and health checks.

---


