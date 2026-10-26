# Tecnologías y alcance

Python 3.10+, SQLite, reglas de clasificación y fechas UTC. Ollama es opcional; se incluyen workflow n8n, Docker y pruebas automatizadas.

| Tecnología | Estado | Uso en este proyecto |
| --- | --- | --- |
| Python 3.10+ / re / datetime | Implementado | Reglas de clasificación, validación y vencimientos UTC. |
| SQLite / sqlite3 | Implementado | Mensajes operativos, colas, estados, idempotencia y eventos. |
| SHA-256 / hashlib | Implementado | Detección de reentregas y de identificadores reutilizados con otra carga. |
| Ollama / salida JSON estructurada | Opcional | Categoría y confianza validadas; no cambia los permisos del dominio. |
| n8n | Workflow incluido | Entrada webhook autenticada; necesita importación y credenciales. |
| Docker / Compose | Archivos incluidos | Empaquetado del servicio y perfil n8n; requiere validación del runtime elegido. |
| unittest / GitHub Actions | Implementado | Regresiones de clasificación, SLA, deduplicación y HTTP. |
| IMAP, Gmail y CRM | Por conectar | No se leen buzones, envían correos ni crean tickets externos. |

“Implementado” describe código disponible y comprobable en el repositorio. “Opcional” requiere un proveedor configurado; “incluido” identifica un archivo o adaptador entregado, sin afirmar una integración externa ejecutada.

[Arquitectura](architecture.md) · [Flujo de operación](flow.md) · [Integraciones](integrations.md)
