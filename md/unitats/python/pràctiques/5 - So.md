# Efectes de so
Fins ara ens hem centrat en el joc i els aspectes visuals del mateix. Ara anem a veure com incorporar sons. pygame proporciona un mesclador per gestionar totes les activitats relacionades amb el so. Utilitzarà les classes i mètodes d’aquest mòdul per proporcionar música de fons i efectes de so per a diverses accions.

El nom de mesclador fa referència al fet que el mòdul barreja diversos sons en un tot cohesionat. Mitjançant el submòdul de música, podeu reproduir fitxers de so individuals en diversos formats. Tota la reproducció es produeix en segon pla, de manera que mentre es reprodueix un so, el mètode s'executa en paral·lel i torna el control immediatament al punt on s'ha fet la crida.

## Inicialització
L'ús del mesclador comença amb la seua inicialització *pygame.mixer.init()*. Si no volem canviar els valors per defecte, no cal passar-li arguments. S'ha d'inicialitzar el mesclador abans que el pygame.

```py
# Setup for sounds. Defaults are good.
pygame.mixer.init()

# Initialize pygame
pygame.init()
```

## Música de fons
Una vegada inicialitzat el sistema, podeu configurar els vostres sons i música de fons com es mostra a continuació.

```py
# Load and play background music
pygame.mixer.music.load("Apoxode_-_Electric_1.ogg")
pygame.mixer.music.play(loops=-1)

# Load all sound files
# Sound sources: Jon Fincher
move_up_sound = pygame.mixer.Sound("Rising_putter.ogg")
move_down_sound = pygame.mixer.Sound("Falling_putter.ogg")
collision_sound = pygame.mixer.Sound("Collision.ogg")
```

Carreguen un clip de so de fons i comencen a reproduir-lo. Podeu dir al clip de so que es reproduisca en bucle i que no acabe mai establint el paràmetre *loop = -1*.

## Sons d'esdeveniments
Després carreguen tres sons que farem servir per a diversos efectes de so. Els dos primers són sons ascendents i descendents, que es reprodueixen quan el jugador es mou cap amunt o cap avall. L’últim és el so que s’utilitza sempre que hi ha una col·lisió. També podeu afegir altres sons, com ara un so per a la creació d'un enemic o un so final per a la finalització del joc.

Llavors, com s’utilitzen els efectes de so? Volem reproduir cada so quan es produeixi un esdeveniment determinat. Per exemple, quan l'avió es mou cap amunt, volem reproduir move_up_sound. Per tant, afegiu una crida a *.play()* sempre que gestioneu este l'esdeveniment.

Per a una col·lisió entre el jugador i un enemic, reproduïu el so *Collisions.ogg*.

Utilitzeu el mètode *stop()* quan vulgau parar un so que encara s'està reproduint i no voleu que es mescle amb un nou.

## Fi del joc
Finalment, quan acabe el joc, tots els sons haurien de parar-se. Per fer-ho, afegiu les línies següents al final del programa després del bucle:

```py
# All done! Stop and quit the mixer.
pygame.mixer.music.stop()
pygame.mixer.quit()
```

Tècnicament, aquestes últimes línies no són necessàries, ja que el programa finalitza i els recursos s'alliberen. No obstant això, si més endavant decidiu afegir una pantalla d'introducció o una pantalla d'eixida al vostre joc, és possible que hi haja més codi executat-se després que finalitze el joc.
