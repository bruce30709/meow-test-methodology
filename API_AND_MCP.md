# Meow Test Lab (喵實測) — Public Read-Only REST API & Remote MCP Server

[![MCP Ready](https://img.shields.io/badge/MCP-Streamable_HTTP-116D4B)](https://meowtestlab.com/api/mcp)
[![OpenAPI 3.1](https://img.shields.io/badge/OpenAPI-3.1.0-DF645B)](https://meowtestlab.com/openapi.json)
[![llms.txt](https://img.shields.io/badge/AI-llms.txt-2D2120)](https://meowtestlab.com/llms.txt)

Free, zero-auth, CORS-enabled public API & **Remote MCP Server** for Taiwan cat owners, developers, and AI Agents — backed by **Meow Test Lab ([meowtestlab.com](https://meowtestlab.com))**.

> **Transparency & Compliance Notice**
> - Nutrition specs (crude protein %, crude fat %, Dry Matter Basis `DMB%` carbs, kibble diameter mm & shape, gum-free labels) are derived from official manufacturer guaranteed analysis.
> - Reference prices & single-cat daily costs are market estimates for comparison only; always check live store links.
> - **Zero veterinary medical advice.** Prescription / therapeutic diets are strictly excluded from rankings and buy links.
> - Returned `buy_links` (`/go/s/*` Shopee & `/go/m/*` momo) are **disclosed affiliate redirects** at standard store pricing.

---

## 1. Remote MCP Server (Zero Install, Streamable HTTP)

Connect any MCP-compatible AI client (Claude Desktop, Cursor, Windsurf, Cline, custom AI Agents) directly to:

```text
https://meowtestlab.com/api/mcp
```

### Example MCP Client Config (`mcp_config.json` / `.cursor/mcp.json`)

```json
{
  "mcpServers": {
    "meow-test-lab": {
      "url": "https://meowtestlab.com/api/mcp"
    }
  }
}
```

### Available MCP Tools (4 Read-Only Tools)

| Tool | Description | Key Arguments |
|---|---|---|
| `search_cat_products` | Filter Taiwan dry cat food, complete wet cat food, cat litter & cat treats; returns official specs, community pros/cons & disclosed `/go/*` buy links | `category` (`food`/`wet`/`litter`/`treat`), `brand`, `q`, `max_carbs`, `max_fat`, `min_protein`, `max_phosphorus`, `wet_texture`, `gum_free`, `litter_type`, `sort`, `limit` |
| `get_cat_product` | Full specs, dual-size momo options, community summary & default 1-cat restock preview | `product_id` (e.g. `farmina-nd`, `royal-in27`, `dogcatstar-wet-silver`, `animonda-carny`, `pidan-mixed`) |
| `calculate_restock_schedule` | Compute exact consumption days, reminder date, recurring `RRULE`, ready-to-insert Google Calendar `calendar_event` (with `/go/*` links), 1-click `google_calendar_url` & `.ics` download link | `product_id`, `cats`, `size` (`small`/`large`), `daily_grams`, `bags`, `scoop_habit`, `sticks_per_week`, `cans`, `cans_per_day`, `lead_days`, `repeat`, `lang` |
| `calculate_dmb_carbs` | Convert any cat food bag label (as-fed %) into Dry Matter Basis `DMB%` carbs/protein/fat + return 3 low-carb catalog references | `protein`, `fat`, `fiber` (opt), `ash` (opt), `moisture` (opt) |

### Example Prompts Once Connected
- 「幫我找台灣乾物比碳水 22% 以下、顆粒小於 9mm 的無穀貓飼料，附上蝦皮跟 momo 連結」
- 「我家兩隻貓吃法國皇家 IN27 4kg 大包、每隻每天 60g，幫我算幾天會吃完，直接排一個循環補貨提醒到我的 Google 日曆！」
- 「幫我找無膠、磷含量 200 mg/100kcal 以下的台灣主食罐，依磷含量由低到高排，並算一箱 24 罐一天吃 2 罐幾天要補貨」
- 「這包飼料背面寫粗蛋白 38%、脂肪 16%、纖維 3.5%、灰分 8%、水分 10%，幫我換算 DMB 碳水高不高？」

---

## 2. Public REST API (`/api/v1/*`) & OpenAPI 3.1

- **OpenAPI 3.1 Spec**: https://meowtestlab.com/openapi.json (1-click import into OpenAI Custom GPT Actions, Gemini Gems, n8n, Dify, LangChain)
- **AI Discovery Index**: https://meowtestlab.com/llms.txt

### Quick `curl` Examples

```bash
# 1. Low-carb dry cat food (<= 22% DMB carbs, sorted lowest first)
curl "https://meowtestlab.com/api/v1/products?category=food&max_carbs=22&sort=carbs"

# 2. Low-fat indoor dry food (<= 15% fat)
curl "https://meowtestlab.com/api/v1/products?category=food&max_fat=15&sort=fat"

# 3. Low-phosphorus gum-free complete wet cat food (<= 200 mg/100kcal, sorted lowest P first)
curl "https://meowtestlab.com/api/v1/products?category=wet&gum_free=true&max_phosphorus=200&sort=phosphorus"

# 4. Gum-free cat treats
curl "https://meowtestlab.com/api/v1/products?category=treat&gum_free=true"

# 4. Single product detail
curl "https://meowtestlab.com/api/v1/products/hills-indoor"

# 5. Precision restock schedule JSON (ready for Google Calendar API events.insert)
curl "https://meowtestlab.com/api/v1/restock?product_id=royal-in27&cats=2&size=large&daily_grams=60&lead_days=3"

# 6. Direct .ics recurring calendar download (Apple Calendar / Google Calendar / Outlook / iOS Shortcuts)
curl -o restock.ics "https://meowtestlab.com/api/v1/restock?product_id=royal-in27&cats=2&size=large&format=ics"

# 7. Calculate DMB% carbs for any cat food label
curl "https://meowtestlab.com/api/v1/calc/dmb?protein=40&fat=18&fiber=3&ash=8&moisture=10"
```

### Quick MCP JSON-RPC 2.0 Test

```bash
curl -X POST "https://meowtestlab.com/api/mcp" \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list"}'
```

---

*Maintained by [Meow Test Lab (喵實測)](https://meowtestlab.com) · [Scoring Methodology & Audit Evidence](./README.md)*
