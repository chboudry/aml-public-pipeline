# aml-public-pipeline

Minimalistic example on how to trigger a AML pipeline from a Github pipeline.

This example comes from the following [official azureml example](https://github.com/Azure/azureml-examples/blob/main/.github/workflows/cli-jobs-pipelines-nyc-taxi-pipeline.yml) but I removed all the unecessary bits that we do not use.

## Note 

### Authentication

There are multiple ways for Github runner to authenticate so that it can run actions in Azure : 
- [Create a service principal](https://learn.microsoft.com/en-us/azure/developer/github/connect-from-azure?tabs=azure-portal%2Cwindows#use-the-azure-login-action-with-a-service-principal-secret) and provide the credentials as part of Github secret, it is the easiest way,but as a secret is used, it implies key rotation which is not ideal.
- Use an App registration with [federated crendentials](https://learn.microsoft.com/en-us/azure/developer/github/connect-from-azure?tabs=azure-portal%2Cwindows#use-the-azure-login-action-with-openid-connect). This is fairly new and the benefit is we don't have to store any credentials. This is what we are going to use here.

## Prerequisites

- [Create an app registration within Azure AD with OpenID Connect](https://learn.microsoft.com/en-us/azure/developer/github/connect-from-azure?tabs=azure-portal%2Cwindows#use-the-azure-login-action-with-openid-connect)
- On the resource group the ML workspace is, provide RBAC contributor permission to the app registration (you can reduce permissions depending on your use case)
- Define the following secrets in Github that are going to be use by the Github pipeline to authenticate: 
   - CLIENT_ID : the app registration client ID
   - TENANT_ID : your tenant ID
   - SUBSCRIPTION_ID : your subscription ID
- Define the following secrets in Github that are going to be use by the setup.sh script: 
   - RESOURCE_GROUP_NAME
   - RESOURCE_LOCATION
   - WORKSPACE_NAME
- Run the workflow trough Github UI.

