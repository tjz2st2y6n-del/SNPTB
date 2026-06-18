import { Hono } from "hono";
import { cors } from "hono/cors";

const app = new Hono();

app.use("*", cors());

app.get("/", (c) => c.text("Hello world"));

app.get("/api/health", (c) =>
  c.json({ status: "ok" })
);

// ✅ ADD THIS (THIS FIXES YOUR 404)
app.post("/webhooks/modern-treasury", async (c) => {
  const body = await c.req.json();

  console.log("📩 Modern Treasury Webhook Received:");
  console.log(body);

  return c.text("ok", 200);
});

export default {
  port: process.env.PORT ?? 3000,
  fetch: app.fetch,
};
