# DenizALejandro_BLACKJACK

import random

def generar_mazo():
    numeros = ['2', '3', '4', '5', '6', '7', '8', '9', '10', 'J', 'Q', 'K', 'A']
    palos = ['Corazones', 'Diamantes', 'Treboles', 'Picas']
    mazo = [{'numero': numero, 'palo': palo} for numero in numeros for palo in palos]
    random.shuffle(mazo)
    return mazo

def valor_carta(carta):
    if carta['numero'] in ['J', 'Q', 'K']:
        return 10
    elif carta['numero'] == 'A':
        return 11  # El valor de 'A' puede ser 1 u 11, se ajustará más adelante según la situación
    else:
        return int(carta['numero'])

def ajustar_valor_ases(mano, total):
    for carta in mano:
        if carta['numero'] == 'A' and total > 21:
            total -= 10  # Cambia el valor de 'A' de 11 a 1
    return total

def mostrar_mano(mano, ocultar_primera_carta=False):
    print("Mano:")
    for i, carta in enumerate(mano):
        if i == 0 and ocultar_primera_carta:
            print("Carta oculta")
        else:
            print(f"{carta['numero']} de {carta['palo']}")

def blackjack():
    mazo = generar_mazo()
    mano_jugador = [mazo.pop(), mazo.pop()]
    mano_dealer = [mazo.pop(), mazo.pop()]

    while True:
        total_jugador = sum(valor_carta(carta) for carta in mano_jugador)
        total_jugador = ajustar_valor_ases(mano_jugador, total_jugador)

        total_dealer = sum(valor_carta(carta) for carta in mano_dealer)
        total_dealer = ajustar_valor_ases(mano_dealer, total_dealer)

        mostrar_mano(mano_jugador)
        print(f"Total jugador: {total_jugador}")

        mostrar_mano(mano_dealer, ocultar_primera_carta=True)

        if total_jugador == 21:
            print("¡Blackjack! ¡Ganaste!")
            break
        elif total_jugador > 21:
            print("¡Te has pasado! Pierdes.")
            break

        accion = input("¿Quieres 'pedir' o 'plantar'? ").lower()

        if accion == 'pedir':
            mano_jugador.append(mazo.pop())
        elif accion == 'plantar':
            while total_dealer < 17:
                mano_dealer.append(mazo.pop())
                total_dealer = sum(valor_carta(carta) for carta in mano_dealer)
                total_dealer = ajustar_valor_ases(mano_dealer, total_dealer)

            mostrar_mano(mano_dealer)

            if total_dealer > 21:
                print("¡Dealer se pasó! ¡Ganaste!")
            elif total_jugador > total_dealer:
                print("¡Ganaste!")
            elif total_jugador < total_dealer:
                print("¡Perdiste!")
            else:
                print("Empate.")
            
            break
        else:
            print("Opción no válida. Intenta de nuevo.")

if __name__ == "__main__":
    print("¡Bienvenido a Blackjack!")
    blackjack()
