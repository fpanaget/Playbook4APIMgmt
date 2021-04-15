## How to register the Client Application credentials (client id/secret) to use with the API Connect REST API

## Question

What are the steps required to registration an application (with its client id and secret) to be able to generate token access to allow API Connect REST API calls?

## Answer

1. Registration an application (with its own **client id** and **secret**) to use with the REST API

    1. First login in to the cloud administration organization 
   for example the default identity provider for the Cloud Manager User Registry 

       ` ./apic login -s {{mgmt_endpoint_name}} -r admin/default-idp-1 -u {{admin_user}} -p {{admin-user-password}}}`


    2. Create a json file (eg `my-toolkit-app-registration.json`) with containing for example :

       ```
       { 
          "name": "my-toolkit-app-registration",
         "client_id": "{{my-client-id}}", 
         "client_secret": "{{my-client-secret}}",
         "client_type": "toolkit"
        }
       ```
       Note: `client_type` can be one of the followings:

            portal
            gateway
            toolkit
            consumer_toolkit
            ui
            consumer_ui
            ibm_cloud
            migration
            juhu

    3. Create the application registration with the command with your `my-toolkit-app-registration.json` file:
    
       `./apic registrations:create  -s {{mgmt_endpoint_name}} my-toolkit-app-registration.json`
      
       Result:
       ```
       my-toolkit-app-registration    [state: enabled]   https://{{mgmt_endpoint_name}}/api/cloud/registrations/d2382964-8368-4b67-8439-4119350b25ee
        ```

See the [Open API Explorer version 2018.4.1.x  Documentation](https://apic-api.apiconnect.ibmcloud.com/v2018/) and the one for the [version 10.0.x](https://apic-api.apiconnect.ibmcloud.com/v10/) for the further informaton on `Obtaining a Client ID and Secret`.


<u>**NOTA BENE**</u>: <br>The steps outlined in this document were tested with the versions **2018.4.1.13** and **10.0.1.1** of API Connect. Although they should be valid with all **2018.4.1.x**  and **10.0.0.x** versions, it was not tested with all of them.
<br>
