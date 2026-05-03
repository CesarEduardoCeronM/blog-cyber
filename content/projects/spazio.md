---
title: "Spazio: Gestión Inmobiliaria Cloud-Native"
date: 2026-05-03
draft: false
summary: "Sistema integral de gestión de inmuebles desarrollado en Go con enfoque en transaccionalidad blindada y metodología IWEB."
tags: ["Go", "Gin", "PostgreSQL", "SvelteKit", "Cloud-Native", "Seguridad"]
---

**Spazio** es una plataforma web para la empresa "Inmuebles a tu alcance" que gestiona el ciclo de vida completo de propiedades. El proyecto destaca por su arquitectura orientada a la nube y el uso de **Vertical Slices** para garantizar un backend robusto y escalable.

## Mi Rol: Desarrollo de Módulos Críticos

En este proyecto, soy responsable del diseño y construcción de dos de los pilares más complejos del sistema:

### 1. Gestión de Visitas y Citas (CU-13, CU-14, CU-15)
Implementé un motor de agendamiento blindado bajo los siguientes estándares:
*   **Transaccionalidad Total:** Uso de `pgx.Tx` para asegurar que las operaciones de agenda sean atómicas.
*   **Gestión de Estados:** Implementación de una máquina de estados para la confirmación y cancelación de visitas.
*   **Sincronización UTC:** Manejo estricto de horarios para evitar conflictos de agenda.

### 2. Módulo de Pagos y Simulación de Pasarela (CU-16, CU-17)
Desarrollé la lógica financiera del sistema, integrando:
*   **Idempotencia Financiera:** Bloqueo de cobros duplicados y gestión de conflictos entre métodos de pago.
*   **Sandboxing:** Motor de simulación para pasarelas de pago (Tarjetas, OXXO, PayPal) que emula flujos de éxito, error y expiración.
*   **Concurrencia Segura:** Uso de bloqueos de fila (`FOR UPDATE`) en la base de datos para prevenir inconsistencias en transacciones críticas.

---

## Arquitectura y Stack Tecnológico

El proyecto sigue la metodología **IWEB (Ingeniería Web)** y se despliega en una infraestructura de 3 capas:

*   **Backend:** Desarrollado en **Go (Gin Gonic)** bajo una arquitectura de **Vertical Slices**, alojado en **Railway**.
*   **Frontend:** Construido con **SvelteKit** y alojado en **Vercel**.
*   **Base de Datos:** PostgreSQL en **Neon** (Serverless), gestionando migraciones con `golang-migrate`.
*   **Seguridad:** Autenticación delegada en **Supabase Auth** mediante JWT.

---

## Blindaje Técnico (Estándar Senior)
Desde el punto de vista de la ingeniería de software y la seguridad, Spazio aplica:
1.  **Seguridad Multi-capa:** Validación de propiedad de recursos (`X-User-ID`) y blindaje contra estados de contrato inválidos.
2.  **Integridad de Datos:** Uso de tipos numéricos exactos para finanzas y auditoría vía JSONB.
3.  **Documentación Técnica:** Sincronización automática con Swagger UI para la API.

---
*Este proyecto se desarrolla bajo la metodología IWEB, cumpliendo con estándares de accesibilidad WCAG y pruebas unitarias al 100%.*
