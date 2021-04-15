
## How to use the SKIP_IBMID_LOGIN environment variable on Microsoft Windows

### Question

When starting the designer, a login screen for the IBM cloud pops up.  
The [instructions below](https://www.ibm.com/docs/en/api-connect/10.0.1.x?topic=toolkit-logging-in-from-api-designer)  mentioned that an environment variable needs to be set in order to stop this from happening. 
> Set SKIP_IBMID_LOGIN=true as an environment variable to bypass the IBM ID login screen for API Designer. This applies to users who are working on an internal network, without access to external networks.

Although this variable is set, you still are asked for an IBM login.

### Answer

If you try to set the variable from a batch file each time you start the api_designer.exe such as:
```
set SKIP_IBMID_LOGIN=true
echo %SKIP_IBMID_LOGIN%
api_designer-win.exe
```
It may not be found and resulting in still requesting an IBM login.

Instead, make sure to set the `SKIP_IBMID_LOGIN=true` variable at the Operation System level the variable so that each time you invoke the designer exe from a command prompt, the variable is correctly picked up.


Here are the steps to following to set an environment variable:
1. Right-click the **Computer** icon and choose **Properties**, or in **Windows Control Panel**, choose **System**.
3. Choose **Advanced system settings**. ...
4. On the **Advanced** tab, click **Environment Variables**. ...
5. Click **New** to create a new environment variable under the system variables pane with the **name** `SKIP_IBMID_LOGIN` and the **value** `true`


