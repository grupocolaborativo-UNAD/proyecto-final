# Clase Reserva
class Reserva(Entity):

    ESTADO_PENDIENTE = "pendiente"
    ESTADO_CONFIRMADA = "confirmada"
    ESTADO_CANCELADA = "cancelada"
  def __init__(
        self,
        cliente: Cliente,
        servicio: Servicio,
        unidades: float
    ) -> None:

        super().__init__()

        if unidades <= 0:
            msg = "Las unidades deben ser positivas."
            logger.error(msg)
            raise InvalidDataError(msg)

        self.cliente = cliente
        self.servicio = servicio
        self.unidades = unidades
        self.estado = self.ESTADO_PENDIENTE

        logger.info(
            f"Reserva creada: cliente={cliente.nombre}, "
            f"servicio={servicio.nombre}"
        )

    def calcular_costo(self) -> float:

        try:
            return self.servicio.calcular_costo(self.unidades)

        except Exception as exc:
            logger.exception(
                f"Error calculando costo: {exc}"
            )
            raise

    def confirmar(self) -> None:

        if self.estado != self.ESTADO_PENDIENTE:
            msg = (
                f"No se puede confirmar la reserva "
                f"{self.id}"
            )

            logger.error(msg)
            raise ReservationError(msg)

        self.estado = self.ESTADO_CONFIRMADA
        logger.info(f"Reserva {self.id} confirmada.")

    def cancelar(self) -> None:

        if self.estado == self.ESTADO_CANCELADA:
            msg = f"La reserva {self.id} ya está cancelada."

            logger.error(msg)
            raise ReservationError(msg)

        self.estado = self.ESTADO_CANCELADA
        logger.info(f"Reserva {self.id} cancelada.")

    def procesar(self) -> float:

        try:
            logger.debug(
                f"Procesando reserva {self.id}"
            )

            costo = self.calcular_costo()

        except Exception as exc:

            logger.exception(
                f"Fallo procesando reserva: {exc}"
            )

            raise

        else:

            self.confirmar()

            logger.info(
                f"Reserva procesada correctamente."
            )

            return costo

        finally:

            logger.debug(
                f"Finalizó reserva {self.id}"
            )
            # Simulación de operaciones
            def simular_operaciones() -> None:

    operaciones: List[str] = []

    clientes: List[Cliente] = []
    servicios: List[Servicio] = []
    reservas: List[Reserva] = []

    # Operación 1
    try:
        cliente1 = Cliente(
            "Andrea Pérez",
            "andrea.perez@example.com",
            "3001234567"
        )

        clientes.append(cliente1)

        operaciones.append(
            "Cliente válido creado"
        )

    except InvalidDataError as e:
        operaciones.append(str(e))

    # Operación 2
    try:
        cliente2 = Cliente(
            "Luis Gómez",
            "correo_invalido",
            "3107654321"
        )

        clientes.append(cliente2)

    except InvalidDataError as e:
        operaciones.append(
            f"Email inválido detectado: {e}"
        )

    # Operación 3
    try:
        sala1 = Sala(
            "Sala Grande",
            "Sala amplia para eventos",
            tarifa_hora=50.0,
            capacidad=20
        )

        servicios.append(sala1)

        operaciones.append(
            "Sala válida creada"
        )

    except InvalidDataError as e:
        operaciones.append(str(e))

    # Operación 4
    try:
        sala2 = Sala(
            "Sala Pequeña",
            "Sala incorrecta",
            tarifa_hora=30.0,
            capacidad=-5
        )

        servicios.append(sala2)

    except InvalidDataError as e:
        operaciones.append(
            f"Capacidad inválida detectada: {e}"
        )

    # Operación 5
    try:
        equipo1 = Equipo(
            "Proyector",
            "Equipo HD",
            tarifa_hora=15.0,
            tipo_equipo="proyector",
            deposito=50.0
        )

        servicios.append(equipo1)

        operaciones.append(
            "Equipo válido creado"
        )

    except InvalidDataError as e:
        operaciones.append(str(e))

    # Operación 6
    try:
        asesoria1 = Asesoria(
            "Consultoría",
            "Asesoría software",
            -100.0
        )

        servicios.append(asesoria1)

    except InvalidDataError as e:
        operaciones.append(
            f"Costo inválido detectado: {e}"
        )

    # Operación 7
    try:

        reserva1 = Reserva(
            clientes[0],
            servicios[0],
            2
        )

        reservas.append(reserva1)

        costo = reserva1.procesar()

        operaciones.append(
            f"Reserva válida procesada. "
            f"Costo total: {costo}"
        )

    except Exception as e:
        operaciones.append(str(e))

    # Operación 8
    try:

        reservas[0].confirmar()

    except ReservationError as e:

        operaciones.append(
            f"Confirmación duplicada: {e}"
        )

    # Operación 9
    try:

        reserva2 = Reserva(
            clientes[0],
            servicios[0],
            -3
        )

        reservas.append(reserva2)

    except InvalidDataError as e:

        operaciones.append(
            f"Reserva inválida: {e}"
        )

    # Operación 10
    try:

        reservas[0].cancelar()
        reservas[0].cancelar()

    except ReservationError as e:

        operaciones.append(
            f"Cancelación duplicada: {e}"
        )

    logger.info(
        "Simulación completada."
    )

    print("Resumen de operaciones:")

    for op in operaciones:
        print("-", op)
        logger.info(op)
        # Ejecución principal
        if __name__ == "__main__":
    simular_operaciones()
    