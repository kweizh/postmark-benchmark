A single API call to the standard Postmark `/email` endpoint has a hard limit of 50 recipients. For sending bulk marketing emails or newsletters, developers must use the batch API and the correct message stream.

You need to implement a batch sending script in Node.js that takes an array of 100 user objects and sends them a promotional email via `POST /email/batch`. 

**Constraints:**
- You MUST target the `broadcast` message stream to avoid mixing marketing emails with transactional traffic.
- You MUST format the request as a batch array and ensure the batch size does not exceed the limit of 500 messages per call.