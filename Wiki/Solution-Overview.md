# Solution Overview

![Flow_diagram](/wiki/images/Flow_diagram.png)

Expert Finder Bot application has the following components :

 - **Expert Finder Bot**: 
This is a web application built using the [Bot Framework SDK v4 for .NET] (https://docs.microsoft.com/en-us/azure/bot-service/bot-service-overview-introduction?view=azure-bot-service-4.0) and [ASP.NET Core 2.1] (https://docs.microsoft.com/en-us/aspnet/core/?view=aspnetcore-2.1). The bot has a conversational interface with personal scope. It also implements a messaging extension with query commands, which the users can use to see the list of user based on skills, interests and schools. The application is powered by Microsoft Graph and SharePoint REST APIs for managing and searching user profiles respectively.

**Bot Commands**

 - **My Profile**: Responds with the user profile card with the profile information as provided in their AAD. User is presented with his/her profile card. The user profile details are fetched through [Update user](https://docs.microsoft.com/en-us/graph/api/user-update?view=graph-rest-1.0&tabs=http)  API from AAD. The card has two action buttons stated below :
	
	- **View more**: User clicks on "View more" button which toggles a show card with form fields for user attributes as skills,interests and schools. The form fields will not be editable.
		The show card will have a button titled ‘Go to profile’. On click, the user will be redirected to a browser view of his/her AAD profile. This lets the user update any other information included in the outlook profile page and which is outside the scope of the bot.
Click [here] (https://docs.microsoft.com/en-us/graph/api/user-get?view=graph-rest-beta&tabs=http) to know more about get user profile from microsoft graph api.
	- **Edit profile**:
		User clicks on the button to Edit his/her Profile.
		A task module opens which will contain the form fields in an adaptive card layout. The adaptive card will be titled as My profile followed by a suggestive text to the user indicating what to expect/fill out in the form fields. Each form field will have a title and a multiline text box for the user to add appropriate details. Existing information in the user’s profile will be prefilled.
Click [here] (https://docs.microsoft.com/en-us/sharepoint/dev/general-development/sharepoint-search-rest-api-overview#sourceid) to know more about update user profile from microsoft graph api.	
		-	Full Name: A non-editable field that reflects the user’s name as reflected in the User profile.

		-	About Me:  A freeform text entry field for the user to describe themselves. Maximum Length: 300 characters.
	
		-	Interests:  A freeform text entry field for the user to describe their interests. Multiple values to be separated by semi-colons.  Maximum Length: 100 characters.
	
		-	Schools:  A freeform text entry field for the user to describe the schools they have attended.  Multiple values to be separated by semi-colons.  Maximum Length: 200 characters
	
		-	Skills:  A freeform text entry field for the user to enumerate their skills.  Multiple values to be separated by semi-colons.  Maximum Length: 100 characters.


End-user interacting with Expert finder Bot with "My profile" command:

![My profile](/wiki/images/MyprofileShowCard.png)

![Edit profile](/wiki/images/EditProfile.png)

 - **Search**: Responds with the card with suggestive text and a button which invokes the task module to add the details to search for experts.
 The user types in the command **“Search”** to search for experts across the organizations based on their interests/skills or schools they attended.
 The page is built using react and leverages SharePoint REST APIs to provide search results.
 The task module will comprise of the following :

	 - A text box which will be used by the users to type the search keyword.
	 - A filter which will essentially let the users pick the search attributes and the selected attributes will be shown as tag below the search box. The users will have the option to take off a search tag by clicking on the cross mark of the tag. By default, all the values in the filter will be selected.
	 - A search results container will which show the list of individuals returned by the API. Each entry will have a checkbox associated with it.
	 - A button titled ‘View’ which on click will let the users look at the individual profile cards for the selected individuals from the list.

The results will be shown based on the filters selected and in the same sequence the API returns. If the user wants to search specific to attributes for e.g.: only for skills, user can modify the filter and press Enter. The results will be refreshed only when a new search is initiated.

The bot uses SharePoint search Rest API to search for users profiles that matches given search criteria.

SharePoint [local people search](https://social.technet.microsoft.com/wiki/contents/articles/25074.sharepoint-online-working-with-people-search-and-user-profiles.aspx) source id is constant across all SharePoint subscriptions i.e "B09A7990-05EA-4AF9-81EF-EDFAB16C4E31".

End-user interacting with Expert Finder Bot with Search command:

![SearchCard](/wiki/images/SearchCard.png)

![SearchTaskModule](/wiki/images/SearchTaskModule.png)

The user can select one or multiple individuals from the search results up to a maximum of 5 and click on ‘View’. The task module will be closed, and the bot responds with the profile cards for each of the selected individuals. Each profile card will have the following details:
- Name of the User 
- Title or role and 
- Skills  
- The card will also have buttons for user actions as below :
  - **Start a chat**: The user can initiate a personal chat with the individual.
  - **View more**: On user click, a show card would be visible that displays the user’s secondary profile info.

![SelectedUserCard](/wiki/images/SelectedUserCard.png)

**Messaging Extension**: The bot is also accompanied with a messaging extension. The ME will have tabs for:
- **Skills**:  The tab when in focus will contain the list of individuals whose skills matched with the user entered search keyword. By default, it will have a suggestive text. 
- **Interests**: The tab when in focus will contain the list of individuals whose interests matched with the user entered search keyword. By default, it will have a suggestive text. 
- **Schools**:   The tab when in focus will contain the list of individuals whose schools matched with the user entered search keyword. By default, it will have a suggestive text.

![MessagingExtension](/wiki/images/MessagingExtension.png)

 ## Azure table storage
 Azure table storage to store application metadata and user favorite rooms.

 ## Microsoft Graph APIs 

Application leverages below listed Microsoft Graph and SharePoint APIs using user delegated permission.

| **Sr No** |**Use Case**|**API**|**Delegated Permission**|
|-----|---------------|------------------------------------|-------------|
|1|Get user profile details from graph APIs|https://graph.microsoft.com/v1.0/users|User.Read|
|2|Get user profile details from SharePoint APIs |https://<SharepointSteName>.sharepoint.com/_api/search/query?querytext='<SearchQuery>'&sourceid=B09A7990-05EA-4AF9-81EF-EDFAB16C4E31|AllSites.Read|  