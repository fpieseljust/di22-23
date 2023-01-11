## Aplicació de llista de tasques per mòdul

Es pretén desenvolupar una aplicació que ens permeta gestionar les tasques per cadascun
dels mòduls que estem cursant.
A la finestra principal tindrem:

- A la part esquerra una llista de mòduls.
- A la part dreta una llista de tasques.
- Al fer clic sobre qualsevol dels mòduls es carregaran les tasques corresponents a
eixe mòdul.
- L’aplicació ens permetrà insertar, eliminar i editar tant els mòduls com les tasques.
- Abans d’eliminar es demanarà confirmació.
- La informació estarà guardada en una base de dades amb dos taules relacionades.

!!!warning "Compte"
    Hem d'aplicar el què hem vist en la teoria d'aquesta unitat per millorar el codi de l'aplicació que estavem desenvolupant, tant la part d'accés a les dades com el patró Model/View.

    Alguns mètodes útils per a realitzar la tasca seran model.setFilter, view.hideColumn, model.removeColumn...

    Com sempre, consulteu la documentació de [QSqlTableModel](https://doc.qt.io/qtforpython/PySide6/QtSql/QSqlTableModel.html) i [QTableView](https://doc.qt.io/qtforpython/PySide6/QtWidgets/QTableView.html) per a més informació.