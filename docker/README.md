# Loki Configuration Overview

This document provides a summary of key Loki server configuration settings.

---

## 🔐 Authentication

- **`auth_enabled: false`**  
  Authentication is disabled. Users do **not** need to provide credentials to access Loki.

---

## 🌐 Server Configuration

- **`http_listen_port: 3100`**  
  The Loki server listens for HTTP requests on **port 3100**.

- **`grpc_listen_port: 9096`**  
  The Loki server listens for gRPC requests on **port 9096**.

---

## ⚙️ Common Configuration

- **`instance_addr: 127.0.0.1`**  
  The instance address is set to **localhost**.

- **`path_prefix: /tmp/loki`**  
  Used as the base directory for Loki’s data storage.

### Storage

- **Type:** `filesystem`  
  Loki stores chunks and rules on the **local filesystem**.

- **`chunks_directory: /tmp/loki/chunks`**  
  Directory where log chunks are stored.

- **`rules_directory: /tmp/loki/rules`**  
  Directory where recording and alerting rules are stored.

- **`replication_factor: 1`**  
  Only **one copy** of each piece of data is maintained.

### Ring Configuration

- **Key-Value Store (KVStore):**
  - **`store: inmemory`**  
    Loki uses an **in-memory** key-value store for managing the ring.

---

## 📦 Query Range Configuration

### Results Cache

- **Cache Type:** `embedded_cache`
- **Max Size:** `100 MB`  
  An embedded cache is enabled to improve query performance with a **maximum size of 100 MB**.

---

# Loki + Promtail Configuration

This configuration sets up Loki to receive logs and Promtail to collect and push logs from the local system.

---

## 🔧 Server Configuration

### Loki Server

- **HTTP Listen Port**: `9080`  
  Loki listens for incoming HTTP requests on port `9080`.

- **gRPC Listen Port**: `0`  
  gRPC server port is set to `0`, meaning it will be dynamically assigned by the system.

---

## 📄 Positions

- **File Path**: `/tmp/positions.yaml`  
  Specifies the path where Loki will store the positions of log entries. This helps Loki keep track of which log entries it has already ingested.

---

## 📤 Clients

Promtail pushes log entries to the following Loki server:

```yaml
url: http://loki:3100/loki/api/v1/push
```

---

## 🔍 Scrape Configurations

Defines how and where Promtail scrapes logs from:

### Job: `system`

#### Static Configs

- **Targets**:
  - `localhost`: Promtail scrapes logs from the local machine.

- **Labels**:
  - `job`: `varlogs`
  - `__path__`: `/var/log/*log`  
    Promtail collects all logs matching this path pattern.

---

## 📝 Notes

- Ensure Loki is accessible at the specified URL.
- Make sure `/var/log/*log` contains the logs you wish to collect.
- Update file paths and URLs as needed for your specific environment.
