# Decisiones

| Decisión | Motivo | Límite |
| --- | --- | --- |
| Reglas legibles como modo predeterminado | Reproducir el recorrido sin servicios externos | El vocabulario necesita ajuste por sector |
| Huella del contenido | Detectar reentregas inconsistentes sin almacenar correos | No sustituye controles del proveedor de correo |
| SLA desde recepción original | Evitar que una reclasificación oculte retrasos | La política usa minutos corridos, sin calendario laboral |
| Revisión de baja confianza | Mantener una cola explícita para casos ambiguos | La confianza local es heurística, no probabilidad calibrada |
| SQLite transaccional | Estado portátil y demostración reproducible | Un despliegue distribuido requiere otro almacenamiento |

La integración se inspira en separar el webhook de la lógica de negocio y en conservar trazabilidad por operación. El código y el contrato del servicio se implementaron para este proyecto.
