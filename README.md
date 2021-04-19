# MySQL Backup to Azure Storage

This is a Docker image that will perform a backup of a remote MySQL database server, encrypt the data, and send it to a Azure Storage Account.

## Features 🚀

- Backup of remote MySQL server
- Encryption of data with a passphrase
- Decryption information added to the final backup
- Transfers backup to a Azure Storage account

## Documentation

### Environment variables

| Name                                       | Description                                     |
| ------------------------------------------ | ----------------------------------------------- |
| `SOURCE_TYPE`                              | `mysql`                                         |
| `SOURCE_NAME`                              | Database server name (will be used in filename) |
| `SOURCE_HOSTNAME`                          | Database hostname                               |
| `SOURCE_USERNAME`                          | Backup username                                 |
| `SOURCE_PASSWORD`                          | Backup password                                 |
| `DESTINATION_AZURE_STORAGE_CONTAINER_NAME` | Azure container name, e.g. `backup`             |
| `DESTINATION_AZURE_STORAGE_ACCOUNT`        | Azure container storage account name            |
| `DESTINATION_AZURE_STORAGE_KEY`            | Azure container storage access key              |
| `ENCRYPTION_PASSPHRASE`                    | Passphrase used to encrypt backup               |
| `INTEGRATION_HEALTHCHECKSIO_URL`           | HealthChecks.io endpoint                        |

## Usage

### Docker

```docker
docker run -it --rm \
  --env SOURCE_TYPE="mysql" \
  --env SOURCE_NAME="" \
  --env SOURCE_HOSTNAME="" \
  --env SOURCE_USERNAME="" \
  --env SOURCE_PASSWORD="" \
  --env DESTINATION_AZURE_STORAGE_CONTAINER_NAME="" \
  --env DESTINATION_AZURE_STORAGE_ACCOUNT="" \
  --env DESTINATION_AZURE_STORAGE_KEY="" \
  --env ENCRYPTION_PASSPHRASE="" \
  --env INTEGRATION_HEALTHCHECKSIO_URL="" \
ghcr.io/sydnod/mysql-backup-azure-storage:latest \
```

### Kubernetes

```yml
apiVersion: v1
kind: ConfigMap
metadata:
  name: mysql-backup-config
  labels:
    app: mysql
data:
  SOURCE_TYPE: "mysql"
  SOURCE_NAME: ""
  SOURCE_HOSTNAME: ""
  SOURCE_USERNAME: ""
  SOURCE_PASSWORD: ""
  DESTINATION_AZURE_STORAGE_CONTAINER_NAME: ""
  DESTINATION_AZURE_STORAGE_ACCOUNT: ""
  DESTINATION_AZURE_STORAGE_KEY: ""
  ENCRYPTION_PASSPHRASE: ""
  INTEGRATION_HEALTHCHECKSIO_URL: ""
---
apiVersion: batch/v1beta1
kind: CronJob
metadata:
  name: mysql-backup
spec:
  schedule: "0 4 * * *"
  concurrencyPolicy: Forbid
  jobTemplate:
    spec:
      template:
        spec:
          restartPolicy: OnFailure
          containers:
            - name: mysql-backup
              image: ghcr.io/sydnod/mysql-backup-azure-storage:latest
              resources:
                requests:
                  memory: "256Mi"
                  cpu: "100m"
                limits:
                  memory: "256Mi"
                  cpu: "500m"
              env:
                - name: SOURCE_TYPE
                  valueFrom:
                    configMapKeyRef:
                      name: mysql-backup-config
                      key: SOURCE_TYPE

                - name: SOURCE_NAME
                  valueFrom:
                    configMapKeyRef:
                      name: mysql-backup-config
                      key: SOURCE_NAME

                - name: SOURCE_HOSTNAME
                  valueFrom:
                    configMapKeyRef:
                      name: mysql-backup-config
                      key: SOURCE_HOSTNAME

                - name: SOURCE_USERNAME
                  valueFrom:
                    configMapKeyRef:
                      name: mysql-backup-config
                      key: SOURCE_USERNAME

                - name: SOURCE_PASSWORD
                  valueFrom:
                    configMapKeyRef:
                      name: mysql-backup-config
                      key: SOURCE_PASSWORD

                - name: DESTINATION_AZURE_STORAGE_CONTAINER_NAME
                  valueFrom:
                    configMapKeyRef:
                      name: mysql-backup-config
                      key: DESTINATION_AZURE_STORAGE_CONTAINER_NAME

                - name: DESTINATION_AZURE_STORAGE_ACCOUNT
                  valueFrom:
                    configMapKeyRef:
                      name: mysql-backup-config
                      key: DESTINATION_AZURE_STORAGE_ACCOUNT

                - name: DESTINATION_AZURE_STORAGE_KEY
                  valueFrom:
                    configMapKeyRef:
                      name: mysql-backup-config
                      key: DESTINATION_AZURE_STORAGE_KEY

                - name: ENCRYPTION_PASSPHRASE
                  valueFrom:
                    configMapKeyRef:
                      name: mysql-backup-config
                      key: ENCRYPTION_PASSPHRASE

                - name: INTEGRATION_HEALTHCHECKSIO_URL
                  valueFrom:
                    configMapKeyRef:
                      name: mysql-backup-config
                      key: INTEGRATION_HEALTHCHECKSIO_URL
```

## Output

Exmaple output

```
SCRIPT: Start

Loading healthchecks...done

Backup source: MySQL

Backup of 'mysql'...done
Backup of 'sys'...done
Backup of 'test1'...done

Packaging...done
Encrypting...done
Zipping...done
Creating destination container...done

Uploading the file to Azure storage...
Finished[#############################################################]  100.0000%
```

## Other

### Backup

- [`sydnod/postgres-backup-azure-storage`](https://github.com/sydnod/postgres-backup-azure-storage)
