## Scenario 1

Maintain top 5 search keyword records based on last search results.

**Suggested Solution:** While searching for individuals, the bot will suggest users the search keywords that they had used in the last 5 searches. The user will have the option to select one of the suggested keywords again or use a new keyword for the search.
The bot will maintain the history of the search keywords for each user in associated custom storage. On each search , if a new search keyword is used ,it will be added to the list of search keywords used by the user and the oldest of the keywords will be erased. At any time, the bot will maintain a max of 5 search keywords used by the user in previous searches.

**Pros:** 
- Code extensibility
- Ease of user to see search history

## Scenario 2

The messaging extension will have tabs for Recents.

**Suggested Solution:** The Messaging extention will have tab for recents.This tab will contain 5 entries (user profile information) which were returned as a part of the previous search.

**Pros:** 
- Code extensibility

## Scenario 3

Show user picture in profile card

**Suggested Solution:** Application can be extended to use SharePoint search result to get user profile picture URLs. The same can be embedded in HTML and adaptive card.