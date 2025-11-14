## 🛠️ Technology Stack

| Category | Tools |
| :--- | :--- |
| **Orchestration** | ![Kubernetes](https://img.shields.io/badge/Kubernetes-%23326CE5.svg?style=for-the-badge&logo=Kubernetes&logoColor=white) ![Minikube](https://img.shields.io/badge/Minikube-%23326CE5.svg?style=for-the-badge&logo=Minikube&logoColor=white) |
| **Security & eBPF** | ![eBPF](https://img.shields.io/badge/eBPF-black?style=for-the-badge&logo=linux&logoColor=white) ![Tetragon](https://img.shields.io/badge/Tetragon-9B59B6?style=for-the-badge) |
| **Observability** | ![Prometheus](https://img.shields.io/badge/Prometheus-%23E6522C.svg?style=for-the-badge&logo=Prometheus&logoColor=white) ![Grafana](https://img.shields.io/badge/Grafana-%23F46800.svg?style=for-the-badge&logo=Grafana&logoColor=white) ![Loki](httpsimg.shields.io/badge/Loki-%23F09541.svg?style=for-the-badge&logo=Loki&logoColor=white) |
| **Configuration** | ![Helm](https://img.shields.io/badge/Helm-%230F1689.svg?style=for-the-badge&logo=Helm&logoColor=white) |
# eBPF-based Runtime Threat Detection for Kubernetes

This project implements an end-to-end DevSecOps workflow to detect, visualize, and alert on runtime security threats in a Kubernetes (Minikube) cluster using eBPF, Tetragon, Prometheus, Grafana, and Loki.

## 🚀 Live Demo

**[Watch the 5-Minute Live Demo Here]**(https://drive.google.com/drive/folders/1VY_yhvc6ipiSG6UAFsYm2tvS8YEd5Bzw?usp=sharing)

This demo shows the full workflow:
1.  Deploying the Tetragon eBPF agent as a DaemonSet.
2.  Simulating two live attacks (malicious shell and file-write).
3.  Capturing the kernel-level system calls (`execve`, `open`) in real-time.

---

## Problem Solved
Traditional security tools often have a "blind spot" in Kubernetes, as they cannot see kernel-level events inside containers. Furthermore, just detecting threats isn't enough; teams need to visualize and correlate these threats over time.

This project solves both problems by using **eBPF (Tetragon)** for deep kernel-level detection and a full **Observability Stack (Prometheus, Grafana, Loki)** to visualize and alert on these threats.

## 🛠️ Technology Stack
* **Orchestration:** Kubernetes (Minikube)
* **Security & eBPF:** Tetragon
* **Observability Stack:**
    * **Metrics:** Prometheus
    * **Logs:** Loki & Promtail
    * **Visualization:** Grafana
* **Configuration:** Helm, Kustomize

## 🎯 Key Achievements
* **Instant Detection:** Successfully detected and alerted on real-time attacks (malicious shell spawn and sensitive file writes) by capturing kernel system calls.
* **High-Fidelity Alerts:** Enriched raw kernel alerts with Kubernetes metadata (pod name, namespace) to create actionable security events.
* **Full Observability Stack:** Engineered a complete monitoring solution by scraping eBPF metrics with **Prometheus** and shipping kernel-level logs to **Loki**.
* **Unified Dashboard:** Designed a **Grafana** dashboard to correlate security events (from Loki) with system performance metrics (from Prometheus) in a single pane of glass.
* [cite_start]**Validated Low-Overhead:** Confirmed minimal performance impact. [cite: 65] [cite_start]The eBPF agent consumed only **6m (milliCPU)** and **~119Mi of Memory** while actively monitoring. [cite: 65]

## 📊 Observability Dashboard

This Grafana dashboard provides a "single-pane-of-glass" view for DevSecOps.

* **Top Panel (Metrics):** Shows real-time alerts and events as time-series data, scraped by **Prometheus** from the Tetragon `/metrics` endpoint.
* **Bottom Panel (Logs):** Shows the *actual* JSON-formatted security alerts with full details, shipped by **Promtail** to **Loki**.

![My Grafana Dashboard](screenshots/grafana-dashboard.png)
## 📄 Project Files
* **`Final_Report.pdf`:** Full academic report, methodology, and performance analysis.
* **`tetragon-daemonset.yaml`:** The Kubernetes manifest for deploying the eBPF agent to all nodes.
* **`test-pod.yaml`:** The manifest for the "victim" pod used for attack simulations.
* **`monitoring/prometheus-values.yaml`:** Helm values file to configure Prometheus to scrape Tetragon.
* **`monitoring/loki-values.yaml`:** Helm values file to configure Promtail to ship Tetragon logs.
* **`monitoring/grafana-dashboard.json`:** The exported JSON model of the custom Grafana dashboard.

## ⚡ How to Run
1.  **Prerequisites:** Ensure you have Minikube and Helm installed.
2.  **Deploy App:**
    ```sh
    # Deploy Tetragon eBPF Agent
    kubectl apply -f tetragon-daemonset.yaml

    # Deploy the victim pod
    kubectl apply -f test-pod.yaml
    ```
3.  **Deploy Monitoring Stack:**
    ```sh
    # Add Helm repos
    helm repo add prometheus-community [https://prometheus-community.github.io/helm-charts](https://prometheus-community.github.io/helm-charts)
    helm repo add grafana [https://grafana.github.io/helm-charts](https://grafana.github.io/helm-charts)
    helm repo update

    # Install Prometheus
    helm install prometheus prometheus-community/kube-prometheus-stack -f monitoring/prometheus-values.yaml

    # Install Loki & Promtail
    helm install loki grafana/loki-stack -f monitoring/loki-values.yaml
    ```
4.  **Import Dashboard:**
    * Log in to your Grafana dashboard.
    * Go to Dashboards > Import.
    * Upload the `monitoring/grafana-dashboard.json` file.
5.  **Simulate Attack & Watch Dashboard:**
    ```sh
    # Open a shell in the victim pod
    kubectl exec -it test-pod -- /bin/sh

    # Run a malicious command (this will be detected)
    /bin/touch /etc/shadow
    ```
