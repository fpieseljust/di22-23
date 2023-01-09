# Execucio de consultes SQL amb PySide

Amb una connexió de base de dades ja podem començar a treballar amb la nostra base de dades. 

Per fer-ho, podem utilitzar consultes i objectes QSqlQuery basats en *strings*. QSqlQuery ens permet executar qualsevol classe de consulta SQL a la nostra base de dades, ja siguen sentències de llenguatge de manipulació de dades (DML), com ara SELECT, INSERT, UPDATE i DELETE, o sentències de llenguatge de definició de dades (DDL), com ara CREATE TABLE, etc.

QSqlQuery té diversos constructors:

- QSqlQuery(query, connection) construeix un objecte de consulta mitjançant una cadena SQL *query* i una connexió a base de dades *connection*. Si no especifiqueu una connexió, o si la connexió especificada no és vàlida, s'utilitzarà la connexió de base de dades per defecte. Si *query* no és una cadena buida, s'executarà immediatament.
- QSqlQuery(connection) construeix un objecte de consulta utilitzant connection.
- QSqlQuery() la consulta utilitzarà la connexió de base de dades predeterminada, si n'hi ha.

Per executar una consulta, utilitzem .exec() de l'objecte de consulta. Torna True si la consulta ha tingut èxit i, en cas contrari, torna False. Podem utilitzar .exec() de dues maneres diferents:

- .exec(query) executa la consulta SQL basada en cadenes continguda a query. 
- .exec() executa una consulta SQL preparada prèviament.

## Consultes estàtiques

Una consulta estàtica és aquella que no obté ningun argument de fora de la consulta.

~~~python
import sys
import os

from PySide6.QtSql import QSqlDatabase, QSqlQuery

# Create the connection
con = QSqlDatabase.addDatabase("QSQLITE")
con.setDatabaseName(
    os.path.join(os.path.dirname(__file__),
                 "contacts.sqlite"))

# Open the connection
if not con.open():
    print("Database Error: %s" % con.lastError().databaseText())
    sys.exit(1)

# Create a query and execute it right away using .exec()
createTableQuery = QSqlQuery()
createTableQuery.exec(
    """
    CREATE TABLE contacts (
        id INTEGER PRIMARY KEY AUTOINCREMENT UNIQUE NOT NULL,
        name VARCHAR(40) NOT NULL,
        job VARCHAR(50),
        email VARCHAR(40) NOT NULL
    )
    """
)

print(con.tables())
~~~

## Consultes dinàmiques: formatació de cadena

Les consultes que accepten paràmetres en el moment de l'execució es coneixen com a consultes dinàmiques. L'ús de paràmetres ens permet ajustar la consulta i recuperar dades en resposta a valors de paràmetres determinats. Valors diferents produiran resultats diferents. 

Hi ha dues formes de construir la consulta utilitzant els valors dels paràmetres d'entrada:

- Crear la consulta de manera dinàmica, utilitzant el format de cadena per interpolar els valors dels paràmetres.
- Preparar la consulta utilitzant paràmetres de marcador de posició i, a continuació, enllaçar els valors específics als paràmetres.

!!!warning "Vulnerabilitat de les consultes construides mitjançant cadenes"
    El primer enfocament ens permet crear consultes dinàmiques ràpidament. Però la nostra **base de dades quedarà exposada a atacs d'injecció SQL**, podent així quedar la informació compromesa.

Ací tenim un exemple de construcció de la consulta utilitzant la formatació de cadenes de text:

~~~python
name = "Ferran"
job = "Professor"
email = "ferran@example.com"
query = QSqlQuery()
query.exec(
    f"""INSERT INTO contacts (name, job, email)
    VALUES ('{name}', '{job}', '{email}')"""
)
~~~

!!!note "Ús de cometes simples en la construcció de la consulta"
    Perquè aquest tipus de consulta dinàmica funcione, hem d'assegurar-nos que els valors que s'han d'inserir tinguen el tipus de dades adequat. Per tant, utilitzem cometes simples al voltant del marcador de posició de la cadena f perquè aquests valors han de ser cadenes.

## Consultes dinàmiques: paràmetres de marcador de posició

El segon enfocament per executar consultes dinàmiques requereix que preparem les consultes prèviament utilitzant una plantilla amb marcadors de posició per als paràmetres. PySide admet dos estils de marcador de posició de paràmetres:

- **L'estil Oracle**, que utilitza marcadors de posició amb nom com ara :name o :email.
- **L'estil ODBC**, utilitza un signe d'interrogació (?) com a marcador de posició.

