---
title: "De la Teoría a la Práctica: Mi primer hallazgo crítico con Burp Suite Pro"
date: 2026-05-10T10:00:00-06:00
draft: false
tags: ["ciberseguridad", "burp-suite", "api", "stride", "idor"]
summary: "Crónica de una sesión de inmersión técnica: desde el estudio del modelo OSI y STRIDE hasta la explotación de una vulnerabilidad BOLA que expuso datos personales sensibles."
---

![Cybersecurity Lab](https://images.unsplash.com/photo-1563986768609-322da13575f3?auto=format&fit=crop&q=80&w=1000)

Este domingo fue el día en que la teoría académica chocó de frente con la realidad técnica. Lo que comenzó como un repaso de modelos conceptuales terminó en el descubrimiento de lo vulnerable que puede ser nuestra información personal si no se protege correctamente.

### Preparando el Entorno de Combate
Para este salto técnico, no bastaba con herramientas simples. Configuré un entorno profesional en **Kali Linux** dentro de una VirtualBox, estableciendo un túnel VPN mediante **OpenVPN** para conectarme a los laboratorios de *TryHackMe*. La joya de la corona: **Burp Suite Professional**, una herramienta que, una vez que aprendes a usarla, te cambia la forma de ver la web.

### La Base: OSI y STRIDE
Antes de tocar el teclado para atacar, dediqué tiempo a los cimientos:
*   **Modelo OSI:** Entender que la seguridad no solo ocurre en la aplicación, sino que cada capa (desde la Física hasta la de Aplicación) tiene sus propios vectores.
*   **Modelo STRIDE:** Esta metodología fue mi mapa. Me ayudó a entender que la **Exposición de Información** y la **Escalada de Privilegios** son amenazas constantes en cualquier sistema.

### El Hallazgo: Explotando IDOR (BOLA)
Usando una máquina de laboratorio, me enfocqué en auditar una API. Tras interceptar las peticiones crudas (**Raw Requests**), identifiqué un patrón en los endpoints: `GET /api/parents/view_accountinfo?user_id=10`.

Lo que vino después fue tan sencillo como alarmante:
1.  **Manual:** Cambié el ID de `10` a `5` en el **Repeater** de Burp. El servidor, confiando ciegamente en el parámetro de la URL, me entregó la información de un usuario que no era yo.
2.  **Automatizado:** Usé el **Intruder** de Burp Pro para barrer 50 IDs en segundos.

### Los Resultados y una Reflexión Necesaria
El resultado fue perturbador. Logré obtener **PII (Personally Identifiable Information)** de múltiples usuarios: correos electrónicos, direcciones físicas completas en España y, lo más grave, **nombres y fechas de nacimiento de menores de edad** vinculados a las cuentas.

**Lo que más me sorprendió fue la facilidad del ataque.** Cambiar un simple número en una petición HTTP no requiere un genio de la computación, pero las consecuencias de que un actor malintencionado lo haga son devastadoras. Esto refuerza mi compromiso: la ciberseguridad no es un lujo, es una necesidad ética en el desarrollo de software.

Hoy aprendí que entre un sistema "seguro" y un desastre de privacidad, a veces solo hay un número de diferencia.
