Documentación del Sistema de Gestión de Préstamos de Pañol Escolar


Sistema elegido: Gestión de Préstamos de Pañol Escolar (control de stock e inventario, entregas y devoluciones a alumnos, historial de estado de herramientas y reporte de insumos dañados).

 1: Marco Conceptual y Clasificación
1.1 Documentación de Usuario vs. Documentación Técnica
Documentación de Usuario. Es el material que explica cómo usar el sistema para cumplir una tarea concreta.
Audiencia: quienes operan el sistema a diario (pañolero, docentes, preceptores, alumnos), sin necesidad de conocimientos informáticos.
Lenguaje: simple, cotidiano, orientado a acciones ("presione", "seleccione"), con capturas y ejemplos; evita tecnicismos.
Objetivo: que la persona pueda realizar su trabajo de forma autónoma y sin errores (instructivos, guías rápidas, manuales de usuario, FAQ).
Documentación Técnica (de Sistema). Es el material que explica cómo está construido, instalado y mantenido el sistema.
Audiencia: desarrolladores, administradores de sistemas, personal de soporte y de base de datos.
Lenguaje: técnico y preciso (modelos de datos, diagramas, código, configuraciones, comandos).
Objetivo: permitir instalar, configurar, mantener, corregir y ampliar el sistema (manual de instalación, diccionario de datos, diagrama entidad-relación, arquitectura, API).
1.2 Clasificación de elementos
Elemento
Tipo
Justificación
a) Manual de configuración de cadenas de conexión a la Base de Datos y despliegue del servidor
Documentación Técnica
Está dirigido a administradores/desarrolladores, usa lenguaje especializado (cadenas de conexión, servidor) y su objetivo es instalar y mantener el sistema, no usarlo.
b) Guía rápida en PDF sobre cómo solicitar el préstamo de una herramienta desde la app
Documentación de Usuario
Está dirigida al usuario final (alumno o docente), explica una tarea funcional de la aplicación con lenguaje simple y su objetivo es que pueda operar el sistema.

Apartado 2: Documentación Paso a Paso
1. Tarea
Registrar un nuevo préstamo de herramienta a un alumno, con validación de sanción activa y de stock.
2. Objetivo
Entregar una herramienta a un alumno habilitado dejando registrado el préstamo (alumno, herramienta, cantidad, fecha y hora de salida, fecha de devolución prevista) y descontando el stock disponible.
3. Prerrequisitos
El pañolero debe haber iniciado sesión con su usuario y contraseña.
El alumno debe estar dado de alta en el sistema (con DNI, curso y taller).
La herramienta debe estar cargada en el inventario con su código y estado "Operativa".
El alumno no debe tener préstamos vencidos sin devolver (el sistema lo verifica).
4. Pasos numerados
Ingrese al sistema y, en el menú principal, presione Préstamos > Nuevo préstamo.
En el campo Alumno, escriba el DNI o el apellido y seleccione al alumno de la lista desplegable.
Revise el aviso que muestra el sistema: si el alumno tiene una sanción activa, aparecerá un mensaje en rojo y no podrá continuar (vea Errores posibles). Si no tiene sanciones, aparecerá "Alumno habilitado" en verde.
En el campo Herramienta, escriba el nombre o escanee/ingrese el código y seleccione el ítem.
Verifique el Stock disponible que se muestra al lado del nombre de la herramienta.
Ingrese la Cantidad a prestar (por defecto 1).
Controle la Fecha de devolución prevista (el sistema propone el final de la jornada; puede modificarla si el docente lo autorizó).
Si desea, agregue una observación (por ejemplo: "Se entrega con maletín").
Presione el botón Confirmar préstamo.
Entregue la herramienta al alumno y, si corresponde, imprima o muestre el comprobante.
5. Resultado esperado
El sistema muestra el mensaje verde "Préstamo registrado correctamente" con un número de comprobante. El stock de la herramienta disminuye en la cantidad prestada y el préstamo aparece en el historial del alumno con estado "Pendiente de devolución".
6. Errores posibles y soluciones
Error
Causa
Solución
"Alumno con sanción activa"
El alumno tiene una sanción vigente (por ejemplo, por devolución tardía o daño)
No se puede prestar. Informe al alumno y derive a preceptoría/docente a cargo. Cuando la sanción se levante, repita la tarea.
"Stock insuficiente"
La cantidad pedida supera las unidades disponibles
Reduzca la cantidad, elija una herramienta equivalente o registre al alumno en lista de espera.
"Herramienta fuera de servicio"
El ítem figura como dañado o en reparación
Elija otra unidad operativa o consulte el historial de estado de la herramienta.
"Alumno no encontrado"
El alumno no está dado de alta o el DNI se escribió mal
Verifique el DNI; si no existe, solicite el alta a Administración y reintente.

