# 05 — Problemas Encontrados y Soluciones

Documentación de los errores encontrados durante el desarrollo y despliegue, y cómo se resolvieron.

---

## Problema 1 — Ejecutable de Win11 no corre en Win7

**Error:** `Failed to load Python DLL python314.dll`

**Causa:** Windows 7 no tiene las APIs internas que Python 3.14 requiere. El ejecutable compilado en Windows 11 asume una versión del sistema operativo que Win7 no satisface.

**Intentos fallidos:**
- Python 3.8 → requiere parche KB2533623 (no instalable en Win7 EOL)
- Python 3.7 → requiere parche KB3063858 (no instalable en Win7 EOL)

**Solución:** Bajar a **Python 3.6.8**, que solo requiere SP1. El código fue recompilado directamente en la VM Windows 7.

---

## Problema 2 — cryptography no instala en Win7

**Error:** Fallo durante `pip install cryptography` por requerir compilador Rust.

**Causa:** Las versiones recientes de la librería `cryptography` incluyen extensiones en Rust que deben compilarse durante la instalación. Windows 7 no tiene una cadena de herramientas Rust compatible.

**Solución:** Instalar **cryptography 2.9.2**, la última versión que no requiere Rust y es compatible con Python 3.6.

```bash
pip install cryptography==2.9.2
```

---

## Problema 3 — Errores de sintaxis en Python 3.6

**Error:** `TypeError` al ejecutar el ransomware en Win7.

**Causa:** El código fue escrito en Python 3.14 usando sintaxis introducida en Python 3.9+:
- `tuple[list, list]` — type hints con tipos built-in como genéricos (Python 3.9+)
- `bytes | None` — union types con `|` (Python 3.10+)

Python 3.6 no reconoce ninguna de estas formas.

**Solución:** Eliminar todos los type hints de las firmas de función. La lógica del código no cambia, solo se remueven las anotaciones de tipo.

---

## Problema 4 — tkinter no encuentra init.tcl con --onefile

**Error:** `Can't find init.tcl` al ejecutar el ransomware compilado con `--onefile`.

**Causa:** PyInstaller con `--onefile` descomprime las dependencias en una carpeta temporal dentro de `AppData\Local\Temp` al ejecutarse. Si el TARGET del ransomware incluye esa ruta, el ransomware cifra sus propias dependencias de Tcl/Tk antes de que tkinter pueda cargarlas.

**Solución:** Se excluyó la ruta `AppData\Local\Temp` para el usuario Public, junto con donde se había cargado el ransomware `Public\Documents`, evitando así que el ransonware cifrara archivos importantes para su propia ejecución.

```bash
pyinstaller --onefile --noconsole xcrypto.py
```

---

## Problema 5 — Pantalla del ransomware invisible

**Síntoma:** El ransomware se ejecutaba (los archivos se cifraban) pero la pantalla de tkinter nunca aparecía en la VM víctima.

**Causa:** EternalBlue entrega acceso inicial en **Session 0** (sesión del sistema en Windows Vista+), que no tiene acceso al escritorio del usuario. Cualquier ventana gráfica lanzada desde Session 0 es invisible para el usuario.

**Solución:** Migrar el proceso de Meterpreter a **`explorer.exe`** antes de ejecutar el ransomware. `explorer.exe` corre en Session 1 (sesión del usuario), donde sí existe escritorio visible.

```
migrate <PID de explorer.exe>
execute -f "C:\\Users\\Public\\Documents\\xcrypto.exe" -H
```

> [!CONTINUAR]
> [Siguiente artículo: Conceptos detrás del ransomware ➔](06-concepts.md)
