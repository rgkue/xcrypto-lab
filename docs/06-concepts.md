# 06 — Conceptos Técnicos

Glosario de los conceptos técnicos relevantes para este laboratorio.

---

## Kali Linux

Distribución de Linux desarrollada y mantenida por Offensive Security, diseñada específicamente para pruebas de penetración y auditorías de seguridad. Viene preinstalada con cientos de herramientas como Metasploit, Nmap, Wireshark y Burp Suite. Es el estándar de la industria para pentesters y equipos de red team.

Sitio oficial: https://www.kali.org

---

## Máquinas Virtuales (VM)

Software que emula un computador completo dentro de otro computador, permitiendo ejecutar sistemas operativos distintos de forma aislada. En este laboratorio se usaron dos VMs en VMware Workstation — una con Kali Linux como atacante y otra con Windows 7 como víctima — conectadas en una red interna sin acceso a internet ni a la red física del host.

Sitio oficial VMware: https://www.vmware.com

---

## Windows 7

Sistema operativo de Microsoft lanzado en 2009 que llegó al fin de su vida útil (EOL) el 14 de enero de 2020. Tras el EOL, Microsoft dejó de publicar parches de seguridad, lo que significa que las vulnerabilidades descubiertas después de esa fecha nunca serán corregidas oficialmente. Millones de equipos en hospitales, empresas y gobiernos aún corren Windows 7, convirtiéndolos en objetivos críticos.

---

## SMB (Server Message Block)

Protocolo de red desarrollado originalmente por IBM en 1983 y adoptado por Microsoft para Windows. Permite compartir archivos, impresoras y recursos entre equipos de una red. Opera principalmente en el **puerto 445**. Ha tenido tres versiones principales:

| Versión | Año | Características |
|---|---|---|
| SMBv1 | 1983 | Sin cifrado, múltiples vulnerabilidades conocidas |
| SMBv2 | 2006 | Más eficiente, mejoras de seguridad |
| SMBv3 | 2012 | Con cifrado |

Windows 7 tiene SMBv1 habilitado por defecto.

Documentación Microsoft: https://learn.microsoft.com/en-us/windows/win32/fileio/microsoft-smb-protocol-and-cifs-protocol-overview

---

## EternalBlue / MS17-010

Vulnerabilidad en la implementación de SMBv1 de Windows que permite a un atacante ejecutar código arbitrario en el sistema remoto **sin necesitar usuario ni contraseña**. Fue descubierta y explotada secretamente por la NSA durante años bajo el nombre EternalBlue. Microsoft publicó el parche MS17-010 el 14 de marzo de 2017 tras ser alertada de una filtración inminente por parte del grupo Shadow Brokers.

CVE oficial: https://msrc.microsoft.com/update-guide/vulnerability/CVE-2017-0144

---

## Python

Lenguaje de programación de alto nivel, interpretado y de propósito general. En este laboratorio se usó para desarrollar el ransomware por su claridad de código y la disponibilidad de librerías criptográficas de calidad. Se requirieron dos versiones distintas por incompatibilidades con Windows 7 — detallado en [04-versions.md](04-versions.md).

Sitio oficial: https://www.python.org

---

## Fernet (criptografía)

Esquema de cifrado simétrico autenticado que internamente combina **AES-128-CBC** para confidencialidad con **HMAC-SHA256** para integridad. Si un archivo cifrado es modificado aunque sea en un solo byte, Fernet lanza `InvalidToken` al intentar descifrar en lugar de devolver datos corruptos. Esto lo hace superior al uso de AES crudo en scripting.

Documentación: https://cryptography.io/en/latest/fernet/

---

## PBKDF2 (Key Derivation Function)

Función de derivación de clave que convierte una contraseña en una clave criptográfica segura. Aplica una función hash miles de veces con un salt para hacer los ataques de fuerza bruta computacionalmente costosos. En este laboratorio se usó con SHA-256 y 100.000 iteraciones, con la limitación didáctica de un salt fijo — en producción el salt debe ser aleatorio y almacenarse junto al archivo cifrado.

---

## Metasploit Framework

