# Renombrar-y-Preparar-Archivos-.ovpn-de-Hack-The-Box
🧠 Cómo Renombrar y Preparar Archivos .ovpn de Hack The Box en Kali Linux

❗ Problemas comunes al descargar .ovpn de Hack The Box:
Archivos se descargan con nombres como lab_cyb3rNAV(4).ovpn, lo que rompe Zsh o mv.

OpenVPN falla si estás detrás de NAT/ISP que bloquean UDP (proto udp).

sudo lanza errores como unable to resolve host.

✅ Solución completa en 3 pasos
🛠️ Paso 1: Renombrar automáticamente archivos .ovpn con paréntesis
Crea un script llamado renombrar_ovpn.sh:

#!/bin/bash

# Contador para nuevos nombres
i=1

# Buscar archivos con paréntesis en el nombre
for file in lab_cyb3rNAV\(*\).ovpn; do
    [ -e "$file" ] || continue
    
    new_name="htb_tcp_$i.ovpn"
    
    mv "$file" "$new_name"
    echo "✅ Renombrado: $file → $new_name"
    
    # Reemplaza proto udp por proto tcp para mayor compatibilidad
    sed -i 's/^proto udp/proto tcp/' "$new_name"
    
    ((i++))
done

▶️ Cómo ejecutar:
Guarda el script:


nano renombrar_ovpn.sh


Dale permisos:


chmod +x renombrar_ovpn.sh


./renombrar_ovpn.sh


🚀 Paso 2: Conéctate con OpenVPN usando TCP
Ahora puedes conectarte usando el nuevo archivo, ya con proto tcp:


sudo openvpn htb_tcp_1.ovpn


🔐 Bonus: Verifica que el archivo se haya modificado correctamente
grep '^proto' htb_tcp_1.ovpn
# Debería mostrar: proto tcp

🧠 Conclusión
Este script te automatiza todo el proceso:

Renombrar archivos .ovpn problemáticos

Corregir el protocolo para aumentar la compatibilidad

