# ZidTech Discovery Service (`discovery-service`)

The **Discovery Service** acts as the central Service Registry (phonebook) for the ZidTech Shopping Cart microservices ecosystem, powered by **Netflix Eureka**. 

In a dynamic microservice architecture where container IP addresses change frequently due to scaling and deployments, the Discovery Service provides dynamic service discovery. This allows the API Gateway and internal microservices to locate each other using abstract names (e.g., `AUTH-SERVICE`) instead of hardcoded IP addresses.

---

## 🌟 Key Architectural Features

* **Dynamic Service Registration:** Microservices automatically register their IP address and port with this server upon startup.
* **Health & Heartbeat Monitoring:** Eureka continuously polls registered services. If a service misses consecutive heartbeats, it is automatically removed from the registry.
* **Client-Side Load Balancing Support:** Works in tandem with Spring Cloud LoadBalancer in the API Gateway to distribute traffic intelligently across available service instances.
* **Visual Dashboard:** Provides a web UI to monitor the real-time health and status of the entire microservice cluster.

---

## 🏗️ Tech Stack & Dependencies

| Technology | Version | Purpose |
| :--- | :--- | :--- |
| **Java** | `21 (LTS)` | Runtime platform |
| **Spring Boot** | `3.2.0` | Core framework |
| **Spring Cloud** | `2023.0.0` | Cloud-native configurations |
| **Netflix Eureka Server** | `4.1.0` | Service registry engine |
| **Spring Boot Actuator** | `3.2.0` | Health metrics & observability |

---

## ⚙️ Configuration (`application.yml`)

For local development, Eureka is configured to run in standalone mode. Self-preservation and peer-fetching are disabled to prevent unnecessary warnings when running a single instance.

```yaml
server:
  port: 8761 # Standard Eureka port

spring:
  application:
    name: discovery-service

eureka:
  instance:
    hostname: localhost
  client:
    # Standalone mode: Do not register with or fetch from other Eureka nodes
    register-with-eureka: false
    fetch-registry: false
    service-url:
      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
  server:
    # Disables the "EMERGENCY" warning for local, single-node development
    enable-self-preservation: false

management:
  endpoints:
    web:
      exposure:
        include: health, info