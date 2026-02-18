# Release Process

This document describes how the project is built, versioned, and published using Jenkins and Nexus. 
It serves as the single source of truth for release automation and manual triggers.

## Table of Contents

- [Deploying JavaScript Files via SFTP](#deploying-javascript-files-via-sftp)

## Overview

All official artifacts (JAR and Docker image) are built and published through a Jenkins pipeline. 
The pipeline is responsible for:
- compiling the project
- running tests
- packaging the Spring Boot application
- publishing the artifact to Nexus
- building and publishing the Docker image
- optionally triggering deployment jobs

The process is fully automated, but developers can trigger releases manually when needed.

## Jenkins Job

### Job name
```
<your-company>-<project>-release
```

### 2.2 Job URL
(Insert internal Jenkins URL here)

### 2.3 Source branch

- main
- master
- release/*

Only these branches are allowed to produce official artifacts.

## 3. Pipeline Stages

### 3.1 Checkout
- Clone repository
- Validate branch

### 3.2 Maven build
```bash
mvn clean install
```

### 3.3 Run unit + integration tests
```bash
mvn verify
```

### 3.4 Build Spring Boot JAR
The build output is placed in:
```
target/<artifact-name>.jar
```

### 3.5 Build Docker image
```bash
docker build -t my-registry/my-service:${VERSION} -f Dockerfile .
```

### 3.6 Publish JAR to Nexus

Artifact path in Nexus:
```
com/company/app/${VERSION}/app-${VERSION}.jar
```

Publishing is done via:
```bash
mvn deploy
```

### 3.7 Publish Docker image to registry
```bash
docker push my-registry/my-service:${VERSION}
```

## 4. Versioning

Releases follow Semantic Versioning:
```
MAJOR.MINOR.PATCH
```

Version is provided by:
- Jenkins build parameter
- Git tag
- or version file (`version.txt`)

## 5. How to Trigger a Release Manually

### 5.1 Via Jenkins UI
1. Open the release job
2. Click "Build with Parameters"
3. Provide a version (example: 1.14.7)
4. Select environment (optional)
5. Start the build

### 5.2 Via Git tag
```bash
git tag -a v1.14.7 -m "Release 1.14.7"
git push origin v1.14.7
```

## 6. Output of a Successful Release

### Published Nexus artifact
```
https://nexus.company.com/repository/releases/com/company/app/1.14.7/app-1.14.7.jar
```

### Published Docker image
```
my-registry/my-service:1.14.7
```

## 7. Verifying Publication

### Check Nexus
```
com/company/app/<VERSION>/app-<VERSION>.jar
```

### Check Docker registry
```bash
docker pull my-registry/my-service:<VERSION>
```

### Check Jenkins artifacts tab
Contains build logs and generated files.

## 8. Failure Handling

Common issues:
- Build fails due to missing dependencies → check Maven settings
- Tests fail → review failing logs
- Cannot push to Nexus → check credentials/URL
- Docker push fails → check registry permissions

## 9. Release Checklist (manual)

Before triggering a manual release:
- Code reviewed & merged
- Version bumped
- CHANGELOG updated
- Tests pass locally
- Jenkins pipeline healthy
- Nexus & registry available

## 10. Contacts

- Build owner: <team/contact>
- DevOps/ITO team: <contact>
- Release manager: <contact>

### Deploying JavaScript Files via SFTP

#### Downloading Original JavaScript Files From the Server

1. Make sure you have in [.gitignore](../.gitignore) the following section:
    ```text
    # Environment secrets
    .env.secrets
    *.secrets
    ```
2. Create in the root folder of the project `.env.secrets` file with sftp credentials.
    ```text
    # SFTP Credentials
    SFTP_HOST=remote.host
    SFTP_USER=sftpUser
    SFTP_PASSWORD=sftpPassword
    ```

```bash
source ../.env.secrets
sshpass -p "$SFTP_PASSWORD" sftp -P 22 "$SFTP_USER@$SFTP_HOST":/var/www/project/js/ <<EOF
get process-customer.js ./scripts/customer/process-customer.origin.js
EOF
```
  * `user@remote.host:/var/www/project/js/` - remote server and directory where the JS files are stored
  * `process-customer.js` - the file you want to retrieve
  * `./scripts/customer/process-customer.origin.js` - local destination; using .origin.js helps keep the untouched reference copy

#### Deploying Local JavaScript Files to the Server

```bash
source ../.env.secrets
sshpass -p "$SFTP_PASSWORD" sftp -P 22 "$SFTP_USER@$SFTP_HOST":/var/www/project/js/ <<EOF
put ./scripts/customer/process-customer.js
EOF
```
  * `./scripts/customer/process-customer.js` - local file you want to upload
  * `user@remote.host:/var/www/project/js/` - remote target directory on the server


