## Part d'ampliació. Projecte. (70%)

En esta segona part pots escollir entre dues opcions:

1. **Primera opció**. Amplia la funcionalitat del joc dels avions:

    1. La primera ampliació proposada és un **sistema de puntuacions** que serà visible a la pantalla del joc. Cada vegada que s'esquive un míssil i aquest sobrepasse la part esquerra de la pantalla, es sumaran 10 punts al marcador.
    2. La segona ampliació serà que el joc anirà canviant cada 20 segons entre **el dia i la nit**, és a dir, el fons canviarà del blau del dia al negre de la nit automàticament.
    3. La tercera ampliació serà un **sistema de nivells**. Cada vegada que el jugador supere un múltiple de 500 punts, el joc canviarà a un nivell superior, començant la partida en el nivell 1. A més, el nivell serà visible en la pantalla, al costat de la puntuació.
    4. La quarta ampliació serà que la **dificultat** del joc anirà creixent amb els nivells. Al principi del joc, es crearan enemics cada 500ms i la seua velocitat serà un número aleatori entre 1 i 10. En el segon nivell, la velocitat de creació seran 450 ms i les velocitats aniran entre 3 i 12. La idea és parametritzar els valors segons el nivell, per exemple, per a la velocitat de creació podria ser:
   
        $$v_c =  100 + (450 - 50 * nivell)$$

        $$v_e = random(2 * nivell, 10 + 3 * nivell)$$

        > On $v_c$ és la velocitat de creació d'enemics  
        > On $v_e$ és la velocitat de desplaçament dels enemics

    5. Fes que la puntuació, en cas de ser un nou rècord, es guarde en un document de text anomenat **punt_max.txt** en la mateixa carpeta on està el codi del joc. Per a saber si és un nou rècord, quan es carrega el joc, haurà de llegir el document i extraure la puntuació màxima.
    6. Afegeix una **pantalla de benvinguda** on es done la benvinguda al joc. A més, ha de mostrar el rècord fins al moment i esperar fins que es polse la tecla p, moment en que es passarà a la pantalla de joc.
    7. Fes una **pantalla final** on s'indique que la partida ha finalitzat, mostre la puntuació i el nivell al que s'ha arribat, i en cas de ser rècord, ho indique i felicite el jugador.
    8. Proposeu una funcionalitat extra al joc per fer-lo més interessant. Qualsevol funcionalitat que resulte interessant serà valorada positivament.
   
2. **Segona opció**. Proposa la creació d'un joc del teu gust del tipus que hem desenvolupat en la pràctica. Pots pensar en jocs tipus el Pong, el Tetris, ... Si agafes aquesta opció conta-li-la al professor abans de començar a desenvolupar perquè et done l'aprovació de la idea, ja que pot implicar una dificultat massa elevada o massa baixa. El joc hauria d'incloure tots els tractaments que hem anat desenvolupant durant la pràctica.