# 08 — Referencias y Contexto

---

## WannaCry

El 12 de mayo de 2017 comenzó a propagarse el ataque de ransomware más devastador de la historia. En menos de 24 horas [WannaCry](https://www.kaspersky.es/resource-center/threats/ransomware-wannacry) había infectado más de 230.000 sistemas en 150 países. Lo que lo hizo diferente a cualquier ransomware anterior fue su mecanismo de propagación: usaba EternalBlue para moverse automáticamente entre equipos vulnerables sin ninguna intervención humana, convirtiéndolo en un **gusano**.

Las víctimas más impactantes incluyeron el Sistema Nacional de Salud del Reino Unido, donde hospitales de todo el país tuvieron que cancelar cirugías, citas médicas y derivaciones de emergencia porque el personal no podía acceder a los registros de pacientes. Se estiman 19.000 citas canceladas. También fueron afectadas Telefónica en España, Renault (detuvo producción en varias plantas), Deutsche Bahn, FedEx y ministerios en varios países. El daño económico total se estima entre 4.000 y 8.000 millones de dólares.

El rescate pedido era de entre 300 y 600 dólares en Bitcoin por equipo. Irónicamente los atacantes recaudaron muy poco porque el sistema de pago tenía errores de diseño que dificultaban completar el pago incluso para quienes querían pagarlo.

### El Kill Switch de los 10 dólares

[Marcus Hutchins](https://es.wikipedia.org/wiki/Marcus_Hutchins), investigador de seguridad británico de 22 años conocido como MalwareTech, estaba analizando el código de WannaCry el mismo día del ataque. Encontró que antes de cifrar cualquier archivo, el malware consultaba un dominio de internet largo y sin sentido. Si el dominio respondía, el malware se detenía. El dominio no estaba registrado, nunca respondía y el malware siempre continuaba. Hutchins lo registró por 10.69 dólares. Todos los procesos activos de WannaCry en el mundo dejaron de cifrar archivos en minutos. El ataque más costoso de la historia fue detenido por menos de once dólares.

WannaCry fue atribuido por los gobiernos de Estados Unidos, Reino Unido, Australia y Japón al grupo Lazarus, vinculado al gobierno de Corea del Norte.

**Recurso recomendado:**

¿Qué fue WannaCry? — TutosPC
<p align="center">
  <a href="https://youtu.be/aAfuNn2URng?si=q__5NVlJOh3KTreK">
    <img src="/img/wannacry-tutos-pc.jpeg" width="600" alt="¿Qué fue WannaCry?">
    <br>
    <b>Ver video en YouTube</b>
  </a>
</p>


Crédigtos: [@TutosPCyoutube](https://youtube.com/@tutospcyoutube?si=Ls_7GcgfPPw0GW3x)


---

## Edward Snowden y EternalBlue

Para entender cómo EternalBlue llegó a manos de Shadow Brokers, es importante conocer el contexto de las filtraciones de años anteriores.

En 2013, [Edward Snowden](https://es.wikipedia.org/wiki/Edward_Snowden), contratista de la [NSA](https://www.nsa.gov/), filtró millones de documentos clasificados que revelaron al mundo la existencia de programas masivos de vigilancia global. Entre los documentos había evidencia de que la NSA había desarrollado capacidades para comprometer sistemas en todo el mundo. Snowden entregó los documentos a periodistas y huyó a Rusia, donde vive actualmente bajo asilo político.

Las filtraciones de Snowden no incluyeron herramientas técnicas como EternalBlue, pero sí revelaron la existencia y el alcance del arsenal cibernético de la NSA. [The Shadow Brokers](https://es.wikipedia.org/wiki/The_Shadow_Brokers) apareció en 2016 — tres años después — afirmando haber comprometido directamente la infraestructura de la NSA y publicando EternalBlue entre otras herramientas.

Lo que sí es claro es que ambos casos — Snowden en 2013 y Shadow Brokers en 2016/2017 — apuntan al mismo problema: las agencias de inteligencia acumulan vulnerabilidades de seguridad en lugar de reportarlas a los fabricantes, y cuando esas vulnerabilidades se filtran, las consecuencias las pagan usuarios comunes, hospitales y empresas que nunca tuvieron nada que ver con el espionaje.

**Película recomendada:**

[Snowden (2016)](https://es.wikipedia.org/wiki/Snowden_(pel%C3%ADcula)) — dirigida por Oliver Stone
Protagonizada por Joseph Gordon-Levitt como Edward Snowden.
Disponible en plataformas de streaming. Recomendada como complemento al estudio de ética en ciberseguridad y vigilancia masiva.

---

## Demostración del Laboratorio

El siguiente video muestra la ejecución completa de este laboratorio en el entorno controlado, incluyendo la explotación con EternalBlue, y el despliegue del ransomware.

**Laboratorio Ransomware - Demo**

<p align="center">
  <a href="https://www.facebook.com/share/v/1Jb13fm3Gj/">
    <img src="/img/demo-video.png" width="600" alt="Laboratorio Ransomware - Demo">
  </a>
</p>

---

## Sobre el Autor

Isaac Muñoz — Estudiante y entusiasta de ciberseguridad.

| Plataforma | Enlace |
|---|---|
| 🌐 Sitio web | [rgkue.github.io](https://rgkue.github.io) |
| 💼 LinkedIn | [linkedin.com/in/isaacmunozp](https://linkedin.com/in/isaacmunozp) |
| 📸 Instagram | [instagram.com/rgkue](https://instagram.com/rgkue) |
| 🟢 Hack The Box | [app.hackthebox.com](https://profile.hackthebox.com/profile/019d3818-db30-7363-b8b6-ed014b57bec5) |
| 🔵 TryHackMe | [tryhackme.com/p/rgkue](https://tryhackme.com/p/rgkue) |

---

> [!NOTE]
> **¡Gracias por leer y pasarte por aquí. Espero te haya gustado y sobretodo, ¡hayas aprendido!**
>
> 🏠 [Volver al README](../README.md)
