# DevOps-Test Kubernetes Deployment (Helm + DevSecOps CI/CD)

## Overview

This project demonstrates an **enterprise‑grade DevOps and DevSecOps implementation** of the original Docker‑Compose application, transformed into a **cloud‑native Kubernetes architecture** using **Helm**, supported by a **fully automated CI/CD pipeline** with integrated **code quality, SAST, container security scanning, and DAST testing**.

The solution covers the full lifecycle:

* Application containerization
* Kubernetes workload design
* Helm‑based packaging and configuration management
* Secure secret handling
* Persistent data layer
* Automated build, security scanning, and deployment

This repository is designed to reflect **real‑world production standards** followed by modern Platform, DevOps, and SRE teams.

---

## Architecture

### High‑Level Architecture

```
┌────────────┐        ┌──────────────────────┐
│   GitHub   │──────▶│   GitHub Actions CI/CD │
└────────────┘        └───────────┬──────────┘
                                  │
                     ┌────────────▼────────────┐
                     │   Build & Security Gates  │
                     │  • SonarQube (Quality)    │
                     │  • Trivy (SAST + Image)   │
                     │  • OWASP ZAP (DAST)       │
                     └────────────┬────────────┘
                                  │
                            Docker Registry
                                  │
                         ┌────────▼────────┐
                         │  Kubernetes       │
                         │  Helm Deployment  │
                         └───────┬──────────┘
                                 │
         ┌───────────────────────┼───────────────────────┐
         │                                               │
┌────────▼────────┐                           ┌──────────▼──────────┐
│  Web Deployment  │──▶ Service (web)          │  MySQL Deployment    │
│  Stateless App   │                           │  Persistent Storage │
└──────────────────┘                           └──────────┬──────────┘
                                                           │
                                                  PersistentVolumeClaim
```

---

## Kubernetes Design

| Layer         | Implementation                         |
| ------------- | -------------------------------------- |
| Application   | Kubernetes Deployment                  |
| Database      | Kubernetes Deployment (Stateful‑ready) |
| Networking    | Kubernetes Services                    |
| Configuration | ConfigMaps                             |
| Secrets       | Kubernetes Secrets                     |
| Persistence   | PersistentVolumeClaim                  |
| Packaging     | Helm Chart                             |
| Automation    | GitHub Actions CI/CD                   |

Key design principles applied:

* Immutable container images
* Environment‑agnostic Helm values
* Secure secret externalization
* Service‑based discovery
* Persistent data separation
* DevSecOps shift‑left pipeline

---

## Helm Chart Structure

```
helm/devops-test/
├── Chart.yaml
├── values.yaml
└── templates/
    ├── web-deployment.yaml
    ├── web-service.yaml
    ├── mysql-deployment.yaml
    ├── mysql-service.yaml
    ├── mysql-pvc.yaml
    ├── configmap.yaml
    └── secret.yaml
```

The Helm chart enables:

* Parameterized deployments
* Environment‑specific overrides
* Controlled rollouts and upgrades

---

## Configuration & Secrets Management

### ConfigMap

Used for all non‑sensitive configuration:

* Database host
* Database name
* Application identifiers

### Secrets

Used for all credentials:

* MySQL root password
* Application database password

Passwords are **never hardcoded into deployments** and can be replaced by:

* SealedSecrets
* HashiCorp Vault
* Cloud‑native secret managers

---

## Persistence Strategy

The MySQL layer uses a **PersistentVolumeClaim**:

* Data remains intact across pod restarts
* Enables backup/restore integration
* Supports StatefulSet migration

The original Docker bind mounts were intentionally removed to enforce:

* Image immutability
* Environment parity
* Cloud‑native deployment standards

---

## CI/CD Pipeline

The GitHub Actions pipeline implements a **multi‑stage DevSecOps workflow**:

### Pipeline Stages

1. Source checkout
2. SonarQube / SonarCloud code quality and security scan
3. SAST filesystem vulnerability scan (Trivy)
4. Container build and registry push
5. Container image vulnerability scanning
6. Kubernetes authentication
7. Helm‑based deployment
8. DAST runtime security testing (OWASP ZAP)

### Security Coverage

| Category                | Tool                   |
| ----------------------- | ---------------------- |
| Code quality & security | SonarQube / SonarCloud |
| Static analysis         | Trivy FS scan          |
| Image scanning          | Trivy image scan       |
| Dynamic security        | OWASP ZAP              |

The pipeline is designed to **fail fast** on HIGH or CRITICAL vulnerabilities.

---

## Required Secrets

| Secret             | Purpose                           |
| ------------------ | --------------------------------- |
| DOCKERHUB_USERNAME | Container registry authentication |
| DOCKERHUB_TOKEN    | Container registry token          |
| SONAR_TOKEN        | SonarQube/SonarCloud token        |
| KUBE_CONFIG_DATA   | Base64‑encoded kubeconfig         |

Create kubeconfig secret:

```
cat ~/.kube/config | base64
```

---

## Local Deployment

### Prerequisites

* Kubernetes cluster (Minikube, kind, EKS, AKS, GKE)
* kubectl
* Helm
* Docker

### Install

```
helm install devops-test helm/devops-test
```

Verify:

```
kubectl get pods
kubectl get svc
```

Access service:

```
minikube service web
```

---

## Upgrade & Rollback

Upgrade:

```
helm upgrade devops-test helm/devops-test
```

Rollback:

```
helm rollback devops-test 1
```

---

## Operational Considerations

* Health probes should be added before production
* MySQL can be migrated to StatefulSet
* Horizontal Pod Autoscaler supported
* Backup solution recommended (Velero, cloud snapshots)
* TLS via cert‑manager
* GitOps via ArgoCD or Flux

---

## Security Enhancements (Production Roadmap)

* OPA / Kyverno policy enforcement
* Vault‑backed secrets
* Runtime security (Falco)
* NetworkPolicies
* PodSecurity admission
* Zero‑trust ingress

---

## Why This Design

This implementation demonstrates:

* Cloud‑native architecture principles
* Kubernetes operational maturity
* DevSecOps best practices
* Enterprise‑grade CI/CD design
* Production‑ready Helm packaging

It is structured to meet expectations for:

* Senior DevOps Engineers
* Platform Engineers
* Cloud Architects
* SRE and DevSecOps roles

---

## Author

Designed and implemented as a professional DevOps reference architecture for Kubernetes‑based application modernization and secure continuous delivery.

---

## Status

Fully containerized
Kubernetes‑ready
Helm‑managed
CI/CD automated
Security‑validated
Prod
