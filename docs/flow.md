# Flujo de operación

![Flujo de operación](diagrams/flow.svg)

[Abrir SVG](diagrams/flow.svg) · [Fuente Mermaid](diagrams/flow.mmd)


<details>
<summary>Versión Mermaid</summary>

```mermaid
flowchart TD
    Start["Recibir submit"] --> Validate["Validar tenant, canal y tamaño"]
    Validate --> Duplicate{"¿Identidad de origen existente?"}
    Duplicate -->|Sí| Compare{"¿Huella idéntica?"}
    Compare -->|Sí| Previous["Devolver mensaje existente"]
    Compare -->|No| Reject["Rechazar reuso del identificador"]
    Duplicate -->|No| Classify["Clasificar asunto y cuerpo"]
    Classify --> SLA["Aplicar prioridad y vencimiento UTC"]
    SLA --> Confidence{"¿Requiere revisión?"}
    Confidence -->|Sí| Override["Corregir con operador y motivo"]
    Confidence -->|No| Queue["Guardar en cola operativa"]
    Override --> Queue
    Queue --> Close["Cerrar y registrar cumplimiento de SLA"]
```

</details>

- La identidad de negocio combina `tenant`, canal e identificador externo; otra carga con esa identidad se rechaza.
- Ollama puede proponer categoría y confianza. Las prioridades y plazos se calculan con reglas del servicio.
- `override` conserva la recepción original al recalcular el vencimiento; `close` exige resolver primero la revisión.

Las operaciones del dominio se guardan junto a auditoría y respuesta idempotente en la transacción del servicio local. Un error revierte los cambios de esa solicitud.

[Arquitectura](architecture.md) · [Tecnologías y alcance](technologies.md)
