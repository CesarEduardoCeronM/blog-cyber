---
title: "Aureum: Simulador Financiero y Seguridad Perimetral"
date: 2026-05-03
draft: false
summary: "Plataforma educativa de inversiones basada en microservicios con un gateway de seguridad centralizado."
tags: ["Desarrollo", "Seguridad", "Microservicios", "Supabase", "JWT"]
---

**Aureum** es una plataforma educativa diseñada para enseñar inversiones mediante simulaciones prácticas en equipo. Como proyecto de arquitectura multicapa, su complejidad reside en la orquestación de múltiples microservicios y la gestión unificada de la identidad y la seguridad.

## Mi Rol: Arquitecto de Seguridad y Gestión de Identidad

En este proyecto, fui responsable de diseñar y desarrollar el **núcleo de gestión de cuentas** y la **infraestructura de seguridad perimetral**.

### 1. Gateway de Seguridad Centralizado
Desarrollé el Gateway encargado de la seguridad perimetral, actuando como el único punto de entrada para todas las peticiones de los frontends (Web y Móvil).
*   **Seguridad Perimetral:** Filtrado y validación de peticiones antes de llegar a los microservicios internos.
*   **Orquestación:** El gateway actúa como balanceador y distribuidor de tareas entre los servicios del backend.
*   **Autenticación JWT:** Implementación de tokens para sesiones seguras y persistentes.

### 2. Gestión de Identidad (IAM) con Supabase
Implementé el ciclo completo de vida de las cuentas de usuario:
*   **Operaciones CRUD:** Registro, autenticación, modificación y eliminación de perfiles.
*   **RBAC (Role-Based Access Control):** Gestión avanzada de permisos y roles para asegurar que cada usuario acceda solo a los recursos permitidos.
*   **Integración con Supabase:** Aprovechamiento de la infraestructura de Supabase para la persistencia segura de credenciales y perfiles.

---

## Arquitectura Técnica

El sistema se diseñó bajo un patrón de **Arquitectura Hexagonal + Modular**, permitiendo que el backend sirviera de manera agnóstica a dos plataformas diferentes:

### Stack Tecnológico
*   **Backend:** Java / Node.js (Microservicios).
*   **Base de Datos y Auth:** Supabase + JWT.
*   **Frontend Web/Desktop:** React, TypeScript, Tailwind CSS.
*   **Frontend Móvil:** React Native, Expo, NativeWind.

### Organización del Proyecto
El código se organiza siguiendo la separación de intereses (SoC):
*   **Capa de Dominio:** Lógica pura de negocio (entidades y casos de uso).
*   **Capa de Infraestructura:** Adaptadores para APIs externas y Supabase.
*   **Capa de Aplicación:** Configuración global y navegación.

---

## Enfoque en Ciberseguridad
Desde la perspectiva de la seguridad, Aureum sirvió como laboratorio para aplicar conceptos de **Defensa en Profundidad**:
1.  **Validación en el Gateway:** Ninguna petición llega al microservicio sin ser validada.
2.  **Tokens Efímeros:** Uso de JWT para reducir la superficie de exposición en caso de intercepción.
3.  **Aislamiento de Entornos:** Cada equipo funciona como un mercado aislado, emulando la segmentación de redes.

---
*Nota: Este proyecto es de código privado y propiedad intelectual protegida.*
