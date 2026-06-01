# Amazon EKS Cluster Infrastructure with AI-Powered GitOps Diagnostics 🚀🤖

An enterprise-grade DevOps pipeline that provisions an Amazon Elastic Kubernetes Service (EKS) cluster using **Terraform** and **Jenkins**, enhanced with a custom, self-healing **Generative AI log analysis layer**.

---

## 🌟 Core Features

* **Infrastructure as Code (IaC):** Modularized Terraform configurations to provision secure, scalable AWS VPC networking, IAM Roles, and an EKS Cluster with managed node groups.
* **Automated Jenkins CI/CD:** A declarative pipeline managing stage validation, execution tracking, resource build states, and environmental cleanup.
* **AI-Driven Troubleshooting:** Features a custom Python integration with the **Google GenAI SDK (`gemini-2.5-flash`)** that automatically catches deployment or syntax failures, processes the terminal logs, and outputs an instant remediation roadmap.

---

## 🧠 Architectural Overview

When an infrastructure build fails (e.g., an invalid AWS credential state, an incorrect subnet group tag, or a Terraform resource limitation), the architecture triggers an automated recovery process:

1. **Pipeline Traps Error:** The `Jenkinsfile` intercepts the shell crash in its `post { failure }` automation block.
2. **Log Consolidation:** The final 100 lines of standard error output are captured into an execution artifact (`error.log`).
3. **GenAI Diagnosis:** The custom engine (`cloud_diagnoser.py`) passes the raw logs to Gemini, securely extracting authentication variables via backend environment properties.
4. **Actionable Remediation Output:** The pipeline prints a structured fix plan directly into the console output, reducing debugging downtime to seconds.

---

## 🛠️ Technology Stack & Tools

* **Cloud Provider:** Amazon Web Services (AWS) — VPC, EKS, EC2, IAM
* **Infrastructure:** Terraform
* **Automation Server:** Jenkins (Declarative Pipeline)
* **AI Integration:** Python 3.11 + Google GenAI SDK (`gemini-2.5-flash`)

---

## 📂 File Architecture

* `Jenkinsfile` — Handles build orchestration, error traps, and credentials management.
* `cloud_diagnoser.py` — The core Python script responsible for log filtering, chunking, and calling the LLM runtime model.
* `main.tf` / `variables.tf` — Terraform templates constructing the cloud topology.

---

## 💼 Enterprise Optimization Note
*To optimize cloud computing consumption budgets and enforce responsible cost management, the live AWS infrastructure components are programmatically torn down when not actively evaluating execution updates. The structural blueprint, pipeline triggers, and self-healing telemetry modules remain fully preserved within the source files for architectural review.*
