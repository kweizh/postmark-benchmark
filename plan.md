# Postmark Evaluation Dataset Research Report

## 1. Library Overview
*   **Description**: Postmark is a premium transactional email service focused on high deliverability and developer experience. It separates transactional email (e.g., password resets) from broadcast email (e.g., newsletters) using "Message Streams" to ensure critical application emails are never delayed by bulk marketing traffic.
*   **Ecosystem Role**: It is a core infrastructure component for SaaS applications, typically replacing generic SMTP or competing services like SendGrid/Mailgun. It provides deep integration via REST APIs, SDKs (Node.js, Ruby, Python, .NET, PHP, Go), and advanced tooling like an official MCP server and CLI.
*   **Project Setup**:
    1.  **Install SDK**: `npm install postmark` (Node.js).
    2.  **Authentication**: Obtain a **Server API Token** from the Postmark dashboard (Server > Settings > API Tokens).
    3.  **Sender Verification**: Create and verify a **Sender Signature** (single email) or **Domain** (DKIM/Return-Path) before sending.
    4.  **Testing**: Use the specialized test token `POSTMARK_API_TEST` to validate API calls without sending actual emails.

## 2. Core Primitives & APIs
*   **Message Streams**: Logical separators for email types.
    *   `outbound` (Default): For transactional emails.
    *   `broadcast`: For bulk/marketing emails.
    *   `inbound`: For processing incoming emails.
*   **Sending Emails**:
    *   **Single Email**: `POST /email`
    *   **Batch Emails**: `POST /email/batch` (Limit: 500 messages per call).
    *   **Templates**: `POST /email/withTemplate`. Uses the **Mustachio** engine (logic-less templates).
*   **Templates & Layouts**:
    *   **Standard Templates**: Individual email designs.
    *   **Layouts**: Global wrappers (Header/Footer) that can be applied to multiple templates.
*   **Webhooks**: Real-time notifications for `Delivery`, `Bounce`, `SpamComplaint`, `Open`, and `Click`.

### Code Snippets

**Basic Email Sending (Node.js)**
```javascript
const postmark = require("postmark");
const client = new postmark.ServerClient("your-server-token");

client.sendEmail({
  "From": "sender@yourdomain.com",
  "To": "recipient@example.com",
  "Subject": "Hello from Postmark",
  "HtmlBody": "<strong>Hello</strong> dear Postmark user.",
  "TextBody": "Hello dear Postmark user.",
  "MessageStream": "outbound" // Optional, defaults to outbound
}).then(response => {
  console.log(response.To);
  console.log(response.SubmittedAt);
});
```

**Sending with Templates**
```javascript
client.sendEmailWithTemplate({
  "From": "sender@yourdomain.com",
  "To": "recipient@example.com",
  "TemplateAlias": "welcome-email",
  "TemplateModel": {
    "user_name": "John Doe",
    "product_name": "My SaaS App"
  }
});
```

**Webhook Signature Verification (Security)**
```javascript
// Postmark signs webhooks with X-Postmark-Signature
// Note: Verification logic usually involves checking the signature against the raw body
// if using a specific SDK method or manual HMAC validation.
```

## 3. Real-World Use Cases & Templates
*   **SaaS Transactional Flows**: Password resets, magic links, invitation emails, and invoice receipts.
*   **Inbound Email Processing**: Creating a support desk or "reply-to-comment" feature by parsing inbound JSON payloads.
*   **Official Templates**: [activecampaign/postmark-templates](https://github.com/activecampaign/postmark-templates) - Responsive, open-source templates for common SaaS events.
*   **Tooling**: 
    *   [Postmark CLI](https://github.com/wildbit/postmark-cli): For managing templates and servers from the terminal.
    *   [Postmark MCP Server](https://github.com/activecampaign/postmark-mcp): For AI-driven email management.

## 4. Developer Friction Points
*   **Sender Signature Requirement**: Developers often forget they *cannot* send from an unverified email address. This results in a `422 Unprocessable Entity` error.
*   **Message Stream Mismatch**: Attempting to send a broadcast email through a transactional stream (or vice versa) triggers a `1221` error.
*   **Recipient Limits**: A single API call to `/email` has a hard limit of **50 recipients** (To + CC + BCC). For more, developers must use the batch API.
*   **Attachment Size**: Hard limit of **10MB** total message size including attachments.

## 5. Evaluation Ideas
*   **Simple**: Send a "Hello World" email using the `POSTMARK_API_TEST` token.
*   **Intermediate**: Implement a "Forgot Password" flow that sends a templated email with a dynamic reset link.
*   **Intermediate**: Create a script to bulk-migrate 100 email templates from one Postmark server to another using the Templates API.
*   **Advanced**: Build a webhook handler that listens for `Bounce` events and automatically marks the user's email as "invalid" in a local database.
*   **Advanced**: Implement an inbound email parser that converts incoming emails into "Comment" objects in a database, handling attachments correctly.
*   **Advanced**: Configure a "Layout" and multiple "Templates", then send a batch email where some users receive Template A and others Template B, all wrapped in the same Layout.

## 6. Sources
1. [Postmark API Overview](https://postmarkapp.com/developer/api/overview) - Official REST API documentation.
2. [Postmark llms.txt](https://postmarkapp.com/llms.txt) - Structured context for AI models.
3. [Postmark Node.js SDK](https://github.com/wildbit/postmark.js) - Official JavaScript library.
4. [Postmark Templates Repo](https://github.com/activecampaign/postmark-templates) - Open-source transactional email templates.
5. [Postmark MCP Server](https://github.com/activecampaign/postmark-mcp) - Model Context Protocol implementation for Postmark.
6. [Postmark Error Codes](https://postmarkapp.com/developer/api/overview#api-error-codes) - Detailed list of API error responses.