RG=resourcegroup07
CONTAINER_APP=$(az containerapp list --resource-group $RG --query "[0].name" -o tsv)
CONTAINER_APP_URL=$(az containerapp show --name $CONTAINER_APP --resource-group $RG --query properties.configuration.ingress.fqdn -o tsv)

echo "Swagger UI URL: http://$CONTAINER_APP_URL/v1/swagger-ui/index.html"

http://msagthack-ca-q6ogac6ju54im.kindcliff-3c640071.swedencentral.azurecontainerapps.io/v1/swagger-ui/index.html



az containerapp list --resource-group resourcegroup07 --query "[0].name" -o tsv

az containerapp show --name msagthack-ca-q6ogac6ju54im --resource-group resourcegroup07 --query properties.configuration.ingress.fqdn -o tsv

echo https://msagthack-ca-q6ogac6ju54im.kindcliff-3c640071.swedencentral.azurecontainerapps.io/v1/v3/api-docs

# MCP SERVER URL

https://msagthack-apim-q6ogac6ju54im.azure-api.net/fraud-alert-manager/mcp



az containerapp logs show --name msagthack-ca-q6ogac6ju54im --resource-group resourcegroup07 --follow