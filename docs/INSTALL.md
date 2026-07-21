# Vortigaunt Installation Guide

## Requisitos

Vortigaunt está diseñado para sistemas Linux ligeros y servidores domésticos de bajo consumo.

Requisitos mínimos:

- Linux con Bash
- systemd
- permisos de administrador (`sudo`)
- `mdadm` (opcional, para monitoreo RAID1)

Hardware recomendado:

- Equipos de bajo consumo
- Servidores domésticos
- Hardware reutilizado
- Sistemas con almacenamiento redundante

---

# Instalación

## Clonar el repositorio

```bash
git clone https://github.com/jhoninsaurralde/vortigaunt.git
cd vortigaunt
```

---

## Configuración

Crear el directorio de configuración:

```bash
sudo mkdir -p /etc/vortigaunt
```

Copiar la configuración predeterminada:

```bash
sudo cp config/vortigaunt.conf /etc/vortigaunt/
```

Instalar los scripts:

```bash
sudo cp scripts/* /usr/local/bin/
```

Verificar que los scripts estén disponibles:

```bash
ls /usr/local/bin/vortigaunt-*
```

---

# Systemd

Copiar las unidades de servicio:

```bash
sudo cp systemd/* /etc/systemd/system/
```

Recargar la configuración de systemd:

```bash
sudo systemctl daemon-reload
```

Activar el servicio de inicio:

```bash
sudo systemctl enable vortigaunt-boot.service
```

Activar los timers automáticos:

```bash
sudo systemctl enable vortigaunt-monitor.timer
sudo systemctl enable vortigaunt-report.timer
sudo systemctl enable vortigaunt-backup.timer
```

Iniciar los timers:

```bash
sudo systemctl start vortigaunt-monitor.timer
sudo systemctl start vortigaunt-report.timer
sudo systemctl start vortigaunt-backup.timer
```

---

# Verificación

Comprobar el estado general:

```bash
vortigaunt-status
```

Ejecutar diagnóstico:

```bash
vortigaunt-check
```

Ver servicios y timers activos:

```bash
systemctl list-timers | grep vortigaunt
```

Generar un reporte manual:

```bash
vortigaunt-report
```

Crear un backup manual:

```bash
vortigaunt-backup
```

---

# Estructura de datos

La ruta principal de Vortigaunt se define mediante:

```
/etc/vortigaunt/vortigaunt.conf
```

La estructura generada por defecto:

```text
vortigaunt/

├── database/
│   ├── identity.md
│   ├── facts.md
│   ├── rules.md
│   └── events.md
│
├── memory/
│   ├── events.log
│   ├── experiences.log
│   ├── milestones.log
│   └── system/
│       ├── current/
│       └── history/
│
├── reports/
│
└── backups/
    ├── archive/
    └── latest/
```

---

# Primer inicio

Después de la instalación, Vortigaunt comenzará a registrar automáticamente:

- Eventos del sistema
- Estado del hardware
- Temperatura del CPU
- Uso de memoria
- Estado RAID
- Experiencias del sistema
- Hitos importantes
- Reportes periódicos
- Backups programados

El sistema funciona de manera autónoma mediante servicios y timers de systemd.

---

# Solución de problemas

## Ver logs de servicios

Monitor:

```bash
journalctl -u vortigaunt-monitor.service
```

Reportes:

```bash
journalctl -u vortigaunt-report.service
```

Backups:

```bash
journalctl -u vortigaunt-backup.service
```

---

## Ver estado de un timer específico

Ejemplo:

```bash
systemctl status vortigaunt-monitor.timer
```

---

# Desinstalación

Detener timers:

```bash
sudo systemctl stop vortigaunt-monitor.timer
sudo systemctl stop vortigaunt-report.timer
sudo systemctl stop vortigaunt-backup.timer
```

Deshabilitar servicios:

```bash
sudo systemctl disable vortigaunt-monitor.timer
sudo systemctl disable vortigaunt-report.timer
sudo systemctl disable vortigaunt-backup.timer
```

Eliminar scripts:

```bash
sudo rm /usr/local/bin/vortigaunt-*
```

Eliminar unidades systemd:

```bash
sudo rm /etc/systemd/system/vortigaunt*
```

Recargar systemd:

```bash
sudo systemctl daemon-reload
```

---

# Notas

Vortigaunt fue diseñado como un sistema simple y transparente.

No utiliza bases de datos pesadas ni dependencias complejas.

Toda la información permanece almacenada en archivos legibles,
permitiendo inspección manual, recuperación sencilla y mantenimiento a largo plazo.
