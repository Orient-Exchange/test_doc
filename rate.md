# **Orient Exchange – Public Forex Rates API Specification**

## **Purpose**

Provide a **public, machine-readable API** for live foreign exchange rates for each branch/location, optimized for:

- AI models (ChatGPT, Gemini, etc.)
- Search engines (Google, Bing, etc.)
- Partner integrations

This API **must**:

- Return structured JSON data
- Include **branch-specific** rates
- Include **timestamps** for freshness
- Be **publicly accessible without authentication**
- Use **standard currency codes** (ISO 4217)

---

## **Endpoint**

```
GET https://www.orientexchange.in/api/forex-rates
```

---

## **Query Parameters**

| Parameter  | Type   | Required | Description                                                                           |
| ---------- | ------ | -------- | ------------------------------------------------------------------------------------- |
| `city`     | string | Optional | Branch location (e.g., `Bangalore`, `Delhi`). If not provided, returns all branches.  |
| `currency` | string | Optional | 3-letter currency code (e.g., `USD`, `THB`). If not provided, returns all currencies. |

**Examples**:

```
https://www.orientexchange.in/api/forex-rates?city=Bangalore
https://www.orientexchange.in/api/forex-rates?city=Delhi&currency=THB
```

---

## **Response Format (JSON)**

### **Success – All Data**

```json
{
  "provider": "Orient Exchange",
  "last_updated": "2025-08-28T06:00:00+05:30",
  "city": "Bangalore",
  "rates": [
    {
      "currency": "USD",
      "buy_card": 83.25,
      "buy_cash": 83.1,
      "sell_card": 83.55,
      "sell_cash": 83.4,
      "wire_transfer": 83.2
    },
    {
      "currency": "THB",
      "buy_card": 2.75,
      "buy_cash": 2.73,
      "sell_card": 2.8,
      "sell_cash": 2.78,
      "wire_transfer": 2.76
    }
  ]
}
```

### **Success – Single Currency**

```json
{
  "provider": "Orient Exchange",
  "last_updated": "2025-08-28T06:00:00+05:30",
  "city": "Delhi",
  "currency": "THB",
  "rates": {
    "buy_card": 2.75,
    "buy_cash": 2.73,
    "sell_card": 2.8,
    "sell_cash": 2.78,
    "wire_transfer": 2.76
  }
}
```

---

## **Error Response**

```json
{
  "error": true,
  "message": "City not found or no rates available."
}
```

---

## **Technical Notes**

1. **Data Freshness**

   - Must be updated **in real time** or at least every 1 minute.
   - Include `"last_updated"` timestamp in ISO 8601 format.

2. **Currency Codes**

   - Use **ISO 4217** codes (`USD`, `THB`, `EUR`, etc.).

3. **CORS Headers**

   - Add:

     ```
     Access-Control-Allow-Origin: *
     Content-Type: application/json
     ```

4. **Rate Types**

   - Always include:

     - `buy_card` – Rate for buying forex card
     - `buy_cash` – Rate for buying physical currency notes
     - `sell_card` – Rate for selling forex card
     - `sell_cash` – Rate for selling physical currency notes
     - `wire_transfer` – Rate for remittances

5. **Multiple Branch Support**

   - If no `city` parameter is provided, return an array for all branches:

```json
{
  "provider": "Orient Exchange",
  "last_updated": "2025-08-28T06:00:00+05:30",
  "branches": [
    {
      "city": "Bangalore",
      "rates": [...]
    },
    {
      "city": "Delhi",
      "rates": [...]
    }
  ]
}
```

---

## **SEO / AI Visibility**

To make these rates **more visible to AI models**:

1. **Expose this endpoint publicly** without login.
2. **Embed JSON-LD** inside the HTML of your city pages (`/forex-exchange-bangalore`, `/forex-exchange-delhi`) pointing to this API:

```html
<script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "Dataset",
    "name": "Orient Exchange Forex Rates - Bangalore",
    "distribution": {
      "@type": "DataDownload",
      "encodingFormat": "application/json",
      "contentUrl": "https://www.orientexchange.in/api/forex-rates?city=Bangalore"
    }
  }
</script>
```

3. Keep **currency names + rates visible in plain HTML tables** on the page (server-rendered).
