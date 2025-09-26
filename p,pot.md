import tkinter as tk
import random

# --- Opciones y Reglas ---
OPCIONES = ["Piedra", "Papel", "Tijera", "Lagarto", "Spock"]

# CLAVE: Lo que gana, VALOR: Una lista con las opciones a las que gana.
REGLAS = {
    "Piedra": ["Tijera", "Lagarto"],
    "Papel": ["Piedra", "Spock"],
    "Tijera": ["Papel", "Lagarto"],
    "Lagarto": ["Spock", "Papel"],
    "Spock": ["Tijera", "Piedra"]
}

# --- Variables Globales para el Puntaje ---
# Definimos las variables de puntaje al inicio, como un programador
# las definiría para que sean accesibles desde cualquier función.
puntos_usuario = 0
puntos_cpu = 0
max_puntos = 3 # El primero que llegue a 3 gana la partida.

# --- Variables de Tkinter para la Interfaz ---
# Las variables para que el puntaje y el resultado se actualicen.
texto_resultado = None
texto_puntaje = None

# --- Función para actualizar el Puntaje en la Interfaz ---
def actualizar_puntaje_ui():
    """Actualiza el texto del marcador en la ventana."""
    global puntos_usuario, puntos_cpu
    texto_puntaje.set(f"Marcador: Tú {puntos_usuario} - CPU {puntos_cpu}")

# --- Función para reiniciar el juego completo ---
def reiniciar_juego():
    """Reinicia los puntajes y el mensaje."""
    global puntos_usuario, puntos_cpu
    puntos_usuario = 0
    puntos_cpu = 0
    actualizar_puntaje_ui()
    texto_resultado.set("¡Juego Reiniciado! Elige tu próxima opción:")
    habilitar_botones() # Aseguramos que se pueda volver a jugar

# --- Función para deshabilitar los botones al terminar la partida ---
def deshabilitar_botones():
    """Bloquea los botones cuando hay un ganador final."""
    for widget in frame_botones.winfo_children():
        widget.config(state=tk.DISABLED) # Deshabilita todos los botones de opción
    # El botón de reiniciar debe estar habilitado para volver a empezar
    boton_reiniciar.config(state=tk.NORMAL)

# --- Función para habilitar los botones ---
def habilitar_botones():
    """Habilita los botones de opción."""
    for widget in frame_botones.winfo_children():
        widget.config(state=tk.NORMAL)
    # El botón de reiniciar puede estar deshabilitado al inicio de una ronda
    boton_reiniciar.config(state=tk.DISABLED)

# --- Función Principal del Juego ---
def jugar(eleccion_usuario):
    global puntos_usuario, puntos_cpu

    # Si ya hay un ganador final, salimos de la función.
    if puntos_usuario >= max_puntos or puntos_cpu >= max_puntos:
        return

    eleccion_cpu = random.choice(OPCIONES)
    print(f"Usuario elige: {eleccion_usuario}, CPU elige: {eleccion_cpu}") # Para depuración

    # 1. Caso de EMPATE
    if eleccion_usuario == eleccion_cpu:
        texto_resultado.set(f"¡Empate! Ambos eligieron {eleccion_usuario}")

    # 2. Caso de VICTORIA (Ganar)
    elif eleccion_cpu in REGLAS[eleccion_usuario]:
        puntos_usuario += 1 # Suma un punto al usuario
        texto_resultado.set(f"¡Ganaste la Ronda! {eleccion_usuario} vence a {eleccion_cpu}")

    # 3. Caso de DERROTA (Perder)
    else:
        puntos_cpu += 1 # Suma un punto a la CPU
        texto_resultado.set(f"¡Perdiste la Ronda! {eleccion_cpu} vence a {eleccion_usuario}")

    # Siempre actualizamos el marcador después de la ronda
    actualizar_puntaje_ui()

    # 4. Revisar si hay un GANADOR FINAL
    if puntos_usuario >= max_puntos:
        texto_resultado.set("¡¡FELICIDADES!! ¡Has ganado la partida!")
        deshabilitar_botones()
    elif puntos_cpu >= max_puntos:
        texto_resultado.set("¡Juego Terminado! La CPU ganó la partida.")
        deshabilitar_botones()


# --- Configuración de la Ventana Principal con Tkinter ---

def configurar_interfaz():
    global texto_resultado, texto_puntaje, frame_botones, boton_reiniciar

    ventana_principal = tk.Tk()
    ventana_principal.title("Piedra, Papel, Tijera, Lagarto, Spock")

    # Marco para el título y el puntaje
    frame_superior = tk.Frame(ventana_principal)
    frame_superior.pack(pady=10)

    # 1. Marcador de Puntaje
    texto_puntaje = tk.StringVar()
    tk.Label(
        frame_superior,
        textvariable=texto_puntaje,
        font=("Arial", 14)
    ).pack()
    actualizar_puntaje_ui() # Inicializa el texto

    # 2. Etiqueta para el resultado
    texto_resultado = tk.StringVar()
    texto_resultado.set(f"El primero en llegar a {max_puntos} gana la partida:")
    tk.Label(
        ventana_principal,
        textvariable=texto_resultado,
        font=("Arial", 16, "bold"),
        fg="darkblue"
    ).pack(pady=20)


    # 3. Contenedor para los botones de opción
    frame_botones = tk.Frame(ventana_principal)
    frame_botones.pack(pady=10)

    # Creación de los Botones de Opción
    for opcion in OPCIONES:
        tk.Button(
            frame_botones,
            text=opcion,
            width=12,
            height=2,
            bg="lightgray",
            command=lambda o=opcion: jugar(o)
        ).pack(side=tk.LEFT, padx=5, pady=5)

    # 4. Botón de Reiniciar
    # Lo ponemos aparte para que se vea como un botón de función.
    boton_reiniciar = tk.Button(
        ventana_principal,
        text="Reiniciar Juego",
        width=18,
        height=2,
        bg="lightblue", # Un color diferente para destacarlo
        command=reiniciar_juego
    )
    boton_reiniciar.pack(pady=20)
    # Al inicio de la aplicación, este botón debe estar deshabilitado
    # hasta que alguien gane para forzar al jugador a jugar una ronda completa.
    boton_reiniciar.config(state=tk.DISABLED)


    ventana_principal.mainloop()

# Ejecutamos la configuración de la interfaz
configurar_interfaz()
