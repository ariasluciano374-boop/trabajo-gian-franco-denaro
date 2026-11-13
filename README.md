# Sistema de Gestión de Préstamos de Pañol Escolar 🛠️

Documentación funcional y técnica correspondiente a la **Opción A – Gestión de Préstamos de Pañol Escolar**, orientada al control de stock e inventario, entregas y devoluciones a alumnos, historial de estado de herramientas y reporte de insumos dañados.

---

## 📋 Contenido del Repositorio

* `Documentación del Sistema de Gestión de Préstamos de Pañol Escolar.docx` — Documento original completo.
* `README.md` — Guía estructurada del proyecto, marco conceptual, instructivo paso a paso, diagrama de flujo, caso de uso formal y prototipo de interfaz.

---

## 1. Marco Conceptual y Clasificación

### 1.1 Documentación de Usuario vs. Documentación Técnica
* **Documentación de Usuario:** Explica cómo usar el sistema para cumplir una tarea concreta. Está dirigida a quienes operan el sistema a diario (pañolero, docentes, preceptores, alumnos) con lenguaje simple, cotidiano y orientado a acciones (instructivos, guías rápidas, manuales de usuario, FAQ).
* **Documentación Técnica (de Sistema):** Explica cómo está construido, instalado y mantenido el sistema. Está dirigida a desarrolladores, administradores de sistemas y soporte, utilizando lenguaje técnico y preciso (manual de instalación, diccionario de datos, diagrama entidad-relación, arquitectura, API).

### 1.2 Clasificación de Elementos

| Elemento | Tipo | Justificación |
| :--- | :--- | :--- |
| **a)** Manual de configuración de cadenas de conexión a la Base de Datos y despliegue del servidor | **Documentación Técnica** | Está dirigido a administradores/desarrolladores, usa lenguaje especializado y su objetivo es instalar y mantener el sistema. |
| **b)** Guía rápida en PDF sobre cómo solicitar el préstamo de una herramienta desde la app | **Documentación de Usuario** | Está dirigida al usuario final (alumno o docente), explica una tarea funcional con lenguaje simple para operar el sistema. |

---

## 2. Instructivo Paso a Paso: Registrar Nuevo Préstamo

* **Objetivo:** Entregar una herramienta a un alumno habilitado dejando registrado el préstamo (alumno, herramienta, cantidad, fecha y hora de salida, fecha de devolución prevista) y descontando el stock disponible.
* **Prerrequisitos:**
  * El pañolero debe haber iniciado sesión con su usuario y contraseña.
  * El alumno debe estar dado de alta en el sistema (con DNI, curso y taller) y sin préstamos vencidos ni sanciones.
  * La herramienta debe estar cargada en el inventario con su código y en estado **"Operativa"**.

### Procedimiento
1. Ingrese al sistema y en el menú principal presione **Préstamos > Nuevo préstamo**.
2. En el campo **Alumno**, escriba el DNI o el apellido y seleccione al alumno de la lista desplegable.
3. Revise el aviso del sistema: si aparece **"Alumno habilitado"** en verde puede continuar; si figura una sanción activa en rojo, el sistema bloqueará la operación.
4. En el campo **Herramienta**, escriba el nombre o escanee/ingrese el código y seleccione el ítem.
5. Verifique el **Stock disponible** que se muestra al lado de la herramienta.
6. Ingrese la **Cantidad** a prestar (por defecto `1`).
7. Controle la **Fecha de devolución prevista** (propuesta automáticamente al final de la jornada, editable con autorización docente).
8. Agregue una **Observación** opcional (ej. *"Se entrega con maletín"*).
9. Presione **Confirmar préstamo**, entregue la herramienta y emita el comprobante.

### Errores Posibles y Soluciones

| Error | Causa | Solución |
| :--- | :--- | :--- |
| **Alumno con sanción activa** | El alumno tiene una sanción vigente (devolución tardía o daño). | No se puede prestar. Informar al alumno y derivar a preceptoría/docente. |
| **Stock insuficiente** | La cantidad pedida supera las unidades disponibles. | Reducir la cantidad, elegir herramienta equivalente o anotar en lista de espera. |
| **Herramienta fuera de servicio** | El ítem figura como dañado o en reparación. | Elegir otra unidad operativa o consultar el historial de estado. |
| **Alumno no encontrado** | El alumno no está dado de alta o el DNI se escribió mal. | Verificar el DNI; si no existe, solicitar el alta a Administración y reintentar. |

---

## 3. Diagrama de Flujo del Proceso

Circuito completo de solicitud de préstamo con 3 puntos de decisión y 3 caminos alternativos de error que finalizan sin modificar el stock:

```mermaid
flowchart TD
    Inicio([Inicio]) --> Ingresar[Ingresar DNI o apellido]
    Ingresar --> D1{¿Alumno registrado?}
    D1 -- No --> Err1[Informar: alumno no encontrado; pedir alta]
    D1 -- Sí --> D2{¿Sanción activa o préstamo vencido?}
    D2 -- Sí --> Err2[Bloquear préstamo y derivar a preceptoría]
    D2 -- No --> Sel[Seleccionar herramienta y cantidad]
    Sel --> D3{¿Herramienta operativa y con stock suficiente?}
    D3 -- No --> Err3[Informar: sin stock o fuera de servicio]
    D3 -- Sí --> Reg[Registrar préstamo y descontar stock]
    Reg --> FinOk([Fin: préstamo registrado])
    Err1 --> FinErr([Fin sin préstamo])
    Err2 --> FinErr
    Err3 --> FinErr
