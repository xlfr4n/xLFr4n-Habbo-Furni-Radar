<div align="center">

# ⚡ xLFr4n / Habbo Furni Radar

**Detectar novedades, contrastar datos y avisar solo cuando hay una señal nueva.**

<p>
  <img src="https://img.shields.io/badge/Python-3.12-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python 3.12">
  <img src="https://img.shields.io/badge/GitHub-Actions-2088FF?style=for-the-badge&logo=githubactions&logoColor=white" alt="GitHub Actions">
  <img src="https://img.shields.io/badge/Habbo-Collectibles-FF3344?style=for-the-badge&labelColor=111318" alt="Habbo Collectibles">
</p>

</div>

---

## Qué hace

**Habbo Furni Radar** consulta la Shop API pública de Habbo Collectibles, compara los códigos `productCode` con el estado guardado y envía un aviso a Discord cuando detecta un artículo nuevo.

La primera ejecución registra el catálogo inicial sin convertir todo el histórico en una avalancha de notificaciones. El radar conserva los elementos conocidos para evitar avisos duplicados.

La Shop API es la señal principal de detección. Los datos de furnidata, precios y conversión de moneda se utilizan como enriquecimiento cuando las fuentes los ofrecen.

## Fuentes de datos

| Fuente | Uso |
| --- | --- |
| **Shop API** | Detección de nuevos artículos. |
| **Prices API** | Datos de mercado disponibles. |
| **Habbo furnidata** | Información técnica complementaria. |
| **CoinGecko** | Conversión de ETH cuando está disponible. |

Cada fuente puede fallar, cambiar o devolver información incompleta. El radar no debe interpretar un dato ausente como un valor confirmado.

## Flujo

```text
GitHub Actions
      |
      v
Consulta programada
      |
      v
Habbo Shop API ------> Estado conocido
      |                      |
      v                      v
¿productCode nuevo? -----> Deduplicación
      |
      +-- No: continuar
      |
      +-- Sí: enriquecer datos
                    |
                    v
                Aviso Discord
```

El estado persistente se guarda en `state/known_shop_items.json`.

## Probar en local

Necesitas Python 3.12 y las dependencias del proyecto instaladas.

Ejecuta las pruebas:

```bash
python -m unittest discover -s tests -v
```

Simula una ejecución sin publicar avisos:

```bash
python radar.py --dry-run
```

`--dry-run` muestra qué coleccionables nuevos detectaría, sin publicar en Discord ni modificar el estado persistente. En un primer arranque, la simulación tampoco inicializa el catálogo: así puedes comprobar el resultado sin consumir los avisos que corresponderían a una ejecución real.

## Automatización

El flujo se ejecuta mediante GitHub Actions e incluye:

- consultas programadas a las fuentes configuradas;
- deduplicación entre ejecuciones;
- reintentos ante límites de peticiones y errores temporales del servidor;
- enriquecimiento solo cuando hay datos disponibles;
- inicialización sin reproducir el histórico completo;
- soporte para pruebas y comprobaciones de funcionamiento.

El webhook de Discord debe configurarse como **GitHub Actions Secret**, usando el nombre que espera el workflow del repositorio. No lo incluyas en archivos versionados ni en la salida de depuración.

## Seguridad y límites de los datos

No necesitas una wallet, private key, seed phrase, VPS ni servidor permanente. El proyecto se ejecuta con GitHub Actions.

Los avisos reflejan lo que devolvieron las fuentes consultadas en ese momento. Los precios son referencias de mercado, no una tasación ni una garantía de venta. Si una fuente no proporciona un dato, debe mantenerse como desconocido en lugar de inventarlo.

Los avisos pueden incluir nombre, rareza, colección, puntuación, coste, oferta, fechas, descripción, metadatos técnicos, referencias ETH/USD/EUR, enlace al mercado y artwork oficial cuando los datos estén disponibles.

## Documentación

- `BRAND.md` — identidad del proyecto.
- `scripts/README.md` — scripts auxiliares.
- `SECURITY.md` — límites y prácticas de seguridad.
- `CONTRIBUTING.md` — guía de contribución.

<div align="center">

**Detectar → Enriquecer → Verificar → Avisar**  
<sub>⚡ xLFr4n · Cyber-red / charcoal · Fuentes públicas · Señal útil</sub>

</div>

<details>
<summary><strong>English</strong></summary>

## What it does

Habbo Furni Radar polls the public Habbo Collectibles Shop API, compares `productCode` values against its saved state, and sends a Discord alert when it detects a new item. The initial catalogue is recorded without replaying the full history as notifications.

The Shop API is the primary detection signal. Furnidata, pricing and ETH conversion are used for enrichment when those sources provide data.

## Local checks

Run the test suite:

```bash
python -m unittest discover -s tests -v
```

Preview newly detected items without sending Discord alerts or changing persistent state:

```bash
python radar.py --dry-run
```

The persistent catalogue is stored in `state/known_shop_items.json`. The scheduled workflow handles polling, deduplication, retries and optional enrichment.

## Security and data quality

No wallet, private key, seed phrase, VPS or always-on server is required. Store the Discord webhook only as a GitHub Actions Secret. Alerts represent the information returned by the configured sources at query time; market prices are references, not guaranteed sale values.

See `SECURITY.md` and `CONTRIBUTING.md` before making changes.

</details>