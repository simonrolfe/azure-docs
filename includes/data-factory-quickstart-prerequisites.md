## Prerequisites

### Azure subscription
If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/) before you begin.

### Azure roles
To create Data Factory instances, the user account that you use to sign in to Azure must be a member of the *contributor* or *owner* role, or an *administrator* of the Azure subscription. In the Azure portal, select your username in the upper-right corner, and select **Permissions** to view the permissions that you have in the subscription. If you have access to multiple subscriptions, select the appropriate subscription. For sample instructions on adding a user to a role, see the [Add roles](../articles/billing/billing-add-change-azure-subscription-administrator.md) article.

### Azure storage account
You use a general-purpose Azure storage account (specifically Blob storage) as both *source* and *destination* data stores in this quickstart. If you don't have a general-purpose Azure storage account, see [Create a storage account](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account) to create one. 

#### Get the storage account name and account key
You use the name and key of your Azure storage account in this quickstart. The following procedure provides steps to get the name and key of your storage account: 

1. In a web browser, go to the [Azure portal](https://portal.azure.com). Sign in by using your Azure username and password. 
2. Select **More services** on the left menu, filter with the **Storage** keyword, and select **Storage accounts**.

   ![Search for a storage account](media/data-factory-quickstart-prerequisites/search-storage-account.png)
3. In the list of storage accounts, filter for your storage account (if needed), and then select your storage account. 
4. On the **Storage account** page, select **Access keys** on the menu.

   ![Get storage account name and key](media/data-factory-quickstart-prerequisites/storage-account-name-key.png)
5. Copy the values for the **Storage account name** and **key1** boxes to the clipboard. Paste them into Notepad or any other editor and save it. You use them later in this quickstart.   

#### Create the input folder and files
In this section, you create a blob container named **adftutorial** in Azure Blob storage. You create a folder named **input** in the container, and then upload a sample file to the input folder. 

1. On the **Storage account** page, switch to **Overview**, and then select **Blobs**. 

   ![Select Blobs option](media/data-factory-quickstart-prerequisites/select-blobs.png)
2. On the **Blob service** page, select **+ Container** on the toolbar. 

   ![Add container button](media/data-factory-quickstart-prerequisites/add-container-button.png)    
3. In the **New container** dialog box, enter **adftutorial** for the name, and then select **OK**. 

   ![Enter container name](media/data-factory-quickstart-prerequisites/new-container-dialog.png)
4. Select **adftutorial** in the list of containers. 

   ![Select the container](media/data-factory-quickstart-prerequisites/seelct-adftutorial-container.png)
1. On the **Container** page, select **Upload** on the toolbar.  

   ![Upload button](media/data-factory-quickstart-prerequisites/upload-toolbar-button.png)
6. On the **Upload blob** page, select **Advanced**.

   ![Select Advanced link](media/data-factory-quickstart-prerequisites/upload-blob-advanced.png)
7. Start **Notepad** and create a file named **emp.txt** with the following content. Save it in the **c:\ADFv2QuickStartPSH** folder. Create the **ADFv2QuickStartPSH** folder if it does not already exist.
    
   ```
   John, Doe
   Jane, Doe
   ```    
8. In the Azure portal, on the **Upload blob** page, browse to and select the **emp.txt** file for the **Files** box. 
9. Enter **input** as a value for the **Upload to folder** box. 

    ![Upload blob settings](media/data-factory-quickstart-prerequisites/upload-blob-settings.png)    
10. Confirm that the folder is **input** and the file is **emp.txt**, and select **Upload**.
    
    You should see the **emp.txt** file and the status of the upload in the list. 
12. Close the **Upload blob** page by clicking **X** in the corner. 

    ![Close upload blob page](media/data-factory-quickstart-prerequisites/close-upload-blob.png)
1. Keep the **Container** page open. You use it to verify the output at the end of this quickstart.