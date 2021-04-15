
## How to replace a product using the API Connect toolkit CLI

### Question 

I have two versions of a product and I want to replace one version with another one so I can keep the existing subscriptions.

### Answer

1. Login (for example using the default apim provider realm) using the command:<br>
  
   `./apic login -s my-apim-server-endpoint -r provider/default-idp-2 -u my-user-name  -p amy-password`

2. Get the list of all the deployed products in your catalog to find the url for the product published to replace with the command:

   `./apic products:list-all -s my-apim-server-endpoint   --scope catalog  -o my-provider-organization -c my-catalog `

   For example :

   ```
   test-product:1.0.0    [state: published]   https://my-apim-server-endpoint/api/catalogs/4c3399b9-d05e-45c7-8709-8c8c5d950113/f649ef0d-9679-4870-9811-9d42944ea3b4/products/101f633d-20c0-4741-9026-cc4937101171
   test-product:1.0.1    [state: staged]      https://my-apim-server-endpoint/api/catalogs/4c3399b9-d05e-45c7-8709-8c8c5d950113/f649ef0d-9679-4870-9811-9d42944ea3b4/products/c8c60db7-8826-4b30-90f1-b4842e91cbfc 
    ```
    
3. Replace the test-product version **1.0.0** with test-product version **1.0.1** that is currently staged in the catalog with the command: <br>

   `./apic products:replace -s my-apim-server-endpoint  test-product:1.0.1 --scope catalog  -o my-provider-organization -c my-catalog  replace.json`

   where `replace.json` contains the **product url** you want to replace that you got in the **step 2** and the **names of the plans** contained in the product)

   For example:
    ```
   {
      "product_url": "https://my-apim-server-endpoint/api/catalogs/4c3399b9-d05e-45c7-8709-8c8c5d950113/f649ef0d-9679-4870-9811-9d42944ea3b4/products/101f633d-20c0-4741-9026-cc4937101171",
      "plans": [
          {
            "source": "default-plan",
            "target": "default-plan"
          }
      ]
   } 
   ```