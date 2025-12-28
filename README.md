# SonarQube Community Self-Hosted Server

This repository provides a setup to self-host [SonarQube](https://www.sonarsource.com/products/sonarqube/) using Docker.

---

### Requirements

- Docker version 29 with Compose
- VM with at least 4 GB RAM and 2 vCPU
- Nginx to serve the app to public

---

### Usage

1. Create a `.env` file from `.env.example`.
2. Start the Docker container:
   ```bash
   sudo docker compose up -d
   ```
3. Visit [http://localhost:9010/](http://localhost:9010/) and log in with:
    - **Username**: `admin`
    - **Password**: `admin`  
      Then set a new password.
4. Create a local project and copy its **project key**.
    - http://localhost:9010/projects/create?mode=manual
5. Create a **Project Analysis Token** from:
    - http://localhost:9010/account/security
6. Download
   the [SonarQube Scanner CLI](https://docs.sonarsource.com/sonarqube/latest/analyzing-source-code/scanners/sonarscanner/)
   and run it in your local project like:
   ```bash
    cd ~/projects/node-demo-1
    
    ~/Downloads/sonar-scanner-8.0.1.6346-linux-x64/bin/sonar-scanner \
      -Dsonar.projectKey=node-demo-1 \
      -Dsonar.sources=. \
      -Dsonar.host.url=http://localhost:9010 \
      -Dsonar.token=sqp_52e5795ccf13078f47f7f0895b4a4db9efb45dbd
    ```

---

### Upgrade Guide

1. Update the Docker image version in `docker-compose.yml`.
    - Ensure you use the [Community edition](https://hub.docker.com/_/sonarqube/tags?name=community).
    - Verify that any custom or 3rd-party plugins are compatible with the new version.
2. Commit, push, and deploy the changes to the server.
3. Take a **database backup** before upgrading.
4. Ensure no Gitea Action pipeline is running SonarQube scans.
5. Stop the containers:
   ```bash
   sudo docker compose down
   ```
6. Start the containers:
   ```bash
   sudo docker compose up -d
   ```
7. Open your SonarQube site, e.g., `https://sonarqube.example.com/setup`.
8. Let SonarQube complete the **database migration**.
9. Access SonarQube in the browser as usual.
10. Retry any failed pipelines caused by the upgrade process.

---

### Reference Links

- [SonarQube Community Forum](https://community.sonarsource.com/)
- [SonarQube Scan GitHub Action](https://github.com/SonarSource/sonarqube-scan-action)
- [SonarQube Setup & Upgrade Docs](https://docs.sonarsource.com/sonarqube-community-build/server-installation/introduction)  
