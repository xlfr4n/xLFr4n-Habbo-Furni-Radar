# 📡 xLFr4n // Habbo Furni Radar

> **Spot the drop. Capture the data. Keep the signal.**

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.12-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python 3.12">
  <img src="https://img.shields.io/badge/GitHub-Actions-2088FF?style=for-the-badge&logo=githubactions&logoColor=white" alt="GitHub Actions">
  <img src="https://img.shields.io/badge/Habbo-Collectibles-111827?style=for-the-badge" alt="Habbo Collectibles">
</p>

## 🇪🇸 Español

### 🎯 Qué hace

Habbo Furni Radar consulta la **Shop API pública de Habbo Collectibles**, compara los `productCode` con el estado guardado y avisa en Discord cuando encuentra uno nuevo. El estado inicial se registra sin mandar avisos históricos.

La Shop se utiliza como fuente de lanzamiento y furnidata, precios y conversión de moneda se usan como enriquecimiento cuando están disponibles.

### 📡 Fuentes

| Source | Role |
|---|---|
| Shop API | detección de nuevos items |
| Prices API | datos de mercado disponibles |
| Habbo furnidata | enriquecimiento técnico |
| CoinGecko | conversión ETH |

### ♻️ Flujo

~~~text
GitHub Actions
     ↓
scheduled poll
     ↓
Habbo Shop API
     ↓
new productCode?
   ↙       ↘
 no         yes
 ↓           ↓
state     enrich
             ↓
          Discord
~~~

El estado conocido se mantiene en `state/known_shop_items.json` para evitar duplicados y spam histórico.

### 🇺🇸 English

Habbo Furni Radar watches the **public Habbo Collectibles Shop API**, detects new `productCode` values and publishes one Discord embed for each newly observed Collectible.

The Shop API is the launch signal; furnidata, pricing and ETH conversion endpoints provide enrichment when available.

---

# 🛠️ Local development

~~~bash
python -m unittest discover -s tests -v
python radar.py --dry-run
~~~

### Automation model

- scheduled GitHub Actions polling;
- persistent deduplication;
- HTTP retry handling including rate limiting and transient 5xx responses;
- enrichment only when source data is available;
- initial bootstrap without replaying the whole historical catalog;
- optional heartbeat/testing workflow support.

---

# 🔐 Security

🇪🇸 No necesita wallet, private key, seed phrase, VPS ni servidor permanente. El webhook de Discord debe guardarse únicamente como **GitHub Actions Secret**.

🇺🇸 No wallet, private key, seed phrase, VPS or always-on server is required. Store the Discord webhook only as a **GitHub Actions Secret**.

---

# 📊 Data policy

El radar refleja lo que devolvieron las fuentes consultadas. Si un dato falta o una fuente falla, no debe tratarse como un hecho confirmado. Los precios son referencias de la consulta, no una tasación ni una garantía de venta.

Alerts may include name, rarity, collection, score, cost, supply, timestamps, description, technical metadata, ETH/USD/EUR information, market link and official artwork when sources provide them.

---

# 📚 Documentation

- `BRAND.md` — project identity
- `scripts/README.md` — helper scripts
- `SECURITY.md` — security boundaries
- `CONTRIBUTING.md` — contribution workflow

---

# ⚡ xLFr4n

<div align="center"><strong>Detect → Enrich → Verify → Alert</strong><br><sub>⚡ xLFr4n · Public sources · Useful signal</sub></div>