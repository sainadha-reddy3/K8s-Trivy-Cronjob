📌 Kubernetes CronJob-Based Vulnerability Scanning with Trivy
📖 Project Overview

This project demonstrates how to implement scheduled vulnerability scanning inside a Kubernetes cluster using:

Kubernetes CronJob

Trivy container image

Persistent storage (PersistentVolumeClaim - PVC)

RBAC (Role-Based Access Control)

The system automatically scans container images (or the entire Kubernetes cluster) at scheduled intervals and stores the vulnerability reports in structured JSON format.

This implementation focuses on understanding Kubernetes workload behavior and automating security scanning in a Kubernetes-native way.

--------------------------------------------------------------------------------------------------------------------
🎯 Objective

The goal of this project was to:

Understand how Kubernetes CronJobs work

Automate vulnerability scanning using Trivy

Persist scan reports inside Kubernetes

Configure RBAC for cluster-level scanning

Debug and verify Kubernetes workload behavior

Apply Infrastructure as Code (IaC) concepts

This is a learning-focused DevSecOps implementation.

---------------------------------------------------------------------------------------------------------------------------------------------------------
🧠 Why This Project?

In real-world environments:

Container images are updated frequently

New vulnerabilities (CVEs) are discovered daily

Manual scanning is unreliable

Security checks must be automated and continuous

----------------------------------------------------------------------------------------------------------------------------------------------------------------
Problems This Project Solves

❌ Manual vulnerability scanning

❌ No scheduled monitoring mechanism

❌ Scan results being lost after execution

❌ Lack of controlled access to cluster resources

❌ No structured reporting for automation

--------------------------------------------------------------------------------------------------------------------------------------------------
This solution provides a repeatable, automated, Kubernetes-native security mechanism.

🏗 Architecture Overview
Minikube (Local Kubernetes Cluster)
        ↓
CronJob (Scheduled task definition)
        ↓
Job (Created per schedule)
        ↓
Pod (Executes container)
        ↓
Trivy Container
        ↓
Scans Image or Cluster
        ↓
Generates JSON Report
        ↓
Persistent Volume (PVC)
        ↓
Stored Vulnerability Reports

-------------------------------------------------------------------------------------------------------------------------------------------
🛠 Technologies Used & Why
1️⃣ Minikube

Used to create a local Kubernetes cluster.

Why?

Allows safe experimentation

No cloud account required

Simulates real Kubernetes behavior locally

----------------------------------------
2️⃣ Kubernetes CronJob

Used to schedule periodic scans.

Why?

Automates vulnerability scanning

Runs at fixed intervals

Creates Jobs automatically

Supports concurrency and history control

Example schedule:

schedule: "*/5 * * * *"


This runs the scan every 5 minutes.

-----------------------------------------------------------
3️⃣ Kubernetes Job

CronJob creates a Job for each scheduled run.

Why?

Ensures task completion

Handles retry logic

Tracks success/failure state

Automatically creates Pods

--------------------------------------------------------------
4️⃣ Trivy (aquasec/trivy)

Used as the vulnerability scanner.

1. Why?

Open-source

Supports container image scanning

Supports Kubernetes cluster scanning

Generates structured JSON reports

Easy container-based execution

Example image scan:

trivy image nginx:latest


Example cluster scan:

trivy k8s cluster

------------------------------------------------------------------------------------
5️⃣ PersistentVolumeClaim (PVC)

Used to store scan reports.

Why?

Containers are ephemeral

Data inside pods disappears after completion

PVC ensures reports are preserved across runs

Without PVC:
Scan results would be lost when the pod exits.

---------------------------------------------------------------------------------------
6️⃣ RBAC (Role-Based Access Control)

Implemented using:

ServiceAccount

ClusterRole

ClusterRoleBinding

Why?

When performing:

trivy k8s cluster


Trivy needs permission to:

List pods

Read namespaces

Access cluster metadata

RBAC ensures:

Controlled access

Principle of least privilege

Secure cluster interaction

-------------------------------------------------------------------------------------------------
7️⃣ Resource Limits & Concurrency Control

Configured:

concurrencyPolicy: Forbid
successfulJobsHistoryLimit: 3
failedJobsHistoryLimit: 1


Why?

Prevent overlapping scans

Avoid resource exhaustion

Maintain cluster hygiene

Control Job retry behavior

-----------------------------------------------------------------------------------------------------
🔍 How to Verify the Setup
Check CronJob
kubectl get cronjobs
kubectl describe cronjob trivy-scan-cron

Check Jobs
kubectl get jobs

Check Pods
kubectl get pods

View Logs
kubectl logs <pod-name>

Check Stored Reports
kubectl exec -it debug-pod -- sh
ls /reports

----------------------------------------------------------------------------------------------------------------
🧩 Key Concepts Learned

This project helped in understanding:

Kubernetes declarative configuration model

Pod lifecycle management

Difference between Pod, Job, and CronJob

Ephemeral vs persistent storage

RBAC permission model

Debugging using kubectl describe

Job retry and backoff behavior

Infrastructure as Code (IaC) principles

Secure workload design

------------------------------------------------------------------------------------------------------
🔐 Security Considerations

Used a dedicated ServiceAccount instead of default

Applied least-privilege RBAC rules

Restricted Job concurrency

Added CPU and memory resource limits

Used structured JSON reporting for automation
