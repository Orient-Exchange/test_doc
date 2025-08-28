1. **Public API JSON format**
2. **Example responses**
3. **JSON-LD `WebAPI` schema** so AI and Google know how to use it

---

## **1. Public API Specification**

### **Endpoint**

```
GET https://www.orientexchange.in/api/forex-rates
```

### **Parameters**

| Name       | Type   | Required | Example   | Description                |
| ---------- | ------ | -------- | --------- | -------------------------- |
| `city`     | string | No       | Bangalore | Branch location name       |
| `currency` | string | No       | USD       | 3-letter ISO currency code |

### **Examples**

```
https://www.orientexchange.in/api/forex-rates?city=Bangalore
https://www.orientexchange.in/api/forex-rates?city=Delhi&currency=THB
```

---

## **2. JSON Response Format**

### **Example – All rates for a city**

```json
{
  "provider": "Orient Exchange",
  "last_updated": "2025-08-28T06:00:00+05:30",
  "city": "Bangalore",
  "available_currencies": ["USD", "THB", "EUR", "GBP", "AUD"],
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

---

### **Example – Single currency for a city**

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

### **Example – All branches**

```json
{
  "provider": "Orient Exchange",
  "last_updated": "2025-08-28T06:00:00+05:30",
  "branches": [
    {
      "city": "Bangalore",
      "rates": [
        { "currency": "USD", "buy_card": 83.25, "sell_card": 83.55 },
        { "currency": "THB", "buy_card": 2.75, "sell_card": 2.8 }
      ]
    },
    {
      "city": "Delhi",
      "rates": [
        { "currency": "USD", "buy_card": 83.3, "sell_card": 83.6 },
        { "currency": "THB", "buy_card": 2.74, "sell_card": 2.79 }
      ]
    }
  ]
}
```

---

## **3. JSON-LD Schema for AI Discovery**

Embed this inside your **city-specific forex pages** (e.g., `/forex-exchange-bangalore`):

```html
<script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "WebAPI",
    "name": "Orient Exchange Forex Rates API",
    "description": "Retrieve live foreign exchange rates for Orient Exchange branches.",
    "endpointUrl": "https://www.orientexchange.in/api/forex-rates",
    "documentation": "https://www.orientexchange.in/api-docs",
    "provider": {
      "@type": "Organization",
      "name": "Orient Exchange",
      "url": "https://www.orientexchange.in"
    },
    "termsOfService": "https://www.orientexchange.in/terms",
    "parameters": [
      {
        "name": "city",
        "description": "Branch location (e.g., Bangalore, Delhi)",
        "required": false
      },
      {
        "name": "currency",
        "description": "3-letter ISO currency code (e.g., USD, THB)",
        "required": false
      }
    ],
    "outputFormat": "application/json",
    "exampleRequests": [
      "https://www.orientexchange.in/api/forex-rates?city=Bangalore",
      "https://www.orientexchange.in/api/forex-rates?city=Delhi&currency=THB"
    ]
  }
</script>
```

---

## **4. Implementation Notes**

- Return **ISO 4217 codes** for all currencies.
- Include `"last_updated"` in **ISO 8601 format**.
- Set HTTP headers:

  ```
  Content-Type: application/json
  Access-Control-Allow-Origin: *
  ```

- Update rates in real time or at least every minute.
- Keep API **read-only** and public.
