
# CONFIGURATION.md

## Overview
This document describes all configuration options required for the component
in different deployment scenarios. The sections below are mutually exclusive:
use only the one that applies to your deployment.

---

## GUI Configuration
In order for the component to function properly, the following GUI settings
must be configured:

- Select the **Enable Component Integration** checkbox.
- In the **System Settings** panel, choose **Operational Mode: Production**.
- Navigate to **Advanced Settings → Networking** and enable the option
  **Allow External Configuration Files**.
- Set the **Configuration Refresh Interval** to a recommended value (e.g.,
  `30 seconds`).
- In **User Permissions**, ensure the role **System Administrator** has
  read/access rights to the configuration folder.

These GUI steps ensure that the component can read its configuration and
communicate with the surrounding system.

---

## Deployment on Product WildFly
When the component is deployed to the **Product WildFly**, the configuration
file must be located on the same system where WildFly runs.

### Configuration File Location
The configuration file must be present in the directory referenced by the
system variable:

```
${CONFIG_FOLDER}
```

### Requirements
- Ensure that `${CONFIG_FOLDER}` is defined before starting WildFly.
- The folder must be readable by the WildFly system user.
- Place the main configuration file inside the folder (example: `my-config.yaml`).
- If multiple environments exist, ensure each has its own `${CONFIG_FOLDER}`
  (e.g., `/etc/my-app/config`).

---

## Deployment as a Docker Container
If the component runs as a **Docker container**, it requires the
`my-config.yaml` file to be mounted or passed in one of the supported ways.

### Option 1: Mount a local file using docker-compose
```yaml
environment:
  - CONFIG_FILE=/config/my-config.yaml
volumes:
  - ./config/my-config.yaml:/config/my-config.yaml:ro
```

### Option 2: Mount a configuration directory
```yaml
volumes:
  - ./config/:/config/:ro
environment:
  - CONFIG_FILE=/config/my-config.yaml
```

### Option 3: Pass the configuration via environment variable
*(Works only if the component supports inline YAML)*
```yaml
environment:
  - CONFIG_INLINE=$(cat ./config/my-config.yaml)
```

### Option 4: Use Docker secrets (recommended for production)
```yaml
secrets:
  my_config:
    file: ./config/my-config.yaml
services:
  my-service:
    secrets:
      - my_config
```

---

## Additional Recommended Sections
You may want to include the following sections later:

### Troubleshooting
Describe common configuration issues and how to resolve them.

### Validation
Explain how to verify that the configuration is correctly applied.

### Example Configuration File
Provide a minimal example of `my-config.yaml` to help users.

### System Requirements
List OS, CPU, memory, and dependencies required for the component.

