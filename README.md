# Real-Time Chat Application with Socket.io

This assignment focuses on building a real-time chat application using Socket.io, implementing bidirectional communication between clients and server.

## Assignment Overview

You will build a chat application with the following features:

1. Real-time messaging using Socket.io
2. User authentication and presence
3. Multiple chat rooms or private messaging
4. Real-time notifications
5. Advanced features like typing indicators and read receipts

## Project Structure

```
socketio-chat/
├── client/                 # React front-end
│   ├── public/             # Static files
│   ├── src/                # React source code
│   │   ├── components/     # UI components
│   │   ├── context/        # React context providers
│   │   ├── hooks/          # Custom React hooks
│   │   ├── pages/          # Page components
│   │   ├── socket/         # Socket.io client setup
│   │   └── App.jsx         # Main application component
│   └── package.json        # Client dependencies
├── server/                 # Node.js back-end
│   ├── config/             # Configuration files
│   ├── controllers/        # Socket event handlers
│   ├── models/             # Data models
│   ├── socket/             # Socket.io server setup
│   ├── utils/              # Utility functions
│   ├── server.js           # Main server file
│   └── package.json        # Server dependencies
└── README.md               # Project documentation
```

## Getting Started

1. Accept the GitHub Classroom assignment invitation
2. Clone your personal repository that was created by GitHub Classroom
3. Follow the setup instructions in the `Week5-Assignment.md` file
4. Complete the tasks outlined in the assignment

## Files Included

- `Week5-Assignment.md`: Detailed assignment instructions
- Starter code for both client and server:
  - Basic project structure
  - Socket.io configuration templates
  - Sample components for the chat interface

## Requirements

- Node.js (v18 or higher)
- npm or yarn
- Modern web browser
- Basic understanding of React and Express

## Submission

Your work will be automatically submitted when you push to your GitHub Classroom repository. Make sure to:

1. Complete both the client and server portions of the application
2. Implement the core chat functionality
3. Add at least 3 advanced features
4. Document your setup process and features in the README.md
5. Include screenshots or GIFs of your working application
6. Optional: Deploy your application and add the URLs to your README.md

## Monitoring Setup

- **Overview:**: Instrument the application to detect downtime, collect logs, track errors, and alert you when something goes wrong. Typical stack components:

  - **Health checks:** Simple HTTP endpoints that return 200 when the app is healthy.
  - **Uptime monitoring:** External services (UptimeRobot, Pingdom) that call health endpoints.
  - **Error tracking:** Sentry (recommended) for uncaught exceptions and performance traces.
  - **Log aggregation:** LogDNA, Papertrail, or a cloud provider log sink for production logs.
  - **Metrics & dashboards:** Prometheus + Grafana or hosted APM (New Relic) for metrics and dashboards.

- **Server health endpoint:** Add a lightweight route in `server/server.js` (or use `/`) that returns basic status and uptime. Example:

  ```js
  // in server/server.js
  app.get("/health", (req, res) => {
    res.json({
      status: "ok",
      uptime: process.uptime(),
      timestamp: new Date().toISOString(),
    });
  });
  ```

- **Uptime checks:** Configure an external monitor (UptimeRobot or equivalent) to hit `https://<your-service-url>/health` every 1–5 minutes. If the endpoint fails or returns non-200, the monitor will notify you.

- **Error tracking (Sentry):**

  - Add the `@sentry/node` package to the server and initialize it early in `server.js` using the `SENTRY_DSN` environment variable.
  - Example env var to set on production: `SENTRY_DSN`.

  Example initialization:

  ```js
  const Sentry = require("@sentry/node");
  if (process.env.SENTRY_DSN) {
    Sentry.init({ dsn: process.env.SENTRY_DSN });
    app.use(Sentry.Handlers.requestHandler());
  }
  // ... routes
  if (process.env.SENTRY_DSN) app.use(Sentry.Handlers.errorHandler());
  ```

- **Logs:**

  - In development, console logs are fine. In production, stream logs to a log aggregation service (LogDNA, Papertrail, or your cloud provider).
  - Ensure `NODE_ENV=production` in production and include sufficient context in logs (request id, user id).

- **Metrics & Dashboards:**

  - Export basic metrics (request rate, response time, memory, CPU) via Prometheus exporters or use a hosted APM.
  - Create Grafana dashboards or use the APM's built-in dashboards to track trends.

- **Alerts & Notification Channels:**

  - Set up alerting thresholds for error rate, latency, and uptime failures.
  - Integrate alerts with Slack, email, or PagerDuty.

- **CI/CD monitoring:**

  - Enable build and deploy notifications from GitHub Actions and Vercel/Render to a Slack channel.
  - Include health checks after deployment in your GitHub Actions workflow (the `server.yml` already contains a basic health check step).

- **Recommended environment variables (add to Render / Vercel / hosting provider):**

  - `MONGO_URI` — MongoDB connection string
  - `JWT_SECRET` — JWT signing secret
  - `SENTRY_DSN` — (optional) Sentry Data Source Name
  - `NEW_RELIC_LICENSE_KEY` — (optional) New Relic license key for APM
  - `NODE_ENV=production`

- **Verification after deploy:**
  1. Visit `https://<your-service>/health` and expect HTTP 200.
  2. Check the logs in your hosting provider or log aggregation service for errors.
  3. Confirm Sentry (or other error tracker) receives test events.

## Quick commands

Run a local health check (PowerShell):

```powershell
curl http://localhost:5000/health
```

Send a test Sentry event (if configured):

```js
// run in node REPL or a small script
const Sentry = require("@sentry/node");
Sentry.init({ dsn: process.env.SENTRY_DSN });
Sentry.captureMessage("Test deployment monitoring event");
```

Add these monitoring env vars to your production host (Render/Vercel) and verify each service after deployment.

## Resources

- [Socket.io Documentation](https://socket.io/docs/v4/)
- [React Documentation](https://react.dev/)
- [Express.js Documentation](https://expressjs.com/)
- [Building a Chat Application with Socket.io](https://socket.io/get-started/chat)
