# MySQL Backup to Azure Storage

This is a Docker image that will take a backup of a remote MySQL database server, encrypt the data, and send it to a Azure Storage Account.

### Features

- Backup of remote MySQL server
- Encryption of data with a passphrase
- Decryption information added to the final backup
- Send

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
