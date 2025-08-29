## **Multi-City Foreign Exchange Rate Table Optimization Guide**

### **1. HTML Table with AI-Friendly Data Attributes**

```html
<div class="table-responsive">
  <table
    class="table text-center liverates-table"
    data-last-updated="2025-08-29T14:30:00+05:30"
    data-timezone="Asia/Kolkata"
  >
    <caption>
      Foreign Exchange Rates - Live Rates (Last Updated: Aug 29, 2025)
    </caption>

    <thead>
      <tr>
        <th rowspan="2" scope="col" data-field="currency">Currency</th>
        <th colspan="2" scope="col" data-category="customer-buy">
          Customer Buy
        </th>
        <th colspan="2" scope="col" data-category="outward-remittance">
          Outward Remittance
        </th>
        <th rowspan="2" scope="col" data-field="sell">
          Customer Sell Currency
        </th>
      </tr>
      <tr>
        <th scope="col" data-field="buy-card">Card</th>
        <th scope="col" data-field="buy-currency">Currency</th>
        <th scope="col" data-field="outward-education">Education/Medical</th>
        <th scope="col" data-field="outward-maintenance">Maintenance/Gift</th>
      </tr>
    </thead>

    <tbody>
      <tr data-currency="USD">
        <td data-iso-code="USD">USD</td>
        <td data-rate-type="buy-card" data-value="88.26">₹88.26</td>
        <td data-rate-type="buy-currency" data-value="88.78">₹88.78</td>
        <td data-rate-type="outward-education" data-value="88.58">₹88.58</td>
        <td data-rate-type="outward-maintenance" data-value="88.73">₹88.73</td>
        <td data-rate-type="sell-currency" data-value="88.15">₹88.15</td>
      </tr>
      <!-- Repeat for other currencies -->
    </tbody>
  </table>
</div>
```

---

### **2. Multi-City Schema.org JSON-LD (Static Example)**

```json
{
  "@context": "https://schema.org",
  "@type": "ItemList",
  "name": "Orient Exchange Forex Rates",
  "description": "Live foreign exchange rates for multiple cities and currencies across Orient Exchange branches.",
  "numberOfItems": 10,
  "mainEntityOfPage": {
    "@type": "WebPage",
    "@id": "https://orientexchange.in/forex_rates."
  },
  "itemListElement": [
    {
      "@type": "Offer",
      "validFrom": "2025-08-29T14:30:00+05:30",
      "availableAtOrFrom": {
        "@type": "Place",
        "name": "Chennai Branch",
        "address": {
          "@type": "PostalAddress",
          "addressLocality": "Chennai",
          "addressRegion": "Tamil Nadu",
          "addressCountry": "IN"
        },
        "telephone": "+91-44-12345678",
        "hasMap": "https://goo.gl/maps/examplechennai"
      },
      "itemOffered": {
        "@type": "ExchangeRateSpecification",
        "@id": "https://orientexchange.in/forex_rates.#Chennai-USD-buy-card",
        "currency": "USD",
        "name": "Buy Card",
        "description": "Buy Card rate for USD in Chennai",
        "currentExchangeRate": {
          "@type": "UnitPriceSpecification",
          "price": "88.26",
          "priceCurrency": "INR"
        }
      }
    },
    {
      "@type": "Offer",
      "validFrom": "2025-08-29T14:30:00+05:30",
      "availableAtOrFrom": {
        "@type": "Place",
        "name": "Bangalore Branch",
        "address": {
          "@type": "PostalAddress",
          "addressLocality": "Bangalore",
          "addressRegion": "Karnataka",
          "addressCountry": "IN"
        },
        "telephone": "+91-80-98765432",
        "hasMap": "https://goo.gl/maps/examplebangalore"
      },
      "itemOffered": {
        "@type": "ExchangeRateSpecification",
        "@id": "https://orientexchange.in/forex_rates.#Bangalore-USD-buy-card",
        "currency": "USD",
        "name": "Buy Card",
        "description": "Buy Card rate for USD in Bangalore",
        "currentExchangeRate": {
          "@type": "UnitPriceSpecification",
          "price": "88.35",
          "priceCurrency": "INR"
        }
      }
    }
    // ... repeat for other rate types and currencies
  ]
}
```

