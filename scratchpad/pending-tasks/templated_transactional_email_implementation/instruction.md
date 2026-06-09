Postmark allows developers to use pre-defined templates with the Mustachio logic-less template engine, making it easy to send dynamic transactional emails like "Forgot Password" flows.

You need to create a Node.js function that utilizes `sendEmailWithTemplate` to send a password reset email to a dynamically provided recipient address. 

**Constraints:**
- The `TemplateAlias` MUST be set exactly to `"welcome-email"`.
- You MUST pass a `TemplateModel` object containing at least the keys `user_name` and `product_name`.