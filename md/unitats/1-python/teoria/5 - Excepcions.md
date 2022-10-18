# Errors i excepcions

Podem cometre errors mentre programem. Estos errors es poden classificar bàsicament en dos tipus:

1. Errors de sintaxi
2. Errors en la lògica (Excepcions)

Els erros de sintaxi es produixen abans de l'execució, mentre que els erros lògics es produixen en temps d'execució.

## Excepcions

Algun exemple d'excepció són els següents:

- Quan intentem obrir un fitxer (per llegir) que no existeix (FileNotFoundError)
- Quan intentem dividir un nombre per zero (ZeroDivisionError)
- Quan intentem importar un mòdul que no existeix (ImportError).

Sempre que es produeixen aquests tipus d’errors d’execució, Python crea un objecte d’excepció. Si no el tractem, interrumpeix l'execució i imprimeix una traça de l'error juntament amb alguns detalls sobre per què ha produït aquest error.

### Excepcions definides en Python

Per a consultar totes les excepcions definides podem utilitzar:

~~~py
>>> print(dir(locals()['__builtins__']))
~~~

- ArithmeticError
- AssertionError
- AttributeError
- BaseException
- BlockingIOError
- BrokenPipeError
- BufferError
- ChildProcessError
- ConnectionAbortedError
- ConnectionError
- ConnectionRefusedError
- ConnectionResetError
- EOFError
- EnvironmentError
- Exception
- FileExistsError
- FileNotFoundError
- FloatingPointError
- GeneratorExit
- IOError
- ImportError
- IndentationError
- IndexError
- InterruptedError
- IsADirectoryError
- KeyError
- LookupError
- MemoryError
- ModuleNotFoundError
- NameError
- NotADirectoryError
- NotImplementedError
- OSError
- OverflowError
- PermissionError
- ProcessLookupError
- RecursionError
- ReferenceError
- RuntimeError
- StopAsyncIteration
- SyntaxError
- SystemError
- TabError
- TimeoutError
- TypeError
- UnboundLocalError
- UnicodeDecodeError
- UnicodeEncodeError
- UnicodeError
- UnicodeTranslateError
- ValueError

### Com funcionen les excepcions

Python llança una d'aquestes excepcions en executar una instrucció que provoca un error. 

Quan es produeixen aquestes excepcions, l'intèrpret de Python detén l'execució del procés actual i el passa al procés que ha fet la crida, fins que algú la tracte. Si ningún procés la tracta, el programa es detendrà.

Per exemple, considerem un programa en què tenim una funció A que crida a la funció B, que al seu torn crida a la funció C. Si es produeix una excepció a la funció C però no es tracta a C, l’excepció passa a B i després a A.

Si no es gestiona mai, es mostrarà un missatge d'error i el nostre programa s'aturarà de sobte.

## Capturant excepcions en Python

A Python, les excepcions es poden gestionar mitjançant una sentència try.

L'operació crítica que pot generar una excepció es col·loca dins de la clàusula **try**. El codi que gestiona les excepcions s’escriu a la clàusula **except** i s'executarà en produïr-se. Per tant, podem escollir quines operacions es realitzaran una vegada que haguem capturat l'excepció. 

Exemple:

~~~py
# importem el mòdul sys per veure el tipus d'excepció
import sys

randomList = ['a', 0, 2]

for element in randomList:
    try:
        print("Element val ", element)
        r = 1/int(element)
        break
    except:
        print("Oops! Excepció capturada -->", sys.exc_info()[0])
        print("Següent iteració")
        print()
print("L'invers de ", element, "és", r)
~~~

Com totes les excepcions hereden de la superclasse Exception, podem reescriure el programa com:

~~~py
# importem el mòdul sys per veure el tipus d'excepció
import sys

randomList = ['a', 0, 2]

for element in randomList:
    try:
        print("Element val ", element)
        r = 1/int(element)
        break
    except Exception as e:
        print("Oops! Excepció capturada -->", e.__class__)
        print("Següent iteració")
        print()
print("L'invers de ", element, "és", r)
~~~

A l'exemple anterior, no hem utilitzat cap excepció específica a la clàusula except.

No és una bona pràctica de programació, ja que capturarà totes les excepcions i gestionarà tots els casos de la mateixa manera. Podem especificar quines excepcions hauria de capturar una clàusula except.