---

### **3. Multi-City PHP Dynamic JSON-LD Generator**

```php
<!-- Multi-City Schema with Offer → ExchangeRateSpecification -->
<script type="application/ld+json">
{
    "@context": "https://schema.org",
    "@type": "ItemList",
    "name": "Orient Exchange Forex Rates",
    "description": "Live foreign exchange rates for multiple cities and currencies across Orient Exchange branches.",
    "numberOfItems": <?php echo !empty($forexRates) && !empty($cities) ? count($forexRates) * count($cities) * 5 : 0; ?>,
    "mainEntityOfPage": {
        "@type": "WebPage",
        "@id": "<?php echo htmlspecialchars('https://orientexchange.in/forex_rates.', ENT_QUOTES); ?>"
    },
    "itemListElement": [
        <?php
        $rateSchemas = [];
        $now = date('Y-m-d\TH:i:sP');

        foreach ($cities as $city) {
            $cityName = htmlspecialchars($city['name'], ENT_QUOTES);
            $cityRegion = htmlspecialchars($city['region'], ENT_QUOTES);
            $branchPhone = htmlspecialchars($city['phone'], ENT_QUOTES);
            $branchMap = htmlspecialchars($city['map_url'], ENT_QUOTES);

            foreach ($forexRates as $code => $rate) {
                $currencyCode = htmlspecialchars($code, ENT_QUOTES);

                $rateTypes = [
                    'Buy Card'      => $rate['buyCard'] ?? null,
                    'Buy Cash'      => $rate['buyCash'] ?? null,
                    'Sell Card'     => $rate['sellCard'] ?? null,
                    'Sell Cash'     => $rate['sellCash'] ?? null,
                    'Wire Transfer' => $rate['wireTransfer'] ?? null
                ];

                foreach ($rateTypes as $type => $value) {
                    if (!empty($value)) {
                        $price = htmlspecialchars($value, ENT_QUOTES);
                        $rateSchemas[] = '{
                            "@type": "Offer",
                            "validFrom": "' . $now . '",
                            "availableAtOrFrom": {
                                "@type": "Place",
                                "name": "' . $cityName . ' Branch",
                                "address": {
                                    "@type": "PostalAddress",
                                    "addressLocality": "' . $cityName . '",
                                    "addressRegion": "' . $cityRegion . '",
                                    "addressCountry": "IN"
                                },
                                "telephone": "' . $branchPhone . '",
                                "hasMap": "' . $branchMap . '"
                            },
                            "itemOffered": {
                                "@type": "ExchangeRateSpecification",
                                "@id": "https://orientexchange.in/forex_rates.#' . urlencode($cityName . '-' . $currencyCode . '-' . strtolower(str_replace(" ", "-", $type))) . '",
                                "currency": "' . $currencyCode . '",
                                "name": "' . $type . '",
                                "description": "' . $type . ' rate for ' . $currencyCode . ' in ' . $cityName . '",
                                "currentExchangeRate": {
                                    "@type": "UnitPriceSpecification",
                                    "price": "' . $price . '",
                                    "priceCurrency": "INR"
                                }
                            }
                        }';
                    }
                }
            }
        }

        echo implode(",\n        ", $rateSchemas);
        ?>
    ]
}
</script>
```

---

### **4. Benefits of This Multi-City Setup**

- **SEO**: Google can show rates directly in search results for each branch.
- **AI Readability**: AI models can answer “What is the USD buy rate in Bangalore?” instantly.
- **Local Targeting**: Each branch has its own contact & Google Maps link.
- **Accessibility**: Data attributes and semantic HTML improve screen reader support.
- **Automation**: PHP loops generate rates dynamically from your database.
