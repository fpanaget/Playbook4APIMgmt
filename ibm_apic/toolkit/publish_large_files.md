
## How to publish large yaml files with the API Connect toolkit

### Problem 
When you publish large files using the Command Line Interface (apic), you may get an error such as the one below:

```
2020/11/01 12:00:07 Response dump:
HTTP/1.1 413 Request Entity Too Large
Connection: close
Content-Length: 176
Access-Control-Allow-Credentials: true
Access-Control-Allow-Headers: DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Authorization
Access-Control-Allow-Methods: GET, PUT, POST, DELETE, PATCH, OPTIONS
Access-Control-Allow-Origin: *
Content-Type: text/html
Date: Sun, 01 Nov 2020 09:00:08 GMT
<html>
<head><title>413 Request Entity Too Large</title></head>
<body>
<center><h1>413 Request Entity Too Large</h1></center>
<hr><center>nginx</center>
</body>
</html>
Error: Server responded(code: 413) with an unexpected content-type(text/html).
```


## Diagnostic

While troubelshooting the reason for the error and inspecting the nginx-ingress-controller pod log, you may notice the following:

```
2020/11/01 09:02:46 [warn] 14390#14390: *149896440 a client request body is buffered to a temporary file /tmp/client-body/0000035738, client: 127.0.0.1, server: {{apim-endpoint}}, request: "PATCH /api/orgs/1a3da2ca-d4e1-4833-bca2-e86a1b35bb5c/drafts/draft-apis/scbrokerservices/1.0.0 HTTP/2.0", host: "{{apim-endpoint}}", referrer: "https://{{apim-endpoint}}/manager/{{org-name}}/develop/api/e93fbd67-e583-48b0-9752-6398c08027ae/assembly/"
2020/11/01 09:02:51 [warn] 14390#14390: *149896440 a client request body is buffered to a temporary file /tmp/client-body/0000035739, client: 127.0.0.1, server: {{apim-endpoint}}, request: "PATCH /api/orgs/1a3da2ca-d4e1-4833-bca2-e86a1b35bb5c/drafts/draft-apis/scbrokerservices/1.0.0 HTTP/2.0", host: "{{apim-endpoint}}", referrer: "https://{{apim-endpoint}}/manager/{{org-name}}/develop/api/e93fbd67-e583-48b0-9752-6398c08027ae/assembly/"
2020/11/01 09:03:37 [error] 14388#14388: *149897911 client intended to send too large body: 1242857 bytes, client: 127.0.0.1, server: {{apim-endpoint}}, request: "POST /api/catalogs/{{org-name}}/{{catalog-name}}/publish HTTP/1.1", host: "{{apim-endpoint}}"
```

<u>_Note_</u>:<br>
The same entries should be as well seen in the juhu pod log.
 
### Resolution
To resolve the problem, you will need to increase the limit of the proxy-body-size to a higher value than the default **1m** <br>
(see: https://docs.nginx.com/nginx-ingress-controller/configuration/global-configuration/configmap-resource/)

#### Steps:
1. Verify that the configmap for the  nginx-ingress-controller exists with: <br>
 `Kubectl get cm -n <namespace> | grep ingress-controller`
2. Edit the configmap for the nginx-ingress-controller with: <br>
 `Kubectl edit cm ingress-nginx-ingress-controller -n <namespace>`
3. Change the nginx-ingress-controller to add/modify the `proxy-body-size` to a greater than the default or current value.
4. Save the changes

For further information see: <br>
https://kubernetes.github.io/ingress-nginx/user-guide/nginx-configuration/annotations/#custom-max-body-size

<u>_Note_</u>: <br>
You may need to increase the timeout: proxy-read-timeout and proxy-send-timeout to for example: "240"  as per: <br>
 https://www.ibm.com/support/knowledgecenter/SSMNED_2018/com.ibm.apic.install.doc/tapic_install_K8s_ingress_ctl_reqs.html

<u>**NOTA BENE**</u>: <br>The steps outlined in this document were tested with the versions **2018.4.1.10** and **2018.4.1.13** of API Connect. Although they should be valid with all **v2018.4.1.x** versions, it was not tested with all of them.