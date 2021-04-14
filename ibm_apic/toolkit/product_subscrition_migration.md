
## How to migrate the plan subscriptions using the IBM API Connnect Toolkit (cli)

This post shows an example of how to migrate the plan subscriptions of a product with name 'testmigrationsub' from version 1.0.0 to version 1.0.1 

To perform this operation, you need to create the `MIGRATE_SUBSCRIPTION_SUBSET_FILE` file.<br>

This can be done with the following steps:

1. get the url of the product you want to migrate the subscriptions for:

    `./apic products:list-all --scope catalog -c {cat-name} -o {org-name} -s {server-name}`

    or
     
    `./apic products:get {product-name}:{version}  --scope catalog -c {cat-name} -o {org-name} -s {server-name} --output - --fields id,name,url`

    Example:<br>
    `/apic products:list  testmigrationsub -scope catalog -c {cat-name} -o {org-name} -s {server-name}`

    ```
    testmigrationsub:1.0.0    [state: published]   https://{{apim-host}}/api/catalogs/c4433f55-d656-4be3-a613-e6350a14503c/7aa7385c-e398-4f0e-affe-135ece42b046/products/1411cfff-927f-4880-88e7-d916dea2dc21
    testmigrationsub:1.0.1    [state: published]   https://{{apim-host}}/api/catalogs/c4433f55-d656-4be3-a613-e6350a14503c/7aa7385c-e398-4f0e-affe-135ece42b046/products/758d0cdc-2fac-440c-a292-1f454ae107f8
    ```

2. get the list of product plan subscriptions you with to migrate:

   `./apic subscriptions:list -a {app-name} --consumer-org {consumer-org-name} -c {cat-name} -o {org-name} -s {server-name}`
   
   or
   
   `./apic subscriptions:get a170289e-96b0-4500-8b5b-baa8e102334a -a {app-name} --consumer-org {consumer-org-name} -c {cat-name} -o {org-name} -s {server-name} --output - --fields id,name,url,plan`

    ```
    id: a170289e-96b0-4500-8b5b-baa8e102334a
    name: a170289e-96b0-4500-8b5b-baa8e102334a
    url: >-
     https://{{apim-host}}/api/apps/c4433f55-d656-4be3-a613-e6350a14503c/7aa7385c-e398-4f0e-affe-135ece42b046/7a1120e5-e8bd-442f-ae8d-41f4c9ac8865/3a3b4470-cb95-4402-bfbf-591f3d3931ae/subscriptions/a170289e-96b0-4500-8b5b-baa8e102334a
    plan: plan1
    ```

3. Migration to version 1.0.1 the subscription of the 1.0.0 product version:

   `./apic products:migrate-subscriptions {product-name}:{version} --scope catalog -c {cat-name} -o {org-name} -s {server-name} sub-migration.yaml --debug`

   with sub-migration.yaml of the following format:<br>

   ```
   product_url: product_url_to_replace
   subscription_urls:
     - subscription_url1_to_migrate
     - subscription_url2_to_migrate
   plans:
     - source: plan1
       target: plan1
     - source: plan2
       target: plan2
   ```

    The more subsciptions you want to migrate the more you need to add the **subscription_urls** list. <br>  
    The more plans you have in your products, the more you need to add to the list of plan **source/target paire**.

    Example: <br>
    `./apic products:migrate-subscriptions testmigrationsub:1.0.1 --scope catalog -c {cat-name} -o {org-name} -s {server-name} sub-migration.yaml --debug`

   ```
   product_url: https://{{apim-host}}/api/catalogs/c4433f55-d656-4be3-a613-e6350a14503c/7aa7385c-e398-4f0e-affe-135ece42b046/products/1411cfff-927f-4880-88e7-d916dea2dc21
   subscription_urls:
     - https://{{apim-host}}/api/apps/c4433f55-d656-4be3-a613-e6350a14503c/7aa7385c-e398-4f0e-affe-135ece42b046/7a1120e5-e8bd-442f-ae8d-41f4c9ac8865/3a3b4470-cb95-4402-bfbf-591f3d3931ae/subscriptions/a170289e-96b0-4500-8b5b-baa8e102334a
   plans:
     - source: plan1
       target: plan1
     - source: plan-2
      target: plan-2
   ```
   In our case we have one only subscription to migrate.

#### Result:

```
testmigrationsub:1.0.1    [state: published]   https://{{apim-host}}/api/catalogs/c4433f55-d656-4be3-a613-e6350a14503c/7aa7385c-e398-4f0e-affe-135ece42b046/products/758d0cdc-2fac-440c-a292-1f454ae107f8
```
<br>

<u>**NOTA BENE**</u>: <br>
The steps outlined in this document were tested with the versions **10.0.1.1** of API Connect. Although they should be valid with all **10.0.0.x** versions, it was not tested with all of them.