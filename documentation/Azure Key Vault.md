Storing API Key in Azure Key Vault
=========================================

You can store your Profisee REST Gateway API keys in an Azure Key Vault. Azure Data Factory retrieves the API key when executing an activity that uses the REST linked service.

Prerequisites
-------------
This feature relies on the data factory managed identity. Learn how it works from [Managed identity for Data factory](https://docs.microsoft.com/en-us/azure/data-factory/data-factory-service-identity) and make sure your data factory have an associated one.

Steps
-----
1. Click in the value field then click **Add Azure Key Vault** to start the process to your store the value in Azure Key Vault.  
2. Creating a new Key Vault
   1. Select **+ New** to create a new Key Vault.
   2. Select your Azure subscription.
   3. Select **+ New** in the **Azure key vault name** dropdown list.
   4. A new browser tab will open and you will be navigated to the **Create key vault** page.
   5. Fill out all the necessary information in the **Basics** tab.
   6. Advance to the **Access policy** tab and click on **+ Add Access Policy**.
      1. Your Key Vault will need an Access Policy allowing the Data Factory's managed identity access.
      2. You can ignore Configure from template, Key permissions, Certificate permissions, and Authorized application.
      3. Secret permissions: **Get**
      4. Select principal: Click on **None selected**.  Search for the name of your Data Factory.  Click on it then click on **Select**.
      5. Click on **Add**
   7.  Click on **Review + Create**.
   8.  Go back to your Azure Data Factory browser tab and click on the refresh button to the right of the **Azure key vault name** dropdown. You should now see your new key vault in the list.
3. Select an existing Key Vault
   1. Select the Key Vault name from the dropdown list.
   2. Click on the **Grant Data Factory service managed identity access to your Azure Key Vault.** link.
   3. This will navigate to your key vault's Access Policies screen to add the necessary access policy.
	  4. You can ignore Configure from template, Key permissions, Certificate permissions, and Authorized application.
	  5. Secret permissions: **Get**
	  6. Select principal: Click on **None selected**.  Search for the name of your Data Factory.  Click on it then click on **Select**.
	  7. Click on **Add**
	  8. Click on **Save** on the Access policies screen.
4. Enter the Secret name you are going to use (e.g. profisee-rest-gateway-api-key).
5. Navigate to your key vault to add the API Key as a secret. 
   1. Click on **Secrets** in the left nav panel.
   2. Click on **Generate/Import**.
   3. Leave **Upload options** set to **Manual**.
   4. Give your secret a name (e.g. profisee-rest-gateway-api-key).
   5. Enter the API Key value in the Value field.
   6. Click **Create**.
6. You may need to refresh your Data Factory in the browser to pick up the new Key Vault settings.

More Information
----------------

-	[Store credential in Azure Key Vault](https://docs.microsoft.com/en-us/azure/data-factory/store-credentials-in-key-vault)

-   [Managed identity for Data factory](https://docs.microsoft.com/en-us/azure/data-factory/data-factory-service-identity) 

