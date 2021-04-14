## How to generate the bearer token to be used for the IBM API Connect Platform - Consumer API 

## Question

## Answer

1. How to identify the realm and identity provider information

   For consumer users, the realm context is given by the catalog in which the user is registered. 

   So this is usually of the form:

   `"realm": "consumer:{{org}}:{{cat}}/{{configured_catalog_user_registry-identity_provider-name}}"`

   See the IBM documentation [How to determine the identity provider](https://www.ibm.com/docs/en/api-connect/2018.x?topic=tool-logging-in-management-server#rapic_cli_login__determine_idp)

The list of user registries for a catalog can be accessed with <br>
`https://{{hostname}}/api/catalogs/{org}/{catalog}/configured-catalog-user-registries`
<br>
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
    "integration_url": "https://{{hostmame}}/api/cloud/integrations/user-registry/96883bd5-403c-4019-a7ad-726adbe21eaf",
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
[..]
```

As a result, for an environment where the provider organization is called 'fxp', the catalog name 'test' and the identity provider of the configured user registry is called 'test-idp' (as seen above) then the **realm** is:  

`"realm": "consumer:fxp:test/test-idp"`

See the [Open API Explorer Documentation](https://apic-api.apiconnect.ibmcloud.com/v10/) for the full description of the API Connect REST API endpoints.


<u>**NOTA BENE**</u>: <br>The steps outlined in this document were tested with the versions **10.0.1.1** of API Connect. Although they should be valid with all **10.0.0.x** versions, it was not tested with all of them.
<br>

