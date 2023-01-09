## Connexió a una base de dades SQL des de Python

!!!note "Ús de llibreries per a gestionar bases de dades"
    Podríem utilitzar llibreries específiques de python3 per gestionar bases de dades, però en esta unitat utilitzarem la tecnologia pròpia de Qt. Açò ens ajudarà a aprofitar la integració entre les classes SQL de PySide i l'arquitectura Model-View. A més, no afegirem dependències addicionals a la nostra aplicació.
    Per exemple, no utilitzarem el mòdul *sqlite3* per gestionar una base de dades SQLite3, sinó que utilitzarem les classes que ens proporciona PySide6.

## Creació d'una connexió de base de dades
Per connectar a base de dades necessiten informació general sobre la seua configuració:

- El sistema de gestió de bases de dades (SGBD)
- El nom d'usuari
- La contrasenya
- El *host* on està allotjada

Nosaltres utilitzarem SQLite 3 per la seua baixa configuració requerida, ja que ens permet llegir i escriure directament a les bases de dades allotjades al disc dur, sense necessitat d'un procés de servidor separat. 

Un altre avantatge és que la seua llibreria està inclosa a les llibreries estàndars de Python3 i també a PySide, de manera que no cal que instal·leu res més per començar a treballar amb elles.

### La classe **QSqlDatabase**
A PySide, podeu crear una connexió de base de dades mitjançant la classe QSqlDatabase. Aquesta classe representa una connexió i proporciona una interfície per accedir a la base de dades. 

Per crear una connexió, utilitzarem el mètode **.addDatabase()** que rep com a arguments:

- Un driver SQL, string amb el nom del controlador compatible amb PySide
- Un nom de connexió (opcional). En cas de no passar-lo, s'assinarà el nom per defecte *qt_sql_default_connection*.

~~~python
QSqlDatabase.addDatabase(
    driver, connectionName=QSqlDatabase.defaultConnection
)
~~~

Els drivers d'SQL disponibles a PySide6 són els següents:

| Nom del driver | Sistema de gestió de bases de dades            |
| -------------- | ---------------------------------------------- |
| QDB2           | IBM Db2 (versió 7.1 i posterior)               |
| QIBASE         | Borland InterBase                              |
| QMYSQL/M       | ARIADB	MySQL o MariaDB (versió 5.0 i superior) |
| QOCI           | Interfície de crides d'Oracle                |
| QODBC          | Connectivitat de base de dades oberta (ODBC)   |
| QPSQL          | PostgreSQL (versions 7.3 i posteriors)         |
| QSQLITE2       | SQLite 2 (obsolet des de Qt 5.14)              |
| QSQLITE        | SQLite 3                                       |
| QTDS           | Sybase Adaptive Server (obsolet des de Qt 4.7) |


Si ja teniu una connexió amb el mateix nom, se sobreescriurà.

El mètode **.addDatabase()** afig una connexió de base de dades a una llista de connexions disponibles. Aquesta llista és un registre global que PySide manté amb les connexions disponibles en una aplicació.

Un vegada creada una connexió, podem d'establir diversos atributs que dependran del controlador que utilitzem. En general, haurem d'establir el *host*, el nom de la base de dades, el nom d'usuari i la contrasenya per accedir a la base de dades. Per fer-ho utilitzarem els mètodes **.setHostName(host)**, .**setDatabaseName(name)**, **.setUserName(username)** i **.setPassword(password)** respectivament.

!!!warning "Contrasenya de connexió"
    La contrasenya què passeu com a argument a .setPassword() s'emmagatzema en text sense format i es pot recuperar més tard amb el mètode .password(). Aquest és un **risc de seguretat greu** que hauríeu d'evitar introduir a les vostres aplicacions de base de dades. Aprendrem a fer-ho més segur més endavant.

En el cas de les bases de dades SQLite, el nom de la base de dades és normalment un nom de fitxer o una ruta que inclou el nom del fitxer de la base de dades. També podem utilitzar el nom especial *:memory:* per a una base de dades allotjada en memòria, de forma que evitem accessos a disc que són molt més lents.

