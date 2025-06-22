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

**Fecha de entrega:** Jueves 5 de junio de 2025  

<!-- ============================================= -->

---

## 1. Evaluación inicial: pruebas de estrés del sistema actual (As-Is)

Primero realizamos una evaluación del sistema actual utilizado por Nysa Arts para gestionar sus reservas, el cual está compuesto por una planilla de Excel alojada en OneDrive y la comunicación informal mediante canales como WhatsApp, Instagram y llamadas telefónicas. Esta arquitectura As-Is, al ser completamente manual, nos llevó a simular un escenario de estrés para observar cómo responde ante una demanda elevada.

En particular, modelamos un caso en que se reciben simultáneamente 20 solicitudes de reserva durante un horario de alta demanda (por ejemplo, viernes entre 18:00 y 20:00 horas). Durante esta prueba, identificamos varios problemas críticos:

Colisiones al momento de escribir en la planilla, cuando dos administradores intentan editar al mismo tiempo.

Riesgo de sobrescritura de datos y pérdida de reservas ya ingresadas.

Tiempos de respuesta prolongados hacia el cliente (entre 12 y 24 horas en promedio).

Ausencia total de alertas automáticas o registro de auditoría.

A partir de estos resultados, concluimos que el sistema actual no está preparado para soportar múltiples usuarios concurrentes ni para garantizar una operación confiable durante momentos clave. La dependencia del trabajo manual genera cuellos de botella, errores y una experiencia deficiente tanto para el cliente como para el equipo administrativo.

## 2. Propuesta de arquitectura To-Be: diseño de la nueva solución

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

## 3. Análisis de Arquitectura Empresarial

### Stakeholder y Usuario Principal

- **Stakeholder principal:** Nysa Arts — empresa que arrienda salas de ensayo musical y producción en Santiago de Chile.
- **Usuario principal:** Clientes (músicos, productores, educadores) que arriendan espacios para fines creativos.

---

### Objetivos Estratégicos y Contribución del Sistema

| Objetivo Estratégico                                 | Contribución del Sistema                                                                 |
|------------------------------------------------------|-------------------------------------------------------------------------------------------|
| Mejorar la eficiencia operativa                      | Disminuye procesos manuales e introduce automatización en reservas y gestión de salas.    |
| Reducir errores y tiempos de espera                  | Sistema disponible 24/7 con confirmaciones automáticas y validación en tiempo real.       |
| Mejorar la experiencia y fidelización de los clientes| Plataforma intuitiva y autogestionable con interfaz clara y transparente.                 |
| Aumentar el porcentaje de ocupación                  | Información en tiempo real para ofrecer automáticamente salas disponibles.                |

---

### Proceso Clave del Negocio

- Desarrollo de un sistema que permita a los clientes arrendar salas de forma cómoda, autónoma y eficiente.
- Gestión del flujo completo: desde la disponibilidad hasta la confirmación.

---

### Componentes Clave

#### Procesos

- Reserva de salas y validación de disponibilidad.
- Gestión de usuarios y equipamiento.

#### Aplicaciones

- Aplicación desarrollada en Node.js y React, con manejo de datos en tiempo real.
- Comunicación a través de APIs RESTful.

#### Datos

- Base de datos estructurada con usuarios, salas, horarios y reservas.
- Datos actualizados en tiempo real, soportados por servicios cloud.

#### Infraestructura

- Hosting en la nube administrado por terceros (Heroku, Firebase, etc.).
- Escalable y con bajo costo de operación.

---

### Modelo de Arquitectura en 7 Capas

| Capa                    | Descripción                                                                                  |
|-------------------------|----------------------------------------------------------------------------------------------|
| **1. Presentación**     | Aplicación web responsive en React, accesible desde dispositivos móviles y escritorio.       |
| **2. Aplicación**       | Backend en Node.js con lógica de negocio: validación, reservas, notificaciones.              |
| **3. Servicios**        | API RESTful para la interacción entre clientes, frontend y base de datos.                    |
| **4. Datos**            | Base de datos relacional con persistencia de usuarios, reservas e inventario.                |
| **5. Integración**      | Espacio para futuras conexiones con pasarelas de pago, Google Calendar o redes sociales.     |
| **6. Infraestructura**  | Servicios cloud con despliegue continuo, backups automáticos y escalabilidad.                |
| **7. Seguridad**        | Autenticación por JWT, HTTPS con TLS 1.2+, control de roles, validaciones y logs de acceso.  |

---

### Diagrama Motivacional (resumen conceptual)

- **Actores:** Clientes, equipo administrativo de Nysa Arts.
- **Objetivos:** Agilizar la reserva, reducir errores, mejorar la experiencia de uso.
- **Drivers:** Alta demanda, procesos manuales ineficientes, necesidad de escalabilidad.


