Objetivo
Modelar el diagrama de clases UML de una plataforma que permite compartir vehículos (coches, motos y bicis eléctricas) entre propietarios y conductores registrados.

Enunciado (contexto de dominio)
La plataforma permite que propietarios publiquen vehículos, fijen precios y reglas de uso. Conductores buscan, reservan, pagan y valoran los viajes. La empresa intermedia los pagos, calcula comisiones y gestiona incidencias.

Requisitos funcionales (derivables a clases/relaciones)
Usuarios

Dos roles principales: Propietario y Conductor (un usuario puede tener ambos).
Datos comunes: id, nombre, email verificado, estado (activo/suspendido).
Seguridad: autenticación multifactor opcional; registro de la última sesión.

Vehículos

Tipos: Coche, Moto, BiciEléctrica (con atributos específicos: plazas, cilindrada, autonomía, etc.).
Cada vehículo tiene propietario único, estado (disponible/mantenimiento/suspendido), ubicación base.
Un vehículo puede tener varias fotos y documentación (ITV/seguro con fecha de expiración).

Publicaciones y precios

El propietario crea una Publicación con: precio por hora, fianza, política de cancelación, ventana de disponibilidad (fechas/horas).
Se pueden definir descuentos por duración y suplementos (ej. limpieza).

Búsqueda y reserva

El conductor realiza Búsquedas por ubicación y fechas.
Una Reserva referencia una Publicación y un intervalo [inicio, fin]; incluye estado (pendiente, confirmada, en curso, finalizada, cancelada).
Validaciones: solapamientos, disponibilidad, verificación de identidad del conductor.

Viaje/uso del vehículo

Al comenzar, se abre un ContratoDeUso con fotos de check-in (kilometraje, daños).
Al finalizar, check-out, cálculo de Coste (tiempo real ± penalizaciones).
Se genera Factura y Pago.

Pagos

Métodos: Tarjeta, PayPal, Cripto (añade un método abstracto y subclases).
Se aplica Comisión de la plataforma (porcentaje configurable).
Reembolsos parciales en cancelaciones según política.

Valoraciones e incidencias

Tras el viaje, ambas partes pueden dejar Valoración (1–5 estrellas, comentario).
Se pueden abrir Incidencias con evidencias (fotos, mensajes) y estado (abierta, en revisión, resuelta).
Notificaciones

Email y push: cambios de estado de Reserva, recordatorios, pagos realizados.

Reglas de negocio (importante para multiplicidades y composiciones)
Un Usuario tiene 0..* Reservas como conductor y 0..* Vehículos como propietario.
Un Vehículo pertenece a exactamente 1 Propietario.
Una Publicación es composición sobre Vehículo (si se elimina el vehículo, caen sus publicaciones).
Una Reserva está asociada a exactamente 1 Publicación y 1 Conductor.
Una Reserva puede generar 0..1 ContratoDeUso, 1 Factura, 1.. Pagos* (pago y posibles reembolsos).
Una Reserva tiene 0..1 Descuento aplicado y 0..* Suplementos.
Un Pago usa exactamente 1 método de pago (polimorfismo).
Un Usuario puede emitir 0..* Valoraciones; cada Valoración se vincula a exactamente 1 Reserva y 1 destinatario (Usuario o Vehículo; elige un modelo).
Una Incidencia se asocia a exactamente 1 Reserva.