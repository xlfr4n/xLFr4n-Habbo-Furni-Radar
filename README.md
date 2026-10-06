# 📡 xLFr4n // Habbo Furni Radar

> **Spot the drop. Capture the data. Keep the signal.**

<p align="center">
![Python](https://img.shields.io/badge/Python-3.12-3776AB?style=for-the-badge&logo=python&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub-Actions-2088FF?style=for-the-badge&logo=githubactions&logoColor=white)
![Habbo](https://img.shields.io/badge/Habbo-Collectibles-111827?style=for-the-badge)
</p>

## 🇪🇸 Español

### 🎯 Qué hace

Habbo Furni Radar observa la **Shop API pública de Habbo Collectibles**, detecta nuevos `productCode` y publica un embed individual en Discord por cada Collectible nuevo detectado.

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

The radar describes what the configured public sources actually returned. Missing values remain missing; temporary failures are not converted into facts; inferred values are not silently presented as source values.

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