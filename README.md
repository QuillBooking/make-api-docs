# Quill Booking Make API Docs

## Connection

### Base URL

- `{{connection.baseUrl}}`

### Authentication

All requests must include `api_key` as a query string parameter:

```http
?api_key={{connection.apiKey}}
```

### Logging

Sensitive fields (`api_key`) are sanitized from logs.

---

## Webhooks

Quill Booking supports attaching and detaching webhooks for booking events.
Currently supported booking statuses are:

- **created** (new booking)
- **cancelled** (booking cancelled)
- **rescheduled** (booking rescheduled)

---

### Attach Webhook

**Endpoint**

```
POST {{connection.baseUrl}}/wp-json/qb/v1/integrations/make/webhooks/subscribe?api_key={{connection.apiKey}}
```

**Request Body**

```json
{
  "hook_url": "{{webhook.url}}",
  "event_id": "{{parameters.event_id}}",
  "booking_status": "<created|cancelled|rescheduled>"
}
```

**Success Response**

```json
{
  "success": true,
  "data": {
    "externalHookId": "12345",
    "token": "abcdef"
  }
}
```

- `externalHookId` is stored and used for detach requests (`{{webhook.externalHookId}}`).
- `token` is stored and can be used for validation (`{{webhook.token}}`).

---

### Detach Webhook

**Endpoint**

```
DELETE {{connection.baseUrl}}/wp-json/qb/v1/integrations/make/webhooks/unsubscribe
```

**Query Parameters**

- `api_key`: `{{connection.apiKey}}`
- `webhook_id`: `{{webhook.externalHookId}}`

**Example**

```
DELETE {{connection.baseUrl}}/wp-json/qb/v1/integrations/make/webhooks/unsubscribe?api_key=XXXX&webhook_id=12345
```

---

## Supported Hooks

### 1. New Booking

- **booking_status:** `created`

### 2. Cancelled Booking

- **booking_status:** `cancelled`

### 3. Rescheduled Booking

- **booking_status:** `rescheduled`
