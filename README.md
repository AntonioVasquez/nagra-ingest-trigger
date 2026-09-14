# nagra-ingest-trigger

Repo público que **solo** dispara, cada 12h, la ingesta de precios de
[Radar Ofertas NAGRA](https://radar-ofertas-nagra.netlify.app) contra la API
en Render. No contiene código de la aplicación, de los adaptadores de
scraping ni de la lógica de negocio -- eso vive en el repo privado original.

Existe como repo aparte y público porque los minutos gratis de GitHub
Actions se agrupan por cuenta entre todos los repos **privados** (2.000
min/mes), y una corrida completa de la ingesta troceada por adaptador tarda
~60 min -- corriendo cada 12h eso solo ya superaba el límite. Los repos
**públicos** tienen minutos ilimitados, y como este repo no tiene nada
sensible que exponer (solo el loop de disparo, que igual requiere el
secret de abajo para funcionar), pasar a público acá no tiene costo real.

## Configuración

Dos secrets en **Settings → Secrets and variables → Actions**:

- `API_URL`: URL pública del servicio de la API en Render (ej.
  `https://radar-ofertas-nagra-api.onrender.com`, sin `/` al final).
- `INGEST_TRIGGER_SECRET`: el mismo valor que tiene configurado como
  variable de entorno el servicio de la API en Render (así el endpoint
  interno de la API reconoce que la request viene de acá).

Sin estos dos secrets el workflow falla de inmediato (401 en la primera
llamada).

## Disparo manual

Pestaña **Actions → Ingesta cada 12h → Run workflow**, para forzar una
corrida sin esperar el próximo disparo programado.

