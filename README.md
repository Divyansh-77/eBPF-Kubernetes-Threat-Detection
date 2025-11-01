# eBPF-based Runtime Threat Detection for Kubernetes

This project implements an end-to-end DevSecOps workflow to detect runtime security threats in a Kubernetes (Minikube) cluster using eBPF and Tetragon.

## 🚀 Live Demo

**[Watch the 4-Minute Live Demo Here]**([https://drive.google.com/drive/folders/1VY_yhvc6ipiSG6UAFsYm2tvS8YEd5Bzw?usp=sharing])

This demo shows the full workflow:
1.  Deploying the Tetragon eBPF agent as a DaemonSet.
2.  Simulating two live attacks (malicious shell and file-write).
3.  Capturing the kernel-level system calls (`execve`, `open`) in real-time.

---

## Problem Solved
Traditional security tools often have a "blind spot" in Kubernetes, as they cannot see kernel-level events inside containers. This project proves that eBPF can solve this by providing high-fidelity, low-impact monitoring.

## 🛠️ Technology Stack
* **Orchestration:** Kubernetes (Minikube)
* **Security Tool:** eBPF (via Tetragon)
* **Monitoring:** `kubectl top pods`, `tetra getevents`

## 🎯 Key Achievements
* **Instant Detection:** Successfully detected and alerted on real-time attacks (malicious shell spawn and sensitive file writes) by capturing kernel system calls.
* **High-Fidelity Alerts:** Enriched raw kernel alerts with Kubernetes metadata (pod name, namespace) to create actionable security events.
* **Validated Low-Overhead:** Confirmed minimal performance impact using `kubectl top pods`. [cite_start]The agent consumed only **6m (milliCPU)** and **~119Mi of Memory** while actively monitoring. [cite: 65]

## 📄 Project Files
* **`Final_Report.pdf`:**  Full academic report, methodology, and performance analysis.
* **`tetragon-daemonset.yaml`:** The Kubernetes manifest for deploying the eBPF agent to all nodes.
* **`test-pod.yaml`:** The manifest for the "victim" pod used for attack simulations.
