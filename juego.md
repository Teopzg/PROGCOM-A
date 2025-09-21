import random

# Diccionario de departamentos con pistas más detalladas
dep_pistas = {
    "Amazonas": "Soy la puerta de entrada a la selva más grande del mundo en Colombia, mi capital es la 'Isla de la Aventura'.",
    "Antioquia": "Soy conocido como la 'Montaña de Oro' y mi capital es la 'Ciudad de la Eterna Primavera'.",
    "Arauca": "Soy una tierra de llanos y soy la 'Puerta de Oro de los Llanos Orientales'.",
    "Atlántico": "Soy el hogar de uno de los carnavales más grandes del mundo y mi capital es el 'Pórtico de Oro de Colombia'.",
    "Bolívar": "Soy el 'Corralito de Piedra' y mi capital es un 'Patrimonio de la Humanidad'.",
    "Boyacá": "Soy la 'Cuna de la Libertad' y mi capital es la 'Ciudad del Milagro'.",
    "Caldas": "Soy el corazón del 'Eje Cafetero' y tengo un nevado que domina el paisaje.",
    "Caquetá": "Soy una región de selva exuberante y mi capital es la 'Puerta de Oro de la Amazonia'.",
    "Casanare": "Soy el centro de la 'Gran Sabana' colombiana, con mucha fauna silvestre.",
    "Cauca": "Soy una tierra de volcanes y tengo una ciudad blanca conocida por su arquitectura colonial.",
    "Cesar": "Soy la tierra del 'Vallenato' y mi capital es la 'Ciudad de los Santos Reyes del Valle del Cacique'.",
    "Chocó": "Soy el único departamento con costas en ambos océanos y soy la región más biodiversa.",
    "Córdoba": "Soy una tierra de sabanas, ganado y gente musical con acento costeño.",
    "Cundinamarca": "Soy el departamento que rodea a la capital del país, y tengo un parque de la sal asombroso.",
    "Guainía": "Mi capital es la 'Flor de la Amazonía' y soy conocido por los cerros de Mavecure.",
    "Guaviare": "Soy una tierra de pinturas rupestres y un río colorado que parece de otro mundo.",
    "Huila": "Tengo el desierto más grande de Colombia, y mi capital se alza a orillas del río Magdalena.",
    "La Guajira": "Soy el departamento del sol, la sal y las rancherías de la cultura Wayúu.",
    "Magdalena": "Tengo la 'Perla de América' y una sierra nevada que es hogar de comunidades indígenas.",
    "Meta": "Soy la 'Puerta del Llano' y mi capital es famosa por su coleo y arpas.",
    "Nariño": "Soy una región de artesanos y volcanes, con un santuario en un puente majestuoso.",
    "Norte de Santander": "Soy la tierra de la 'Batalla de Cúcuta' y mi capital es una ciudad de 'árboles de manga'.",
    "Putumayo": "Soy una región de cascadas y naturaleza vibrante, con un río que me da el nombre.",
    "Quindío": "Soy el departamento más pequeño de la región cafetera y mi capital es la 'Ciudad Milagro'.",
    "Risaralda": "Soy una tierra de puentes colgantes, y mi capital es la 'Perla del Otún'.",
    "San Andrés y Providencia": "Soy una isla en el Caribe, con aguas de 'siete colores' y cultura 'rasta'.",
    "Santander": "Soy la tierra del 'Cañón del Chicamocha' y mi gente es conocida por ser berraca.",
    "Sucre": "Soy una tierra de sabanas y mi capital tiene un festival de acordeones y corralejas.",
    "Tolima": "Soy el corazón musical de Colombia y mi capital es conocida como la 'Ciudad de los Parques'.",
    "Valle del Cauca": "Soy la 'capital de la salsa' y mi río le da el nombre a mi capital.",
    "Vaupés": "Soy una región de selva con un nombre que significa 'río del arcoíris' en la lengua local.",
    "Vichada": "Soy una tierra de llanos y mi capital se encuentra a orillas del río Orinoco."
}

depdis = list(dep_pistas.keys())
puntuacion = 0

print("¡Bienvenido al Desafío de los 32 Departamentos de Colombia! ")
print("Te daré una pista y tu debes adivinar a qué departamento me refiero.")
print("Veamos que tan conocedor eres ¡good luck!")

while depdis:
    dep_sec = random.choice(depdis)
    pista = dep_pistas[dep_sec]

    print("\n----------------------------------------------------")
    print(f"Pista: {pista}")

    respuesta = input("¿A qué departamento me refiero? (O escribe 'salir' para terminar): ").capitalize()

    if respuesta.lower() == 'salir':
        print(f"\n¡Gracias por jugar! Tu puntuación final es: {puntuacion} de {len(depdis)}.")
        print("¡Hasta la próxima, colega!")
        break

    if respuesta == dep_sec:
       puntuacion += 1
        print(f"¡Correcto! ¡Vas bien!  Llevas {puntuacion} puntos.")
        depdis.remove(dep_sec)

    if not depdis:
       print(f"\n¡Lo has logrado! ¡Has adivinado todos los departamentos! Tu puntuación final es de {puntuacion}.")
        print("¡Eres un verdadero conocedor de Colombia! ¡Congrats! ")

    else:
        print(f"¡Oops, respuesta incorrecta! La respuesta correcta era: {dep_sec}.")
        print("¡No te preocupes! ¡Aún quedan más oportunidades para intentarlo! ")

if not depdis and puntuacion == len(dep_pistas):
    print("\n¡Misión cumplida! ¡Has dominado la geografía de Colombia!")

