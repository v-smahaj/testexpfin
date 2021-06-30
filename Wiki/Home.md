# Home
## Expert Finder
Expert Finder Bot allows employees to search for experts in an organization based on their skills, interests and schools.It also provides users the ability to update details into their AAD profile and keep it up to date.

Expert Finder app has following teams capabilities :
- Personal scope bot
- Messaging extension

**Expert Finder bot**
 - **My Profile**: Using this command users will be able to view their AAD profile information in a card. The card will have call to action buttons that will let them add or modify information in their AAD profile or view details about other attributes.

![MyProfileCard](/wiki/images/MyProfileCard.png)

![EditProfile](/wiki/images/EditProfile.png)

 - **Search**: This command allows the users to search for experts within the organization whose attributes match with the search keyword. They will be able to select a max of 5 user profiles and view details pertaining to them.
 
![SearchCard](/wiki/images/SearchFeature.png)

![SearchTaskModule](/wiki/images/SearchTaskModule.png)

 **Expert Finder messaging extention**
 
 Users can search for individuals within the organization whose attributes match the user search keyword using the messaging extension.

![MessagingExtension](/wiki/images/MessagingExtension.png)

**Applicable scenarios:**
1. Best used in a large organization with multiple work locations. 
2. New joinees to initiate personal conversations with individuals who share similar skills/interests or to connect with the school alumni group.

### **Bot Commands**
The supported BOT commands for the app are described in this section.
| **Bot Command** |**Bot Response**
|----------------|-------------------------------|-----------------------------|
|**Help**|Presents a list of commands that the BOT supports |
|**My profile**|Responds with the user profile card with the profile information as provided in Azure Active Directory|
|**Search**|Responds with the card with suggestive text and a button which invokes the task module to add the details to search for experts|

-   [Solution overview](Solution-Overview)

      -   [Data stores](Data-Stores)
      -   [Cost estimates](Cost-Estimates)

-   Deploying the app

    -  [Deployment guide](Deployment-Guide)
    -  [Troubleshooting](Troubleshooting)