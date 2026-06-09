Common developer friction points in Postmark include attempting to send from an unverified sender signature (triggering a `422` error) or mixing up message streams (triggering a `1221` error). 

You need to create an error-handling wrapper function around Postmark's `sendEmail` method that gracefully intercepts and formats these specific API exceptions. 

**Constraints:**
- If a `422` error is thrown, the function MUST return the string: `"Error: Unverified Sender Signature."`
- If a `1221` error is thrown, the function MUST return the string: `"Error: Message Stream Mismatch."`
- Any other errors MUST be thrown normally.