# MySQL Backup to Azure Storage

This is a Docker image that will take a backup of a remote MySQL database server, encrypt the data, and send it to a Azure Storage Account

```

```

-e SOURCE_TYPE: "mysql"
-e SOURCE_NAME: ""
-e SOURCE_HOSTNAME: ""
-e SOURCE_USERNAME: ""
-e SOURCE_PASSWORD: ""
-e DESTINATION_AZURE_STORAGE_CONTAINER_NAME: ""
-e DESTINATION_AZURE_STORAGE_ACCOUNT: ""
-e DESTINATION_AZURE_STORAGE_KEY: ""
-e ENCRYPTION_PASSPHRASE: ""
-e INTEGRATION_HEALTHCHECKSIO_URL: ""
