# rowing-optimal-cinematic
Analisis del movimiento del remero en la fase aerea de recuperación segun la dinámica del sistema fisico remero+barco teniendo en cuenta la fuerza de arrastre del agua

La fase aerea en el remo de banco móvil consiste en un desplazamiento del remero hacia popa de su barco para recuperar una posicion encogida para poder volver a dar una palada y atacar. La correcta realizacion de esta fase (entre otros aspectos) diferencia un mejor de un peor remero. 

Es de especial relevancia la cinemática que sigue el remero a la hora de desplazarse hacia popa ya que es el momento en el que el barco va más rápido relativo al agua. Como cabe esperar los desplzamiento del remero sobre el barco afectan su desplazamiento relativo al agua. La intuición de los que saben refiere a un desplazamiento sin brusquedades. 
Recordar que la pérdida de velocidad (y potencia) en forma de rozamiento con el agua depende de la velocidad del barco, de ahí que la habilidad del remero en el desplazamiento a popa sea clave. Esto es posible "matematizarlo".

Considerar el sistema embarcacion+remero sobre el agua. Donde

$m$ es la masa del remero
$M$ es la masa del barco
$v_{cm}$ es la velocidad del centro de masas del sistema remero+barco
$v_b$ es la velocidad del barco relativa al agua
$v_r$ es la velocidad del remero relativo al barco

Notese que las velocidades pueden ser referidas como $v = \frac{dx}{dt} = \dot{x}$.

El rozamiento a través de un fluido puede modelarse como $\propto v^2$ tal que

$$F_d = \frac{1}{2}C_s \rho A v_b^2$$

El momento lineal del sistema remero+barco toma la forma:

$$p = (M+m)v_{cm}= (M+m)v_b + m v_r$$

Recordando que

$$\frac{dp}{dt} = \sum_i F_i$$

se tiene

$$\frac{dp}{dt} = F_d = (M+m)\frac{dv_b}{dt} + m \frac{dv_r}{dt}$$


Teniendo en cuenta la modelización de la fuerza de rozamiento:

$$- \frac{1}{2}C_s \rho A v_b^2 = (M+m)\frac{dv_b}{dt} + m \frac{dv_r}{dt}$$

Esto nos deja una EDO cuya función incógnita es la velocidad del barco, $v_b$, para una cinemática de el remero $v_r$ dada. Esta es:

$$\frac{dv_b}{dt} = \frac{-1}{M+m}\left( m\frac{dv_r}{dt}- \frac{1}{2}C_p\rho A v_b^2 \right)$$


Resolver esta EDo daría la velocidad del barco en función del tiempo teniendo en cuenta el movimiento del remero. Ahora bien, el movimiento del remero debe plantearse (como trabajo a futuro plantear el problema de optimización de la cinemática de desplazamiento óptima para minimizar pérdida de velocidad del barco). Por el momento es posible plantear una funcion de posicion, velocidad y aceleracion del remero con suficiente libertad que cumpla unas condiciones dadas. 
- El ciclo de desplazamineto sera un periodo $t \in [0, T]$.
- La posicion del remero al inicio debe ser $0$ y al final $$L$ fin de ciclo es, implica $x(t=0)=0$ y $x(T)=L$.
- La velocidad debe ser nula al inicio y fin del ciclo, es decir, $\dot{x}(t=0) = \dot{x}(t=T) = 0$

Con estas condiciones puede resolverse la EDO de la cinematica propuesta con ciertos parametros libres que seran parametro a ajustar posteriormente $a,b,c,d$ aunque exigiendo las condiciones de frontera solo habra dos grados de libertad o parámetros libres $a,b$.

$$\ddot{x}(t) = at^3+bt^2+ct+d $$

Resolviendo para la velocidad:
$$
\dot{x}(t) = \int \left( a t^3 + b t^2 + c t + d \right) \, dt
$$
$$
\dot{x}(t) = \frac{a}{4} t^4 + \frac{b}{3} t^3 + \frac{c}{2} t^2 + d t + C_1
$$
Resolviendo para la posicion

$$
x(t) = \int \left( \frac{a}{4} t^4 + \frac{b}{3} t^3 + \frac{c}{2} t^2 + d t + C_1 \right) \, dt
$$
$$
x(t) = \frac{a}{20} t^5 + \frac{b}{12} t^4 + \frac{c}{6} t^3 + \frac{d}{2} t^2 + C_1 t + C_2
$$

Aplicando las condiciones de contorno:

$$
\dot{x}(t) = \frac{a}{4} t^4 + \frac{b}{3} t^3 + \frac{c}{2} t^2 + d t
$$
$$
x(t) = \frac{a}{20} t^5 + \frac{b}{12} t^4 + \frac{c}{6} t^3 + \frac{d}{2} t^2
$$

donde los parámetro a,b,c,d estan restringidos por estas dos igualdades venidas de las condiciones de contorno:

$$
0 = \frac{a}{4}T^3+\frac{b}{3}T^2+\frac{c}{2}T+d
$$

$$
L = \frac{a}{20}T^5+\frac{b}{12}T^4+\frac{c}{6}T^3+\frac{d}{2}T^2
$$

Dando lugar a 
$$
\begin{cases}
a = a \\
b = b \\
c = -\left( \frac{12 L}{T^3} + \frac{9}{10} T^2 a + T b \right) \\
d = -\left( \frac{T^3}{4} a + \frac{T^2}{3} b + \frac{c T}{2} \right)
\end{cases}
$$

Como se prueba en este notebook, cualquier valor de $a,b$ dan lugar a soluciones cinematicas "ranzonables" para el desplazamiento del remero.

![Texto alternativo](figures/cinematics2.png)
![Texto alternativo](figures/cinematics3.png)




Con los coeficientes del polinomio de $\ddot{x(t)}$ podemos sustituirlo en la EDO de la dinámica global $\frac{dv_b}{dt}$.

$$\frac{dv_b}{dt} = \frac{1}{M+m}\left(\frac{1}{2}C_p\rho A v_b^2 - m(at^3+bt^2+ct+d) \right)$$
