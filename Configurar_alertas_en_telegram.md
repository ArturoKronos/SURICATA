
# Alertas en Telegram 

![img](img/img10..png)

## ¿Que es?
**Telegram** es una aplicación de mensajería instantánea que permite enviar mensajes, hacer llamadas y videollamadas, compartir archivos y crear grupos o canales. Es conocida por su enfoque en la seguridad y la velocidad, ofreciendo chats cifrados, almacenamiento en la nube y la posibilidad de usar bots para automatizar tareas. Además, permite crear comunidades grandes con miles de miembros y compartir archivos sin límites de tamaño.

## ¿Como creamos el bot en Telegram?: / ¿Como lo configuro?:
En el siguiente enlace tenemos un video hecho por mi de la creacion del bot en  Telegram y de otro uso distinto que seguramente sea de interes:

[MIRA COMO CREAMOS EL BOT Y SUS MULTIPES USOS ](https://www.youtube.com/watch?v=3ZwDs3u6pHc)

## Configuración de la alerta: 
Para activar las notificaciones en Telegram, solo sigue estos pasos y ejecuta el script proporcionado (bobito.sh), que viene en este repositorio.

### Pasos:
-1 Copia el script bobito.sh en el directorio /etc/suricata. [SCRIPT](script.md)
  -Expecificaciones:
✅ Validación de existencia de archivos antes de procesarlos.

✅ Se eliminan espacios extra innecesarios en los archivos de log.

✅ Se agregan comentarios más claros.

✅ Se mejora la ejecución de curl para evitar problemas de formato.

✅ Se cambia eval "$curl_command" por una ejecución directa más segura.
