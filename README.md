<!-- ============================================= -->
<!--                Portada del Informe             -->
<!-- ============================================= -->

  ![Logo](docs/logo-uai2.png)

---

# **Informe 2**

---

**Asignatura:** ARQUITECTURA DE SISTEMAS Sec. 2  
**Docente:** Eliana Jackeline Vivas Rafael  

---

## Integrantes
- Nevile Olguin  
- Mateo Moreira  
- Javiera Soto  
- Ian Schmidt  

---

**Fecha de entrega:** Jueves 24 de junio de 2025  

<!-- ============================================= -->

---

## Índice

1. Evaluación inicial (PoC)  
2. Propuesta de arquitectura to-be  
   - Arquitectura de Procesos  
   - Arquitectura de Aplicaciones y Datos  
   - Arquitectura de Infraestructura  
3. Justificación de decisiones arquitectónicas  
4. Mejoras a nivel de código y patrones  
5. Discusión y conclusiones  
6. Bibliografía  
7. Anexos

---

## 1. Evaluación inicial: pruebas de estrés del sistema actual (As-Is)

Primero realizamos una evaluación del sistema actual utilizado por Nysa Arts para gestionar sus reservas, el cual está compuesto por una planilla de Excel alojada en OneDrive y la comunicación informal mediante canales como WhatsApp, Instagram y llamadas telefónicas. Esta arquitectura As-Is, al ser completamente manual, nos llevó a simular un escenario de estrés para observar cómo responde ante una demanda elevada.

En particular, modelamos un caso en que se reciben simultáneamente 20 solicitudes de reserva durante un horario de alta demanda (por ejemplo, viernes entre 18:00 y 20:00 horas). Durante esta prueba, identificamos varios problemas críticos:

Colisiones al momento de escribir en la planilla, cuando dos administradores intentan editar al mismo tiempo.

Riesgo de sobrescritura de datos y pérdida de reservas ya ingresadas.

Tiempos de respuesta prolongados hacia el cliente (entre 12 y 24 horas en promedio).

Ausencia total de alertas automáticas o registro de auditoría.

A partir de estos resultados, concluimos que el sistema actual no está preparado para soportar múltiples usuarios concurrentes ni para garantizar una operación confiable durante momentos clave. La dependencia del trabajo manual genera cuellos de botella, errores y una experiencia deficiente tanto para el cliente como para el equipo administrativo.

## 1.2. Propuesta de arquitectura To-Be: diseño de la nueva solución

Frente a estas limitaciones, proponemos una arquitectura moderna, automatizada y escalable que permita a Nysa Arts operar de forma más eficiente y entregar una mejor experiencia tanto a clientes como al equipo administrativo.

### Frontend (Capa de Presentación)

Desarrollaremos una aplicación web responsiva en **React**, que los usuarios podrán usar desde cualquier dispositivo (móvil o escritorio). Esta interfaz permitirá visualizar en tiempo real la disponibilidad de salas, completar reservas, editarlas o cancelarlas, y recibir confirmaciones automáticas. Será intuitiva y accesible, pensada para usuarios no técnicos.

### Backend (Lógica de Aplicación)

La lógica de negocio estará implementada en **Node.js**, donde se procesarán las solicitudes, se validará la disponibilidad y se enviarán notificaciones automáticas. Este backend actuará como intermediario entre el frontend, la base de datos y otros servicios, asegurando consistencia en las operaciones y mayor velocidad de respuesta.

### Servicios (API RESTful)

Toda la comunicación entre el frontend, el backend y la base de datos se realizará a través de una **API REST**, lo que permite mantener una arquitectura desacoplada, más fácil de mantener y escalar. Esto también permitirá integrar futuros servicios como pasarelas de pago, Google Calendar o redes sociales.

### Base de Datos (Capa de Datos)

Utilizaremos una base de datos **PostgreSQL**, donde se almacenará toda la información estructurada de usuarios, salas, horarios, reservas e inventario de equipos. Esta base permitirá hacer consultas rápidas, generar reportes históricos y alimentar futuros módulos de análisis predictivo.

### Infraestructura en la nube

La solución será desplegada en servicios en la nube como **Heroku o Firebase**, lo que garantiza alta disponibilidad, escalabilidad automática, respaldo continuo y facilidad de mantenimiento. Además, incorporaremos un **balanceador de carga** que distribuirá el tráfico entre múltiples instancias del backend, mejorando la estabilidad en horarios de alta demanda.

### Seguridad

La seguridad será un pilar clave. Usaremos **autenticación basada en JWT (JSON Web Tokens)**, cifrado con **HTTPS/TLS 1.2 o superior**, control de roles y validaciones en cada punto crítico del sistema. Esto nos permitirá proteger tanto la información de los usuarios como las operaciones realizadas.

### Monitoreo y Logs

Integraremos un sistema de monitoreo y visualización de logs basado en **ELK Stack** (Elasticsearch, Logstash y Kibana), que permitirá hacer seguimiento en tiempo real del funcionamiento del sistema, detectar errores y emitir alertas automáticas ante fallas o comportamientos anómalos.

### Respaldo y recuperación ante fallos

Se implementarán **backups automáticos** y un sistema de recuperación rápida en caso de fallos. Por ejemplo, ante la caída de un contenedor, la plataforma podrá restablecer el servicio en menos de 2 minutos, lo que asegura una disponibilidad ≥ 99%, incluso en escenarios críticos.

### Módulo predictivo (futuro)

En una siguiente etapa, consideramos integrar un módulo de análisis predictivo basado en modelos como **SARIMA**, que nos permitirá anticipar la ocupación de las salas y mejorar la planificación operativa y comercial.


## 2. Propuesta de arquitectura to-be  

   - Arquitectura de Procesos
     
   - Arquitectura de Aplicaciones y Datos
     
   - Arquitectura de Infraestructura
     
## 3. Justificación de decisiones arquitectónicas  

### a. Automatización del proceso de reservas

- **Brecha:** Proceso manual, errores frecuentes, duplicidad de datos  
- **Solución:** Web App + API RESTful  
- **Mejora:** Autonomía, disponibilidad 24/7, menos errores humanos

### b. Consistencia y control de datos

- **Brecha:** Datos no estructurados, sin auditoría  
- **Solución:** PostgreSQL + módulo de auditoría  
- **Mejora:** Informes confiables, trazabilidad

### c. Seguridad y control de acceso

- **Brecha:** Sin autenticación  
- **Solución:** JWT + HTTPS  
- **Mejora:** Confidencialidad y control de acceso por roles

### d. Escalabilidad y mantenimiento

- **Brecha:** Sistema monolítico, difícil de escalar  
- **Solución:** Microservicios en Docker  
- **Mejora:** Escalabilidad horizontal y modularidad

### e. Gestión y monitoreo

- **Brecha:** Sin logs ni monitoreo  
- **Solución:** ELK Stack  
- **Mejora:** Observabilidad y respuesta ante fallas

### f. Resiliencia y continuidad operativa

- **Brecha:** Sin respaldos ni recuperación  
- **Solución:** Backups automáticos en AWS  
- **Mejora:** Tolerancia a fallos y continuidad del servicio

---

## 4. Mejoras a nivel de código y patrones  

## 5. Discusión y conclusiones  

## 6. Bibliografía  

## 7. Anexos


