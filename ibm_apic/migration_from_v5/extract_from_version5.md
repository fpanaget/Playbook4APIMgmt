## How to extract the data from an API Connect version 5 deployment

Exporting the API Connect version 5 data from an existance instance 
It is as simple as going accessing the primary node with ssh and issuing the command: `config dbextract <options>` <br>

For example:

`config dbextract sftp <ftp_server_name> user ftp_server_user_name`

<u>**NOTA BENE**</u>: <br>The steps outlined in this document were tested with the versions **5.0.8.8** of API Connect. Although they should be valid with all **5.0.8.x ** versions after 5.0.8.5, it was not tested with all of them.