## 4 Requerimientos del Sistema

## Funcionales

| Categoría                  | Requerimiento Funcional         | Explicación                                                                 |
|---------------------------|----------------------------------|------------------------------------------------------------------------------|
| Reservas de Salas         | Verificación de disponibilidad   | El sistema consulta disponibilidad de sala y equipos según bloques de tiempo. |
| Reservas de Salas         | Disponibilidad continua          | El sistema está disponible al público todo el tiempo para realizar reservas. |
| Reservas de Salas         | Mail de confirmación             | Se envía un mail para confirmar la identidad y la reserva.                  |
| Reservas de Salas         | Registro de reserva              | Se registran en la base de datos los detalles de sala, horario y equipamiento. |
| Reservas de Salas         | Notificación al centro           | Se envía automáticamente al centro la información de la reserva realizada.  |
| Inventario                | Validación de disponibilidad     | Se valida que el equipo solicitado esté disponible en la sala antes de confirmar. |
| Inventario                | Actualización de inventario      | El sistema actualiza el estado de los equipos según reservas confirmadas/liberadas. |
| Inventario                | Gestión de inventario            | El sistema permite registrar, actualizar y eliminar equipos por sala.        |
| Recopilación y análisis   | Registro histórico de reservas   | Se almacenan todas las reservas para análisis posterior.                    |
| Recopilación y análisis   | Análisis de patrones             | Se detectan tendencias de uso de salas y equipos a partir de los datos.     |
| Recopilación y análisis   | Generación de reportes           | El sistema genera reportes automáticos sobre uso, frecuencia y cancelaciones. |

## No funcionales

| Categoría       | Requerimiento No Funcional | Explicación                                                                 |
|-----------------|----------------------------|------------------------------------------------------------------------------|
| Disponibilidad  | 24/7                        | El sistema debe estar disponible para los usuarios en todo momento.         |
| Rendimiento     | Respuesta rápida            | El sistema debe responder en menos de 1 segundo bajo carga normal.          |
| Seguridad       | Cifrado y control de acceso | Debe usar TLS 1.2 o superior, con roles definidos y validación de acceso.   |
| Escalabilidad   | Autoescalado                | El sistema debe escalar automáticamente al superar el 70 % de uso de CPU.   |
| Usabilidad      | Interfaz amigable           | Debe contar con una interfaz intuitiva y optimizada para dispositivos móviles. |
| Mantenibilidad  | Código modular               | El sistema debe estar desarrollado con arquitectura modular y pruebas automatizadas. |

---

## 5. Perfil Operacional

### 5.1 Escenarios de Uso (Proceso Clave: Reserva de Salas)

| Escenario clave               | Descripción                                                                 |
|------------------------------|-----------------------------------------------------------------------------|
| Consulta de disponibilidad   | Visualización en tiempo real de salas e instrumentos según bloque horario. |
| Reserva y modificación       | Solicitud, edición o cancelación de reservas por parte del cliente.        |
| Gestión de inventario        | Asignación y control de equipos disponibles por sala.                      |
| Notificaciones automáticas   | Confirmaciones y recordatorios automáticos de reservas.                   |
| Análisis y predicción de uso | Registro histórico y análisis de demanda futura.                           |

---

### 5.2 Usuarios

| Usuario                  | Rol                                                                 |
|--------------------------|----------------------------------------------------------------------|
| Clientes                 | Músicos o bandas que reservan salas para ensayo o producción.       |
| Personal administrativo  | Gestionan reservas, validan inventario y atención al cliente.       |
| Dueños / Gestión         | Analizan datos para toma de decisiones estratégicas.                |

---

### 5.3 Restricciones

- **Técnicas**: Tecnologías open-source (Node.js, React, PostgreSQL), bajo costo.
- **Económicas**: Sin subscripciones mensuales ni servidores caros.
- **Temporales**: Tiempo limitado (4–6 meses de desarrollo).
- **Operativas**: Alta demanda en horario pico, necesidad de disponibilidad continua.

---

### 5.4 Atributos de Calidad Prioritarios

| Atributo         | Prioridad | Justificación                                                                                   |
|------------------|-----------|--------------------------------------------------------------------------------------------------|
| Rendimiento      | Alta      | Respuesta rápida evita que los usuarios abandonen el proceso de reserva.                        |
| Disponibilidad   | Alta      | Debe estar operativo 24/7, especialmente durante horas pico.                                     |
| Seguridad        | Media-Alta| Se manejan datos personales; se requiere control de acceso y protección de información.         |
| Escalabilidad    | Media     | Aumento progresivo de usuarios y reservas requiere infraestructura adaptable.                   |
| Mantenibilidad   | Media     | El sistema debe poder ser actualizado fácilmente por equipos técnicos reducidos.                |
| Usabilidad       | Media     | Usuarios no técnicos requieren una interfaz clara y sencilla para reservar.                    |

