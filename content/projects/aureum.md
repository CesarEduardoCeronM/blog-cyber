---
title: "Aureum: Simulador Financiero y Seguridad Perimetral"
date: 2026-05-03
draft: false
summary: "Plataforma educativa de inversiones basada en microservicios con un gateway centralizado en C# y despliegue en Railway."
tags: ["Desarrollo", "C#", "Python", "Microservicios", "Railway", "Supabase"]
---

**Aureum** es una plataforma educativa de alto rendimiento diseñada para la simulación de mercados financieros. Su arquitectura se centra en el aislamiento de entornos y la seguridad centralizada de los flujos de datos.

## Arquitectura de Backend y Despliegue

El sistema fue diseñado bajo una filosofía de **políglota en microservicios**, permitiendo que cada componente utilizara la herramienta más eficiente para su tarea:

### 1. Gateway Perimetral (C# / .NET)
Desarrollé el Gateway central usando **C#**, el cual actúa como el "escudo" y orquestador del sistema:
*   **Seguridad Perimetral:** Validación rigurosa de cada petición entrante mediante JWT.
*   **Routing Inteligente:** Distribución de carga y tareas hacia los microservicios internos.
*   **Consolidación de Identidad:** Integración directa con Supabase para la validación de sesiones.

### 2. Microservicio de Usuarios (Python)
Implementé el microservicio encargado de la gestión de identidad y perfiles utilizando **Python**:
*   **Gestión de Cuentas:** Ciclo de vida completo (CRUD) de usuarios.
*   **Lógica de Negocio:** Manejo de permisos y roles (RBAC) integrados con la base de datos.
*   **Eficiencia:** Procesamiento rápido de metadatos de usuario y sincronización con Supabase.

### 3. Infraestructura y Despliegue
*   **Railway:** El backend completo fue desplegado en **Railway**, aprovechando su capacidad de escalado y gestión de variables de entorno seguras.
*   **Supabase + JWT:** Uso de Supabase como proveedor de autenticación y base de datos relacional, garantizando la persistencia segura de la información financiera simulada.

---

## Interfaz Multiplataforma
El backend de Aureum fue diseñado para ser agnóstico al frontend, alimentando simultáneamente dos interfaces distintas desarrolladas bajo **Arquitectura Hexagonal**:

1.  **Aureum Desktop/Web:** Desarrollado con React, TypeScript y Tailwind CSS.
2.  **Aureum Mobile:** Desarrollado con React Native, Expo y NativeWind.

---

## Enfoque en Ciberseguridad
Este proyecto permitió la implementación de patrones de seguridad críticos:
*   **Gateway como Punto Único de Falla Controlado:** Centralización de logs y auditoría en un solo punto de entrada.
*   **Tokens JWT Efímeros:** Minimización de riesgos de secuestro de sesión.
*   **Aislamiento de Mercados:** Segmentación lógica de los datos de cada equipo para evitar fugas de información transaccional.

---
*Nota: Este proyecto es de código privado y propiedad intelectual protegida.*
