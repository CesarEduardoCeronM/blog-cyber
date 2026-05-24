---
title: "Wireshark y Host Headers: La anatomía de un compromiso"
date: 2026-05-17T13:00:00-06:00
draft: false
description: "Análisis técnico de cómo el tráfico en texto plano y la validación incorrecta de cabeceras pueden comprometer un servidor."
tags: ["Wireshark", "Network Security", "Host Header Injection", "Pentesting", "TryHackMe"]
categories: ["Laboratorios", "Ciberseguridad"]
showTableOfContents: true
---

## Introducción: De la teoría a la trinchera

Esta semana enfoqué mis estudios en la **infraestructura de red**. Tras dominar la teoría detrás del Handshake de TCP y la jerarquía de DNS, dediqué mi práctica dominical a ver cómo estos conceptos se traducen en ataques reales. La conclusión es simple: **la seguridad es tan fuerte como su eslabón más débil**, ya sea un protocolo antiguo o una configuración de servidor web descuidada.

---

## Caso 1: Engañando al Guardián (Host Header Bypass)

Comencé analizando cómo, incluso con el tráfico cifrado, la lógica interna de una aplicación puede ser su perdición. En el laboratorio de **PortSwigger**, exploré la manipulación de la cabecera `Host`.

### La Explotación:
1.  El acceso a la ruta `/admin` estaba estrictamente reservado para "usuarios internos".
2.  Utilizando **Burp Suite**, intercepté mi petición HTTP y modifiqué la cabecera original por `Host: localhost`.
3.  El servidor, confiando ciegamente en esta cabecera para validar la procedencia de la petición, me otorgó acceso total al panel administrativo.

**Amenaza identificada (STRIDE):** Spoofing y Elevación de Privilegios. Quedó claro que validar metadatos controlados por el cliente es una práctica de alto riesgo.

---

## Caso 2: El espía en la red (Laboratorio: h4cked)

Para la segunda parte de la sesión, utilicé **Wireshark** para analizar una captura de red real. El objetivo: reconstruir un compromiso iniciado por un ataque de fuerza bruta.

### Mis hallazgos clave:
*   **Protocolos Inseguros:** Observé de primera mano el peligro de FTP. Al transmitir todo en texto plano, me bastó con filtrar por `ftp` y localizar el código de respuesta `230 Login successful` para interceptar las credenciales exactas del atacante: `jenny:password123`.
*   **Análisis de Flujo (TCP Stream):** Siguiendo el flujo TCP de la reverse shell que el atacante plantó, pude leer cada comando que ejecutó. Vi su progresión desde el `whoami` inicial hasta la instalación de **Reptile**, un rootkit diseñado para mantener persistencia indetectable.

---

## Combatiendo fuego con fuego: Hackeando de vuelta

La mejor forma de entender un ataque es replicarlo. En la fase final del laboratorio, el reto era recuperar el control del servidor comprometido utilizando las mismas estrategias que el atacante.

Decidí "combatir fuego con fuego". Sabiendo que la contraseña había sido cambiada, lancé **Hydra** contra el servicio FTP y logré descifrar la nueva clave (`987654321`). Con el acceso restaurado, repliqué el vector de ataque inicial: cargué mi propia Reverse Shell en PHP directamente en el directorio público del servidor. 

Al forzar la ejecución de la shell desde mi navegador, obtuve acceso remoto. A partir de ahí, estabilicé la terminal y utilicé los privilegios previamente comprometidos para escalar a root, obteniendo la ansiada flag final: `ebcefd66ca4b559d17b440b6e67fd0fd`.

---

## Conclusión

Esta sesión me demostró que la ciberseguridad no se trata solo de conocer herramientas, sino de visualizar el camino que recorren los datos. Ver una contraseña viajar "desnuda" por el aire en Wireshark y luego usar un bypass de lógica pura para entrar a un panel administrativo refuerza mi convicción: el **Modelado de Amenazas (STRIDE)** no es opcional, debe ser el punto de partida de cualquier desarrollo robusto.
