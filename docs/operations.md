# Operación

La API escucha en loopback por defecto y exige un token de al menos 24 caracteres. Configure el token mediante el entorno y mantenga `var/` fuera de Git. `.env` se usa con Docker Compose; al ejecutar Python directamente exporte las variables en la consola.

Utilice una clave de idempotencia estable para la reentrega de una misma solicitud. Cambie la clave al ejecutar una acción diferente. Reutilizarla con otro cuerpo devuelve conflicto. Los identificadores externos deben permanecer estables durante el reintento del sistema de origen.

Antes de integrar un CRM, pruebe remitentes vacíos, adjuntos no admitidos, mensajes extensos, clasificación ambigua, duplicados y caída del modelo. Los adjuntos y su extracción no forman parte de este servicio.

Para recuperar el estado, detenga el proceso y copie la base SQLite con sus archivos WAL asociados o utilice la API de backup de SQLite. Pruebe la restauración en una carpeta separada. El cierre humano y sus motivos son evidencia operativa; un `actor` recibido en el cuerpo no prueba identidad autenticada.

La latencia de `/api/metrics` mide transacciones locales completadas. El cumplimiento del SLA se registra al cerrar mensajes; aún no hay un planificador que envíe alertas por vencimiento.
