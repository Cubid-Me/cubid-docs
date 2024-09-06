## CUBID Webhooks (TBD - coming soon)

CUBID provides a comprehensive suite of webhooks designed to help App developers and founders stay informed about key changes in user identity and trust scores. These webhooks allow Apps to receive real-time updates about users’ identity actions, enabling them to respond effectively to identity strengthening, potential threats (such as Sybil attacks), and changes in trust levels.

By integrating with CUBID’s webhooks, Apps can dynamically adapt to identity events across the ecosystem, helping improve security, trust, and the overall user experience.

---

### **Overview**

CUBID’s webhooks notify Apps about events related to user identity, such as stamps being blacklisted, trust scores increasing or decreasing, and account merges. These real-time events allow Apps to take appropriate actions, such as updating permissions, sending notifications, or locking accounts.

Webhooks are triggered whenever a significant event occurs within the CUBID ecosystem that affects a user connected to your App. The webhooks provide essential data for Apps to react to changes in user trustworthiness and security without requiring continuous polling.

While you will only receive webhooks for users that registered with our App, webhook triggers could originate from within your App or from any other App within the CUBID ecosystem. 

---

### **Subscription**

To set up webhooks, App developers must register their webhook URLs either through the **Admin Console at [console.cubid.me](https://console.cubid.me)** or via the API. Once registered, the system will automatically send event notifications to the provided URL when relevant events occur. A webhook secret will also be provisioned, which is required for verifying the authenticity of incoming requests.

#### **Registering a Webhook URL Using the Admin Console**

Developers can easily register their webhook URLs through the **Admin Console**:

1. Log in to [console.cubid.me](https://console.cubid.me).
2. Navigate to the **Webhooks** section under your app settings.
3. Provide the endpoint URL where webhook events will be sent.
4. Select the events you wish to subscribe to (e.g., `"blacklisted_stamp"`, `"score_increased"`, `"stamp_removed"`).
5. Upon successful registration, a webhook secret will be generated and displayed in the console.

This webhook secret is essential for validating incoming webhook requests (see **Webhook Secret** section below for details on verifying signatures).

#### **Registering a Webhook URL Using the API**

For programmatic webhook registration, developers can also register via the API. To register or unsubscribe from a webhook, both an `apikey` and a `dapp_id` must be passed in the request, and both should be UUIDv4 format.

##### **API to Register a Webhook:**

```json
POST /api/v2/webhooks/register
{
  "url": "https://your-dapp.com/webhook-endpoint",
  "events": ["blacklisted_stamp", "score_increased", "stamp_removed"],
  "apikey": "your-api-key",
  "dapp_id": "your-dapp-id"
}
```

- `url`: The endpoint in your application that will handle webhook events.
- `events`: An array of event names that you wish to subscribe to.
- `apikey`: The unique API key provisioned for your app.
- `dapp_id`: The UUIDv4 identifier for your dapp.

To unsubscribe, send a similar request to the **unsubscription API**, using the same parameters.

### **Configuration / Webhook Secret**

When registering a webhook (either via the Admin Console or the API), CUBID provides a webhook secret. This secret is crucial for ensuring the authenticity of incoming webhook requests.

#### Signature
CUBID sends a signature header (`X-Cubid-Signature`) with each webhook request. The signature is an HMAC-SHA256 hash of the payload, signed using your webhook secret. Your app should verify this signature before processing the payload to ensure it hasn’t been tampered with.

**Example in Node.js:**

```js
const crypto = require('crypto');

function verifyWebhookSignature(payload, signature, secret) {
  const hash = crypto
    .createHmac('sha256', secret)
    .update(payload)
    .digest('hex');
  
  return hash === signature;
}
```

You can retrieve the webhook secret from the **Admin Console** under your app’s webhook settings. Use this secret to configure your app for signature verification.

---

### **Summary of CUBID Webhooks**

CUBID offers a range of webhooks that provide real-time notifications about important identity events for users interacting with your App. These webhooks help Apps manage user accounts dynamically, ensure security, and maintain up-to-date identity data across the CUBID ecosystem.

1. **Blacklisted Stamp (Sybil Attack Detected)**: Triggered when a user's stamp is blacklisted due to suspicious activity or duplicate accounts. Useful for locking accounts or prompting corrective actions.

2. **Merged Users**: Triggered when a user merges duplicate accounts, indicating one account is un-blacklisted and the other is marked obsolete.

3. **Score Increased**: Triggered when a user's identity strength increases due to actions like adding new stamps, leading to a higher score.

4. **Score Decreased**: Triggered when a user's identity strength decreases, usually due to blacklisted or expired stamps, resulting in a lower score.

5. **New Stamp Added**: Triggered when a user adds a new stamp (e.g., phone, email) to their profile, strengthening their identity.

6. **Stamp Expired**: Triggered when a user’s stamp (e.g., email, phone) expires, potentially weakening their identity.

7. **Stamp Removed (Deauthorized)**: Triggered when a user revokes a stamp previously shared with your App, requiring you to update their profile.

8. **User Deactivated**: Triggered when a user's CUBID account is deactivated or suspended, requiring their account in your App to be locked.

These webhooks ensure your App can adapt dynamically to identity events, improving security, user experience, and trust in real-time.

---

### **HTTP Response & Retry Logic**

Whether registered via the **Admin Console** or the **API**, CUBID applies the following retry policy to all webhooks.

- Each webhook request expects a `200 OK` response to indicate successful processing. 

- If no `2xx` response is received, CUBID will retry delivering the webhook up to 5 times over 24 hours with exponential backoff.

- If a webhook delivery fails, CUBID will automatically retry sending the webhook up to **5 times within 24 hours**.
- If all attempts within that window fail, a **warning email** will be sent to the email address on file for the app.
- After an additional **10 unsuccessful attempts**, CUBID will disable the webhook, and another **email notification** will be sent, informing you of the deactivation.

---

### **Testing & Debugging**

To test webhook integrations, CUBID offers a sandbox mode where you can simulate webhook events and verify your application’s response. Additionally, you can replay webhook events for debugging purposes through the CUBID dashboard.

---

By integrating these webhooks, Apps can enhance user security, maintain up-to-date identity data, and respond to real-time identity events in a seamless and efficient way.
