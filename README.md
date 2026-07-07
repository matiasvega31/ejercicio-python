# ejercicio-python
ejercicio de python
# ==========================
# DICCIONARIOS
# ==========================

planes = {
    "F001": ["Plan Básico", "mensual", 1, True, False, "todo horario"],
    "F002": ["Plan Full", "mensual", 1, True, True, "todo horario"],
    "F003": ["Plan Premium", "trimestral", 3, True, True, "todo horario"],
    "F004": ["Plan Familiar", "trimestral", 3, False, True, "mañana"],
    "F005": ["Plan Elite", "anual", 12, True, True, "todo horario"],
    "F006": ["Plan Nocturno", "mensual", 1, False, True, "noche"]
}

inscripciones = {
    "F001": [14990, 30],
    "F002": [22990, 10],
    "F003": [39990, 0],
    "F004": [35990, 6],
    "F005": [159990, 2],
    "F006": [18990, 15]
}

# ==========================
# FUNCIONES
# ==========================

def leer_opcion():
    while True:
        try:
            op = int(input("Ingrese una opción: "))
            if 1 <= op <= 6:
                return op
            else:
                print("seleccione una opción válida")
        except:
            print("seleccione una opción válida")


def cupos_tipo(tipo):
    total = 0

    for codigo, datos in planes.items():
        if datos[1].lower() == tipo.lower():
            total += inscripciones[codigo][1]

    print("cupos disponibles:", total)


def busqueda_precio(p_min, p_max):
    lista = []

    for codigo, datos in inscripciones.items():
        precio = datos[0]
        cupos = datos[1]

        if p_min <= precio <= p_max and cupos > 0:
            nombre = planes[codigo][0]
            lista.append(nombre + "--" + codigo)

    if len(lista) == 0:
        print("No hay planes en ese rango de precios.")
    else:
        lista.sort()
        print("Los planes encontrados son:", lista)


def buscar_codigo(codigo):
    codigo = codigo.upper()

    for c in inscripciones:
        if c.upper() == codigo:
            return True

    return False


def actualizar_precio(codigo, nuevo_precio):
    codigo = codigo.upper()

    if buscar_codigo(codigo):
        inscripciones[codigo][0] = nuevo_precio
        return True

    return False


# ==========================
# VALIDACIONES
# ==========================

def validar_codigo(codigo):
    if codigo.strip() == "":
        return False

    if buscar_codigo(codigo):
        return False

    return True


def validar_nombre(nombre):
    return nombre.strip() != ""


def validar_tipo(tipo):
    return tipo.lower() in ["mensual", "trimestral", "anual"]


def validar_duracion(duracion):
    return duracion > 0


def validar_sn(valor):
    return valor.lower() in ["s", "n"]


def validar_horario(horario):
    return horario.strip() != ""


def validar_precio(precio):
    return precio > 0


def validar_cupos(cupos):
    return cupos >= 0


def agregar_plan(codigo, nombre, tipo, duracion,
                 acceso_piscina, incluye_clases,
                 horario, precio, cupos):

    if buscar_codigo(codigo):
        return False

    planes[codigo] = [
        nombre,
        tipo,
        duracion,
        acceso_piscina,
        incluye_clases,
        horario
    ]

    inscripciones[codigo] = [precio, cupos]

    return True


def eliminar_plan(codigo):
    codigo = codigo.upper()

    if buscar_codigo(codigo):
        del planes[codigo]
        del inscripciones[codigo]
        return True

    return False


# ==========================
# PROGRAMA PRINCIPAL
# ==========================

while True:

    print("\n========== MENÚ PRINCIPAL ==========")
    print("1. Cupos por tipo de plan")
    print("2. Búsqueda de planes por rango de precio")
    print("3. Actualizar precio de plan")
    print("4. Agregar plan")
    print("5. Eliminar plan")
    print("6. Salir")
    print("=====================================")

    opcion = leer_opcion()

    if opcion == 1:

        tipo = input("Ingrese tipo de plan a consultar: ")
        cupos_tipo(tipo)

    elif opcion == 2:

        while True:
            try:
                p_min = int(input("Ingrese precio mínimo: "))
                p_max = int(input("Ingrese precio máximo: "))

                if p_min >= 0 and p_max >= 0 and p_min <= p_max:
                    break

            except:
                print("Debe ingresar valores enteros")

        busqueda_precio(p_min, p_max)

    elif opcion == 3:

        while True:

            codigo = input("Ingrese código del plan: ").upper()

            while True:
                try:
                    nuevo = int(input("Ingrese nuevo precio: "))
                    if nuevo > 0:
                        break
                except:
                    pass

            if actualizar_precio(codigo, nuevo):
                print("Precio actualizado")
            else:
                print("El código no existe")

            seguir = input("¿Desea actualizar otro precio (s/n)?: ").lower()

            if seguir == "n":
                break

    elif opcion == 4:

        codigo = input("Ingrese código del plan: ").upper()

        nombre = input("Ingrese nombre del plan: ")

        tipo = input("Ingrese tipo (mensual/trimestral/anual): ").lower()

        try:
            duracion = int(input("Ingrese duración (meses): "))
        except:
            duracion = -1

        piscina = input("¿Incluye acceso a piscina? (s/n): ").lower()

        clases = input("¿Incluye clases grupales? (s/n): ").lower()

        horario = input("Ingrese horario: ")

        try:
            precio = int(input("Ingrese precio: "))
        except:
            precio = -1

        try:
            cupos = int(input("Ingrese cupos: "))
        except:
            cupos = -1

        if not validar_codigo(codigo):
            print("Código inválido o ya existe")

        elif not validar_nombre(nombre):
            print("Nombre inválido")

        elif not validar_tipo(tipo):
            print("Tipo inválido")

        elif not validar_duracion(duracion):
            print("Duración inválida")

        elif not validar_sn(piscina):
            print("Acceso piscina inválido")

        elif not validar_sn(clases):
            print("Clases inválidas")

        elif not validar_horario(horario):
            print("Horario inválido")

        elif not validar_precio(precio):
            print("Precio inválido")

        elif not validar_cupos(cupos):
            print("Cupos inválidos")

        else:

            acceso_piscina = piscina == "s"
            incluye_clases = clases == "s"

            if agregar_plan(
                codigo,
                nombre,
                tipo,
                duracion,
                acceso_piscina,
                incluye_clases,
                horario,
                precio,
                cupos
            ):
                print("Plan agregado")
            else:
                print("El código ya existe")

    elif opcion == 5:

        codigo = input("Ingrese código del plan: ").upper()

        if eliminar_plan(codigo):
            print("Plan eliminado")
        else:
            print("El código no existe")

    elif opcion == 6:

        print("Programa finalizado.")
        break
        
