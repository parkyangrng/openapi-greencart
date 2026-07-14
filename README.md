# GreenCart Inventory Query API

A public, read-only OpenAPI documentation portal for GreenCart's supermarket inventory system. Built as a static site and deployed on [Render](https://render.com).

**Live:** https://greencart-inventory-query-api.onrender.com

---

## Overview

This repository contains the mockup OpenAPI 3.1 documentation portal for the GreenCart Inventory Query API — a fully public, no-auth-required REST API for querying real-time supermarket stock levels, products, store locations, and categories.

No build step. No framework. No dependencies to install. Drop the two files, push, deploy.

---

## API summary

| Group | Endpoints | Description |
|---|---|---|
| Inventory | 4 | Stock levels, low-stock alerts, search |
| Products | 3 | Catalog, product detail, availability |
| Stores | 2 | Store locations, inventory snapshot |
| Categories | 2 | Category list, category stock overview |

**Base URL:** `https://inventory.greencart.onrender.com/v1`  
**Auth:** None — fully public  
**Rate limit:** 120 requests / min per IP  
**Stock refresh:** Every 60 seconds from POS systems  

---

## Endpoints

### Inventory

| Method | Path | Description |
|---|---|---|
| GET | `/inventory` | Browse all stock levels across stores |
| GET | `/inventory/{sku}` | Stock for a single product by SKU |
| GET | `/inventory/low-stock` | Items at or below restock threshold |
| GET | `/inventory/search` | Full-text search by name or SKU |

### Products

| Method | Path | Description |
|---|---|---|
| GET | `/products` | Full product catalog |
| GET | `/products/{sku}` | Single product detail |
| GET | `/products/{sku}/availability` | Product + live stock combined |

### Stores

| Method | Path | Description |
|---|---|---|
| GET | `/stores` | All store locations |
| GET | `/stores/{storeId}/summary` | Store inventory snapshot |

### Categories

| Method | Path | Description |
|---|---|---|
| GET | `/categories` | List all categories with counts |
| GET | `/categories/{slug}/inventory` | Stock overview for a category |

---

## Schemas

### `InventoryItem`

| Field | Type | Description |
|---|---|---|
| `sku` | string | Product SKU identifier |
| `name` | string | Product display name |
| `store_id` | string | Store location identifier |
| `qty_available` | integer | Units currently in stock |
| `low_stock` | boolean | True when qty ≤ 20 |
| `unit` | string | `each`, `lb`, `kg`, `oz`, `pack` |
| `price` | number | Retail price in USD |
| `last_updated` | date-time | ISO 8601 stock timestamp |

### `Product`

| Field | Type | Description |
|---|---|---|
| `sku` | string | Unique product code |
| `name` | string | Display name |
| `brand` | string | Brand or label |
| `category` | string | Category slug |
| `unit` | string | Selling unit |
| `price` | number | Retail price USD |
| `organic` | boolean | USDA Organic certified |
| `allergens` | array | List of allergen strings |
| `description` | string | Short product description |

### `StoreStock`

| Field | Type | Description |
|---|---|---|
| `store_id` | string | Store identifier |
| `store_name` | string | Human-readable store name |
| `qty_available` | integer | Current on-hand count |
| `low_stock` | boolean | Below restock threshold |
| `last_updated` | date-time | Last sync from POS |

### `Store`

| Field | Type | Description |
|---|---|---|
| `id` | string | Store identifier (`store_…`) |
| `name` | string | Store display name |
| `address` | string | Full street address |
| `hours` | string | Opening hours (`HH:MM-HH:MM`) |
| `timezone` | string | IANA timezone string |

---

## Repository structure

```
greencart-inventory-api/
├── index.html        # Self-contained portal (HTML + CSS + JS, no build needed)
├── render.yaml       # Render static site deploy config
└── README.md
```

---

## Deploy to Render

### Option 1 — One-click via Render dashboard

1. Fork or push this repo to GitHub
2. Go to [dashboard.render.com](https://dashboard.render.com) → **New +** → **Static Site**
3. Connect your GitHub account and select this repo
4. Render auto-reads `render.yaml` — no manual config needed
5. Click **Create Static Site**

Live in ~30 seconds at `https://greencart-inventory-query-api.onrender.com`.

### Option 2 — Render CLI

```bash
render deploy
```

### Option 3 — Manual settings (if not using render.yaml)

| Setting | Value |
|---|---|
| Build command | *(leave blank)* |
| Publish directory | `.` |
| Branch | `main` |

---

## Local preview

No server needed — just open the file directly:

```bash
open index.html
# or
python3 -m http.server 8080
```

Then visit `http://localhost:8080`.

---

## Customization

All content lives in `index.html` as a single self-contained file. To adapt it:

- **Rename the API** — search and replace `GreenCart` with your chain name
- **Change the base URL** — update the server pill in the `.servers` section
- **Add endpoints** — duplicate any `.ep` block and update the method, path, and description
- **Add SKUs** — update the example JSON in the `.cb` (code block) elements
- **Add a category** — duplicate a `.sb` section block and update `data-tag`

The portal uses no external JS frameworks. Dark mode is supported automatically via `prefers-color-scheme`.

---

## Tech

- Plain HTML5, CSS3, vanilla JS — zero dependencies, zero build step
- Icons via [Tabler Icons](https://tabler-icons.io) webfont (CDN)
- Hosted as a Render static site (free tier)
- OpenAPI 3.1 spec format (documentation only — no live backend)

---

## License

MIT — free to use, fork, and adapt for your own supermarket or retail inventory API portal.
