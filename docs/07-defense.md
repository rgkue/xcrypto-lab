# 07 — Defensa y Mitigación

---

### Mantener el sistema actualizado

Lo más importante que puedes hacer es mantener tu sistema operativo actualizado. Windows Update no es opcional — cada actualización contiene parches para vulnerabilidades que los atacantes ya conocen y están explotando activamente. Si tu versión de Windows ya no recibe actualizaciones (como Windows 7), considera actualizar a Windows 10 u 11.

### Hacer backups

Los backups son tu única garantía real contra el ransomware. Ningún antivirus puede prometerte que nunca serás infectado, pero un backup actualizado garantiza que aunque pierdas acceso a tus archivos, puedas recuperarlos.

**Regla 3-2-1:** tres copias de tus datos, en dos medios distintos, con uno desconectado de la red. Un disco externo que no está conectado a tu computador no puede ser cifrado por un ransomware.

### Precaución con archivos adjuntos

Si recibes un archivo adjunto por correo que no esperabas, aunque sea de alguien conocido, no lo abras. La mayoría de infecciones de ransomware comienzan con un archivo de Office con macros, un PDF malicioso o un ejecutable disfrazado de documento. Si tienes dudas, contacta a quien lo envió por otro medio antes de abrirlo.

### Desconfiar de mensajes urgentes

Desconfía de mensajes que te pidan hacer clic en un enlace o descargar algo con urgencia. Esta técnica se llama ingeniería social y es la forma más común de entrada del malware — no por exploits técnicos sino por manipulación humana.

---

## Deshabilitar SMBv1

SMBv1 es la versión del protocolo responsable de EternalBlue y no tiene ningún uso legítimo en sistemas modernos. Deshabilitarlo no afecta el funcionamiento normal de Windows y elimina completamente el vector de ataque más explotado de la última década.

**En Windows 10 y 11:**
Panel de Control → Programas → Activar o desactivar características de Windows → desmarcar "Compatibilidad con el protocolo para compartir archivos SMB 1.0/CIFS" → Aceptar → reiniciar.

**En entornos corporativos (PowerShell como administrador):**

```powershell
Set-SmbServerConfiguration -EnableSMB1Protocol $false
```

**Verificar que quedó deshabilitado:**

```powershell
Get-SmbServerConfiguration | Select EnableSMB1Protocol
```

---

## Revisar Conexiones Activas con netstat

`netstat` es una herramienta nativa de Windows que muestra todas las conexiones de red activas. Permite detectar conexiones sospechosas hacia IPs desconocidas que podrían indicar acceso remoto no autorizado o comunicación con un servidor C2.

```cmd
netstat -ano
```

La columna PID muestra el identificador del proceso que tiene cada conexión. Con ese número se puede ir al Administrador de Tareas → pestaña Detalles para ver exactamente qué programa mantiene esa conexión. Una conexión saliente constante hacia una IP externa desde un proceso desconocido es una señal de alerta.

Para usuarios no técnicos, **TCPView** de Microsoft Sysinternals muestra la misma información de forma visual.

Descarga TCPView: https://learn.microsoft.com/en-us/sysinternals/downloads/tcpview

---

## Revisar Programas que Arrancan con Windows

El malware que usa persistencia se registra en el sistema para ejecutarse automáticamente en cada inicio. En Windows, abrir el Administrador de Tareas → pestaña Inicio. Cualquier programa desconocido en esa lista merece investigación.

Para una revisión más completa, **Autoruns** de Microsoft Sysinternals muestra absolutamente todo lo que se ejecuta al arrancar Windows, incluyendo entradas en el registro que el Administrador de Tareas no muestra.

Descarga Autoruns: https://learn.microsoft.com/en-us/sysinternals/downloads/autoruns

En el contexto de este laboratorio, el ransomware escribía su ruta en:

```
HKCU\Software\Microsoft\Windows\CurrentVersion\Run
```

bajo el nombre `"WindowsSecurityUpdate"` para camuflarse como una actualización legítima del sistema.

---


**Principio de mínimo privilegio:** los usuarios del día a día no deben tener cuentas de administrador. Si un ransomware corre con una cuenta limitada, solo puede cifrar los archivos de ese usuario — no puede tocar archivos del sistema ni instalar persistencia a nivel de sistema.

**Segmentación de red con VLANs:** impide que un ransomware que infecta un equipo pueda propagarse lateralmente al resto de la red. Si el equipo infectado está en una VLAN aislada, el gusano no encuentra otros equipos a los que saltar.

**Inventario de software actualizado:** permite detectar instalaciones no autorizadas. Python, PyInstaller o cualquier herramienta de scripting instalada en un equipo que no debería tenerlas es una señal de alerta.

**Monitoreo de comportamiento (EDR):** detecta patrones como el cifrado masivo de archivos en un corto período de tiempo, que es la firma característica de cualquier ransomware independientemente de su código.
