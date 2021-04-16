# Playbook4APIMgmt
 My Personal playbook for API Mamagement

### The goal of this reposiory is to provide information and/or tips I have discovered while working API Management tools such as IBM API Connect versions 5.0.8.x, 2018.x and 10.0.x

* [IBM API Connect](./ibm_apic) topics:

   * [Installation / Configuration](./ibm_apic/installation)
      * [How to enable the IBM DataPower Web GUI on a Kubernetes deployment](./ibm_apic/installation/enable_datapower_webgui_k8s.md)
      * [How to enable the IBM DataPower Web GUI on an Openshift deployment](./ibm_apic/installation/enable_datapower_webgui_ocp.md)
      * [How to regenerate expired certificate for a kubernetes installation](./ibm_apic/installation/regenerating_certificate_kubernetes.md)

   * [Migration from version 5](./ibm_apic/migration_from_v5) 
      * [How to extract the data from an API Connect version 5 deployment](./ibm_apic/migration_from_v5/extract_from_version5.md)
      * [How to retain Version 5 vanity endpoint behavior for a given Catalog in API Connect v10.0.x](./ibm_apic/migration_from_v5/retaining_v5_vanity_endpoint_behavior.md)

   * [Using the Toolkit: CLI](./ibm_apic/toolkit)
      * [How to publish large yaml files with the API Connect toolkit](./ibm_apic/toolkit/publish_large_files.md)
      * [How to migrate the plan subscriptions using the IBM API Connnect Toolkit (cli)](./ibm_apic/toolkit/product_subscrition_migration.md)
      * [How to use the SKIP_IBMID_LOGIN environment variable on Microsoft Windows](./ibm_apic/toolkit/skip_login.md)

   * [Using the REST API](./ibm_apic/rest_api)
      * [How to register the Client Application credentials (client id/secret) to user REST API](./ibm_apic/rest_api/auth/registrating_application_creds.md)
      * [How to generate the bearer token to be used for the IBM API Connect Platform - Consumer API](./ibm_apic/rest_api/auth/consumer_api_bearer_token.md)  
      * [How to update the catalog settings with the API Connect REST API](./ibm_apic/rest_api/catalog/updating_catalog_settings.md)  
      
   * [Testing APIs](./ibm_apic/testing_apis)
      * [Why invoking an API in the IBM API Connect Test tab fails with errors ?](./ibm_apic/testing_apis/sandbox_api_testing.md) 

