# Indicadores de control y estándares de desempeño

**Proyecto:** Sistema Centralizado de Gestión de Recursos Humanos — Baterías Elpra S.A.
**Grupo N.º 15:** Cristian Ahumada · Tomas Ahumada
**Documento:** complemento del apartado 7 del [README](../../README.md)

---

## 1. Propósito

Este documento define **cómo se controla** el sistema de RR.HH. una vez desarrollado e implementado, y cómo se audita técnicamente su desempeño. Aplica la etapa de **Dirección y Control** del proceso administrativo, que sigue cuatro pasos:

| Paso | Qué significa | Dónde está en este documento |
|---|---|---|
| 1. Fijar el estándar | Definir el nivel de desempeño esperado | Columna "Estándar" de cada ficha |
| 2. Medir | Obtener el valor real con una fuente de datos concreta | "Fuente de datos" y "Método de medición" |
| 3. Comparar | Contrastar el valor real con el estándar | "Semáforo de desvío" |
| 4. Corregir | Actuar sobre el desvío | "Acción correctiva" |

Los estándares 1 a 3 provienen de las **metas cuantitativas de la Entrega 2**. Los estándares 4 a 6 se incorporan en la Entrega 4 para cubrir la seguridad industrial, la calidad de los datos y el cumplimiento del cronograma.

---

## 2. Tablero resumen

| N.º | Indicador | Estándar | Frecuencia | Responsable | Vínculo |
|---|---|---|---|---|---|
| 1 | Tiempo de generación del reporte mensual | ≤ 3 s para hasta 500 colaboradores | Cada cierre mensual | Equipo técnico / QA | Meta 1 (Ent. 2) · HU2 |
| 2 | Legajos migrados desde Excel | 100 % antes de las pruebas de usuario | Única vez, al migrar | Administrador de BD | Meta 2 (Ent. 2) · HU1 |
| 3 | Tasa de adopción del sistema | > 95 % durante el primer mes | Semanal | Líder de proyecto / RR.HH. | Meta 3 (Ent. 2) |
| 4 | Operarios con capacitación vigente en puestos críticos | 100 % | Diaria | Seguridad e Higiene | HU3 · BPMN |
| 5 | Errores de carga en asistencia y horas extra | < 1 % | Mensual | Jefe de planta / RR.HH. | HU2 · BPMN |
| 6 | Cumplimiento del cronograma | ≤ 12 semanas (razón ≤ 1) | Semanal | Líder de proyecto | Gantt (Ent. 2) |

---

## 3. Fichas de indicadores

### Indicador 1 — Tiempo de generación del reporte mensual

| Campo | Detalle |
|---|---|
| **Qué mide** | Rapidez con la que el sistema consolida asistencia y horas extra del mes. |
| **Fórmula** | Tiempo de respuesta (segundos) entre la solicitud del reporte y su visualización completa. |
| **Fuente de datos** | Entidad `REGISTRO_ASISTENCIA` (campos `fecha`, `estado_asistencia`, `horas_extra`). |
| **Método de medición** | Ejecutar el reporte 5 veces con una base de prueba de 500 colaboradores y tomar el **promedio**. |
| **Estándar** | ≤ 3 segundos. |
| **Frecuencia** | En cada cierre mensual (y en las pruebas de carga de la fase 4.1 del Gantt). |
| **Responsable** | Equipo técnico / Especialista QA. |
| **Acción correctiva** | Optimizar consultas, agregar índices sobre `id_empleado` y `fecha`, y revisar la paginación del reporte. |

| Semáforo | Condición |
|---|---|
| 🟢 Verde | ≤ 3 s |
| 🟡 Amarillo | > 3 s y ≤ 5 s |
| 🔴 Rojo | > 5 s |

### Indicador 2 — Legajos migrados desde Excel

| Campo | Detalle |
|---|---|
| **Qué mide** | Completitud de la migración desde las planillas desarticuladas hacia la base centralizada. |
| **Fórmula** | (Legajos migrados correctamente ÷ legajos activos en las planillas) × 100 |
| **Fuente de datos** | Entidad `LEGAJO_EMPLEADO` (`id_empleado`, `cuil`, `fecha_ingreso`, `sector_asignado`) contra el conteo de legajos activos en Excel. |
| **Método de medición** | Comparar el total de filas activas en origen contra los registros cargados y verificar que no haya `cuil` duplicados ni vacíos. |
| **Estándar** | 100 % antes del inicio de las pruebas de usuario (UAT). |
| **Frecuencia** | Una única vez, en la migración (tareas 2.1 y 5.1 del Gantt). |
| **Responsable** | Administrador de BD. |
| **Acción correctiva** | Reprocesar los registros fallidos, depurar los datos de origen y repetir la verificación. |

| Semáforo | Condición |
|---|---|
| 🟢 Verde | 100 % |
| 🟡 Amarillo | ≥ 98 % y < 100 % (con lista de pendientes) |
| 🔴 Rojo | < 98 % |

### Indicador 3 — Tasa de adopción del sistema

| Campo | Detalle |
|---|---|
| **Qué mide** | Qué parte del personal administrativo y de los jefes de planta usa efectivamente el sistema. |
| **Fórmula** | (Usuarios con al menos un ingreso semanal ÷ usuarios previstos) × 100 |
| **Fuente de datos** | Registro de accesos del sistema. **No figura en el diccionario de datos actual** (ver sección 5). |
| **Método de medición** | Contar usuarios con actividad en la semana y compararlos con el padrón de usuarios habilitados. |
| **Estándar** | > 95 % durante el primer mes de operación. |
| **Frecuencia** | Semanal. |
| **Responsable** | Líder de proyecto / RR.HH. |
| **Acción correctiva** | Reforzar la capacitación, designar referentes por sector y relevar las causas de no uso. |

