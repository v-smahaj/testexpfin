# Cost Estimates

## Assumptions

The estimate below assumes:

-   500 users in the tenant.
-   Each user performs 5 profile updates per month.
-   Each user uses messaging extension search up to 20 times per month	.

## SKU recommendations
The recommended SKU for a production environment is:

 - App Service: Standard (S1)
 
## [](/wiki/costestimate#estimated-load)Estimated load

**Data storage**: up to 1 GB usage of azure table storage.

**Table data operations**:
- Storage is used to maintain the state of user profile activity information which eventually is used to refresh profile card.
- Total number of read calls in storage = 500 users * 5 profile updates/user/month = 2500 profile updates/month.
- Total number of write calls in storage = 500 users * 5 profile updates/user/month = 2500 profile updates/month.

## Estimated cost
**IMPORTANT:**  This is only an estimate, based on the assumptions above. Your actual costs may vary.

Prices were taken from the  [Pricing](https://azure.microsoft.com/en-us/pricing/)  on 14 December 2019, for the West US 2 region.

Use the  [Azure Pricing Calculator](https://azure.com/e/c78364acc9284d6ba36e9b1a33de7a6f)  to model different service tiers and usage patterns.
| Resource | Tier |Load|Monthly price|
|--|--|--|--
| Bot Channels Registration | F0 |N/A|Free
| App Service Plan|S1|744 hours|$74.40
| App Service (Messaging Extension)| -|  |(charged to App Service Plan)|
| Application Insights|-|< 5GB data|(free up to 5 GB)
| Storage account (Table)| Standard_LRS|< 1GB data, 5,000 operations|  $0.05 + $0.01 = $0.06 |
|**Total**|||**$74.46**|