# Introduction
In order to track Brave Ads metrics, it sends confirmation events to the server.
For non-Brave Rewards users, the client will send a private and anonymous reduced payload if the user has interacted with Brave Ads. The payload contains the properties as described below.


## Confirmation token payload (non-Brave Rewards Users)
ex. https://search.anonymous.ads.brave.com/v3/confirmation/{transactionId}


| Description | Format | Example | Note |
| ----------- | ------ | ------- | ---- |
| creativeInstanceId | UUID | e4958d00-e35c-4134-a408-1fbcf274d5ae | An id that references the specific ad creative that the user engaged with. This will be the same for any user that engages with this ad. |
| transactionId | UUID | c14d370c-1178-4c73-8385-1cfa17200646 | A unique id for the event that is not re-used. This cannot be linked between ad events or users. |
| type | view, click, landed, conversion | {"type":"view"} | |