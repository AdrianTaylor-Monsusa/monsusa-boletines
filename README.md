# Boletines del Agua — Monsusa

Contenido mensual del **Boletín del Agua** que el workflow n8n `boletin_agua_mensual`
envía a todos los leads del CRM el **primer martes de cada mes a las 8:30 (hora México)**.

> Este repositorio contiene **solo los HTML de boletines**. NO contiene credenciales
> ni secretos. Es público para que n8n pueda leerlo por su URL raw.

## Convención de archivos

Un archivo por mes:

```
boletin-YYYY-MM.html   ->  ej: boletin-2026-07.html  (julio 2026)
```

El workflow calcula el mes en curso (zona `America/Mexico_City`) y descarga
`boletin-<AAAA-MM>.html` desde la URL raw de este repo. **Si el archivo del mes
no existe, el workflow termina sin enviar nada** (mecanismo seguro).

Carga el HTML del mes siguiente **antes** de que termine el mes en curso.

## Manifest embebido (imágenes + asunto)

Cada HTML debe empezar con un bloque de comentario que le dice al workflow qué
imágenes embeber (inline `cid:`) y qué asunto usar. Formato exacto:

```html
<!--BOLETIN
SUBJECT: Boletín del Agua · Julio 2026 — Monsusa
IMAGES:
logo|https://res.cloudinary.com/drkmrvahq/image/upload/v1782497515/monsusa_email_v2/logo.png|image/png
ptar|https://res.cloudinary.com/drkmrvahq/image/upload/v1782497526/monsusa_email_v2/ptar.jpg|image/jpeg
cliente_heineken|https://res.cloudinary.com/drkmrvahq/image/upload/v1782497527/monsusa_email_v2/cliente_heineken.png|image/png
-->
```

Reglas del manifest:

- Línea `SUBJECT:` — asunto del correo (obligatoria).
- Bloque `IMAGES:` — una imagen por línea, formato `cid|url|mime`.
  - `cid` = el identificador que usas en el HTML como `src="cid:ptar"`.
  - `url` = URL directa de Cloudinary (carpeta `monsusa_email_v2`, **sin** transformaciones
    en la URL — los assets ya están optimizados como base).
  - `mime` = `image/png` o `image/jpeg`.
- En el cuerpo del HTML, referencia cada imagen con `src="cid:<cid>"`.
- El comentario `<!--BOLETIN ... -->` NO se ve en el correo (el workflow lo elimina antes de enviar).

## Imágenes disponibles en Cloudinary (carpeta monsusa_email_v2)

| cid | archivo | mime |
|-----|---------|------|
| logo | logo.png | image/png |
| torre | torre.jpg | image/jpeg |
| chiller | chiller.jpg | image/jpeg |
| caldera | caldera.jpg | image/jpeg |
| osmosis | osmosis.jpg | image/jpeg |
| ultrafiltracion | ultrafiltracion.jpg | image/jpeg |
| ptar | ptar.jpg | image/jpeg |
| cliente_heineken | cliente_heineken.png | image/png |
| cliente_cemex | cliente_cemex.png | image/png |
| cliente_clarios | cliente_clarios.png | image/png |
| cliente_modelo | cliente_modelo.png | image/png |
| cliente_linde | cliente_linde.jpg | image/jpeg |
| cliente_greenpaper | cliente_greenpaper.png | image/png |

Base URL: `https://res.cloudinary.com/drkmrvahq/image/upload/<version>/monsusa_email_v2/<archivo>`

## Personalización opcional

Si el HTML incluye `{{nombre}}`, el workflow lo reemplaza por el campo `Nombre`
del lead. Si no aparece, el boletín se envía igual (genérico).
