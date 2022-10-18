# Control de fluxe

## If ... else

~~~py
num = int(input("Número: "))
if num > 0:
    print("Positiu")
elif num == 0:
    print("Zero")
else:
    print("Negatiu")
~~~

## For

No existeix un for a l'estil de C o Java. Amés podem afegir un else al final de bucle.

~~~py
nums = [6, 5, 3, 8, 4, 2, 5, 4, 11]

suma = 0

for val in nums:
	suma = suma + val
else:
    print("Hem acabat de sumar")

print("La suma és", suma)
~~~

Podem combinar el bucle for amb la funció **range(principi, fi, pas)**. 

#### Activitat 8

Fes una aplicació que imprimisca els números imparells entre l'1 i el 100.

#### Activitat 9

Fes una aplicació que donada la següent llista, imprimisca els seus membres: aficions = ['esports', 'cine', 'teatre']

## While

~~~py
contador = 0

while contador < 3:
    print("Dins del while")
    contador = contador + 1
else:
    print("Fora del bucle")
~~~

## Break i continue

S'utilitzen igual que a Java. El continue passa a la següent iteració, mentre que el break ix del bucle. En cas de bucles anidats, ix del bucle intern.

## Switch - Case

Fins a la versió 3.10, python no implementava el switch-case d'altres llenguatges i s'havia d'utilitzar un bloc d'if-elseif:

~~~py
edat = 120

if age > 90:
    print("Esta festa és sols per a joves.")
elif age < 0:
    print("Encara no has nascut!!")
elif age >= 18:
    print("Endavant!!")
else: 
    "Ers massa jove per entrar a esta festa"

# Output: Esta festa és sols per a joves.
~~~

O definir una funció que executara aquesta funcionalitat:

```py
def switch(lang):
    if lang == "JavaScript":
        return "Seràs programador web."
    elif lang == "PHP":
        return "Seràs programador de backend."
    elif lang == "Python":
        return "Seràs científic de dades."
    elif lang == "Solidity":
        return "Seràs desenvolupador de Blockchain."
    elif lang == "Dart":
        return "Seràs desenvolupador d'aplicacions mòbils."

print(switch("JavaScript"))   
print(switch("Python"))   
print(switch("Dart"))  

"""
Eixida: 
Seràs programador web.
Seràs científic de dades.
Seràs desenvolupador d'aplicacions mòbils
"""
```

A partir de la versió 3.10, Python disposa de les paraules reservades **match-case**:

```py
match terme:
    case patró-1:
         acció-1
    case patró-2:
         acció-2
    case patró-3:
         acció-3
    case _:
        acció-per-defecte
```

### Exemple

```py
lang = input("Quin llenguatge de programació vols aprendre? ")

match lang:
    case "JavaScript":
        print("Seràs programador web.")

    case "Python":
        print("Seràs científic de dades.")

    case "PHP":
        print("Seràs programador de backend.")
    
    case "Solidity":
        print("Seràs desenvolupador de Blockchain.")

    case "Dart":
        print("eràs desenvolupador d'aplicacions mòbils.")
    case _:
        print("L'idioma no importa, el que importa és resoldre problemes.")
```