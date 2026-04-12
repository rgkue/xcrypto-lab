# 🔐 Crypto Ransomware — Laboratorio de Ciberseguridad

> **Repositorio educativo.** Este laboratorio fue desarrollado en un entorno completamente aislado, sin acceso a internet ni a redes reales, con el único propósito de comprender cómo funciona el ransomware y concientizar sobre la importancia de mantener los sistemas actualizados.

---

## ⚠️ Disclaimer

Este repositorio documenta un ejercicio académico realizado en una red virtual aislada (VMnet Host-Only) sin conexión a internet ni a ninguna red real. El código fuente del malware **no está incluido ni será publicado**. Toda la información aquí presentada tiene fines exclusivamente educativos. Reproducir estas técnicas fuera de un entorno controlado y sin autorización explícita es ilegal.

---

## 🎯 Objetivo

Demostrar de forma práctica cómo un ransomware se despliega a través de una vulnerabilidad real (EternalBlue / MS17-010) en un entorno de laboratorio, para generar conciencia sobre:

- La importancia de mantener los sistemas operativos actualizados
- El riesgo real de protocolos obsoletos como SMBv1
- El impacto que tiene el ransomware en usuarios, empresas e infraestructura crítica

---

## 👤 Autor

| Plataforma | Enlace |
|---|---|
| 🌐 Sitio web | [rgkue.github.io](https://rgkue.github.io) |
| 💼 LinkedIn | [linkedin.com/in/isaacmunozp](https://linkedin.com/in/isaacmunozp) |
| 📸 Instagram | [instagram.com/rgkue](https://instagram.com/rgkue) |
| 🟢 Hack The Box | [app.hackthebox.com/profile/rgkue](https://profile.hackthebox.com/profile/019d3818-db30-7363-b8b6-ed014b57bec5) |
| 🔵 TryHackMe | [tryhackme.com/p/rgkue](https://tryhackme.com/p/rgkue) |

---

## 📁 Estructura del Repositorio

```
crypto-ransomware-python/
│
├── README.md                       ← Este archivo
├── LICENSE
│
├── docs/
│   ├── 01-setup.md                 ← Entorno VMware y configuración de red
│   ├── 02-development.md           ← Desarrollo del ransomware en Python
│   ├── 03-attack-walkthrough.md    ← Paso a paso del ataque completo
│   ├── 04-versions.md              ← Versiones de Python y librerías
│   ├── 05-issues.md                ← Errores encontrados y soluciones
│   ├── 06-concepts.md              ← Conceptos técnicos clave
│   ├── 07-defense.md               ← Mitigaciones y detección
│   └── 08-references-and-context.md ← WannaCry, Snowden, recursos
│
└── img/
    ├── image1.png   ← Kali Linux
    ├── image2.png   ← Windows 7
    ├── image3.png   ← Configuración VMnet aislada
    ├── image4.png   ← IP y Hostname de Windows 7
    ├── image5.png   ← Escaneo Nmap
    ├── image6.png   ← Iniciar Metasploit
    ├── image7.png   ← Buscar exploits EternalBlue
    ├── image8.png   ← Opciones del exploit
    ├── image9.png   ← Ajustar payload
    ├── image10.png  ← Ejecutar exploit y cargar ransomware
    ├── image11.png  ← Visualizar procesos en la víctima
    ├── image12.png  ← Migrar a explorer.exe
    ├── image13.png  ← Archivo .txt antes del cifrado
    ├── image14.png  ← Pantalla del ransomware en víctima
    ├── image15.png  ← Archivo .txt cifrado
    ├── image16.png  ← Archivo bootmgr cifrado
    └── image17.png  ← Escritorio de víctima inutilizable
```

---

## 📄 Documentación

| Documento | Contenido |
|---|---|
| [01 — Setup](docs/01-setup.md) | Configuración de VMware, redes y máquinas virtuales |
| [02 — Development](docs/02-development.md) | Lógica del ransomware, cifrado Fernet, persistencia |
| [03 — Attack Walkthrough](docs/03-attack-walkthrough.md) | Paso a paso completo del ataque |
| [04 — Versions](docs/04-versions.md) | Versiones de Python, librerías y PyInstaller |
| [05 — Issues](docs/05-issues.md) | Problemas encontrados y cómo se resolvieron |
| [06 — Concepts](docs/06-concepts.md) | Glosario técnico del laboratorio |
| [07 — Defense](docs/07-defense.md) | Cómo protegerse: usuarios y administradores |
| [08 — References](docs/08-references-and-context.md) | WannaCry, Snowden, recursos recomendados |

---

*Laboratorio realizado con fines educativos en entorno controlado y aislado.*