Apartado 3: Diagrama de Flujo del Proceso
Proceso: circuito completo de solicitud de préstamo, verificación de alumno y stock, y confirmación.
Simbología: óvalo = inicio/fin; rectángulo = proceso; rombo = decisión.
Caminos alternativos contemplados: (1) alumno inexistente o con sanción activa y (2) herramienta no disponible (sin stock o fuera de servicio).

circuito de solicitud de préstamo · 3 decisiones, 3 caminos de error
Los tres caminos de error (alumno inexistente, sanción o préstamo vencido, y herramienta sin disponibilidad) terminan sin registrar el préstamo ni modificar el stock.
Apartado 4: Caso de Uso Formal
Nombre: CU-01 Registrar préstamo de herramienta
Actor principal: Pañolero (actúa en nombre del alumno solicitante). Actores secundarios: Alumno (solicita la herramienta).
Precondiciones:
El pañolero está autenticado en el sistema.
El alumno y la herramienta están registrados.
El sistema tiene conexión con la base de datos.
Postcondiciones (éxito): el préstamo queda registrado, el stock se actualiza y el historial del alumno y de la herramienta se actualizan.
Flujo principal (camino feliz)
El pañolero selecciona la opción "Nuevo préstamo".
El sistema muestra el formulario de préstamo.
El pañolero busca e indica al alumno.
El sistema verifica que el alumno exista, no tenga sanciones activas ni préstamos vencidos, y muestra "Alumno habilitado".
El pañolero busca e indica la herramienta y la cantidad.
El sistema verifica que la herramienta esté operativa y que haya stock suficiente.
El sistema propone la fecha de devolución prevista.
El pañolero confirma el préstamo.
El sistema registra el préstamo, descuenta el stock y genera el comprobante.
El sistema muestra el mensaje "Préstamo registrado correctamente" y el caso de uso finaliza.
Flujos alternativos y excepciones
A1. Alumno no encontrado (en el paso 4).
El sistema informa "Alumno no encontrado".
El pañolero corrige la búsqueda o solicita el alta al administrador.
Si se corrige, el flujo continúa en el paso 4; si no, el caso de uso termina sin registrar nada.
A2. Alumno con sanción activa o préstamo vencido (en el paso 4).
El sistema muestra el motivo y la fecha de fin de la sanción y bloquea el botón de confirmación.
El pañolero informa al alumno y lo deriva a preceptoría.
El caso de uso termina sin registrar el préstamo.
A3. Stock insuficiente (en el paso 6).
El sistema informa las unidades disponibles.
El pañolero puede (a) reducir la cantidad y volver al paso 5, (b) elegir otra herramienta y volver al paso 5, o (c) cancelar.
A4. Herramienta fuera de servicio (en el paso 6).
El sistema indica que el ítem está dañado o en reparación.
El pañolero selecciona otra unidad (vuelve al paso 5) o cancela el caso de uso.
A5. Falla de conexión al guardar (en el paso 9).
El sistema informa que no pudo guardar y no descuenta stock.
El pañolero reintenta; si persiste, registra el préstamo manualmente y avisa a soporte técnico.
Apartado 5: Capturas Anotadas e Interfaz
Interfaz: zona de interacción de la pantalla "Nuevo préstamo" (se recorta únicamente el formulario, sin menú lateral ni encabezado general).
┌───────────────────────────────────────────────┐
│  NUEVO PRÉSTAMO                               │
├───────────────────────────────────────────────┤
│  Alumno:  [ 1  Buscar por DNI o apellido  🔍 ]│
│  ● Alumno habilitado   (2)                    │
│                                               │
│  Herramienta: [ 3  Nombre o código        🔍 ]│
│  Stock disponible: 5 unidades   (4)           │
│                                               │
│  Cantidad: [ 5  1 ]  Devolución: [ 6 25/11 ]  │
│  Observaciones: [ 7                          ]│
│                                               │
│        [ 8 Cancelar ]   [ 9 Confirmar préstamo]│
└───────────────────────────────────────────────┘
Referencias
N.º
Elemento
Descripción
1
Campo Alumno
Búsqueda por DNI o apellido; despliega una lista de coincidencias.
2
Indicador de estado del alumno
Verde "Alumno habilitado" o rojo "Sanción activa"; si es rojo, bloquea la confirmación.
3
Campo Herramienta
Búsqueda por nombre o código del ítem.
4
Stock disponible
Muestra las unidades libres; se actualiza al elegir la herramienta.
5
Campo Cantidad
Número de unidades a prestar; no puede superar el stock.
6
Fecha de devolución
Fecha límite de devolución; se propone automáticamente y es editable.
7
Observaciones
Texto libre opcional (estado al entregar, accesorios incluidos).
8
Botón Cancelar
Descarta el formulario sin registrar el préstamo.
9
Botón Confirmar préstamo
Valida los datos, registra el préstamo y descuenta el stock.

