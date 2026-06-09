Postmark sends real-time webhooks for various email lifecycle events. Processing these events, especially bounces, is critical for maintaining domain reputation and deliverability.

You need to implement an Express.js webhook route (`POST /webhooks/postmark`) that parses the incoming Postmark event payload and marks a user's email as "invalid" in a mocked local database. 

**Constraints:**
- You MUST only trigger the database update if the webhook event type strictly indicates a `Bounce`.
- You MUST ignore and return a `200 OK` status for all other event types (e.g., `Delivery`, `Open`, `Click`).