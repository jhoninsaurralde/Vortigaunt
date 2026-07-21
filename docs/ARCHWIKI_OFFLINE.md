# ArchWiki Offline en Homelab con Kiwix

## Objetivo

Montar una copia offline de ArchWiki dentro del homelab utilizando el formato `.zim` y servirla mediante Kiwix Web.

Esto permite consultar la documentación de Arch Linux desde cualquier equipo de la red local sin depender de Internet.

---

## 1. Instalar Kiwix

Actualizar repositorios:

```bash
sudo apt update
```

Instalar herramientas Kiwix:

```bash
sudo apt install kiwix-tools
```

---

## 2. Crear directorio para libros ZIM

Crear la carpeta donde se almacenarán los archivos offline:

```bash
mkdir -p /mnt/raid/vortigaunt/knowledge/zim
```

Estructura utilizada:

```
/mnt/raid/vortigaunt/knowledge/
└── zim/
    └── archlinux_en_all_maxi_2026-04.zim
```

---

## 3. Copiar ArchWiki ZIM al servidor

El archivo `.zim` descargado se debe copiar al almacenamiento del homelab.

Ejemplo si el archivo está en un USB montado:

```bash
cp /mnt/usb/archlinux_en_all_maxi_2026-04.zim \
/mnt/raid/vortigaunt/knowledge/zim/
```

Verificar:

```bash
ls -lh /mnt/raid/vortigaunt/knowledge/zim
```

Ejemplo:

```
-rwxr-xr-x 1 jonathan jonathan 34M archlinux_en_all_maxi_2026-04.zim
```

---

## 4. Crear servicio Systemd

Crear servicio:

```bash
sudo nano /etc/systemd/system/kiwix.service
```

Contenido:

```ini
[Unit]
Description=Kiwix ArchWiki Offline
After=network.target

[Service]
Type=simple
User=jonathan
ExecStart=/usr/bin/kiwix-serve --port=8080 /mnt/raid/vortigaunt/knowledge/zim/archlinux_en_all_maxi_2026-04.zim
Restart=always

[Install]
WantedBy=multi-user.target
```

Guardar y salir.

---

## 5. Activar servicio

Recargar systemd:

```bash
sudo systemctl daemon-reload
```

Activar inicio automático:

```bash
sudo systemctl enable kiwix
```

Iniciar servicio:

```bash
sudo systemctl start kiwix
```

---

## 6. Verificar estado

```bash
systemctl status kiwix
```

Debe mostrar:

```
Active: active (running)
```

---

## 7. Verificar puerto

```bash
ss -tlnp | grep 8080
```

Ejemplo:

```
LISTEN 0 4096 *:8080 users:(("kiwix-serve"...))
```

---

## 8. Acceder desde la red

Obtener IP del servidor:

```bash
hostname -I
```

Ejemplo:

```
192.168.0.101
```

Abrir desde cualquier equipo:

```
http://192.168.0.101:8080
```

Resultado esperado:

```
ArchWiki
Arch Linux documentation
Powered by Kiwix
```

---

## Comandos útiles

Reiniciar Kiwix:

```bash
sudo systemctl restart kiwix
```

Detener:

```bash
sudo systemctl stop kiwix
```

Ver logs:

```bash
journalctl -u kiwix -f
```

Ver proceso:

```bash
ps aux | grep kiwix
```

---

## Notas

* El archivo `.zim` contiene una copia estática de ArchWiki.
* No requiere base de datos.
* No necesita conexión a Internet una vez descargado.
* El servicio queda disponible automáticamente después de reiniciar el homelab.