Una clàusula try pot tindre un nombre indeterminat de clàusules except per gestionar diferents excepcions, però, només s'executarà una en cas que es produeixi una excepció.

Podem utilitzar una tupla de valors per especificar diverses excepcions en una clàusula except.

Exemple:

~~~py
try:
   # fer alguna cosa
   pass

except ValueError:
   # manegem ValueError
   pass

except (TypeError, ZeroDivisionError):
   # manegem múltiples excepcions
   # TypeError i ZeroDivisionError
   pass

except:
   # manegem totes les altres excepcions
   pass
~~~

### Try ... except ... finally


La sentència try de Python pot tindre una clàusula final opcional. Aquesta clàusula s’executa independentment de si es produix una excepció o no i s’utilitza generalment per alliberar recursos.

Per exemple, podem estar connectats a un centre de dades remot a través de la xarxa o treballar amb un fitxer o una interfície gràfica d'usuari (GUI). En totes aquestes circumstàncies, hem de netejar recursos abans que el programa ature la seua execució, tant si ho fa de forma controlada com si es para bruscament. Aquestes accions (tancar un fitxer, GUI o desconnectar de la xarxa) es realitzen normalment a la clàusula final.

Exemple:

~~~py
try:
   f = open("test.txt",encoding = 'utf-8')
   # operacions amb el fitxer
finally:
   f.close()
~~~

## Llançant excepcions en Python

Les excepcions es generen normalment quan es produeixen errors en temps d'execució, però també podem generar/llançar excepcions manualment mitjançant la paraula reservada **raise**. Opcionalment, podem passar arguments a l’excepció per aclarir per què s’ha generat aquesta excepció.

~~~py
import sys
try:
    nombre = int(input("Dona'm un número positiu: "))
    if nombre <= 0:
        raise ValueError(str(nombre) + " no és un número positiu!")
    1/0
except ValueError as ve:
    print("Excepció capturada:", ve)
except:
    print("Unexpected error:", sys.exc_info()[0])
    raise
~~~

### Assert

Amb la paraula reservada **assert** llancem una excepció sempre i quan l'expressió que la segueix s'avalua a *Fals*. Si s'avalua a *True*, es continua l'execució del programa de forma seqüencial.

~~~py
try:
    num = int(input("Introdueix un nombre positiu: "))
    assert num > 0
except AssertionError:
    print("AssertionError: No és positiu!")
except ValueError:
    print ("ValueError: No has introdït un número!")
else:
    print("És positiu")
~~~

## Excepcions definides per l'usuari

De vegades necessitem definir excepcions que no estan disponibles a Python quan es dóna alguna condició. En este cas, hem de crear les nostres propies excepcions. Per a fer-ho, hem de definir noves classes que **hereden de *Exception***, ja siga directa o indirectament. Esta nova excepció que hem creat també podrà ser llançada amb *raise*. 

~~~py
>>>class CustomError(Exception):
...    pass

>>> raise CustomError
Traceback (most recent call last):
...
__main__.CustomError

>>> raise CustomError("S'ha produït un error")
Traceback (most recent call last):
...
__main__.CustomError: S'ha produït un error
~~~

Quan estem desenvolupant un programa gran, és una bona pràctica col·locar totes les excepcions definides per l'usuari que el nostre programa definix en un fitxer separat *exceptions.py* o *errors.py*.

#### Activitat 12

Anem a implementar un xicotet joc per consola. El programa generarà un número aleatori entre 0 i 100 (utilitzeu randint() del mòdul random) i demanarà a l'usuari que introduïsca un número.

Mentre el número siga massa menut, llançarà una excepció ErrorEnterMassaMenut indicant-li-ho. Si per contra és massa gran llançarà ErrorEnterMassaGran.

Si s'introdueix un valor no numèric, es llançarà una excepció de tipus ErrorNoEsEnter.

El joc acabarà quan s'introduïsca l'enter buscat, felicitant a l'usuari.

#### Activitat 13 

Modifica el codi de l'activitat 11 per a que no es produïsquen errors en l'execució, ja siga per introdïur valor no definits per a les funcions, valors que no són numèrics o operacions desconegudes. Controla també que no es produïsquen errors en la lectura/escriptura dels arxius.
