---
name: clima
description: Obtiene el clima actual (o pronóstico) de Lima, Perú usando wttr.in vía curl, sin necesidad de API key. Úsala cuando el usuario pregunte por el clima, temperatura, pronóstico o condiciones meteorológicas — la ciudad por defecto es Lima, Perú.
---

# Clima

Consulta el clima actual o el pronóstico de **Lima, Perú** (ciudad del usuario) usando el
servicio público [wttr.in](https://wttr.in), sin necesidad de registrarse ni configurar una
API key.

La ciudad por defecto es **Lima**. Úsala siempre que el usuario pregunte por "el clima" sin
especificar otra ciudad. Solo consulta otra ciudad si el usuario la menciona explícitamente.

## Uso rápido

Resumen en una línea (ideal para respuestas cortas):

```bash
curl -s "wttr.in/Lima?format=3"
# Lima: ☀️  +19°C
```

## Reporte detallado (ASCII art, 3 días)

```bash
curl -s "wttr.in/Lima?m"
```

El parámetro `?m` fuerza unidades métricas (°C, km/h).

Para solo el día de hoy, sin pronóstico extendido:

```bash
curl -s "wttr.in/Lima?m0"
```

## Datos estructurados (JSON)

Cuando necesites parsear valores específicos (temperatura, humedad, viento, etc.) en vez de
mostrar texto:

```bash
curl -s "wttr.in/Lima?format=j1"
```

Campos útiles dentro de `current_condition[0]`: `temp_C`, `FeelsLikeC`, `humidity`,
`windspeedKmph`, `weatherDesc[0].value`, `precipMM`.

## Otras ciudades

Si el usuario pide explícitamente el clima de otra ciudad, sustituye `Lima` por el nombre de
esa ciudad (reemplaza espacios por `+`, ej. `New+York`).

## Notas

- El comando requiere conexión a internet; no hay caché ni almacenamiento local de datos.
- Si `curl` falla o tarda demasiado, usa `-m 5` (timeout de 5s) y reporta al usuario que el
  servicio no está disponible en vez de reintentar en bucle.
