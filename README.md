
## 🚀 DevOps Challenge: Productionize an Application

This is a test of your skills in **Docker, Kubernetes (k8s), and DevOps**.

### Your Starting Point

You are given a basic application that is already working and can be started using `docker-compose`.

---

### Your Mission

Your task is to take this basic app and make it **production-ready** for deployment into a Kubernetes cluster.

### Key Requirements

* **Helm Chart:** Both parts of the application must be packaged and installable using a **Helm chart**.
* **Kubernetes Deployment:** The application must be configured to start and run successfully within the k8s cluster.
* **Network Accessibility:** The app must be accessible at the domain `https://target.example.com`.
    * **Note:** We know this domain won't actually resolve. However, your cluster configuration (e.g., an Ingress resource) must be set up correctly *as if* it would.

---

### CI/CD Pipeline Update

The repository also contains a CI/CD tool. You must **update the CI/CD pipeline** with the necessary steps to support this new deployment.

* **Goal:** The pipeline steps themselves do not need to be fully functional. The objective is to demonstrate a basic understanding of how to **structure and set up the pipeline** for this kind of deployment.



### How to make the Test


- Download ore fork this repo
- Update and make your changes
- Submit back by zipping the folder and send to contact point


### You will need 

- Docker
- Helm
- k8s cluster (minikube will work)