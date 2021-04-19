# MySQL Backup to Azure Storage

This is a Docker image that will perform a backup of a remote MySQL database server, encrypt the data, and send it to a Azure Storage Account.

### Features

- Backup of remote MySQL server
- Encryption of data with a passphrase
- Decryption information added to the final backup
- Transfers backup to a Azure Storage account

### Test

```
docker run -it --rm \
  --env SOURCE_TYPE="mysql"
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