Per crear aquest tipus de consulta dinàmica, primer creem una plantilla amb un marcador de posició per a cada paràmetre de consulta i després passem aquesta plantilla com a argument al mètode **.prepare()**, que analitza, compila i prepara la plantilla de consulta per a l'execució. Si la plantilla té problemes, com ara un error de sintaxi SQL, no es pot compilar la plantilla i retorna *False*.

Si el procés de preparació té èxit, **.prepare()** torna *True*. Després d'aquest pas, estem en situació de poder passar un valor específic a cada paràmetre amb:

- **.bindValue()** amb nom o posicionals o 
- **.addBindValue()** amb paràmetres posicionals. 

**.bindValue()** té les dues variants següents:

- .bindValue(placeholder, val)
- .bindValue(pos, val)

A la primera variació, placeholder representa un marcador de posició d'estil Oracle. En la segona variació, pos representa un nombre enter amb la posició d'un paràmetre a la consulta, començant pel 0. En ambdues variacions, val manté el valor que s'ha d'enllaçar a un paràmetre específic.

**.addBindValue()** afegeix un valor a la llista de marcadors de posició mitjançant l'enllaç posicional. Això vol dir que l'ordre de les crides a **.addBindValue()** determina quin valor s'associarà a cada paràmetre de marcador de posició a la consulta preparada.

Per començar a utilitzar consultes preparades, podeu preparar un *INSERT INTO* per omplir la vostra base de dades amb algunes dades de mostra. Ampliem l'exemple anterior:

~~~python
import sys
import os

from PySide6.QtSql import QSqlDatabase, QSqlQuery

# Create the connection
con = QSqlDatabase.addDatabase("QSQLITE")
con.setDatabaseName(
    os.path.join(os.path.dirname(__file__),
                 "contacts.sqlite"))

# Open the connection
if not con.open():
    print("Database Error: %s" % con.lastError().databaseText())
    sys.exit(1)

# Create a query and execute it right away using .exec()
createTableQuery = QSqlQuery()
createTableQuery.exec(
    """
    CREATE TABLE contacts (
        id INTEGER PRIMARY KEY AUTOINCREMENT UNIQUE NOT NULL,
        name VARCHAR(40) NOT NULL,
        job VARCHAR(50),
        email VARCHAR(40) NOT NULL
    )
    """
)

insertDataQuery = QSqlQuery()
# Estil OBDC
prepared = insertDataQuery.prepare(
    """
    INSERT INTO contacts (
        name,
        job,
        email
    )
    VALUES (?, ?, ?)
    """
)

if prepared:
    data = [
        ("Lara", "Senior Web Developer", "lara@example.com"),
        ("David", "Project Manager", "david@example.com")
    ]
    # Inserció amb addBindValue
    for name, job, email in data:
        insertDataQuery.addBindValue(name)
        insertDataQuery.addBindValue(job)
        insertDataQuery.addBindValue(email)
        insertDataQuery.exec()

insertDataQuery = QSqlQuery()
# Estil Oracle
prepared = insertDataQuery.prepare(
    """
    INSERT INTO contacts (
        name,
        job,
        email
    )
    VALUES (:name, :job, :email)
    """
)

if prepared:
    data = [
        ("Davinia", "Data Analyst", "davinia@example.com"),
        ("Juli", "Senior Python Developer", "juli@example.com")
    ]

    for name, job, email in data:
        insertDataQuery.bindValue(2, email)  # Inserció per posició
        insertDataQuery.bindValue(":job", job)  # Inserció amb placeholder
        insertDataQuery.bindValue(":name", name)
        insertDataQuery.exec()
~~~

!!!note "Nota"
    Fixeu-se a les línies 32 i 68, on s'han utilitzat els dos estils de preparació de sentència SQL.
    Fixeu-se també a les línies 50-53, on s'està utilitzant la posició per a inserir amb *addBindValue*. Mentre que a les línies 75-78, s'està utilitzant els placeholders i la posició per a fer *binding*.

Aquest enfocament per crear consultes dinàmiques és útil quan voleu personalitzar les vostres consultes utilitzant valors que provenen de l'entrada de l'usuari.

!!!warning "Injecció d'SQL"
    Recordeu que cada vegada que utilitzeu l'entrada de l'usuari per completar una consulta en una base de dades, us enfronteu al risc de seguretat de la injecció SQL.
    
    A PySide, la combinació de .prepare(), .bindValue(), i .addBindValue() us protegeix completament dels atacs d'injecció SQL, de manera que **aquesta és la manera correcta** d'utilitzar entrades no fiables per completar les vostres consultes.

