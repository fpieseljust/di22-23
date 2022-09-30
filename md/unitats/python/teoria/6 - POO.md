# Programació orientada a objectes amb Python

És un dels paradigmes més populars per resoldre problemes a través de la programació. 

Un objecte té dues característiques:

- atributs (estat)
- funcions (comportament)
  
Per exemple:

Una persona pot ser un objecte, ja que té les propietats següents:

- nom, edat com a atributs
- cantar, ballar com a comportament
  
El concepte de POO a Python se centra en la reutilització de codi. Aquest concepte també es coneix com DRY (Don't Repeat Yourself).

## Classes

Una classe és una definició d'un objecte abstracte, que representa algun ent de la realitat al nostre programa. Conté tots els detalls comuns sobre tots els objectes del mateix tipus. 

L'exemple de classe de lloro pot ser:

```py
class Parrot():
    pass

```

Ací fem servir la paraula clau class per definir una classe Loro buida. 

## Objectes

Quan es defineix la classe, només es defineix la descripció de l'objecte. Per tant, no s’assignen recursos per a la seua execució, ni s'assignen valors als seus atributs. Quan este fet es produeix, aleshores tenim un objecte en memòria sobre el que podem actuar.

L'exemple d'objecte de classe lloro pot ser:

```py
obj = Loro ()
```

Aquí, obj és un objecte de la classe Loro.

Suposem que tenim detalls de lloros. Ara, podem construir la classe i crear un objecte per a cada lloro.

~~~py
class Parrot:

    # class attribute
    species = "bird"

    # instance attribute
    def __init__(self, name, age):
        self.name = name
        self.age = age

# instantiate the Parrot class
blu = Parrot("Blu", 10)
woo = Parrot("Woo", 15)

# access the class attributes
print("Blu is a {}".format(blu.__class__.species))
print("Woo is also a {}".format(woo.__class__.species))

# access the instance attributes
print("{} is {} years old".format( blu.name, blu.age))
print("{} is {} years old".format( woo.name, woo.age))

---
Eixida
Blu is a bird
Woo is also a bird
Blu is 10 years old
Woo is 15 years old
~~~

Al programa anterior, hem creat una classe Parrot. A continuació, hem definit uns atributs, que prendran valors diferents en la instanciació d'objectes diferents.

Aquests atributs es defineixen dins del mètode __init__ de la classe, que és el mètode inicialitzador que s’executarà només creem objectes.

Després, creem instàncies de la classe Parrot. *blu i woo* són referències (valor) als nostres objectes nous.

Podem accedir als atributs de classe mitjançant `__class__.species`. Els atributs de classe són els mateixos per a totes les instàncies d’una classe. De la mateixa manera, accedim als atributs de la instància mitjançant `objecte.nom_atribut`. Els atributs d’instància (valors) són diferents per a cada instància d’una classe.

## Mètodes

Són funcions definides dins el cos d'una classe. S'utilitzen per a definir el comportament de l'objecte.

```py
class Parrot:
    
    # instance attributes
    def __init__(self, name, age):
        self.name = name
        self.age = age
    
    # instance method
    def sing(self, song):
        return "{} sings {}".format(self.name, song)

    def dance(self):
        return "{} is now dancing".format(self.name)

# instantiate the object
blu = Parrot("Blu", 10)

# call our instance methods
print(blu.sing("'Happy'"))
print(blu.dance())
---
Blu sings 'Happy'
Blu is now dancing
```

Hem definit dos mètodes sing() i dance(), que són mètodes d'instància, ja que es criden sobre un objecte.

## Herència

L'herència és una forma de reutilitzar codi sense tindre-lo que reescriure. Açò facilita el manteniment de les aplicacions. 

Les noves classes s'anomenen classes derivades (o classe filla). La classes de les que deriven són les classes base (o classe pare).

~~~py
# parent class
class Bird:
    
    def __init__(self):
        print("Bird is ready")

    def who_is_this(self):
        print("Bird")

    def swim(self):
        print("Swim faster")

# child class
class Penguin(Bird):

    def __init__(self):
        # call super() function
        super().__init__()
        print("Penguin is ready")

    def who_is_this(self):
        print("Penguin")

    def run(self):
        print("Run faster")

peggy = Penguin()
peggy.whoisThis()
peggy.swim()
peggy.run()
---
Bird is ready
Penguin is ready
Penguin
Swim faster
Run faster
~~~

En l'anterior programa, la classe Penguin hereda de la classe Bird. La classe derivada hereda el mètode *swim()*, modifica el mètode *who_is_this()* i extén amb un nou mètode *run()*.

Utilitzem `super().__init__()` dins de l'`__init()__` per a inicialitzar la classe pare. 

## Encapsulament

Podem restringir l’accés a mètodes i variables, és a dir, definir-los com a privats. Això impedeix que les dades es modifiquen directament accedint als atributs, és el que anomenem encapsulament. Definim atributs o mètodes privats utilitzant el guió baix com a prefix, és a dir, simple _ o doble __.

```py
class Computer:

    def __init__(self):
        self.__maxprice = 900

    def sell(self):
        print("Selling Price: {}".format(self.__maxprice))

    def setMaxPrice(self, price):
        self.__maxprice = price

c = Computer()
c.sell()

# change the price
c.__maxprice = 1000
c.sell()

# using setter function
c.setMaxPrice(1000)
c.sell()
```

```
Eixida
Selling Price: 900
Selling Price: 900
Selling Price: 1000
```

Com vegem, per canviar el valor, hem d’utilitzar una funció modificadora *setter*, és a dir, setMaxPrice (), que pren el preu com a paràmetre.

## Polimorfisme

El polimorfisme és la capacitat d’utilitzar una interfície comuna (crides amb els mateixos noms) en diferents classes derivades.

Suposem que hem de pintar una forma i que hi ha diverses opcions: rectangle, quadrat, cercle, ... Podríem utilitzar el mateix mètode per a pintar qualsevol forma. Aquest concepte s’anomena polimorfisme.

```py
class Parrot:

    def fly(self):
        print("Parrot can fly")
    
    def swim(self):
        print("Parrot can't swim")

class Penguin:

    def fly(self):
        print("Penguin can't fly")
    
    def swim(self):
        print("Penguin can swim")

# common interface
def flying_test(bird):
    bird.fly()

#instantiate objects
blu = Parrot()
peggy = Penguin()

# passing the object
flying_test(blu)
flying_test(peggy)
```

```
Parrot can fly
Penguin can't fly
```

Al programa anterior, hem definit dues classes Parrot i Penguin. Cadascun d'elles té un mètode comú fly(). No obstant això, les seues funcions són diferents.

Per utilitzar el polimorfisme, hem creat una interfície comuna, és a dir, la funció flying_test() que pren com a paràmetre qualsevol objecte i crida al mètode fly() de l’objecte. Així, quan passem els objectes blu i peggy a la funció flying_test(), s'executa el mètode corresponent a cadascuna.

### Activitat 14

Defineix una jerarquia de figures amb les classes *Figura*, *Cercle*, *Triangle*, *Rectangle* i *Quadrat*.

- La clase Figura tindrá dos métodes abstractes *area* i *perimetre*, que implementarán la resta de classes. La classe figura serà el que s'anomena una interfície informal, ja que tots els seus mètodes són abstractes. Per a definir que són abstractes, simplement utilitzeu la instrucció **pass** al bloc de la funció.

    ```py
    def area() -> int:
        """Torna l'àrea d'una Figura"""
        pass
    ```

- El Cercle rebrá el radi com a argument al seu constructor, el Triangle el costat i el Rectangle la base i l'altura.
- Cercle, Triangle i Rectangle heredarán de Figura directament.
- Quadrat heredará de Rectangle, però al constructor sols rebrá un argument, el costat.
- Crea un objecte de cada tipus i imprimeix les seues característiques.

    !!! info
        En realitat la classe Figura es pot implementar com a una **classe interfície**, on tots els seus mètodes siguen abstractes. En Python existeixen dos tipus d'interfícies, formals i informals.

        Per altra banda, podem declarar els atributs de les classes com a **atributs privats** i utilitzar els **decoradors @property i @*atribut*.setter** per a indicar els mètodes getters i setters, que seran públics. Fer-ho d'esta forma té alguns avantatges que veurem més endavant.