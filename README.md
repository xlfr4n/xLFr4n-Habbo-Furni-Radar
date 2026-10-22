<div align="center">

# ⚡ xLFr4n / Habbo Furni Radar

**Detectar novedades, contrastar datos y avisar solo cuando hay algo nuevo.**

<p>
  <img src="https://img.shields.io/badge/Python-3.12-FF3344?style=for-the-badge&logo=python&logoColor=white&labelColor=111318" alt="Python 3.12">
  <img src="https://img.shields.io/badge/GitHub_Actions-Automatización-252A34?style=for-the-badge&logo=githubactions&logoColor=white" alt="GitHub Actions">
  <img src="https://img.shields.io/badge/Habbo-Collectibles-252A34?style=for-the-badge&labelColor=111318" alt="Habbo Collectibles">
</p>

<sub>Fuentes públicas · Detección de novedades · Avisos en Discord</sub>

</div>

---

<p align="center">
  <a href="#qué-hace">Qué hace</a> ·
  <a href="#fuentes-de-datos">Fuentes</a> ·
  <a href="#flujo-de-trabajo">Flujo</a> ·
  <a href="#pruebas-en-local">Pruebas</a> ·
  <a href="#automatización">Automatización</a> ·
  <a href="#seguridad-y-límites">Seguridad</a>
</p>

## Qué hace

**Habbo Furni Radar** consulta la Shop API pública de Habbo Collectibles, compara los códigos **productCode** con el estado guardado y publica un aviso en Discord cuando detecta un artículo nuevo.

La primera ejecución registra el catálogo inicial en vez de convertir todo el inventario existente en una avalancha de notificaciones. En las siguientes ejecuciones compara los datos con el estado persistente para reducir avisos duplicados.

La Shop API es la señal principal de detección. Los datos de furnidata, precios y conversión de moneda se usan como complemento cuando las fuentes los ofrecen.

## Fuentes de datos

| Fuente | Para qué se utiliza |
| --- | --- |
| **Shop API** | Detectar artículos y cambios en el catálogo consultado. |
| **Prices API** | Incorporar información de mercado cuando está disponible. |
| **Habbo furnidata** | Añadir información técnica complementaria. |
| **CoinGecko** | Obtener una referencia de conversión de ETH cuando responde la fuente. |

Una fuente puede fallar, cambiar de formato o devolver datos incompletos. La ausencia de un valor no debe interpretarse como cero ni como una cifra confirmada.

## Flujo de trabajo

~~~text
GitHub Actions
      |
      v
Consulta programada
      |
      v
Habbo Shop API -------> Estado conocido
      |                       |
      v                       v
¿productCode nuevo? ------> Deduplicación
      |
      +-- No: continuar
      |
      +-- Sí: enriquecer si hay datos
                    |
                    v
                Aviso Discord
~~~

El catálogo persistente se guarda en **state/known_shop_items.json**.

## Pruebas en local

Necesitas Python 3.12 y las dependencias del proyecto instaladas. Ejecuta los comandos desde la raíz del repositorio.

**Suite de pruebas**

~~~bash
python -m unittest discover -s tests -v
~~~

**Simulación sin publicar avisos**

~~~bash
python radar.py --dry-run
~~~

La opción **--dry-run** muestra qué artículos nuevos detectaría sin publicar en Discord ni modificar el estado persistente. En un primer arranque tampoco inicializa el catálogo, por lo que la simulación no consume los avisos de la primera ejecución real.

## Automatización

El workflow de GitHub Actions realiza las consultas programadas y compara los resultados con el estado guardado. Incluye deduplicación, reintentos ante errores temporales, enriquecimiento opcional y una inicialización que evita reproducir todo el catálogo como alertas.

Para habilitar los avisos, configura el webhook de Discord como un **GitHub Actions Secret** con el nombre que espera el workflow del repositorio. No lo añadas al código, a los archivos versionados ni a la salida de depuración.

## Qué puede incluir un aviso

Según los datos disponibles, una notificación puede contener nombre, rareza, colección, puntuación, coste, oferta, fechas, descripción, metadatos técnicos, referencias ETH/USD/EUR, enlace al mercado y artwork oficial.

No todos esos campos están presentes en todas las respuestas. La información debe leerse como una instantánea de las fuentes consultadas, no como una tasación definitiva.

## Seguridad y límites

- No hace falta una wallet, clave privada, seed phrase, VPS ni servidor permanente.
- Los avisos reflejan los datos que devolvieron las fuentes en el momento de la consulta.
- Los precios son referencias de mercado, no una garantía de venta ni de liquidez.
- Si una fuente no aporta un valor, debe mantenerse como desconocido.
- No publiques secretos, tokens o credenciales en commits, incidencias o logs.

## Documentación

- **BRAND.md** — identidad visual y criterios del proyecto.
- **scripts/README.md** — scripts auxiliares.
- **SECURITY.md** — límites y prácticas de seguridad.
- **CONTRIBUTING.md** — guía para proponer cambios.

Si detectas un comportamiento incorrecto, aporta los pasos para reproducirlo y, cuando sea posible, una respuesta de ejemplo sin credenciales ni datos sensibles.

<div align="center">

**Detectar → Enriquecer → Verificar → Avisar**  
<sub>⚡ xLFr4n · Fuentes públicas · Señal útil</sub>

</div>

<details>
<summary><strong>English</strong></summary>

## What it does

Habbo Furni Radar polls the public Habbo Collectibles Shop API, compares productCode values with saved state, and sends a Discord alert when a new item is detected. The initial run records the existing catalogue instead of replaying the full inventory as notifications.

The Shop API is the primary detection signal. Furnidata, pricing and ETH conversion are used for enrichment when those sources provide data.

## Local checks

Run the test suite:

~~~bash
python -m unittest discover -s tests -v
~~~

Preview detected items without sending Discord alerts or changing persistent state:

~~~bash
python radar.py --dry-run
~~~

The persistent catalogue is stored in state/known_shop_items.json. On the first run, dry-run mode does not initialise the catalogue, so the preview does not consume first-run notifications.

## Automation and data quality

The scheduled GitHub Actions workflow handles polling, deduplication, retries and optional enrichment. Store the Discord webhook as a GitHub Actions Secret using the name expected by the workflow; never commit it or print it to logs.

No wallet, private key, seed phrase, VPS or always-on server is required. Alerts represent the information returned by the configured sources at query time. Market prices are references, not guaranteed sale values.

See SECURITY.md and CONTRIBUTING.md before making changes.

</details>