Framework de código abierto para pruebas de penetración desarrollado por H.D. Moore en 2003 y mantenido actualmente por Rapid7. Organiza miles de exploits en módulos reutilizables. Es la herramienta estándar de la industria para pentesters. En este laboratorio se usó para explotar EternalBlue y obtener acceso al sistema víctima.

Sitio oficial: https://www.metasploit.com

---

## Meterpreter

Payload avanzado de Metasploit que corre completamente en memoria del sistema víctima sin escribir archivos en disco. Ofrece una shell interactiva con capacidades de subir/bajar archivos, ejecutar programas, capturar pantalla y migrar entre procesos. Al correr en memoria es más difícil de detectar por antivirus basados en firmas de archivos.

---

## Ransomware

Tipo de malware que cifra los archivos de la víctima y exige un pago (generalmente en criptomonedas) para entregar la clave de descifrado. Es el tipo de malware más lucrativo actualmente — en 2023 se registraron más de 1.000 millones de dólares en pagos de rescate documentados. Los sectores más afectados son salud, educación, infraestructura crítica y gobiernos.

---

## Persistencia en Registro de Windows

Técnica que permite que un programa se ejecute automáticamente cada vez que el usuario inicia sesión. Windows consulta ciertas claves del registro al arrancar para determinar qué programas ejecutar. La clave más común usada por malware es:

```
HKCU\Software\Microsoft\Windows\CurrentVersion\Run
```

No requiere privilegios de administrador. Es uno de los primeros lugares que revisa un analista forense al investigar un sistema comprometido.

---

## Kill Switch

Mecanismo que permite detener la ejecución de un malware bajo ciertas condiciones. En este laboratorio el kill switch destruye la clave de cifrado en memoria cuando el timer llega a cero, haciendo el descifrado imposible. WannaCry tenía un kill switch involuntario basado en la respuesta de un dominio de internet — detallado en [08-references-and-context.md](08-references-and-context.md).

---

## PyInstaller

Herramienta que empaqueta scripts Python y todas sus dependencias en un ejecutable que puede correr en sistemas sin Python instalado. Fue fundamental en este laboratorio para desplegar el ransomware en la VM víctima.

Sitio oficial: https://pyinstaller.org

---

## Session 0 (Windows)

Desde Windows Vista en adelante, los servicios del sistema corren en una sesión separada llamada Session 0, que no tiene acceso al escritorio del usuario. Cuando EternalBlue compromete un sistema, el acceso inicial ocurre en Session 0. Cualquier ventana gráfica lanzada desde Session 0 es invisible para el usuario. La solución es migrar al proceso `explorer.exe`, que corre en Session 1 donde sí existe escritorio visible.

---

## --onefile vs --onedir (PyInstaller)

PyInstaller ofrece dos modos de empaquetado:

| Modo | Comportamiento |
|---|---|
| `--onefile` | Genera un único ejecutable que descomprime en `AppData\Local\Temp` al ejecutarse |
| `--onedir` | Genera una carpeta con el ejecutable y todas sus dependencias en ruta fija | 

---

## Clave Hardcodeada

Una clave hardcodeada está escrita directamente en el código fuente. Cualquier persona con acceso al ejecutable puede extraerla con herramientas básicas como `strings`. En este laboratorio la contraseña está hardcodeada intencionalmente como limitación didáctica.

En ransomware real la clave nunca existe en el cliente — se genera localmente, se cifra con la clave pública RSA del atacante y solo el servidor C2 puede descifrarla.

---

## AppData\Local\Temp y PyInstaller

Cuando PyInstaller empaqueta con `--onefile`, al ejecutarse descomprime dependencias en `AppData\Local\Temp`. Si el TARGET del ransomware incluye esa ruta, el ransomware cifrará sus propias dependencias de tkinter causando el error `Can't find init.tcl`. Solución: agregar `AppData\Local\Temp` a la lista de exclusiones del cifrado, o compilar con `--onedir`.


> [!CONTINUAR]
> [Siguiente artículo: Defensa contra este tipo de ataques ➔](07-defense.md)
