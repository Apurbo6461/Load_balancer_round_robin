# Nginx Round-Robin Load Balancer with Ngrok Setup

A simple, complete implementation of an **Nginx Layer 7 Load Balancer** distributing incoming HTTP traffic across three local backend servers using a round-robin routing algorithm, exposed publicly via **Ngrok**.

---

## 📌 Architecture Overview

```text
               [ Public Internet / Browser ]
                             │
                             ▼
                    [ Ngrok HTTP Tunnel ]
                             │
                             ▼
              [ Nginx Load Balancer (Port 80) ]
                             │
       ┌─────────────────────┼─────────────────────┐
       │ (Round-Robin)       │ (Round-Robin)       │ (Round-Robin)
       ▼                     ▼                     ▼
[ Phase 1 Backend ]   [ Phase 2 Backend ]   [ Phase 3 Backend ]
 127.0.0.1:8081        127.0.0.1:8082        127.0.0.1:8083
  (Blue Header)         (Green Header)         (Red Header)
  
  
Features
Round-Robin Load Balancing: Rotates incoming traffic sequentially across three isolated backend servers.

Custom HTTP Headers: Injects X-Backend response headers to verify which specific backend node served the request.

Cache Control: Configured with Cache-Control: "no-store, no-cache, must-revalidate" to prevent browser caching from disrupting traffic distribution.

Favicon Handling: Intercepts /favicon.ico requests (returns 204 No Content) to avoid consuming round-robin slots during browser testing.

Public Tunneling: Exposed securely via Ngrok for external access and testing.

🛠️ Configuration Details
1. Backends Configuration (conf/phase-backends.conf)
Defines three separate Nginx server blocks listening on 127.0.0.1 on ports 8081, 8082, and 8083, each serving its respective HTML page from /var/www/phase[1-3].

2. Load Balancer Configuration (conf/load-balancer.conf)
Configures an upstream block (phase_servers) using Nginx's default round-robin algorithm and proxies traffic from port 80 to the defined backend pool.

⚙️ How to Deploy Locally
Prerequisites
Linux / Ubuntu 22.04 LTS

Nginx (sudo apt install nginx)

Ngrok (sudo snap install ngrok)
