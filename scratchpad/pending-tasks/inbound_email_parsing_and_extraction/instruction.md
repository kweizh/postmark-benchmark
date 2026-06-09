Postmark can route incoming emails to a webhook URL, transforming complex MIME data into a clean, easy-to-use JSON payload. This is heavily used for features like "reply-to-comment" or support desk ticketing.

You need to write a parsing function that accepts a simulated Postmark Inbound JSON payload and extracts the necessary data to create a "Comment" object. 

**Constraints:**
- The function MUST extract and return the sender's email address and the parsed text body.
- The function MUST gracefully handle attachments by returning an array of attachment filenames, or an empty array if no attachments exist.