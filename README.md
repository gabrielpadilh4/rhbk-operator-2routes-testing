# Red Hat build of Keycloak Operator Deployment (with Dual Route Exposure)

This repository contains OpenShift YAML manifests to deploy the Red Hat build of the Keycloak Operator, including a PostgreSQL backing database and exposing the Keycloak instance through two different OpenShift routes.

## Project Overview

This setup is designed for local or testing environments and includes:

- An ephemeral PostgreSQL deployment.
- Secrets for database authentication.
- A `Keycloak` custom resource (CR) for the Red Hat build of Keycloak Operator.
- Two OpenShift routes pointing to the Keycloak service for testing multiple access endpoints.

## Files

| File Name                          | Description |
|-----------------------------------|-------------|
| `1-ephemeral-postgresql.yaml`     | Deploys a PostgreSQL StatefulSet with an accompanying service. |
| `2-keycloak-db-secret.yaml`       | Defines a Kubernetes secret for the Keycloak database credentials. |
| `3-keycloak-crd.yaml`             | The custom Keycloak resource that connects to PostgreSQL and defines runtime configuration. |
| `4-keycloak-testing-route.yaml`   | First route exposing the Keycloak service (`keycloak-testing.apps-crc.testing`). |
| `5-keycloak-testing-second-route.yaml` | Second route exposing the same Keycloak service (`keycloak-testing-second.apps-crc.testing`). |

## Prerequisites

- OpenShift cluster or local CRC instance
- Installed Red Hat build of Keycloak Operator
- Sufficient privileges to apply resources to the `rhbk` namespace

## Deployment Instructions

1. **Create the namespace:**
   ```bash
   oc create namespace rhbk
   ```
2. **Install the Red Hat build of Keycloak Operator**

3. **Apply the manifests in order**
   ```bash
   oc apply -f 1-ephemeral-postgresql.yaml -n rhbk
   oc apply -f 2-keycloak-db-secret.yaml -n rhbk
   oc apply -f 3-keycloak-crd.yaml -n rhbk
   ```

4. **Create routes to expose Keycloak**
   ```bash
   oc apply -f 4-keycloak-testing-route.yaml -n rhbk
   oc apply -f 5-keycloak-testing-second-route.yaml -n rhbk
   ```

5. **Access Keycloak**
   - https://keycloak-testing.apps-crc.testing
   - https://keycloak-testing-second.apps-crc.testing


## Notes
This deployment uses an ephemeral database; all data will be lost if the pod is restarted.
Modify the hostnames in the route files to match your cluster domain if needed.

