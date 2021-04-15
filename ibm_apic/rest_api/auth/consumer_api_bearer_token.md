## How to generate the bearer token to be used for the IBM API Connect Platform - Consumer API 

## Question

I need to generate a token for consumer API calls, what are the steps to find the required realm, identity provider and user registry information to use.

## Answer

1. Identify the realm and identity provider information

   For consumer users, the realm context is given by the catalog in which the user is registered. 

   So this is usually of the form:

   `"realm": "consumer:{{org}}:{{cat}}/{{configured_catalog_user_registry-identity_provider-name}}"`

   See the IBM documentation [How to determine the identity provider](https://www.ibm.com/docs/en/api-connect/2018.x?topic=tool-logging-in-management-server#rapic_cli_login__determine_idp)

2. List the user registries for your catalog: 

   `https://{{hostname}}/api/catalogs/{org}/{catalog}/configured-catalog-user-registries`

   For example, here is an example of output of the above call using the provider organization generated user token for a catalog using a local user registry:

   ```
   {
    "type": "configured_catalog_user_registry",
    "api_version": "2.0.0",
    "id": "624d9db8-29f8-4c0c-ac28-39afafe57687",
    "name": "test-catalog",
    "title": "test Catalog User Registry",
    "summary": "test Catalog User Registry",
    "owned": true,
    "integration_url": "https://{{apim-hostmame}}/api/cloud/integrations/user-registry/96883bd5-403c-4019-a7ad-726adbe21eaf",
    "registry_type": "lur",
    "user_managed": true,
    "user_registry_managed": true,
    "onboarding": "active",
    "case_sensitive": false,
    "identity_providers": [
       {
          "name": "test-idp",
           "title": "test Identity Provider"
       }
     ],
       [...] 
   }
   ```

3. Putting all the pieces together :<br>
For an environment where the provider organization is called '**fxp**', the catalog name '**test**' and the identity provider of the configured user registry is called '**test-idp**' (as seen above) then the **realm** is:  

   `"realm": "consumer:fxp:test/test-idp"`


4. Gernerate the token with the above **realm** for a given **registered/signed-up consumer user**:

   ```
    https://{{consumer-hostname}}/api/token' \
    -H 'Content-Type: application/json' \
    -H 'Accept: application/json' \
    --data-raw '{
    "client_id": "{{app-client-id}}",
    "client_secret": "{{app-client-secret}}",
    "grant_type": "password",
    "password": "{{consumer-password}}",
    "realm": "consumer:fxp:test/test-idp",
    "username": "{{consumer-username}}" 
    }'
   ```
   and the output will be:

   ```
   {
    "access_token": "{{access_token_string}}",
    "token_type": "Bearer",
    "expires_in": 18000,
    "refresh_token": "80fdf1da-1949-4ce1-854b-376df0211c9e",
    "refresh_expires_in": 57600
   }
   ```

See the [Open API Explorer version 2018.4.1.x  Documentation](https://apic-api.apiconnect.ibmcloud.com/v2018/) and the one for the [version 10.0.x](https://apic-api.apiconnect.ibmcloud.com/v10/) for the full further information about `Obtaining and Using a Bearer Token`.


<u>**NOTA BENE**</u>: <br>The steps outlined in this document were tested with the versions **2018.4.1.13** and **10.0.1.1** of API Connect. Although they should be valid with all **2018.4.1.x**  and **10.0.0.x** versions, it was not tested with all of them.
<br>
