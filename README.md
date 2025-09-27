
import random




class Entrenador:
    def __init__(self, nombre: str):
        self.nombre = nombre


class Pokemon:
    def __init__(self, nombre: str):
        self.nombre = nombre
        self.vida = random.randint(150, 400)
        self.ataque = random.randint(20, 100)
        self.vida_actual = self.vida

    def recuperar(self):
        """Restaura la vida actual a la máxima"""
        self.vida_actual = self.vida



e1, p1 = None, None
e2, p2 = None, None
ganadas = 0
perdidas = 0



def crearEntrenadorPokemon(jugador: int):
    
    global e1, p1, e2, p2

    nombre_entrenador = input(f"Ingrese el nombre del entrenador {jugador}: ")
    nombre_pokemon = input(f"Ingrese el nombre del Pokémon de {nombre_entrenador}: ")

    entrenador = Entrenador(nombre_entrenador)
    pokemon = Pokemon(nombre_pokemon)

    if jugador == 1:
        e1, p1 = entrenador, pokemon
    else:
        e2, p2 = entrenador, pokemon

    print("\n=== Entrenador y Pokémon creados ===")
    print(f"Entrenador: {entrenador.nombre}")
    print(f"Pokémon: {pokemon.nombre} | Vida: {pokemon.vida} | Ataque: {pokemon.ataque}\n")


def valorDeAtaque(jugador: int) -> int:
    
    global p1, p2
    if jugador == 1:
        return random.randint(0, p1.ataque)
    else:
        return random.randint(0, p2.ataque)


def defender(jugador: int, ataque: int) -> int:
    
    global p1, p2

    
    dado = random.randint(1, 6)
    if dado == 6:
        ataque = 0

    if jugador == 1:
        p1.vida_actual -= ataque
        return p1.vida_actual
    else:
        p2.vida_actual -= ataque
        return p2.vida_actual



# PROGRAMA PRINCIPAL

if __name__ == "__main__":

    
    crearEntrenadorPokemon(1)

    while True:
        opcion = input("¿Desea Pelear (P) o Finalizar (F)? ").upper()

        if opcion == "F":
            print("\n=== FIN DEL JUEGO ===")
            print(f"Entrenador: {e1.nombre}")
            print(f"Pokémon: {p1.nombre}")
            print(f"Vida máxima: {p1.vida} | Ataque: {p1.ataque}")
            print(f"Batallas ganadas: {ganadas}")
            print(f"Batallas perdidas: {perdidas}")
            break

        elif opcion == "P":
            
            p1.recuperar()
 
            crearEntrenadorPokemon(2)
            p2.recuperar()

            turno = 1  
            while p1.vida_actual > 0 and p2.vida_actual > 0:
                if turno == 1:
                    ataque = valorDeAtaque(1)
                    vida_restante = defender(2, ataque)
                    print(f"{e1.nombre} ({p1.nombre}) ataca con {ataque} → "
                          f"{e2.nombre} ({p2.nombre}) queda con {vida_restante} de vida.")
                    turno = 2
                else:
                    ataque = valorDeAtaque(2)
                    vida_restante = defender(1, ataque)
                    print(f"{e2.nombre} ({p2.nombre}) ataca con {ataque} → "
                          f"{e1.nombre} ({p1.nombre}) queda con {vida_restante} de vida.")
                    turno = 1

            # Resultado de la pelea
            if p1.vida_actual <= 0 and p2.vida_actual <= 0:
                print("\n¡Es un empate!")
            elif p2.vida_actual <= 0:
                print(f"\n{e1.nombre} y su Pokémon {p1.nombre} GANAN la batalla!")
                ganadas += 1
            else:
                print(f"\n{e2.nombre} y su Pokémon {p2.nombre} GANAN la batalla!")
                perdidas += 1

            print("=================================\n")

        else:
            print("Opción inválida. Elija P o F.\n")
