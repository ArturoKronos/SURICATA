#!/bin/bash

# Definir rutas de los archivos de registro
LOG_FILE="/var/log/suricata/fast.log"
BOTITO_LOG="/var/log/suricata/botito.log"
MENSAJE_LOG="/var/log/suricata/Alerta.txt"
FECHA_ACTUAL=$(date +"%Y-%m-%d")

# Validar si el archivo de log existe antes de continuar
if [ ! -f "$LOG_FILE" ]; then
    echo "Error: Archivo de log $LOG_FILE no encontrado."
    exit 1
fi

# Obtener la última IP y el último mensaje del archivo de registro
LAST_IP=$(awk '{print $1}' "$LOG_FILE" | tail -n 1)
LAST_MSG=$(cat "$LOG_FILE")

# Leer la última IP almacenada en el archivo botito.log (si existe)
if [ -f "$BOTITO_LOG" ]; then
    STORED_IP=$(cat "$BOTITO_LOG")
else
    STORED_IP=""
fi

# Comparar la última IP con la almacenada
if [ "$LAST_IP" != "$STORED_IP" ]; then
    # Crear un archivo temporal para el mensaje
    TEMP_FILE=$(mktemp)
    echo "$LAST_MSG" > "$TEMP_FILE"

    # Filtrar líneas que no contengan la dirección IP específica
    grep -v " {TCP} 172.26.17.35:" "$TEMP_FILE" > "$MENSAJE_LOG"

    # Definir credenciales de Telegram
    TELEGRAM_BOT_TOKEN="Escribe-tu-token"
    CHAT_ID="Escribe-tu-chat-id"

    # Enviar alerta a Telegram con el archivo adjunto
    curl -F "document=@${MENSAJE_LOG}" \
         -H "Content-Type:multipart/form-data" \
         "https://api.telegram.org/bot${TELEGRAM_BOT_TOKEN}/sendDocument?chat_id=${CHAT_ID}&caption=IP detectada: ${LAST_IP}"

    # Limpiar archivos temporales y actualizar logs
    rm -f "$TEMP_FILE" "$MENSAJE_LOG"
    echo "$LAST_IP" > "$BOTITO_LOG"

    # Hacer una copia del log actual antes de limpiarlo
    cp "$LOG_FILE" "/var/log/suricata/copias_fast/copia.$FECHA_ACTUAL"
    echo "" > "$LOG_FILE"
else
    echo "No hay novedades."
fi