## Navegació pels registres en una consulta
Si executem una *SELECT*, el nostre objecte QSqlQuery mantindrà registres que complisquen amb els criteris de la consulta. Si cap dada coincideix amb els criteris, la nostra consulta estarà buida.

QSqlQuery proporciona un conjunt de mètodes que ens permeten navegar a través dels registres, o files, resultat d'una consulta:

| Mètode                       | Navega a ...                      |
| ---------------------------- | --------------------------------- |
| .next()                      | El següent registre               |
| .previous()                  | El registre anterior              |
| .first()                     | El primer registre                |
| .last()                      | L'últim registre                  |
| .seek(index, relative=False) | El registre en la posició *index* |

Per a accedir als valors dels registres o columnes, els següents mètodes també ens poden ser útils:

| Mètode           | Recupera                                          |
| ---------------- | ------------------------------------------------- |
| .value(index)    | El valor de la columna *index*                    |
| .record()        | El registre actual                                |
| .indexOf(column) | L'índex de la columna *column*. -1 si no existeix |


Tots els mètodes tornen *True* o *False*, i ens permeten navegar a través del resultat d'una consulta sense haver de consultar de nou la base de dades. Els podem utilitzar en bucles per recórrer tots els registres.

~~~python
import sys
import os

from PySide6.QtSql import QSqlDatabase, QSqlQuery

connection = QSqlDatabase.addDatabase("QSQLITE")
connection.setDatabaseName(
    os.path.join(os.path.dirname(__file__),
                 "contacts.sqlite")
)

if not connection.open():
    print("Error connectant a la base de dades!",
          f"Database Error: {connection.lastError().databaseText()}")
    sys.exit(1)
else:  # Connected
    query = QSqlQuery()
    if query.exec("SELECT name, job, email FROM contacts"):
        while query.next():
            print(
                f"Nom: {query.value('name')} \tTreball: {query.value('job')} \tCorreu: {query.value('email')}")
    query.finish()
~~~

!!!warning "Tancament de la consulta"
    Tingau en compte que mentre tenim una consulta activa, l'accés a la base de dades estarà bloquejat i des d'una aplicació externa no es podran modificar els seus registres fins que la desactivem. Per fer-ho, i a més alliberar la memòria que té associada, utilitzem el mètode **.finish()**. 
    
    Pots fer la prova posant un punt d'interrupció al *while* i amb una altra aplicació intentar modificar els registres. Et donarà un error:
    
    *SQL Error [5]: [SQLITE_BUSY] The database file is locked (database is locked)*.

## Tancament i eliminació de connexions de base de dades

Per tancar una connexió a PySide, utilitzem el mètode **.close()** de la connexió. Aquest mètode tanca la connexió i allibera els recursos adquirits. També invalida els objectes QSqlQuery associats perquè no poden funcionar correctament sense una connexió activa. Amb el mètode **.isOpen()** podem comprovar si la connexió està oberta o no.

!!!warning "Tancament de connexions"
    Tingueu en compte que els objectes QSqlQuery queden a la memòria després de tancar la seua connexió associada, de manera que hem d'inactivar les consultes amb *.finish()* o *.clear()*, o suprimir l'objecte QSqlQuery abans de tancar la connexió.

!!!note "Reutilització de connexions"
    Podem reobrir i reutilitzar qualsevol connexió prèviament tancada ja que *.close()* no elimina les connexions de la llista de connexions disponibles.

També podem eliminar completament les connexions de la nostra base de dades mitjançant **.removeDatabase()**.

En definitiva, primer tanquem les consultes amb *.finish()*, després tanqueu la base de dades amb *.close()*, i finalment eliminem la connexió amb *.removeDatabase()*.

~~~python
import sys
import os

from PySide6.QtSql import QSqlDatabase, QSqlQuery

connection = QSqlDatabase.addDatabase("QSQLITE")
connection.setDatabaseName(
    os.path.join(os.path.dirname(__file__),
                 "contacts.sqlite")
)

if not connection.open():
    print("Error connectant a la base de dades!",
          f"Database Error: {connection.lastError().databaseText()}")
    sys.exit(1)
else:  # Connected
    query = QSqlQuery()
    if query.exec("SELECT name, job, email FROM contacts"):
        while query.next():
            print(
                f"Nom: {query.value('name')} \tTreball: {query.value('job')} \tCorreu: {query.value('email')}")
    query.finish()
    connection.close()
    print("Connexions disponibles: {}".format(QSqlDatabase.connectionNames()))
    QSqlDatabase.removeDatabase(QSqlDatabase.database().connectionName())
    print("Connexions disponibles: {}".format(QSqlDatabase.connectionNames()))
~~~