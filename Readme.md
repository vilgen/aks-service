## 🔐 Set Secrets in GitHub
Go to your app repo → Settings → Secrets and variables → Actions → New repository secret, and add:

| Name                | Value                                                     |
| ------------------- | --------------------------------------------------------- |
| `AZURE_CREDENTIALS` | JSON output of `az ad sp create-for-rbac --sdk-auth`      |
| `ACR_NAME`          | Your ACR name (e.g., `ewpacrregistry`)                    |
| `ACR_LOGIN_SERVER`  | Your ACR login server (e.g., `ewpacrregistry.azurecr.io`) |


To generate AZURE_CREDENTIALS:

```bash
az ad sp create-for-rbac --name "gh-acr-push" \
  --role contributor \
  --scopes /subscriptions/<sub-id>/resourceGroups/<rg-name>/providers/Microsoft.ContainerRegistry/registries/<acr-name> \
  --sdk-auth
```

Then copy the output JSON into AZURE_CREDENTIALS secret.

✅ What happens on push to main?
- The CI pipeline builds the app as a Docker image

- Tags it with the Git commit SHA

- Pushes it to your ACR under:

```bash
<your-acr>.azurecr.io/fastapi-helloworld:<sha>
```