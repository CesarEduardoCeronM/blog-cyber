---
title: "De la Petición al Exploit: Evaluando APIs con Burp Suite"
date: 2026-05-24
tags: ["API Security", "BOLA", "IDOR", "Pentesting", "Burp Suite"]
author: "Cesar"
showToc: true
---

En la auditoría de aplicaciones modernas, la superficie de ataque a menudo no está en lo que el usuario ve, sino en las tuberías que mueven los datos: las APIs. Hoy, durante el "Bloque Profundo" de mi preparación para el posgrado, resolví un escenario que demuestra cómo la falta de higiene en la exposición de documentación puede llevar a un compromiso total de recursos.

## El Hallazgo: Enlistar es demasiado fácil

El reconocimiento comenzó con una tarea rutinaria: **Content Discovery**. Utilizando las herramientas de *engagement* de Burp Suite Pro, el objetivo era mapear archivos y directorios no vinculados. 

Fue alarmantemente fácil. En pocos segundos, la herramienta identificó el directorio `/api`. Al acceder, no encontré un error 403 o 404, sino una tabla HTML que servía como documentación técnica improvisada. Esta "mina de oro" exponía tres métodos críticos: `GET`, `PATCH` y, el más peligroso, `DELETE`.

## El Instinto del Atacante

La documentación indicaba la siguiente estructura para el borrado de cuentas:
`DELETE /user/[username]`

Mi primer instinto, tras autenticarme con una cuenta de prueba (`wiener`), fue probar la integridad de la lógica de autorización. ¿Qué pasa si intento borrar a un usuario que no soy yo? 

### La Anatomía del Exploit (BOLA)

Utilizando **Burp Repeater**, capturé una petición legítima y realicé una modificación quirúrgica. El objetivo: el usuario `carlos`.

1. **Método:** Cambiado de `GET` a `DELETE`.
2. **Endpoint:** `/api/user/carlos`.
3. **Identidad:** Mi cookie de sesión de `wiener` seguía activa.

Al enviar la petición, la API respondió con un `200 OK`. Sin preguntas, sin verificaciones adicionales. El usuario `carlos` había sido eliminado del sistema.

## ¿Qué falló aquí?

Este es un caso de texto de **BOLA (Broken Object Level Authorization)**, anteriormente conocido como IDOR. El fallo no está en la falta de autenticación (yo era un usuario válido), sino en la **falta de autorización**.

### El Error del Desarrollador
La aplicación confió ciegamente en el identificador proporcionado en la URL (`carlos`). El backend procesó la solicitud asumiendo que, si el usuario estaba logueado, tenía permiso para ejecutar cualquier acción sobre cualquier recurso definido por el parámetro.

### La Solución Correcta
Las APIs nunca deben confiar en los IDs de objeto enviados por el cliente para determinar la propiedad del recurso. La validación debe ocurrir en el servidor:
1. Extraer la identidad del usuario directamente del token de sesión/cookie.
2. Comparar esa identidad con el dueño del recurso que se intenta manipular.
3. Si no coinciden (y no es un administrador), retornar `403 Forbidden`.

## Conclusión

La seguridad por oscuridad (esconder la documentación en `/api`) no es seguridad. Una evaluación de APIs efectiva requiere identificar estos endpoints expuestos y probar los límites de los permisos. Hoy, un simple cambio de nombre en una URL fue suficiente para comprometer la integridad de la base de usuarios.

---
*Este post es parte de mi registro de prácticas técnicas y resolución de laboratorios.*