---

### 5.5 Escenarios de Calidad (Formato ATAM)

#### Escenario 1: Rendimiento

> **Cuando** un cliente accede al sistema para consultar disponibilidad de salas e instrumentos,  
> **el sistema debe** mostrar los resultados en menos de 2 segundos,  
> **bajo** una carga de hasta 30 usuarios concurrentes y con 10.000 registros en la base de datos.

**Justificación**: Mejora la tasa de reservas exitosas y evita que los clientes recurran a canales manuales como WhatsApp.

---

## 6 Aplicación de ATAM (básico)

## Atributos de calidad

1. **Rendimiento**: < 500 ms para 100 usuarios simul.  
2. **Seguridad**: Protección contra inyección SQL/XSS.  
3. **Disponibilidad**: Recuperación tras fallo de contenedor.  
4. **Escalabilidad**: Provisionar nuevas réplicas en < 2 min.  
5. **Mantenibilidad**: MTTC < 1 hora para parches críticos.

## Escenarios de calidad

- 100 usuarios realizan consultas simultáneas.  
- Intento de inyección SQL bloqueado por WAF.  
- Falla de un contenedor y recuperación automática.




#### Escenario 2: Disponibilidad

> **Cuando** se produce una falla parcial del sistema (como la caída de un contenedor),  
> **el sistema debe** recuperarse automáticamente en menos de 2 minutos,  
> **bajo** una arquitectura con monitoreo activo y balanceo de carga, asegurando ≥ 99% de disponibilidad.

**Justificación**: Asegura continuidad durante horarios críticos, evitando pérdida de reservas y frustración de usuarios.

---

## 7 Análisis de Brechas

| Aspecto               | As-Is                                  | To-Be                                            | Brecha | Sugerencia de mejora                                                                                         |
|-----------------------|----------------------------------------|--------------------------------------------------|-------:|----------------------------------------------------------------------|
| Automatización        | Manual (Excel)                         | Web app con base de datos relacional             | Alta   | Desarrollar una aplicación web que automatice el flujo de reservas y  |                       |                                        |                                                  |        | notifique a los usuarios en tiempo real 
| Viabilidad            | Ninguna en tiempo real                 | Dashboard en React                               | Alta   | Implementar un dashboard en React que muestre el estado de las salas y métricas clave en tiempo real         |
| Escalabilidad         | Limitada al Excel                      | Microservicios en Docker                         | Media  | Migrar a una arquitectura de microservicios contenedorizados para permitir escalado horizontal               |
| Predicción de demanda | No existe                              | Módulo SARIMA                                    | Alta   | Integrar un modelo SARIMA que analice datos históricos y genere pronósticos automáticos de ocupación         |
| Seguridad             | Nula                                   | JWT + TLS                                        | Alta   | Implementar autenticación basada en JWT y cifrado TLS para proteger las comunicaciones y los datos            |
| Disponibilidad        | Archivo local, disponibilidad solo local | Alta disponibilidad con balanceador de carga    | Alta   | Desplegar la solución en la nube con balanceador de carga y réplicas de base de datos                        |
| Usabilidad            | Interfaz de Excel, poco intuitivo      | UI responsive y accesible                        | Media  | Diseñar una interfaz web responsiva y accesible que simplifique la experiencia de reserva                     |
| Monitoreo y logging   | No existe                              | Logs centralizados y alertas con ELK Stack       | Alta   | Configurar ELK Stack (Elasticsearch, Logstash, Kibana) para centralizar logs y emitir alertas automáticas    |
| Respaldo y recuperación | Copias manuales de Excel              | Backups automáticos y recuperación programada    | Alta   | Establecer un sistema de backups automáticos y realizar pruebas periódicas de restauración                    |
| Mantenibilidad        | Código y lógica en Excel               | Código modular en Node.js/React + tests          | Media  | Refactorizar la lógica a un código modular en Node.js/React, acompañado de pruebas unitarias e integrales    |
| Cumplimiento normativo| No existe                              | Gestión de datos con GDPR/Privacy Shield         | Media  | Implementar políticas de privacidad, gestión de consentimientos y registros de auditoría para cumplimiento GDPR |


---

# Bibliografía

- Informe de Avance Capstone (Grupo 6), Capstone Project, Nysa Arts, 2024.  
- Nevile8. (s.f.). *final*. GitHub. Recuperado el 5 de junio de 2025, de https://github.com/Nevile8/final  
- Nysa Arts. (s.f.). *Nysa Arts Demo*. Recuperado el 5 de junio de 2025, de https://nyssaa.netlify.app/

