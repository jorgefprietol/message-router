# Message Router

Servicio de clasificación de correos y tickets con colas operativas, prioridades y vencimientos de SLA. Conserva el identificador del sistema de origen, evita duplicados y permite corregir una clasificación sin reiniciar el reloj de atención.

## Ejecutar

Requiere Python 3.10 o posterior. El modo local utiliza la biblioteca estándar y SQLite.

```powershell
python -m app demo
python -m unittest discover -s tests -v
$env:SERVICE_TOKEN = 'una-clave-local-de-al-menos-24-caracteres'
python -m app serve
```

La consola de pruebas está en `http://127.0.0.1:8080`. `demo` usa datos sintéticos y una base temporal; `serve` conserva el estado en `var/service.db`.

```powershell
python -m app run --input data/message.json --key email-demo-001
python -m app metrics
```

## Flujo de trabajo

1. `submit` valida canal, identidad y tamaño del mensaje.
2. El clasificador selecciona categoría, prioridad y cola. Una coincidencia ambigua o insuficiente pasa a revisión.
3. `override` registra operador y motivo; calcula el nuevo SLA desde la recepción original.
4. `close` registra el cierre y si ocurrió dentro del vencimiento. Un mensaje pendiente de revisión debe reclasificarse primero.

El contenido del correo no se almacena. La deduplicación utiliza una huella SHA-256 del remitente, asunto y cuerpo. La tabla operativa conserva identificadores, clasificación y tiempos; el sistema emisor sigue siendo la fuente del mensaje.

| Acción | Campos adicionales a `tenant` |
| --- | --- |
| `submit` | `external_id`, `channel`, `sender`, `subject`, `body` |
| `get` | `id` |
| `queues` | Ninguno |
| `override` | `id`, `actor`, `reason`, `category`, `priority` |
| `close` | `id`, `actor` |

Categorías: `security`, `billing`, `technical`, `sales`, `general`. SLA del ejercicio: P1 15 minutos, P2 60, P3 240 y P4 1440. Estos tiempos son una política configurable en código, no resultados de una operación real.

## API e integración

`POST /api/run` recibe el mismo objeto que la CLI. Requiere `Authorization: Bearer <SERVICE_TOKEN>` e `Idempotency-Key` para cada operación. `GET /health` comprueba el proceso y `GET /api/metrics` devuelve mediciones locales de solicitudes completadas.

El webhook de n8n incluido actúa como entrada hacia esta API. IMAP, Gmail y CRM requieren sus propios adaptadores y credenciales; este repositorio no envía correos ni crea tickets en sistemas externos.

Para probar clasificación con un modelo instalado en Ollama:

```powershell
$env:MODEL_BACKEND = 'ollama'
$env:OLLAMA_URL = 'http://127.0.0.1:11434'
$env:OLLAMA_MODEL = '<modelo-instalado>'
python -m app demo
```

El resultado del modelo debe cumplir el contrato y nunca concede permisos de ejecución. La prioridad continúa bajo reglas del servicio. La calidad de clasificación requiere evaluación con mensajes etiquetados del dominio objetivo.

## Medición y decisiones

Los eventos permiten calcular distribución por cola, tasa de revisión y cumplimiento del SLA en el conjunto procesado. Para medir precisión y ahorro de tiempo hace falta un conjunto etiquetado y una línea base manual. No se incluyen porcentajes de mejora estimados.

[Arquitectura](docs/architecture.md) · [Decisiones](docs/decisions.md) · [Operación](docs/operations.md) · [Referencias](docs/references.md)
