# Data stores
The app uses the following data store:

All these resources are created in your Azure subscription. None are hosted directly by Microsoft.

1.  **Azure Storage Account**
	- [Table] to store user profile card acitivity so that same card can be updated after updating the profile.
	- [Blob] to store bot state to check whether welcome card is sent to the user. Blob storage has container name as "bot-state".

2. **Azure Active Directory**
	- Azure Active Directory is queried using the search keyword and user profiles are fetched based on attributes as skills, interests and schools.
	
## Storage account

**UserProfileActivityInfo Table**

The table has following rows :
|Attribute|Comment  |
|--|--|
| PartitionKey | "UserProfileActivityInfo", which is a constant value.
|RowKey | New guid to uniquely identify user card activity.
|TimeStamp| Date time of row creation/updation.
|MyProfileCardActivityId| Activity id of My profile card.
|MyProfileCardId| Unique id created in app for each my profile card.