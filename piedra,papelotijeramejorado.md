import tkinter as tk
import random

# --- 1. CONFIGURACIÓN DEL JUEGO (9 Opciones) ---

OPCIONES = ["Piedra", "Papel", "Tijera", "Lagarto", "Spock", "Papa", "Pirata", "Heavy Metal", "Robot"]

# CLAVE: Lo que gana, VALOR: Una lista con las opciones a las que gana.
REGLAS = {
    "Piedra": ["Tijera", "Lagarto", "Papa", "Heavy Metal"],
    "Papel": ["Piedra", "Spock", "Pirata", "Robot"],
    "Tijera": ["Papel", "Lagarto", "Heavy Metal", "Papa"],
    "Lagarto": ["Spock", "Papel", "Papa", "Robot"],
    "Spock": ["Tijera", "Piedra", "Pirata", "Heavy Metal"],
    
    "Papa": ["Spock", "Tijera", "Piedra", "Robot"],
    "Pirata": ["Piedra", "Lagarto", "Heavy Metal", "Papa"],
    "Heavy Metal": ["Papel", "Papa", "Lagarto", "Tijera"],
    "Robot": ["Spock", "Papel", "Pirata", "Piedra"],
}

# --- 2. VARIABLES DE ESTADO Y TKINTER ---

# Variables Globales para el Puntaje
puntos_usuario = 0
puntos_cpu = 0
max_puntos = 3 

# Variables de la Interfaz (Inicializadas como None para asignarles el widget después)
texto_resultado = None
texto_puntaje = None
frame_botones = None
boton_reiniciar = None


# --- 3. FUNCIONES DE MANEJO DE ESTADO Y LÓGICA ---

def actualizar_puntaje_ui():
    """Actualiza el texto del marcador en la ventana."""
    global puntos_usuario, puntos_cpu, texto_puntaje
    texto_puntaje.set(f"Marcador: Tú {puntos_usuario} - CPU {puntos_cpu}")

def deshabilitar_botones():
    """Bloquea los botones cuando hay un ganador final."""
    global frame_botones, boton_reiniciar
    # Deshabilita todos los botones de opción
    for widget in frame_botones.winfo_children():
        widget.config(state=tk.DISABLED) 
    # Habilita el botón de reiniciar
    boton_reiniciar.config(state=tk.NORMAL) 

def habilitar_botones():
    """Habilita los botones de opción."""
    global frame_botones, boton_reiniciar
    # Habilita todos los botones de opción
    for widget in frame_botones.winfo_children():
        widget.config(state=tk.NORMAL)
    # Deshabilita el botón de reiniciar
    boton_reiniciar.config(state=tk.DISABLED) 
    
def reiniciar_juego():
    """Reinicia los puntajes y el mensaje."""
    global puntos_usuario, puntos_cpu
    puntos_usuario = 0
    puntos_cpu = 0
    actualizar_puntaje_ui() 
    texto_resultado.set("¡Juego Reiniciado! Elige tu próxima opción:")
    habilitar_botones()

def jugar(eleccion_usuario):
    """Lógica principal del juego."""
    global puntos_usuario, puntos_cpu, max_puntos, texto_resultado

    # Si ya hay un ganador final, ignora la jugada.
    if puntos_usuario >= max_puntos or puntos_cpu >= max_puntos:
        return

    eleccion_cpu = random.choice(OPCIONES)
    
    # 1. Caso de EMPATE
    if eleccion_usuario == eleccion_cpu:
        texto_resultado.set(f"¡Empate! Ambos eligieron {eleccion_usuario}")

    # 2. Caso de VICTORIA (Ganar)
    elif eleccion_cpu in REGLAS[eleccion_usuario]:
        puntos_usuario += 1 
        texto_resultado.set(f"¡Ganaste la Ronda! {eleccion_usuario} vence a {eleccion_cpu}")

    # 3. Caso de DERROTA (Perder)
    else:
        puntos_cpu += 1 
        texto_resultado.set(f"¡Perdiste la Ronda! {eleccion_cpu} vence a {eleccion_usuario}")

    actualizar_puntaje_ui()

    # 4. Revisar si hay un GANADOR FINAL
    if puntos_usuario >= max_puntos:
        texto_resultado.set("🥳 ¡¡FELICIDADES!! ¡Has ganado la partida!")
        deshabilitar_botones()
    elif puntos_cpu >= max_puntos:
        texto_resultado.set("🤖 ¡Juego Terminado! La CPU ganó la partida.")
        deshabilitar_botones()


# --- 4. CONFIGURACIÓN DE LA INTERFAZ ---

def configurar_interfaz():
    global texto_resultado, texto_puntaje, frame_botones, boton_reiniciar, max_puntos

    ventana_principal = tk.Tk()
    ventana_principal.title("Súper Piedra, Papel, Tijera... (9 Opciones)")

    # Marco para el título y el puntaje
    frame_superior = tk.Frame(ventana_principal)
    frame_superior.pack(pady=10)

    # 1. Marcador de Puntaje
    global texto_puntaje 
    texto_puntaje = tk.StringVar()
    tk.Label(frame_superior, textvariable=texto_puntaje, font=("Arial", 14)).pack()
    actualizar_puntaje_ui() 

    # 2. Etiqueta para el resultado
    global texto_resultado 
    texto_resultado = tk.StringVar()
    texto_resultado.set(f"El primero en llegar a {max_puntos} gana la partida:")
    tk.Label(
        ventana_principal,
        textvariable=texto_resultado,
        font=("Arial", 16, "bold"),
        fg="#0A7E8C" # Color azul oscuro
    ).pack(pady=20)

    # 3. Contenedor para los botones de opción
    global frame_botones 
    frame_botones = tk.Frame(ventana_principal)
    frame_botones.pack(pady=10, padx=10)

    # Creación de los 9 Botones de Opción (distribuidos en 2 filas)
    for i, opcion in enumerate(OPCIONES):
        tk.Button(
            frame_botones,
            text=opcion,
            width=10,
            height=2,
            bg="#DDEEFF", # Un color de fondo suave
            command=lambda o=opcion: jugar(o)
        ).grid(row=i // 5, column=i % 5, padx=3, pady=3) # Filas de 5 y 4

    # 4. Botón de Reiniciar
    global boton_reiniciar
    boton_reiniciar = tk.Button(
        ventana_principal,
        text="Reiniciar Juego",
        width=18,
        height=2,
        bg="#FFDDCC", # Color para destacarlo
        command=reiniciar_juego
    )
    boton_reiniciar.pack(pady=20)
    boton_reiniciar.config(state=tk.DISABLED) # Deshabilitado al inicio

    ventana_principal.mainloop()

# --- 5. EJECUCIÓN ---

if __name__ == "__main__":
    configurar_interfaz()
