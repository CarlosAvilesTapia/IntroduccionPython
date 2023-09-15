# **Introducción a Python**
## Máquina Enigma
¡Bienvenido Agente! Nos encontramos en pleno conflicto y hemos encontrado los planos de una  [Máquina Enigma](https://es.wikipedia.org/wiki/Enigma_(máquina)) enemiga. Necesitamos que configures una máquina que nos ayude a descifrar los mensajes que interceptamos.
La máquina posee tres rotores y un reflector. Cada rotor es un disco circular plano con 26 contactos eléctricos en cada cara, uno por cada letra del alfabeto. Cada contacto de una cara está conectado o cableado a un contacto diferente de la cara contraria. Por ejemplo, en un rotor en particular, el contacto número 1 de una cara puede estar conectado con el contacto número 14 en la otra cara y el contacto número 5 de una cara con el número 22 de la otra.

Como cada rotor está en contacto con el otro, lo anterior permite ir cambiando el indice de las letras de entrada, configurando así una criptografía muy compleja de resolver en aquellos tiempos. Estos rotores se conocen como.

* Rotor derecho
* Rotor medio
* Rotor izquierdo
* Reflector

Mira este video para que entiendas mejor el concepto le la [Máquina Enigma](https://youtu.be/VnsTHAH5yAE).

## Funcionamiento general
Cada vez que un usuario presiona una tecla en el Teclado (columna derecha del diagrama anterior), ocurre lo siguiente:
1. El Rotor_derecho avanza una posición hacia arriba, i.e., la combinación 'A - B' pasa al final de la lista y el primer lugar lo ocupa 'B - D'
2. Se obtiene la posición en la lista (Teclado) de la tecla presionada por el operador .
3. La posición obtenida en el punto 2, se utiliza para buscar la letra en la sección derecha del Rotor_derecho que está en la misma posición (frente a frente). En esa posición existe una letra a la cual llamaremos Letra_entrada.
4. En el Rotor_derecho se busca la posición de la Letra_entrada en la sección de salida. Esta será la posición de salida del rotor.
5. Se repiten los pasos anteriores, 3 y 4, en el Rotor_medio; esta vez la posición de entrada es equivalente a la posición de salida del punto 4.
6. Se repiten los pasos anteriores, 3 y 4, en el Rotor_izquierdo.
7. Con la posición de salida del Rotor_izquierdo se entra en el Reflector. En la posición de entrada en el Reflector hay una letra. Se buscará entonces la otra letra equivalente dentro del Reflector. Esto determinará la posición de salida.
8. Con esta posición (la de salida del Reflector) se invierte el proceso, es decir, se busca la letra que está en contacto con el rotor anterior y se busca la posición de dicha letra en la salida del rotor. Este proceso se repite sucesivamente con los rotores izquierdo, medio y derecho.
9. La posición de salida del Rotor_derecho marcará la posición en el Teclado, indicando la letra encriptada.

10. ## Estructura de los rotores

Los tres rotores (izquierdo, medio y derecho) tienen el alfabeto de 26 letras ordenadas en su sección de entrada y las mismas 26 letras desordenadas en su sección de salida.<br>
El reflector, posee solo 13 letra, las cuales están repetidas 2 veces cada una y están repartidas aleatoriamente en el dispositivo. El punto en donde la señal del rotor izquierdo pasa al reflector, determina la letra de entrada; la salida será por la letra que conforma la pareja.

Esta es la configuración que hemos encontrado:

```python
"""
Reflector  Rot_izd   Rot_med   Rot_der  Teclado
    A       A - E     A - A     A - B      A
    B       B - K     B - J     B - D      B
    C       C - M     C - D     C - F      C
    D       D - F     D - K     D - H      D
    E       E - L     E - S     E - J      E
    F       F - G     F - I     F - L      F
    G       G - D     G - R     G - C      G
    D       H - Q     H - U     H - P      H
    I       I - V     I - X     I - R      I
    J       J - Z     J - B     J - T      J
    K       K - N     K - L     K - X      K
    G       L - T     L - H     L - V      L
    M       M - O     M - W     M - Z      M
    K       N - W     N - T     N - N      N
    M       O - Y     O - M     O - Y      O
    I       P - H     P - C     P - E      P
    E       Q - X     Q - Q     Q - I      Q
    B       R - U     R - G     R - W      R
    F       S - S     S - Z     S - G      S
    T       T - P     T - N     T - A      T
    C       U - A     U - P     U - K      U
    V       V - I     V - Y     V - M      V
    V       W - B     W - F     W - U      W
    J       X - R     X - V     X - S      X
    A       Y - C     Y - O     Y - Q      Y
    T       Z - J     Z - E     Z - O      Z
"""
```
## Algunos detalles adicionales
Cada vez que procesamos una letra, primero rotamos una posición el Rotor_derecho (hacia arriba).
Cuando la letra "V" de la componente ordenada (izquierda) del Rotor_derecho alcanza la posición inicial en la lista, en el siguiente movimiento, arrastrará al Rotor_medio, haciéndolo girar una posición. Esto se debe a que la máquina es un dispositivo electro-mecánco.
Lo mismo ocurre cuando la letra "Q" de la componente ordenada (izquierda) del Rotor_medio llega al inicio, en este caso, el el siguiente movimiento, hará  avanzar una posición hacia arriba al Rotor_izquierdo.

## Más detalles
Esto no termina aquí. Para hacer más difícil el trabajo de desencriptación, la máquina tiene la posibilidad de fijar la posición inicial de los tres rotores centrales (izquierdo, medio y derecho). Para esto se elige una clave de tres letras, las cuales marcan la posición inicial de la primera letra de la sección ordenada (izquierda) de cada rotor. Por ejemplo la clave <font color='red'>'MCK'</font> dejaría los rotores de la siguiente forma:

```Python
"""
Reflector  Rot_izd   Rot_med   Rot_der  Teclado
    A       M - O     C - D     K - X      A
    B       N - W     D - K     L - V      B
    C       O - Y     E - S     M - Z      C
    D       P - H     F - I     N - N      D
    E       Q - X     G - R     O - Y      E
    F       R - U     H - U     P - E      F
    G       S - S     I - X     Q - I      G
    D       T - P     J - B     R - W      H
    I       U - A     K - L     S - G      I
    J       V - I     L - H     T - A      J
    K       W - B     M - W     U - K      K
    G       X - R     N - T     V - M      L
    M       Y - C     O - M     W - U      M
    K       Z - J     P - C     X - S      N
    M       A - E     Q - Q     Y - Q      O
    I       B - K     R - G     Z - O      P
    E       C - M     S - Z     A - B      Q
    B       D - F     T - N     B - D      R
    F       E - L     U - P     C - F      S
    T       F - G     V - Y     D - H      T
    C       G - D     W - F     E - J      U
    V       H - Q     X - V     F - L      V
    V       I - V     Y - O     G - C      W
    J       J - Z     Z - E     H - P      X
    A       K - N     A - A     I - R      Y
    T       L - T     B - J     J - T      Z
"""
```
Fíjate que ni el Teclado ni el Reflector cambian, solo los rotores izquierdo, medio y derecho.
