# Kubernetes and Helm Resources for API Manager and WSO2 Identity as Key Manager

*Kubernetes and Helm Resources for container-based deployments of WSO2 API Manager and Identity Server as KM deployment

## Deploy Kubernetes resources

In order to deploy Kubernetes resources , follow the **Quick Start Guide
given below:

## Deploy Helm resources

In order to deploy Helm resources for each deployment pattern, follow the **Quick Start Guide** for each deployment pattern
given below:

* [A simple deployment of WSO2 API Manager and Identity Server as KM](advanced/am-is-pattern-1/README.md)

## Configure Identity Server as Key manager 

First you need to obtain the consumer key/secret key pair by calling the dynamic client registration (DCR) endpoint. You can add your preferred grant types in the payload. A sample payload is shown below.
  `{
  "callbackUrl":"www.google.lk",
  "clientName":"rest_api_admin",
  "owner":"admin",
  "grantType":"client_credentials password refresh_token",
  "saasApp":true
  }`
Create a file (payload.json) with the above sample payload, and use the cURL shown bellow to invoke the DCR endpoint. Authorization header of this should contain the base64 encoded admin username and password. Format of the request

  `curl -X POST -H "Authorization: Basic Base64(admin_username:admin_password)" -H "Content-Type: application/json"
  \ -d @payload.json https://<host>:<servlet_port>/client-registration/v0.17/register`
  
Sample payload.json can be found in data/payload.json
Sample request `curl -X POST -H "Authorization: Basic YWRtaW46YWRtaW4=" -H "Content-Type: application/json" -d @payload.json https://am.wso2.com/client-registration/v0.17/register -k`

Following is a sample response after invoking the above curl.
`{
"clientId": "fOCi4vNJ59PpHucC2CAYfYuADdMa",
"clientName": "rest_api_admin",
"callBackURL": "www.google.lk",
"clientSecret": "a4FwHlq0iCIKVs2MPIIDnepZnYMa",
"isSaasApplication": true,
"appOwner": "admin",
"jsonString": "{\"grant_types\":\"client_credentials password refresh_token\",\"redirect_uris\":\"www.google.lk\",\"client_name\":\"rest_api_admin\"}",
"jsonAppAttribute": "{}",
"tokenType": null
}`

Next you must use the above client id and secret to obtain the access token. We will be using the password grant type for this, you can use any grant type you desire. You also need to add the proper scope when getting the access token. All possible scopes for Admin REST API can be viewed in OAuth2 Security section of this document and scope for each resource is given in authorizations section of resource documentation. Following is the format of the request if you are using the password grant type.

`curl -k -d "grant_type=password&username=<admin_username>&password=<admin_passowrd>&scope=<scopes seperated by space>"
\ -H "Authorization: Basic base64(cliet_id:client_secret)"
\ https://<host>:<gateway_port>/token`

Sample `request curl https://gateway.am.wso2.com/token -k -H "Authorization: Basic MF9FekhMTV9WOHc3eUFiZXQ1ZEl1bkpJTVJBYTplQ2RNS252X1V1ZVVmM1FsbE1RQWVFNVkyY01h" -d "grant_type=password&username=admin&password=admin&scope=apim:admin apim:tier_view" -k`


Next add a new API Key Manager

Sample request `curl -k -X POST -H "Authorization: Bearer <access-token>" -H "Content-Type: application/json" -d @data1.json "https://am.wso2.com/api/am/admin/v1/key-managers"`

Sample data1.json can be found in data/data1.json


Then get all the key managers to find the default key manager id which we will be using to disable the default key manager

Sample request `curl -k -H "Authorization: Bearer <access-token>" "https://am.wso2.com/api/am/admin/v1/key-managers"`

Sample response 
`{"count":2,"list":[{"id":"2e9be751-57de-4e26-82bf-5d97b7d49361","name":"WSO2 Identity Server","type":"WSO2-IS","description":"This is a key manager for Developers","enabled":true},{"id":"9e814607-0f92-4205-ae03-e20d6b7a4a98","name":"Resident Key Manager","type":"default","description":"This is Resident Key Manager","enabled":true}]}`


Finally disable Resident Key Manager 

The id of Resident key manager has to be passed in the url as shown in the following sample request

Sample request `curl -k -X PUT -H "Authorization: Bearer eca2de3e-7740-3907-90b8-fa97c3acb019" -H "Content-Type: application/json" -d @data2.json "https://am.wso2.com/api/am/admin/v1/key-managers/9e814607-0f92-4205-ae03-e20d6b7a4a98"`

data2 file can be found in data/data2.json 




  
