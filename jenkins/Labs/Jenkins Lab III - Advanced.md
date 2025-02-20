
<div style="text-align:center; font-size: 24px;">
<p><strong>🔥 ADVANCED JENKINS TRAINING 🔥</strong></p>
</div>
<div style="text-align:center; font-size: 20px;">
<p><strong>📌Laboratory III Advanced - Jenkins Monitoring, Performance & Optimization</strong></p>
</div>

## 🎯 By the end of this lab, trainees will:
- Configure Jenkins to capture and analyze logs.
- Implement automatic retry mechanisms for failing jobs.
- Monitor disk space and prevent storage-related failures.
- Track CPU and memory usage to prevent system overload.
- Manual cleanup tasks for optimal Jenkins performance.

---

## 📌 Objective
This lab guides you through advanced **failure monitoring**, **performance optimization**, and **agent resource management** in Jenkins to ensure a stable and efficient CI/CD environment.

---

## 1️⃣ Installing and Configuring Monitoring Plugins

### ✅ Objective: Install essential monitoring plugins for Jenkins.
🔹 **Tasks:**

- Navigate to **Jenkins Dashboard > Manage Jenkins > Manage Plugins**.
- Search for and install the following plugins:
  - **Monitoring**
  - **Disk Usage Plugin**
  - **Prometheus Metrics Plugin** (for advanced monitoring)
- Restart Jenkins after installation.

✅ **Outcome:** Jenkins is now equipped with monitoring tools for better performance tracking.

---

## 2️⃣ Monitoring Failures and Alerts in Jenkins

### 📌 Configure Detailed Logs
- Go to **your pipeline**
- Add `timestamps()` after **agent** in your pipeline script for better debugging.
```groovy

  options {
        timestamps() // Adds timestamps to logs for easier debugging
    }

```

---

## 3️⃣ Configuring Prometheus for Jenkins Monitoring

### 📌 Install Prometheus Metrics Plugin in Jenkins

- Install **Prometheus Metrics Plugin** in Jenkins.
- Enable **Expose Prometheus metrics** and set the endpoint (e.g., `prometheus`).

### 📌 Setting up Prometheus Server

#### 1️⃣ Create a Configuration File

- Create a new file named **`prometheus.yml`** on your system. The location is up to you.

  - **Example:** `c:/duarte.silva/prometheus/config/prometheus.yml`

- Paste the following custom configuration into the file:

  ```yaml
  global:
    scrape_interval: 15s  # Metrics collection interval

  scrape_configs:
    - job_name: 'jenkins_job'
      metrics_path: '/prometheus'
      static_configs:
        - targets: ['host.docker.internal:8080']  # Access Jenkins on the local machine
  ```

#### 2️⃣ Run Prometheus with Custom Configuration

Run the following command to start Prometheus with the custom configuration:

```sh
docker run -d --name prometheus -p 9090:9090 -v /path/to/prometheus.yml:/etc/prometheus/prometheus.yml prom/prometheus
```

✅ **Outcome:** Prometheus will now scrape metrics from Jenkins.

### 📌 Confirm the Configuration is Loaded

- Access Prometheus at: [http://localhost:9090](http://localhost:9090)
- Go to **Status → Configuration** to see if your settings are loaded.

### 📌 Verify the Docker Mount

Run the following command to verify that the Prometheus container is correctly using the mounted configuration file:

```bash
docker exec -it prometheus /bin/sh
cat /etc/prometheus/prometheus.yml
```
---

 ### 🔥 Challenge (Optional) - Integrate Grafana with Prometheus
  - Create container for grafana

```bash
docker run -d --name grafana -p 3000:3000 grafana/grafana

 ```
- Open Grafana in web browser: [Grafana](http://localhost:3000/)

#### Integrate Prometheus in Grafana:

- Create Data source and Dashboard to visualize metrics from prometheus
---

## 4️⃣ Monitoring Disk Space and Performance

### 📌 Monitor and Manage Disk Space
- In the **Jenkins Dashboard**, click on Disk Usage.
- Identify the jobs with the **most disk space usage**.
- Go to the specific job, click on **Workspace**, and select **Wipe Out Current Workspace.**
- This will **delete the workspace** for that job, freeing up disk space without affecting - other jobs.


---


