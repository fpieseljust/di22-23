# Tractament de fitxers

## Fitxers

### Entrada eixida utilitzant fitxers

Per a llegir o escriure en un fitxer, primer l’hem d’obrir. Quan acabem, s’ha de tancar perquè s’alliberen els recursos relacionats amb el fitxer.

Per tant, a Python, utilitzem la següent seqüència d'operacions per traballar amb fitxers:

1. Obrir un fitxer
2. Llegir o escriure 
3. Tancar el fitxer

### Open

Per obrir un fitxer utilitzem la funció **open()**.

~~~py
>>> f = open("test.txt")    # arxiu en el mateix directori
>>> f = open("C:/Python38/README.txt")  # path sencer
~~~

A més, podem especificar el mode d'apertura i la codificació.

| Mode | Descripció                               |
| ---- | ---------------------------------------- |
| r    | lectura                                  |
| w    | escriptura                               |
| x    | creació exclusiva (falla si ja existeix) |
| a    | afegir al final, el crea si no existeix  |
| t    | mode lectura de text (per defecte)       |
| b    | mode binari                              |
| +    | actualització (lectura i escriptura \)   |


~~~py
>>> f = open("test.txt",'w') # obert per a escriptura
>>> f = open("test.txt", mode='r', encoding='utf-8')
~~~

!!! warning

    És important que tingau en compte que quan l'intèrpret de python s'executa, ho fa des del directori del què s'ha llançat(el podeu obtindre mitjançant *os.getcwd()*), per això la càrrega de fitxers en rutes relatives en funcions tipus *open*, *load*, etc potser vos dóne un error indicant que no el troba.
 
    f = open("ruta relativa/arxiu.txt")

   **S'han d'evitar les rutes absolutes**.

   El que podeu fer per evitar estos errors és obtindre la ruta des del fitxer font de la següent forma:
   ```py
   ruta_base = os.path.dirname(__file__)
   ruta_a_recurs = os.path.join(ruta_base, "arxiu.txt")
   f = open(ruta_a_recurs)
   ```


### Close

Python utilitza un *garbage collector* per netejar objectes sense referències, però no hem de confiar per tancar el fitxer.

~~~py
try:
   f = open("test.txt", encoding = 'utf-8')
   # operacions sobre l'arxiu
finally:
   f.close()
~~~

Altra possibilitat és amb **with**. En este cas no hem de tancar-lo explícitament.

~~~py
with open("test.txt", encoding = 'utf-8') as f:
   # operacions sobre l'arxiu
~~~

### Escriptura

Per a escriure, necessitem haver-lo obert amb les opcions w, a o x. Compte amb l'opció w, perquè sobreescriu els arxius.

~~~py
with open("test.txt",'w',encoding = 'utf-8') as f:
   f.write("Primer arxiu\n")
   f.write("Este arxiu\n")
   f.write("conté tres línies\n")
~~~

### Lectura

Utilitzarem el mètode **read()** per a llegir. La funció **tell()** ens diu en quina posició tenim el cursor i amb **seek()** el podem modificar.

~~~py
>>> f = open("test.txt",'r',encoding = 'utf-8')
>>> f.read(6)
'Primer'

>>> f.read(6)
' arxiu'

>>> f.read()     # llig fins al final
'\nEste arxiu\nconté tres línies\n'

>>> f.read()  # posteriors lectures tornen la cadena buida
''
~~~

~~~py
>>> f.tell()
45

>>> f.seek(0)
0

>>> print(f.read())
Primer arxiu
Este arxiu
conté tres línies
~~~

També podem utilitzar la funció **readline()** per a llegir una línia, o **readlines()** per a que ens torne una llista de línies llegides.

#### Activitat 11

Crea una aplicació que vaja llegint operacions d'un fitxer "operacions.txt" que contindrà una operació per línia. Per exemple:
4 + 4

Haurà de guardar els resultats en un altre arxiu "resultats.txt". Per exemple:
4 + 4 = 8

Utilitza funcions anònimes per a implementar les operacions de suma, resta, multiplicació i divisió, que seran les úniques operacions reconegudes per la nostra aplicació.

## Directoris

Si hi ha una gran quantitat de fitxers i directoris amb els que tractar, disposem del mòdul os (operating system), que ens proporciona mètodes per al seu tractament.

Per a veure el directori de treball, utilitzem **getcwd()**.

~~~py
>>> import os

>>> os.getcwd()
~~~

Per a canviar de directori, **chdir()**.

~~~py
>>> os.chdir('/home/ferran')
~~~

Per a llistar els directoris ens servim de **listdir()**.

~~~py
>>> os.listdir('/home')
~~~

Per crear un directori usem **mkdir()**.

~~~py
>>> os.mkdir('Nova_carpeta')
~~~

Si volem renombrar un directori.

~~~py
>>> os.rename('Nova_carpeta','Vella_carpeta')
~~~

Per a eliminar un arxiu utilitzarem **remove()**. Si el que volem eliminar és una carpeta buida **rmdir()**.

~~~py
>>> os.remove('arxiu.txt')
>>> os.rmdir('Vella_carpeta')
~~~

En el cas que la carpeta no estiga buida, hem d'importar el mòdul **shutil** i utilitzar la funció **rmtree()**.

~~~py
>>> import shutil
>>> shutil.rmtree('Carpeta')
~~~
