**Prerequisites**

To begin, you will need:

-   App Service
    
-   App Service plan
    
-   Bot Channels Registration
    
-   Azure Storage account
    
-   Application Insights

A copy of the Expert Finder app GitHub [repo](https://github.com/OfficeDev/microsoft-teams-apps-expertfinder)

### Step 1: Register Azure AD application:
Register one Azure AD applications in your tenant's directory
 1. Log in to the Azure Portal for your subscription, and go to the "App registrations" blade [here](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps).
 2. Click on "New registration", and create an Azure AD application.
	 - **Name**: The name of your Teams app - if you are following the template for a default deployment, we recommend "Expert Finder Bot".
	 - **Supported account types**: Select "Accounts in any organizational directory (Any Azure AD directory - Multitenant)"
	 - For Redirect URI
		 - Select Web
		 - Set the URL to [https://token.botframework.com/.auth/web/redirect](https://token.botframework.com/.auth/web/redirect)	

[[/Images/multitenant_creation.PNG|Register Azure AD application]]

3.  Click on the "Register" button.
4.  When the app is registered, you'll be taken to the app's "Overview" page. Copy the  **Application (client) ID**; we will need it later. Verify that the "Supported account types" is set to  **Multiple organizations**.

[[/Images/multitenant_app_overview.PNG|Register Azure AD application]]

5. On the side rail in the Manage section, navigate to the "Certificates & secrets" section. In the Client secrets section, click on "+ New client secret". Add a description (Name of the secret) for the secret and select an expiry time (As per the requirement). Click "Add".

[[/Images/multitenant_app_secret.PNG|Register Azure AD application]]

6. Once the client secret is created, copy its **Value**; we will need it later.
At this point you have 3 values:
-   Application (client) ID for the bot
-   Client secret for the bot
-   Directory (tenant) ID  

We recommend that you copy these values into a text file, using an application like Notepad. We will need these values later.

7. In the navigation pane, click API permissions to open the API permissions panel. It is a best practice to explicitly set the API permissions for the app.

-   Click Add a permission to show the Request API permissions pane.
-   Select Microsoft APIs and Microsoft Graph : Choose Delegated permissions and make sure the permissions you need are selected. Expert finder requires User.Read permission.
-	Select SharePoint APIs : Choose Delegated permissions and make sure the permissions you need are selected. Expert finder requires AllSites.Read permission.
-   Click Add permissions.

[[/images/Apipermission.png|Register Azure AD application]]

### Step 2: Deploy to your Azure subscription

1.  Click on the "Deploy to Azure" button below.

[![Deploy to Azure](https://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FOfficeDev%2Fmicrosoft-teams-apps-expertfinder%2Fmaster%2FDeployment%2Fazuredeploy.json)

2.  When prompted, log in to your Azure subscription.
3.  Azure will create a "Custom deployment" based on the ARM template and ask you to fill in the template parameters.

 [[/images/customdeploymenttemp.png|Deploy to your Azure subscription]]

4.  Select a subscription and resource group.

-   We recommend creating a new resource group.

- The resource group location MUST be in a datacenter that supports: Application Insights; Storage Account. For an up-to-date list, click  [here](https://azure.microsoft.com/en-us/global-infrastructure/services/?products=logic-apps,cognitive-services,search,monitor), and select a region where the following services are available:
    
    -   Application Insights
    -   Storage Account
    
5.  Enter a "Base Resource Name", which the template uses to generate names for the other resources.

	-   The app service names [Base Resource Name] must be available(not taken); otherwise, the deployment will fail with a Conflict error.
	-   Remember the base resource name that you selected. We will need it later.

6.  Fill in the various IDs in the template:
    
7.  **Bot Client ID**: The application (client) ID of the Microsoft Teams Bot app.
    
8.  **Bot Client Secret**: The client secret of the Microsoft Teams Bot app.
    
9.  **Tenant Id**: The tenant ID where the bot will be installed.

10. **Site Url** : SharePoint site URL. For e.g. https://<<TenantName>>.sharepoint.com/.

11. **Token Encryption Key** : By default, app will generate a unique id. This is used to generate custom JWT for user authentication in HTML based task module.
    
 Make sure that the values are copied as-is, with no extra spaces. The template checks that GUIDs are exactly 36 characters.

12.  If you wish to change the app name, description, and icon from the defaults, modify the corresponding template parameters.

13. Click on "Review+Create" to start the deployment.It will validate the parameters provided in the template .Once the validation is passed,click on create to start the deployment.

    [[/images/Validationpassed.png|Deploy to your Azure subscription]]

14. Wait for the deployment to finish. You can check the progress of the deployment from the "Notifications" pane of the Azure Portal. It can take more than 20 minutes for the deployment to finish.
15. After the deployment is successful, open the deployment details blade and click on "Outputs" option visible in the navigation menu on the left and note down the mentioned values below. We will need them later in the deployment process.
    * **botId:** This is the Microsoft Application ID for the ExpertFinder app. (We will refer to the value as `%botId%` in the following steps.)
    * **appDomain:** This is the base domain for the ExpertFinder app. (We will refer to the value as `%appDomain%` in the following steps.)

    [[/images/output.png|Deploy to your Azure subscription]]

### Step 3: Set up authentication for bot

1. Note the name of the bot that you deployed, which is [BaseResourceName].

2. Go to azure portal [here](https://portal.azure.com/) and search for your bot.

3. Click on the bot in the application list. Under "Settings", click on "Add Setting".

4. Fill in the form as follows:

	a. For Name, enter "ExpertFinderAuth". You'll use it in your bot code.

	b. For Service Provider, select Azure Active Directory. Once you select this, the Azure AD-specific fields will be displayed.

	c. For Client id, enter the application (client) ID that you recorded earlier.

	d. For Client secret, enter the secret that you created to grant the bot access to the Azure AD app.

	e. For Tenant ID, enter the directory (tenant) ID that your recorded earlier for your AAD app.
	This will be the tenant associated with the users who can be authenticated.

	f. For Grant type , enter "authorization_code".

	g. For Login URL , enter "https://login.microsoftonline.com".

	h. For Resource URL , enter "https://graph.microsoft.com/".

	i. For Scopes, enter the names of the permissions you choose from application registration. Enter space separated values:
	User.Read AllSites.Read

5. Click Save.

### Step 4: Create the Teams app packages
Create Teams app package: to install in personal chat.

1.  Open the Manifest\manifest.json file in a text editor.
2.  Change the placeholder fields in the manifest to values appropriate for your organization.

-   developer.name ([What's this?](https://docs.microsoft.com/en-us/microsoftteams/platform/resources/schema/manifest-schema#developer))
-   developer.websiteUrl
-   developer.privacyUrl
-   developer.termsOfUseUrl

4.  Change the <botId> placeholder to your Azure Active Directory application's ID from above. This is the same GUID that you entered in the template under "Bot Client ID".
    
5.  In the "validDomains" section, replace the <validdomains> with your Bot App Service's domain. This will be [BaseResourceName].azurewebsites.net. For example if you chose "ExpertFinder" as the base name, change the placeholder to ExpertFinder.azurewebsites.net.
	Make sure to check "token.botframework.com" is added under valid domains to allow Azure AD authentication using bot OAuth service.

[[/Images/manifest.PNG|Create the Teams app packages]]

6.  Create a ZIP package with the manifest.json,color.png, and outline.png. The two image files are the icons for your app in Teams.

-   Name this package ExpertFinder.zip.
-   Make sure that the 3 files are the  _top level_  of the ZIP package, with no nested folders.
 
### Step 5: Run the apps in Microsoft Teams
1.  If your tenant has sideloading apps enabled, you can install your app by following the instructions  [here](https://docs.microsoft.com/en-us/microsoftteams/platform/concepts/apps/apps-upload#load-your-package-into-teams)
2.  You can also upload it to your tenant's app catalog, so that it can be available for everyone in your tenant to install. See  [here](https://docs.microsoft.com/en-us/microsoftteams/tenant-apps-catalog-teams)
3.  Install the expert finder bot (the ExpertFinder.zip package) to your personal chat.

### Troubleshooting

Please see our [Troubleshooting](https://github.com/OfficeDev/microsoft-teams-apps-expertfinder/wiki/Troubleshooting) page.