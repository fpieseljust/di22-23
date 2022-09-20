# Funcions

## Definició de funcions

~~~py
def nom_funcio(paràmetres):
	"""docstring"""
	instruccions(s)
~~~

Nota: Recordeu làmbit de les variables, ja que hi haurà variables locals a la funció.

## Arguments

### Valors per defecte

Els arguments de les funcions poden tindre un valor per defecte. En cas de no assignar-li un valor per defecte, necessitem fer la crida passant-li el valor de l'argument. 

~~~py
def saluda(nom="desconegut", msg="Benvingut!"):
    """
    Funció per saludar a un usuari

    Entrada:
        nom="desconegut": String, nom de l'usuari
        msg="Benvingut!": String, missatge de salutació

    Si no li proporcionem valors en la crida, 
    utilitzarà els valors per defecte
    """

    print("Hola", nom + '.', msg)
~~~

La crida a la funció la podem fer sense arguments, amb un o amb dos. És una forma de fer una sobrecàrrega de mètodes de forma molt ràpida.

~~~py
saluda("Tomàs", "Què fas?")
saluda("Pau")
saluda()
saluda(msg="", nom="Artur")
saluda(msg="","Artur")
~~~

### Nombre arbitrari d'arguments

Si no sabem a priori quants arguments rebrà la funció, podem utilitzar el caràcter "*" en la definició de la funció.

~~~py
def saluda(*noms):
    """
    Funció per saludar a un conjunt d'usuaris

    Entrada:
        noms: llistat de noms
    """
    for nom in noms:
        print("Hola", nom)



saluda("Alex","Guillem","Javier")
~~~

## Funcions recursives

La recursió és el procés de definir alguna cosa en termes d'eixa mateixa cosa. Aleshores, una funció recursiva és aquella que es crida a sí mateix.
S'ha d'anar molt en compte en crear correctament la condició d'eixida de la funció, ja que d'altra forma entrariem en bucle infinit.

~~~py
def factorial(x):
    """Funció per a calcular el fatorial d'un enter.
    
        Entrada:
            - x: int, el nombre del que volem calcular el factorial

        Eixida:
            - x!: int, factorial d'x
    """

    if x == 1:
        return 1
    else:
        return (x * factorial(x-1))


num = int(input("Número: "))
print(num, "! =", factorial(num))
~~~

## Funcions anònimes

Les **funcions anònimes o funcions lambda**, són funcions sense nom. Poden tindre un nombre indeterminat d'arguments, però sols una expressió, que serà avaluada i retornat el seu resultat.

~~~py
quadrat = lambda x: x ** 2

print(quadrat(5))
~~~

Normalment les funcions lambda s'utilitzen en combinació amb altres funcions com filter(), map(), etc.

La ***funció map()*** rep com a arguments una funció i una llista, i torna una llista del mateix tamany on cada element és el resultat d'aplicar la funció sobre l'element que ocupa la mateixa posició a la llista original.

La **funció filter()**, rep una funció i una llista com a arguments, i torna com a resultat una llista amb els elements que avaluen a True la funció.

~~~py
llista = [1, 5, 4, 6, 8, 11, 3, 12]

nova_llista = list(map(lambda x: x ** 2 , llista))

print(nova_llista)

"""
Eixida:
[1, 25, 16, 36, 64, 121, 9, 144]
"""
~~~

### Activitat 10

Definix una llista i utilitzant filter, que la separe en dues llistes, una amb els elements parells i l'altra amb els senars.

## Packages

Igual que la informació al disc dur està organitzada en carpetes i subcarpetes, un programa en Python es pot organitzar en paquets, sub paquets i mòduls. Açò fa un programa més fàcil de gestionar i conceptualment més clar.
Una carpeta ha de contindre un arxiu anomenat \_\_init\_\_.py. Este arxiu pot estar buit o no, però normalment conté codi d'inicialització.

![paquets](images/PackageModuleStructure.jpg "Estructura en paquets, subpaquets i mòduls")

Per a importar un mòdul d'un paquet utilitzariem **import** o **from ... import**.

~~~py
import Game.Level.start
from Game.Level import start
~~~