~~~python
from PySide6.QtSql import QSqlDatabase

connection = QSqlDatabase.addDatabase("QSQLITE")
connection.setDatabaseName("contacts.sqlite")
database_name = connection.databaseName()
connection_name = connection.connectionName()
~~~

### Gestió de múltiples connexions

Potser necessitem utilitzar diverses connexions. Per exemple, és possible que vulgam registrar les interaccions dels usuaris amb la base de dades mitjançant una connexió específica per a cada usuari o que la nostra aplicació haja de connectar a diverses bases de dades per recollir informació i actualitzar les dades locals.

Per gestionar aquestes situacions, podem proporcionar noms específics per a les nostres connexions i fer referència a cada connexió pel seu nom.

~~~python
from PySide6.QtSql import QSqlDatabase

connection1 = QSqlDatabase.addDatabase("QSQLITE", "connection1")
connection1.setDatabaseName("contacts.sqlite")

connection2 = QSqlDatabase.addDatabase("QSQLITE", "connection2")
connection2.setDatabaseName("contacts.sqlite")

database_name1 = connection1.databaseName()
connection_name1 = connection1.connectionName()

database_name2 = connection2.databaseName()
connection_name2 = connection2.connectionName()
~~~

### Obertura d'una connexió de base de dades

Una vegada establida la connexió, hem d'obrir aquesta connexió per poder interactuar amb la nostra base de dades. Per fer-ho, utilitzem el mètode **.open()** de la connexió. Es pot utilitzar de dues formes:

- .open() obre una connexió de base de dades utilitzant els valors de connexió actuals.
- .open(username, password) obre una connexió a la base de dades mitjançant el fitxer username i password.
  
El mètode torna  *True* si la connexió té èxit. En cas contrari, torna *False*. En cas de no poder establir la connexió, podeu utilitzar **.lastError()** per obtenir informació sobre el que ha passat.

!!!note 
    *.setPassword(password)* emmagatzema les contrasenyes com a text sense format, la qual cosa suposa un risc de seguretat. D'altra banda, *.open()* no emmagatzema mai les contrasenyes, sinó que la passa directament al controlador en obrir la connexió. Després d'això, descarta la contrasenya. Per tant, utilitzar *.open()* per gestionar les nostres contrasenyes és el camí a seguir si voleu evitar problemes de seguretat.

!!!note
    En utilitzar *.open()* amb una connexió que utilitza el controlador SQLite, si el fitxer de base de dades no existeix, es crearà automàticament un fitxer de base de dades nou i buit.

Hauriem d'assegurar-nos que tenim una connexió vàlida abans d'intentar fer qualsevol operació amb les nostres dades. En cas contrari, la nostra aplicació pot fallar. 

Per exemple, què passa si no tenim permisos d'escriptura per al directori en què estem intentant crear aquest fitxer de base de dades? Hem d'assegurar-nos que estem gestionant qualsevol error que es puga produir en obrir una connexió.

Una manera habitual d'utilitzar *.open()* és la següent:

~~~python
import sys
import os

from PySide6.QtSql import QSqlDatabase
from PySide6.QtWidgets import QApplication, QMessageBox, QLabel

connection = QSqlDatabase.addDatabase("QSQLITE")
connection.setDatabaseName(
    os.path.join(os.path.dirname(__file__),
                 "contacts.sqlite")
)
# connection.setDatabaseName("/contacts.sqlite")
application = QApplication(sys.argv)

if not connection.open():
    QMessageBox.critical(
        None,
        "Error connectant a la base de dades!",
        "Database Error: %s" % connection.lastError().databaseText(),
    )
    sys.exit(1)

window = QLabel("Connectat a la base de dades!")
window.setWindowTitle("Database open")
window.resize(200, 100)
window.show()
sys.exit(application.exec_())
~~~

