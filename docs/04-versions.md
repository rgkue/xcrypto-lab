# 04 — Versiones y Compatibilidad

Se requirieron dos entornos Python distintos por incompatibilidades de Windows 7 con versiones modernas. Los detalles de los problemas encontrados están en [05-issues.md](05-issues.md).

---

## Entorno de Desarrollo — Windows 11 (Host)

| Componente | Versión |
|---|---|
| Sistema Operativo | Windows 11 |
| Python | 3.14.2 |
| cryptography | 46.0.6 |
| PyInstaller | 6.19.0 |
| tkinter | Incluido con Python |
| Editor | Visual Studio Code |

---

## Entorno de Compilación y Despliegue — Windows 7 SP1 (VM Víctima)

| Componente | Versión |
|---|---|
| Sistema Operativo | Windows 7 Home Basic SP1 |
| Python | 3.6.8 |
| cryptography | 2.9.2 |
| PyInstaller | 4.10 |
| tkinter | Incluido con Python |
| Flag de compilación | `--onefile --noconsole` |

---

## Por qué Python 3.6.8 en Windows 7

Windows 7 EOL (fin de vida: enero 2020) no recibe actualizaciones de seguridad. Las versiones modernas de Python requieren APIs internas de Windows que no existen en Win7:

- **Python 3.14** — falla con `python314.dll not found`
- **Python 3.8 / 3.7** — requieren parches KB2533623 y KB3063858 que Windows Update ya no puede instalar en Win7 EOL
- **Python 3.6.8** — solo requiere SP1, que sí está instalado ✅

## Por qué cryptography 2.9.2

Las versiones recientes de la librería `cryptography` requieren un compilador **Rust** durante la instalación. Windows 7 no tiene cadena de herramientas Rust compatible. La versión 2.9.2 es la última que instala sin necesidad de Rust. ✅

---

> [!NOTE]
> [Siguiente artículo: Issues encontrados durante el laboratorio ➔](05-issues.md)
