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

# FRONTEND


RG=resourcegroup07
APIM_NAME=$(az apim list --resource-group $RG --query "[0].name" -o tsv)
echo "API Management: $APIM_NAME"

API Management: msagthack-apim-q6ogac6ju54im

## GET URL

RG=resourcegroup07
APIM_NAME=$(az apim list --resource-group $RG --query "[0].name" -o tsv)
APIM_GATEWAY=$(az apim show --name $APIM_NAME --resource-group $RG --query "gatewayUrl" -o tsv)
echo "API Management Gateway: $APIM_GATEWAY"
# Expected format: https://msagthack-apim-q6ogac6ju54im.azure-api.net

## Create container FRONTEND

RG=resourcegroup07
FRONTEND_APP_NAME="fraud-alert-frontend"
CONTAINER_ENV=$(az containerapp env list --resource-group $RG --query "[0].name" -o tsv)

APIM_NAME=$(az apim list --resource-group $RG --query "[0].name" -o tsv)
APIM_GATEWAY=$(az apim show --name $APIM_NAME --resource-group $RG --query "gatewayUrl" -o tsv)

APIM_SUBSCRIPTION_KEY="4fb781d24349412b86ee451b572b511c"

az containerapp create \
  --name $FRONTEND_APP_NAME \
  --resource-group $RG \
  --environment $CONTAINER_ENV \
  --image ghcr.io/dsanchor/fraud-alert-management-front:main-54f15c4 \
  --target-port 80 \
  --ingress external \
 --secrets \
   api-key=${APIM_SUBSCRIPTION_KEY} \
 --env-vars \
   API_URL=${APIM_GATEWAY} \
   API_KEY=secretref:api-key \
  --cpu 0.5 \
  --memory 1Gi \
  --min-replicas 1 \
  --max-replicas 3

  # GET FRONTEND URL

  FRONTEND_URL=$(az containerapp show \
  --name $FRONTEND_APP_NAME \
  --resource-group $RG \
  --query "properties.configuration.ingress.fqdn" -o tsv)

echo "🎉 Frontend deployed at: https://$FRONTEND_URL"

## CORS Production ready

# Get your frontend URL
FRONTEND_URL=$(az containerapp show --name $FRONTEND_APP_NAME --resource-group $RG --query "properties.configuration.ingress.fqdn" -o tsv)

echo "Update CORS policy to allow: https://$FRONTEND_URL"