| Semáforo | Condición |
|---|---|
| 🟢 Verde | > 95 % |
| 🟡 Amarillo | 85 % a 95 % |
| 🔴 Rojo | < 85 % |

### Indicador 4 — Operarios con capacitación vigente en puestos críticos

| Campo | Detalle |
|---|---|
| **Qué mide** | Cumplimiento de las inducciones obligatorias de seguridad (manejo de plomo y ácido). |
| **Fórmula** | (Operarios en puestos críticos con capacitación vigente ÷ operarios asignados a puestos críticos) × 100 |
| **Condición de "vigente"** | `estado_aprobacion = True` **y** `fecha_vencimiento` ≥ fecha actual. |
| **Fuente de datos** | Entidades `CAPACITACION_SEGURIDAD` y `LEGAJO_EMPLEADO` (`sector_asignado`). |
| **Método de medición** | Consulta automática diaria. En el BPMN, la compuerta *¿Capacitación vigente?* aplica la misma regla en cada fichaje. |
| **Estándar** | 100 %. Es el único indicador con tolerancia cero, por el riesgo del manejo de residuos peligrosos. |
| **Frecuencia** | Diaria. |
| **Responsable** | Seguridad e Higiene. |
| **Acción correctiva** | Bloqueo automático de puestos críticos, alerta al responsable y gestión de la renovación con el proveedor externo. |

| Semáforo | Condición |
|---|---|
| 🟢 Verde | 100 % |
| 🟡 Amarillo | Hay certificaciones que vencen en los próximos 30 días |
| 🔴 Rojo | Hay un operario sin capacitación vigente asignado a un puesto crítico |

### Indicador 5 — Errores de carga en asistencia y horas extra

| Campo | Detalle |
|---|---|
| **Qué mide** | Calidad de los datos que alimentan la liquidación de haberes. |
| **Fórmula** | (Registros de asistencia corregidos posteriormente ÷ registros de asistencia totales del mes) × 100 |
| **Fuente de datos** | Entidad `REGISTRO_ASISTENCIA`. Requiere un historial de modificaciones (ver sección 5). |
| **Método de medición** | Al cierre del mes, contar los registros que fueron modificados después de su carga inicial. |
| **Estándar** | < 1 %. *Es una meta propuesta por el grupo.* |
| **Frecuencia** | Mensual. |
| **Responsable** | Jefe de planta / RR.HH. |
| **Acción correctiva** | Revisar las validaciones del sistema (por ejemplo, horas extra negativas o fechas futuras) y reforzar el control antes de la liquidación. |

| Semáforo | Condición |
|---|---|
| 🟢 Verde | < 1 % |
| 🟡 Amarillo | 1 % a 3 % |
| 🔴 Rojo | > 3 % |

### Indicador 6 — Cumplimiento del cronograma

| Campo | Detalle |
|---|---|
| **Qué mide** | Avance real del proyecto frente a la planificación de 12 semanas. |
| **Fórmula** | Semanas reales transcurridas ÷ semanas planificadas |
| **Fuente de datos** | Diagrama de Gantt de la Entrega 2 y estado de las tarjetas del tablero de GitHub Projects. |
| **Método de medición** | Comparar las fechas reales de finalización de cada tarea con las planificadas. |
| **Estándar** | ≤ 12 semanas en total (razón ≤ 1). |
| **Frecuencia** | Semanal. |
| **Responsable** | Líder de proyecto. |
| **Acción correctiva** | Replanificar tareas, reasignar responsables y revisar dependencias críticas. |

| Semáforo | Condición |
|---|---|
| 🟢 Verde | Razón ≤ 1,00 |
| 🟡 Amarillo | Razón > 1,00 y ≤ 1,15 (hasta 2 semanas de atraso) |
| 🔴 Rojo | Razón > 1,15 |

---

## 4. Mecanismo de auditoría técnica

1. **Medición:** cada responsable registra el valor real en la planilla de la sección 6, con la fecha y la fuente de los datos.
2. **Comparación:** se contrasta el valor con el estándar y se asigna el semáforo.
3. **Escalamiento:**
   - 🟡 Amarillo: el responsable informa al líder de proyecto y propone una acción.
   - 🔴 Rojo: se convoca una revisión y la acción correctiva queda registrada como *Issue* en el tablero.
4. **Seguimiento:** el siguiente período verifica si la acción logró volver al estándar.
5. **Trazabilidad:** todo cambio de un estándar se hace mediante *Pull Request* con revisión de pares, de modo que queda en el historial de Git.

---

## 5. Supuestos y ajustes pendientes del diccionario de datos

Para medir estos indicadores, el sistema debería registrar datos que **hoy no figuran en el diccionario de datos de la Entrega 3**. Se dejan como ajustes a evaluar:

| Indicador | Dato necesario | Propuesta |
|---|---|---|
| 3 | Registro de accesos de los usuarios | Agregar una entidad `REGISTRO_ACCESO` (`id_usuario`, `fecha_hora`). |
| 5 | Historial de modificaciones de asistencia | Agregar un campo `fecha_modificacion` en `REGISTRO_ASISTENCIA` o una entidad de auditoría. |
| 4 | Fecha de realización del curso | Agregar `fecha_realizacion` en `CAPACITACION_SEGURIDAD`. |

Además, el campo `horas_extra` está definido como `Integer`, por lo que no admite fracciones de hora. Si se cambia, debe actualizarse también el cálculo del indicador 1.

---

## 6. Plantilla de registro de mediciones

| Fecha | Indicador | Valor real | Estándar | Semáforo | Desvío y causa | Acción correctiva | Responsable | Issue / PR |
|---|---|---|---|---|---|---|---|---|
| | | | | | | | | |
| | | | | | | | | |
| | | | | | | | | |
