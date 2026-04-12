# 02 — Desarrollo del Ransomware

> El código fuente no está incluido en este repositorio. Esta sección documenta las decisiones de diseño y la lógica del malware con fines exclusivamente educativos.

---

## Entorno de Desarrollo

El desarrollo completo ocurrió en el equipo host con Windows 11, usando Python 3.14.2 y Visual Studio Code. Posteriormente el código fue compilado en Windows 7 para garantizar compatibilidad — el motivo de esto se detalla en [05-issues.md](05-issues.md).

---

## Lógica Central

### Cifrado

Al ejecutarse, el ransomware cifra silenciosamente todos los archivos dentro de una ruta objetivo (`TARGET`) usando **Fernet**, un esquema de cifrado simétrico autenticado que internamente combina AES-128-CBC para confidencialidad con HMAC-SHA256 para integridad. Si un archivo cifrado es modificado aunque sea en un solo byte, Fernet lanza `InvalidToken` al intentar descifrar en lugar de devolver datos corruptos.

La clave se deriva de una contraseña usando **PBKDF2HMAC** con SHA-256 y 100.000 iteraciones. 

### Alcance del Cifrado

El `TARGET` define el alcance del cifrado. Durante el desarrollo se probaron distintas configuraciones, desde una carpeta de pruebas en `C:\Users\Isaac\Desktop` hasta el `C:\` completo con una lista de exclusiones para evitar dejar el sistema completamente inutilizable durante las pruebas.

### Persistencia

La persistencia se implementó escribiendo la ruta del ejecutable en la clave del registro de Windows:

```
HKCU\Software\Microsoft\Windows\CurrentVersion\Run
```

bajo el nombre `"WindowsSecurityUpdate"` para camuflarse como una actualización legítima del sistema. Esta clave no requiere privilegios de administrador y garantiza que el ransomware se ejecute en cada inicio de sesión del usuario.

### Kill Switch

Cuando el timer llega a cero, el kill switch destruye la clave de cifrado en memoria usando `ctypes.memset`, haciendo el descifrado imposible. Este mecanismo replica la presión psicológica que los ransomwares reales ejercen sobre las víctimas.

### Interfaz

La pantalla de rescate ocupa todo el escritorio del usuario, muestra un contador regresivo y el campo para ingresar la clave de descifrado. Se implementó con **tkinter**.

---

## Limitaciones Didácticas Intencionales

| Limitación | Comportamiento real en ransomware profesional |
|---|---|
| Contraseña hardcodeada en el ejecutable | La clave nunca existe en el cliente — se genera localmente, se cifra con RSA pública del atacante y solo el servidor C2 puede descifrarla |
| Salt fijo en PBKDF2 | Salt aleatorio generado con `os.urandom(16)` y almacenado junto al archivo cifrado |
| Clave simétrica única para todos los archivos | Clave única por archivo o por víctima |

Estas limitaciones son intencionales — el objetivo del laboratorio es comprender el comportamiento del ransomware, no construir uno operacionalmente seguro para un atacante.

---

## Empaquetado

El script fue convertido a ejecutable `.exe` con **PyInstaller** usando el flag `--onedir --noconsole` para que corriera en Windows 7 sin Python instalado y sin mostrar ventana de consola. Los motivos por los que se usó `--onedir` en lugar de `--onefile` se explican en [05-issues.md](05-issues.md).
