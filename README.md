

# Jenkins CI/CD Pipeline with Trivy Scan

This repository demonstrates a Jenkins pipeline that builds a Docker image, scans it for vulnerabilities using **Trivy**, and proceeds to deployment only if no **HIGH/CRITICAL** vulnerabilities are detected.

---

## 📂 Pipeline Overview

The Jenkins pipeline performs the following stages:

1. **Checkout the Git repository**

   * Clones the repository from GitHub.

2. **Build Docker Image**

   * Builds a Docker image using the provided `Dockerfile`.
   * Pushes the image to the local Docker registry (`localhost:5000`).

3. **Trivy Scan**

   * Runs a **Trivy vulnerability scan** on the built Docker image.
   * Blocks the pipeline if HIGH or CRITICAL vulnerabilities are found.
   * Saves scan results into `trivy_report.txt`.

4. **Deploy or Push to Production**

   * Executes deployment steps only if the Trivy scan passes.

---

## ⚙️ Environment Variables

The pipeline defines the following environment variables:

| Variable          | Description                                 |
| ----------------- | ------------------------------------------- |
| `GIT_REPO`        | GitHub repository URL                       |
| `GIT_BRANCH`      | Branch to checkout (default: `main`)        |
| `DOCKER_REGISTRY` | Docker registry (default: `localhost:5000`) |
| `IMAGE_NAME`      | Name of the Docker image                    |
| `IMAGE_TAG`       | Image tag (default: `latest`)               |
| `DOCKERFILE_PATH` | Path to the Dockerfile                      |

---

## 🛠️ Prerequisites

* Jenkins with **Pipeline** plugin installed
* Docker installed and running
* Local Docker registry (`localhost:5000`) set up
* [Trivy](https://aquasecurity.github.io/trivy) installed on Jenkins agents

---

## ▶️ Running the Pipeline

1. Push the `Jenkinsfile` to your repository.
2. Configure a Jenkins pipeline job pointing to your GitHub repo.
3. Run the pipeline.

---

## 📜 Example Trivy Report

When vulnerabilities are found, the pipeline saves results in `trivy_report.txt` and fails the build:

```json
{
  "Target": "localhost:5000/myimage:latest",
  "Vulnerabilities": [
    {
      "VulnerabilityID": "CVE-2023-1234",
      "PkgName": "openssl",
      "Severity": "CRITICAL",
      "InstalledVersion": "1.1.1k",
      "FixedVersion": "1.1.1n"
    }
  ]
}
```

---

## ✅ Deployment

If the Trivy scan passes, the pipeline proceeds with deployment steps.
You can add Kubernetes manifests, Helm, or Docker Compose commands in the **Deploy** stage.

---

## 📦 Artifact Storage

* `trivy_report.txt` is archived as a build artifact for every pipeline run.

---

Would you like me to also include a **diagram (workflow pipeline image in Markdown)** so your README looks more professional?
