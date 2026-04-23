# Getting Started with Webhooks

!!! note "Context"
    **Audience:** Developers integrating a webhook system for the first time — familiar with HTTP and a server-side language, but new to the platform.  
    **Goal:** First successful event received and verified within a single session.  
    **Constraints:** Needed to work across multiple language ecosystems; Node.js shown as the primary example.

---

## What you'll build

A local Express server that receives, verifies, and logs webhook events from the platform. By the end you'll have a working endpoint ready to extend.

**Time required:** ~10 minutes  
**Prerequisites:** A free account, an active API key, and Node.js 18+ installed locally.

---

## Step 1 — Install the SDK and create a server

```bash
npm install express @platform/webhooks
```

Create `server.js`:

```javascript
const express = require('express');
const { Webhook } = require('@platform/webhooks');

const app = express();
app.use(express.raw({ type: 'application/json' }));

app.post('/webhook', (req, res) => {
  const sig = req.headers['x-platform-signature'];
  let event;

  try {
    event = Webhook.constructEvent(req.body, sig, process.env.WEBHOOK_SECRET);
  } catch (err) {
    return res.status(400).send(`Webhook error: ${err.message}`);
  }

  console.log('Received:', event.type);
  res.json({ received: true });
});

app.listen(3000, () => console.log('Listening on :3000'));
```

---

## Step 2 — Expose your local server

Use ngrok to give the platform a reachable URL during development.

```bash
ngrok http 3000
```

Copy the `https://` forwarding URL — you'll need it in the next step.

!!! info
    ngrok URLs expire when the session ends. For persistent testing, use a fixed domain with ngrok's paid plan or a similar tunnelling service.

---

## Step 3 — Register your endpoint

In the dashboard, go to **Settings → Webhooks → Add endpoint**. Paste the ngrok URL, select the event types you want to receive, and save.

Copy the **Signing Secret** shown on the endpoint detail page and set it as an environment variable:

```bash
export WEBHOOK_SECRET=whsec_••••••••••••••••
```

---

## Step 4 — Send a test event

Click **Send test event** in the dashboard. Your terminal should show:

```
Received: payment.succeeded
```

!!! success
    Your endpoint is working. From here, add handlers for specific event types and follow the [production deployment checklist](#) before going live.

---

## Next steps

- Read the [Event reference](#) for all available event types and their payload schemas
- Learn how to [handle failed deliveries and retries](#)
- Review the [production checklist](#) before deploying
