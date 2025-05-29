<!-- ============================================= -->
<!--                Portada del Informe             -->
<!-- ============================================= -->

<p align="center">
  <img src="docs/logo-uai.png"  width="180" />
</p>

---

# **Informe 1**

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

# Descripción del proyecto y contexto

En un mercado musical cada vez más competitivo y con demanda variable, **Nysa Arts** busca modernizar su sistema de gestión de reservas. Actualmente los músicos y productores solicitan salas de ensayo y producción mediante WhatsApp, Instagram o llamadas telefónicas, y los administradores registran las solicitudes en una planilla de Excel en OneDrive, lo cual genera:

- Demoras en la respuesta.  
- Errores manuales.  
- Falta de visibilidad en tiempo real de la ocupación de espacios.  

**Ubicación**: Santiago de Chile  
**Clientes principales**: Músicos independientes, bandas emergentes, productoras y educadores musicales.

# Arquitectura As-Is

## Componentes principales

- **Clientes**: Usuarios que contactan a través de WhatsApp o Instagram.  
- **Administración**: Personal de Nysa Arts que actualiza la planilla Excel.  
- **Base de datos**: Hoja de cálculo en Excel.  
- **Comunicación**: WhatsApp, Instagram y llamadas telefónicas.  

![Diagrama de Componentes As-Is](docs/figura_1.png){#fig:componentes}

*Figura 1. Diagrama de Componentes As-Is*

## Flujo de datos

1. El cliente se contacta por WhatsApp, Instagram o llamada.  
2. El administrador revisa manualmente la planilla de Excel.  
3. Si hay disponibilidad, registra la reserva en la planilla.  
4. Confirma la reserva al cliente por el mismo canal.

![Diagrama de Actividad As-Is](docs/figura_2.png){#fig:flujo}

*Figura 2. Diagrama de Actividad (flujo de datos)*

## Restricciones principales

- **Sin transacciones atómicas**: Riesgo de sobrescritura al editar simultáneamente.  
- **No hay histórico estructurado**: Dificultad para análisis de tendencias.  
- **Latencia**: Demoras de minutos u horas en reflejar cambios.  
- **Sin monitoreo o control de concurrencia**: No hay bloqueo ni alertas automáticas.

# Análisis de Arquitectura Empresarial

- **Stakeholder principal**: Nysa Arts.  
- **Usuario principal**: Clientes que reservan espacios.  
- **Objetivos estratégicos**:
  - Mejorar eficiencia operativa.  
  - Reducir errores y tiempos de espera.  
  - Aumentar fidelización y ocupación.

# Requerimientos del Sistema

## Funcionales

1. **Reservas de Salas**  
   - Verificar disponibilidad en tiempo real.  
   - Enviar confirmación por correo.  
   - Registrar reserva en base de datos.  
2. **Inventario**  
   - Validar equipamiento antes de confirmar.  
   - Actualizar estado de equipos.  
3. **Análisis de datos**  
   - Guardar histórico de reservas.  
   - Generar reportes de uso y tendencias.

## No funcionales

- **Disponibilidad**: 24/7.  
- **Rendimiento**: Respuesta < 1 s bajo carga normal.  
- **Seguridad**: TLS 1.2+, roles y validación de acceso.  
- **Escalabilidad**: Autoescalado al superar 70 % de CPU.  
- **Usabilidad**: Interfaz intuitiva y móvil.  
- **Mantenibilidad**: Código modular y tests automatizados.

# Aplicación de ATAM (básico)

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

# Análisis de Brechas

| Aspecto         | As-Is                      | To-Be                                      | Brecha  |
|-----------------|----------------------------|--------------------------------------------|--------:|
| Automatización  | Manual (Excel)            | Web app con base de datos relacional       | Alta    |
| Escalabilidad   | Limitada a Excel          | Microservicios en Docker                  | Media   |
| Predicción      | No existe                 | Módulo SARIMA                              | Alta    |
| Seguridad       | Nula                       | JWT + TLS                                  | Alta    |
| Disponibilidad  | Local (archivo)          | Alta disponibilidad con load balancer     | Alta    |

# Bibliografía

- Informe de Avance Capstone (Grupo 6), Capstone Project, Nysa Arts, 2024.  
- Nevile8. (s.f.). *final*. GitHub. Recuperado el 5 de junio de 2025, de https://github.com/Nevile8/final  
- Nysa Arts. (s.f.). *Nysa Arts Demo*. Recuperado el 5 de junio de 2025, de https://nyssaa.netlify.app/

