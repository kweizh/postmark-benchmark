Postmark requires specific server tokens for authentication and provides a secure way to test API calls without sending actual emails. Transactional emails should be logically separated from other types using message streams.

You need to write a Node.js script using the `postmark` SDK to send a basic "Hello World" email with both `HtmlBody` and `TextBody`. 

**Constraints:**
- You MUST authenticate using the `POSTMARK_API_TEST` token.
- You MUST explicitly set the `MessageStream` property to `outbound`.