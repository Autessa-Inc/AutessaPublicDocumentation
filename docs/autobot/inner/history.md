# Viewing Your History
All requests made through the paid features in Autobot are tracked and available in your history table. Check out our [security policy](https://autessa.com/policies#security) to understand how we protect your data. All historical data is role-based - by default, you have access to viewing all interactions with your products. However, if you add additional users to an account, you can limit the data that they are able to see. 

## Terms
**Utterance**: The input sentence from the user 


## For the Bot
In the history tab for the Autobot dashboard, click the "Bot Experience" tab. In this, you will see all the utterances that have been passed through your bot in order of most recent. Every utterance has an associated **utterance id**, this is a unique ID representing the sentence that the user typed into the bot. Every user session is contained in a **conversation id**. To see the historical conversation up until the point the user typed the utterance in question, click the **historical conversation** icon.

Is there more information you'd like to see in the history table that's not currently supported, [let us know](https://autessa.com/contact-us)! 

### Filtering Requests

#### Conversation ID
When you want to see all the records in the history table during a single user session, use the conversation ID to filter. When you submit this request, you will only see values for this specific conversation.

#### Utterance ID
If you want to search for a specific utterance ID in the table, you can use this filter to do so.


#### Experience ID and Version
When you want to filter by experience ID and version, get the experience ID and version from the history table. **SCREENSHOTS** 

If I have the experience ID "055bb5fb-ce11-46f0-8be9-66370a3a1e77" and version 3, I can form the key with a delimeter of === to do the query: **055bb5fb-ce11-46f0-8be9-66370a3a1e77===3**

After submitting this query, the table will be filtered to show only requests made to that experience ID and version. 

#### Date Ranges
For any of the above filters, including none of them, you can filter by a date range to only get interactions for a certain period. 

## For the API
In the history tab on the Autobot dashboard, click the Model API tab to view all historical utterances for the API. Every request to the API is given a unique ID called an **utterance ID**. The prediction from the API is stored in the **predicted intent** column. Using the predicted intent column can help you understand how your API is responding, in aggregation, to different utterances as information to use when training the model again in the future. 

If there's more information you would like to see stored in the history table, [let us know](https://autessa.com/contact-us)!

### Filtering Requests

#### Model ID and Version
When you want to filter by model ID and version, get the model ID and version from the history table. **SCREENSHOTS** 

If I have the model ID "055bb5fb-ce11-46f0-8be9-66370a3a1e77" and version 3, I can form the key with a delimeter of === to do the query: **055bb5fb-ce11-46f0-8be9-66370a3a1e77===3**

#### Utterance ID
If you want to search for a specific utterance ID in the table, you can use this filter to do so.

#### Date Ranges
For any of the above filters, including none of them, you can filter by a date range to only get interactions for a certain period. 
