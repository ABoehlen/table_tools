# table_tools

## Purpose
On the one hand, Table Tools simplifies the work with dbf tables within ArcView 3 by enabling frequently required functions, which are normally only accessible via menus or require several steps, to be carried out with as few mouse clicks as possible. On the other hand it offers the possibility to perform complex calculations in these tables. In contrast to the standard Field Calculator, Table Tools allows the use of largely freely programmable avenue code for calculations. In addition, this code is directly in the table and does not have to be re-entered into the field calculator each time calculations are repeated. Finally, Table Tools also offers an export function to convert the tables into the easy-to-print HTML format.

## Background
ESRI's ArcView GIS is a rather old (1995 – 2002), but for many purposes still useful GIS application. Due to its age it's very fast on modern hardware and has full support for the still widely used vector data format _Shape_ (SHP).

Avenue is ArcView's built-in object oriented scripting language. "By using Avenue, you can customize the program and further extend its power", describes Amir H. Razavi the language's purpose in his 1999 book \[1\].

"dBASE was one of the first database management systems for microcomputers and the most successful in its day. […] dBASE's underlying file format, the .dbf file, is widely used in applications needing a simple format to store structured data." \[2\]. ArcView supports different formats to store tabular data but dBASE files are the only ones that can be edited within ArcView.

## System requirements
An ArcView GIS 3.x installation is required.

## Installation
Download the repository into your desired directory:

```
cd <directory>
git clone https://github.com/ABoehlen/table_tools
```

* Copy TableTools.avx into the ext32 directory of your ArcView installation.
* Open ArcView GIS with a new empty project or with an existing one.
* Choose File –> Extensions
* Turn on the Extension TableTools and close the Extension dialog again.
* In the Table documents add an existing table or create a new one.
* In the button list of the Table document GUI click on the new button "Start TableTools toolbar". The toolbar should open.

## License

This project is licensed under the MIT License - see the LICENSE file for details

***

# Dokumentation

Currently only available in German.

## Inhalt
* Zweck
* Aktuelle Version
* Installation und Start des Programms
* Beschreibung der Funktionen
* Berechnungen mit dem programmierbaren Calculate-Tool durchführen
	* Detaillierte Beschreibung
	* Beispiele
	* Fehlermeldungen
* Anhang

## Zweck
*Table Tools* erleichtert einerseits die Arbeit mit dbf-Tabellen innerhalb von ArcView, indem oft benötigte Funktionen, die standardmässig nur über Menus erreichbar sind oder mehrere Schritte erfordern, mit möglichst wenigen Mausclicks erledigt werden können. Zum andern bietet es die Möglichkeit in diesen Tabellen komplexe Berechnungen durchzuführen. Im Gegensatz zum standardmässigen *Field Calculator* kann mit *Table Tools* weitgehend frei programmierbarer Avenue-Code für Berechnungen verwendet werden. Zudem steht dieser Code direkt in der Tabelle und muss bei sich wiederholenden Berechnungen nicht jedesmal wieder neu in den *Field Calculator* eingegeben werden. Schlussendlich bietet *Table Tools* auch eine Exportfunktion, um die Tabellen ins einfach zu druckende HTML-Format zu konvertieren.

## Aktuelle Version

V1.5, 28.05.2017

## Installation und Start des Programms

* kopieren des Files "TableTools.avx" ins *ext32*-Verzeichnis der ArcView-Installation
* Neustart von ArcView und über *File –> Extensions* den Extensions-Dialog öffnen. Dort den Eintrag "TableTools" aktivieren
* über den neuen Button die «Table Tools-Toolbar» öffnen.

## Beschreibung der Funktionen

### Starts or stops edit session (Tabellenbearbeitung starten oder beenden)

Damit startet man eine Edit-Session auf der Tabelle oder beendet sie, falls sie schon läuft. Diese Funktion ist grundsätzlich nur für Standard-Operationen notwendig. Alle Tools der *Table Tools* laufen auch ohne aktive Edit-Session, indem im Hintergrund, falls erforderlich, automatisch eine solche aufgerufen wird (Ausnahme: Zellen bearbeiten).

### Creates a new, empty table (Neue Tabelle erstellen)

Dieses Tool erstellt eine neue, leere Tabelle im dbf-Format. Name und Speicherort sind im Dialog anzugeben.

### Copies the value of the selected records (Zeilen kopieren)

Kopiert den Inhalt selektierter Zeilen in eine Zwischenablage. Dies tangiert die «normale» Zwischenablage nicht.

### Pastes the copied record values to the selected records (kopierte Zeilen einfügen)

Kopierte Zeilen können so in selektierte Zeilen eingefügt werden. Es wird nur so viel Inhalt eingefügt, wie Zeilen selektiert sind. Wenn mehr Zeilen kopiert wurden, als beim Einfügen selektiert sind, geht der Inhalt der letzten Zeilen verloren.
Wenn beim Einfügen mehr Zeilen selektiert sind, als beim Kopieren, so wird der Inhalt wiederholend eingefügt. Der kopierte Inhalt steht so lange zur Verfügung, bis neue Zeilen kopiert werden

### Adds one or more records to the table (neue Zeilen anfügen)

Fügt neue Zeilen unten an die Tabelle an. Das Einfügen an einer beliebigen Position ist nicht möglich. Die Anzahl anzufügender Zeilen muss angegeben werden. Default ist 1. Die maximale Anzahl Zeilen ist nahezu beliebig. Da eine dbf-Tabelle maximal 2 GB gross sein kann, sind bei einer minimalen Feldlänge von 1 Zeichen theoretisch nahezu 1 Milliarde Zeilen möglich.

### Adds a new field to the table (Neues Feld hinzufügen)

Fügt ein neues Feld (Spalte) zur Tabelle hinzu. Neue Felder werden stets rechts an das letzte existierende Feld angefügt. Ein Einfügen an einer beliebigen Position ist nicht möglich, jedoch können Felder nachträglich verschoben werden (siehe dazu nächstes Tool). Es sind maximal 255 Felder pro Tabelle möglich.

Folgende Angaben müssen gemacht werden:
* Name
	* Feldnamen in dbf-Tabellen dürfen maximal 8 Zeichen lang sein. Wird ein längerer Name eingegeben, erscheint eine Warnung, wonach der Name entsprechend gekürzt und der eingegebene, längere Name als *Alias* verwendet wird. Dieser *Alias* wird aber nur im ArcView-Projekt gespeichert, und nicht in der Tabelle selbst. Mit dem Field Aliases-Tool können die Feld-Aliase aber auch in ein ein separates File exportiert werden. Beschreibung siehe unten.
* Type
	* Folgende Datentypen stehen zur Auswahl:
		* Number (Ganz- oder Fliesskommazahl)
		* String (Text)
		* Boolean (wahr/falsch-Wert)
		* Date (Datumstyp)
* Width
	* Dieses Feld steht nur bei den Datentypen *Number* und *String* zur Verfügung. Zu definieren ist die Anzahl Zeichen, die in dem Feld pro Zeile gespeichert werden sollen.
		* **Achtung**: Textfelder können maximal 254 und numerische Felder maximal 20 Zeichen lang sein.
* Decimal Places
	* Dieses Feld steht nur beim Datentyp *Number* zur Verfügung. Zu definieren ist die gewünschte Anzahl Nachkommastellen.
		* **Achtung**: Die Nachkommastellen sind Teil der Gesamtfeldlänge. Wenn also z.B. eine *Width* von 10 Zeichen und 3 *Decimal Places* definiert werden, heisst das, dass 6 Zeichen vor dem Komma, der Dezimaltrenner (.) und 3 Zeichen nach dem Komma verfügbar sein werden. Es sind somit maximal 18 Nachkommastellen möglich, wenn die Zahl vor dem Komma lediglich einstellig ist.
		
### Moves field(s) (Felder verschieben)

Felder können grundsätzlich per drag&drop verschoben werden. Dieses Tool vereinfacht diesen Schritt und bietet zusätzliche Funktionen.

Wird das Tool bei einem aktivierten Feld aufgerufen, so ist im Dialog anzugeben, hinter welchem bestehenden Feld dieses neu einzuordnen ist. Wird das Tool aufgerufen, ohne dass ein Feld aktiv ist, so ist zuerst auszuwählen, welche(s) Feld(er) verschoben werden soll(en). Da nicht mehrere Felder aktiviert werden können, ist dies also eine Möglichkeit, mehrere Felder auf einmal zu verschieben. Als nächstes ist ebenfalls anzugeben, nach welchem Feld der zu verschiebende Inhalt neu einzuordnen ist.

**Achtung**
Die Felder werden nicht wirklich verschoben, sondern nur so angezeigt. Die Einstellungen können aber im aktiven ArcView-Projekt (.apr) gespeichert werden, nicht jedoch in der Tabelle selbst. Soll dies bezweckt werden, so muss die Tabelle mit *File –> Export* als neue Tabelle gespeichert werden.

### Set Field Diesplay Width (Feldbreite einstellen)

Stellt die Breite aller Felder zusammen ein. Dies kann in drei verschiedenen Varianten erfolgen:
1. pro Klick wird die Breite aller Felder um 20% erhöht
2. wird gleichzeitig die Shift-Taste betätigt, reduziert sich die Breite aller Felder pro Klick um 20%
3. wird gleichzeitig die Ctrl-Taste betätigt, wird jedes Feld so breit, wie es mindestens erforderlich ist, damit weder ein eingetragener Wert noch die Feldbezeichnung abgeschnitten wird.

### Deletes the selected record(s) or the selected field from the table (Zeile(n) oder Feld löschen)

Je nach Ausgangslage verhält sich dieses Tool wie folgt:
* nichts ist selektiert bzw. aktiv
	* Es erscheint eine Fehlermeldung, wonach eine Zeile oder ein Feld selektiert werden sollen
* eine oder mehrere Zeilen sind selektiert
	* Diese Zeilen werden nach Rückfrage gelöscht
* ein Feld ist aktiviert
	* Dieses Feld wird nach Rückfrage gelöscht
* eine oder mehrere Zeilen sind selektiert und ein Feld ist aktiviert
	* Die selektierte(n) Zeile(n) werden nach Rückfrage gelöscht. Das Feld bleibt stehen, ist aber weiterhin aktiviert. Um es auch zu löschen, ist das Tool erneut zu betätigen.

### Selects rows between two defined records (Selektiert definierte Zeilen)

Es ist die erste und die letzte zu selektierende Zeile zu definieren. Dies kann in auf- oder absteigender Reihenfolge geschehen. Zuvor schon existierende Selektionen werden aufgehoben.

### Selects records in the table (Zeilen manuell selektieren)

Hiermit können Zeilen manuell selektiert werden, wobei dies immer ganze Zeilen betrifft. Mehrere Zeilen können mit gedrückter Shift-Taste selektiert werden. (entweder Shift + Click oder Shift, Click und ziehen)

### Edits cell (Zellen bearbeiten)

Dient dem manuellen Erfassen und Bearbeiten von Werten in der Tabelle. Um das Tool zu verwenden, muss zuerst mittels dem Starts or stops edit session-Tool eine Edit-Session gestartet werden. Navigiert in der Tabelle wird wie folgt:
* eine Spalte nach rechts: *Tab*
* eine Spalte nach links: *Shift + Tab*
* eine Zeile nach unten: *Enter*
* eine Zeile nach oben: *Shift + Enter*

**Achtung**
Eingaben müssen immer mit *Enter* oder *Tab* quittiert werden. Nur in eine andere Zelle clicken reicht nicht!

### Select none (Selektionen aufheben)

Hebt alle vorhandenen Selektionen auf.

### Sorts the table ascending or descending permanently (Tabelle dauerhaft sortieren)

Bei bereits aktivem Feld erscheint ein Dialog, wo definiert werden muss, ob die Tabelle nach diesem Feld auf- oder absteigend sortiert werden soll. Mit *Cancel* kann die Aktion abgebrochen werden. Ist kein Feld selektiert, muss erst in einem separaten Dialog das zu sortierende Feld definiert werden.

*Number*-Felder werden numerisch, *String*-Felder (auch wenn sie nur Zahlen beinhalten) lexikografisch sortiert.

### Changes the field type or makes a copy of the field (Feldtyp ändern und/oder eine Kopie des Feldes erzeugen)

Grundsätzlich kann die Felddefinition (Name, Datentyp) in einer dbf-Tabelle nicht verändert werden. Mit diesem Tool wird dies dennoch ermöglicht, indem im Hintergrund eine Kopie des Feldes mit den gewünschten neuen Felddefinition angelegt und das originale Feld anschliessend (sofern gewünscht) gelöscht wird. Folgende Schritte sind erforderlich:

1. Im ersten Dialog ist auszuwählen, welches Feld modifiziert werden soll. Es können auch mehrere ausgewählt werden.
2. Der Name des neuen Feldes ist zu definieren. Wenn der originale Name erhalten werden soll, ist der Dialog mit Shift und OK zu schliessen.
3. Wenn nur der Name geändert werden soll, kann der nächste Dialog mit *No* geschlossen werden. Falls weitere Modifikationen gewünscht sind, ist *Yes* auszuwählen (Default)
4. Falls zuvor *Yes* ausgewählt wurde, sind nun die gewünschten Felddefinitionen anzugeben. Dies sind die gleichen wie beim Erstellen eines neuen Feldes (siehe dort). Abweichend dazu gilt, dass der *Type* in Textform anzugeben ist (gemäss im Dialog aufgeführten Kürzel)
5. Zum Schluss muss angegeben werden, ob das ursprüngliche Feld gelöscht werden soll. Soll der gleiche Name wiederverwendet werden, wird dies empfohlen, da andernfalls zwei Felder mit dem gleichen Namen resultieren, was später Probleme verursachen kann. Wird dennoch *No* ausgewählt, erhält das ursprüngliche Feld ein Aliasname mit der Erweiterung "_ori", der aber nur innerhalb des ArcView-Projektes gespeichert werden kann, nicht in der Tabelle selbst!

Wurden ursprünglich mehrere Felder zum Modifizieren ausgewählt, so läuft dieser Prozess für jedes betreffende Feld einzeln durch.

### Copies the field content and pastes it to another existing field (Inhalt eines Feldes in ein anderes existierendes Feld übertragen)

Diese Funktion ähnelt der vorherigen, mit dem Unterschied, dass hier der Inhalt eines Feldes in ein anderes Feld übertragen wird, welches bereits existiert. Dabei wird, falls nötig, automatisch eine Konversion von Zahlen- in Textfelder oder umgekehrt vorgenommen. Andere Feldtypen (boolean, date) werden nicht unterstützt.

Wenn eine Selektion vorhanden ist, wird nur der Inhalt der selektierten Zeilen übertragen.

### Combines the content of selected fields to a new string and writes it to a user defined field (Felder verbinden)

Der Inhalt der im ersten Dialog ausgewählten Felder wird zusammengefügt (konkateniert) und in einem neuen Feld gespeichert (Typ: String). Ein Separator kann angegeben werden, wenn gewünscht. Ebenso ist der Name und die *Width* des neuen Feldes zu definieren.

### Calculates the sum and writes it into the selected cell (Summe aller Zeilen)

Berechnet die Summe aller Zeilen des aktiven Feldes. Die Zeile, in die das Ergebnis zu schreiben ist, muss vorher selektiert werden. Wenn keine, oder mehr als eine Zeile selektiert sind, kann das Tool kein Ergebnis liefern.

Das Berechnen der Summe funktioniert in Zahlen und Textfeldern. Befindet sich in einem Textfeld in einer oder mehreren Zeilen «echter» Text, wird dieser als 0 interpretiert.

Im Gegensatz zu numerischen Feldern, wo die Anzahl Nachkommastellen bereits im Feld definiert ist, muss in Textfeldern die gewünschte Anzahl Nachkommastellen angegeben werden. Default ist 3, aber beliebige andere Werte sind auch möglich.

### Calculates the sum from selected cells and writes it into the defined cell (Summe ausgewählter Zeilen)

Wenn nicht alle Zeilen addiert werden sollen, muss dieses Tool benutzt werden. Auch hier muss das Feld aktiviert werden, in welchem addiert werden soll. Zudem müssen mindestens 2 Zeilen selektiert sein, nämlich jene mit dem zu addierenden Inhalt. Nach dem Aktivieren des Tools muss bei Textfeldern die Anzahl Nachkommastellen (siehe oben) und die Zeile angegeben werden, in welche das Resultat geschrieben werden soll.

Das Berechnen der Summe funktioniert in Zahlen und Textfeldern. Befindet sich in einem Textfeld in einer oder mehreren Zeilen «echter» Text, wird dieser als 0 interpretiert.

Die zuvor selektierten Zeilen bleiben nach Ausführen der Funktion weiterhin selektiert.

In Textfeldern kann bei entsprechender Angabe der Nachkommastellen die Summe korrekt gerundet werden.

### Calculates the expression from the second row (Programmierbares Calculate-Tool, welches den Ausdruck, der in der zweiten Zeile steht, berechnet)

Diese Funktion steht nur in Textfeldern zur Verfügung. Sie erwartet, dass in der zweiten Zeile ein Ausdruck in Form von Avenue-Code steht. Existiert ein Feld namens «Type» können weitere Codezeilen an beliebiger Stelle eingefügt werden. Falls benutzerdefinierte Variablen verwendet werden, müssen diese in der ersten Zeile initialisiert werden. Dadurch sind wesentlich komplexere Berechnungen als mit dem *Field Calculator* möglich. Zudem ist es von Vorteil, dass der Ausdruck fix in der Tabelle steht und nicht bei jeder Berechnung wieder neu eingegeben werden muss. Dies ist vor allem bei wiederholenden Berechnungen von Vorteil.

Wenn eine Selektion vorhanden ist, wird die Berechnung nur auf den selektierten Zeilen durchgeführt.

Wird das Tool mit gedrückter Shift-Taste aufgerufen, so wird der bestehende Feldinhalt mit Ausnahme von Codezeilen gelöscht.

Eine detaillierte Dokumentation mit Beispielen findet sich im nächsten Kapitel.

### Calculates index (Berechnet einen Index)

Dieses Tool eignet sich sehr gut zum Durchnummerieren aller Zeilen. Es ist ein Initialwert und eine Schrittweite einzugeben. Dies funktioniert sowohl in Zahlen- wie auch in Textfeldern (hier können allerdings nur Ganzzahlen verwendet werden).

Wenn eine Selektion vorhanden ist, wird die Berechnung nur auf den selektierten Zeilen durchgeführt.

### Calculates dates inside the defined range (Berechnet Datumswerte zwischen zwei definierten Eckdaten)

Mit diesem Tool lässt sich ein Datumsfeld mit Datumswerten füllen. Es ist ein Start- und ein Enddatum anzugeben. Das Enddatum wird nur erreicht, wenn genügend Zeilen vorhanden sind. Sind mehr Zeilen vorhanden als nötig, bleiben die übrigen leer

Wenn eine Selektion vorhanden ist, wird die Berechnung nur auf den selektierten Zeilen durchgeführt.

Start- und Enddatum müssen in folgender Formatierung eingegeben werden:
<Jahr><Monat><Tag>

Die Funktion geht auch mit Schaltjahren korrekt um.

Um aus der Standardformatierung «sprechende» Datumsangaben abzuleiten, ist programmierbare Calculate-Tool zu verwenden (siehe Beispiele im nächsten Kapitel). 

### Finds duplicate values (Findet mehrfache identische Einträge)

Mit diesem Tool lässt sich ein Feld nach mehrfach vorhandenen identischen Einträgen durchsuchen. Folgende Optionen stehen nach der durchgeführten Suche zur Auswahl:
* Apply previous selection: Damit lässt sich eine vor der Suche allenfalls vorhandene Selektion wieder herstellen.
* Report/select unique values: Damit wird von jedem vorkommenden Wert die zugehörige Zeile selektiert. Bei mehrfach vorhandenen Einträgen wird die Zeile mit dem erstmaligen Vorkommen selektiert.
* xy (n records): Hier steht aufgeführt, welche Werte mehrfach und wie oft gefunden werden. Mittels eines Clicks auf diesen Eintrag werden alle betreffenden Zeilen in der Tabelle selektiert.

### Shows the properties of the active field (Zeigt die Eigenschaften des aktiven Feldes an)

Bedingung ist, dass ein Feld aktiv ist. Das Tool zeigt einem anschliessend den Datentyp und die weiteren Eigenschaften (falls vorhanden) an. Dies sind:
* Feldname: Erscheint in der Titelleiste des Fensters
* Feld-Alias: erscheint, falls vorhanden, in Klammer hinter dem Feldnamen
* Feldtyp
* Width: nur bei den Datentypen *String* und *Number*
* Precision: nur beim Datentyp *Number*

### Exports and loads field aliases (Exportiert Feld-Aliase in Textdatei und importiert solche Dateien wieder)

Im dbf-Format kann ein Feldname maximal 8 Stellen lang sein. Dies ist oft zu wenig um sprechende Namen zu vergeben. Daher gibt es die Möglichkeit, zusätzlich einen längeren Namen als so genannten Alias zu definieren. Dieser Name wird aber nicht in der Tabelle gespeichert, sondern nur im aktuellen ArcView-Projekt (.apr). Daher gehen die Aliase verloren, wenn eine entsprechende Tabelle in ein anderes Projekt importiert wird.

Mit diesem Tool ist es möglich, die Aliase in ein separates Textfile zu exportieren und bei Bedarf wieder zu importieren. Wird das Tool direkt aufgerufen, erscheint der Export-Dialog, wo das zu exportierende File definiert werden muss. Die Dateiendung lautet .alias.

Zum Importieren muss das Tool mittels Shift-Click aufgerufen werden. Im Dialog ist dann das zu importierende .alias-File auszuwählen. 

### Exports the table to HTML (Exportiert Tabelle als HTML-Dokument)

Es kann zwischen verschiedenen Stilen ausgewählt werden. Grundsätzlich gilt folgendes:
* Der Name des Tabelle wird als Titel gesetzt.
* Der Inhalt von Zahlenfeldern wird rechtsbündig, jener von Textfeldern linksbündig angeordnet (wie in ArcView).
* Der Feldname wird in fetter Schrift und in der Mitte des Feldes dargestellt.

### Close Table Tool toolbar (Schliesst Table Tools-Toolbar)

Button zum Schliessen der Table Tools-Toolbar

***

## Berechnungen mit dem programmierbaren Calculate-Tool durchführen
### Detaillierte Beschreibung

Dies ist die mit Abstand mächtigste und vielseitigste Funktion. Da mit frei programmierbarem Avenue-Code gearbeitet wird, eröffnen sich damit mannigfaltige Möglichkeiten. Die Inspiration dazu stammt von Franz Kottiras Commodore 64-Programm «datatool» von 2003. \[3\]

Um diese Funktion zu verwenden, muss das betreffende Feld vom Typ «String» sein. Um genügend Platz auch für komplexe Ausdrücke zur Verfügung zu haben, empfiehlt es sich, die Anzahl Zeichen möglichst hoch zu setzen, z.B. auf 100. Maximal sind 254 Zeichen möglich.

Berechnungen sind für ein gesamtes Feld möglich oder nur für eine bestimmte Zeile.

Die ersten zwei Zeilen der betreffenden Tabelle stehen nicht für die Aufnahme der Daten zur Verfügung, sondern dienen folgenden Zwecken:

1. Zeile: Definition der gewünschten Anzahl Nachkommastellen (Default: 3), der Anzahl Berechnungsdurchläufe (Default: 1) und Initialisierung benutzerdefinierter Variablen. Damit diese vom System verarbeitet werden können, müssen sie «global» sein, d.h. mit dem Unterstrich (_) beginnen. Wenn die Defaultwerte übernommen und keine benutzerdefinierte Variablen benötigt werden, bleibt diese Zelle leer. Mehrere Variablenzuweisungen werden einfach durch Leerzeichen getrennt hintereinander geschrieben. 
2. Zeile: Hier sind die Anweisungen, die für das gesamte Feld gelten, als Avenue-Code zu formulieren. Dieser Code wird, sofern nichts selektiert ist, für jede Zeile angewendet, andernfalls nur für die selektierten Zeilen. Im Gegensatz zu «normalem» Programmcode werden die Anweisungen hier einfach durch Leerzeichen getrennt hintereinander geschrieben.

Da bei gewissen Berechnungen die Zellen leer sein sollen, gibt es die Möglichkeit, allfällig bestehende Daten im betreffenden Feld zu löschen. Dazu muss das Tool mit gedrückter Shift-Taste aufgerufen werden. Die beiden ersten Zeilen, sowie weitere Code- und Resultatszeilen (siehe unten) werden durch diese Funktion nicht beeinflusst.

Nebst den vom Benutzer zu definierenden Variablen stehen einige Systemvariablen zur Verfügung, die nicht durch benutzerdefinierte Variablen überschrieben werden dürfen. Andernfalls riskiert man ein fehlerhaftes Verhalten oder sogar einen Absturz. Ebenso sollten die Systemvariablen `_z` und `_f` nur in Ausdrücken oder Bedingungen, d.h. lesend verwendet werden, da sie durch die laufende Berechnung automatisch mit Werten gespiesen werden.

### Systemvariablen und ihre Bedeutung
* `_a`: Diese Variable speichert die Anzahl der Durchläufe. Normalerweise ist nur ein Durchlauf nötig, aber manche mathematische Operationen erfordern zwei oder eine noch höhere Anzahl von Durchläufen. Default ist 1.
* `_d`: Dieser Variable speichert die gewünschte Anzahl Nachkommastellen. Bis zu 20 sind möglich. Default ist 3.
* `_r`: Diese Variable nimmt das Ergebnis der Feldberechnungen (2. Zeile) auf. Was schlussendlich in die einzelnen Zeilen geschrieben werden soll, muss daher immer dieser Variable zugewiesen werden
* `_c`: Diese Variable nimmt das Ergebnis der Berechnungen einzelner Zeilen auf. Was in die folgende Resultatszeile geschrieben werden soll, muss daher immer dieser Variable zugewiesen werden
* `_z`: In dieser Variable ist die Zahl des aktuellen Durchlaufs gespeichert. Anhand des Wertes von `_z` kann das Verhalten jedes Durchlaufs gesteuert werden.
* `_f`: Diese Variable speichert die Nummer der aktuellen Zeile, unabhängig davon, ob Zeilen selektiert sind oder nicht. Wird diese Variable in Bedingungen verwendet, kann z.B. genau gesteuert werden, in welche Zeile welches Ergebnis geschrieben werden soll. 
* `_<Feldname>`: Jedes Feld kann als Variable mit seinen Namen für Berechnungen verwendet werden. Damit die Variable gültig ist, muss allerdings, wie bei allen anderen Variablen der Unterstrich (_) vorangestellt werden. Verwendet wird immer der effektive Feldname, nicht der allenfalls vorhandene Aliasname.

**Achtung**
Da Avenue nicht case-sensitive ist, dürfen keine Feldnamen verwendet werden, die gleich lauten wie Systemvariablen. So würde ein Feld namens «A» zu einem Konflikt mit der Variable `_a` führen, da das Feld «A» intern mit `_A` angesprochen wird, was mit `_a` identisch ist.

## Beispiele

### Quadratwurzel von Daten in anderem Feld berechnen

**Aufgabe**
Von den Werten in einem Feld sind Quadratwurzeln zu berechnen, welche in ein anderes Feld geschrieben werden. 

Zunächst sind zwei Felder anzulegen, welche z.B. mit «A1» und «B1» bezeichnet werden können. Feld A1, welches die zu berechnenden Werte aufnimmt, kann vom Typ *Number* oder vom Typ *String* sein. Feld B1 muss dagegen vom Typ *String* sein, da in die zweite Zeile die Anweisung zum Berechnen der Quadratwurzel geschrieben wird. Diese Anweisung lautet: `_r=_A1.Sqrt`. Die Resultatsvariable `_r` soll also den Wert des Feldes A1 aufnehmen, auf welchem die Quadratwurzel berechnet wurde. Dieser Wert wird über die Feldvariable `_A1` gewonnen, und die Methode `.Sqrt` berechnet daraus die Quadratwurzel:

|A1|B1|
|--|--|
|0.000| |
|0.000|_r=_A1.Sqrt|
|1.000|1.000|
|2.000|1.414|
|3.000|1.732|
|4.000|2.000|
|5.000|2.236|

### Prozentwerte anhand eines anderen Feldes berechnen

**Aufgabe**
Von den Werten in einem Feld sind die Prozentwerte zu deren Summe zu berechnen und in ein anderes Feld zu schreiben.

Wiederum werden die beiden Felder «A1» und «B1» angelegt. Feld A1 dient zur Aufnahme der absoluten Werte und Feld B1 soll die zugehörigen Prozente erhalten.

In diesem Fall sind zwei Durchgänge für die erfolgreiche Berechnung nötig. Im ersten Durchlauf werden die Werte in Feld A1 zusammengezählt und anschliessend daraus mittels eines Dreisatzes der prozentuale Wert ermittelt und in die Zellen von Feld B1 geschrieben.

Die Anzahl Durchgänge wird über die Systemvariable `_a` mit 2 festgelegt. Ebenso wird die benutzerdefinierte Variable `_x` in der ersten Zeile mit dem Wert 0 initialisiert

Nach dem Auslösen der Berechnung passiert folgendes: Im ersten Durchgang, d.h. wenn die Zählervariable `_z` den Wert 1 hat, wird `_x` mit den Werten jeder Zeile von `_A1` aufsummiert (`_x=_x+_A1`), sodass nach der letzten Zeile die Summe aller Werte des Feldes A1 darin gespeichert ist.
Im zweiten Durchgang, d.h. sobald die Zählervariable `_z` nicht mehr den Wert 1 hat (`else`), wird mittels des Dreisatzes «aktueller Wert von Feld A1 mal 100 geteilt durch die Summe von Feld A1» der Prozentwert jedes Wertes ermittelt und der Systemvariable `_r` zugewiesen, damit sie in die Zellen des Feldes B1 geschrieben werden. Danach präsentiert sich die Tabelle wie folgt:

|A1|B1|
|--|--|
|0.000|_a=2 _x=0|
|0.000|if (_z=1)  then  _x=_x+_A1  else  _r=_A1*100/_x  end|
|1.000|3.704|
|3.000|11.111|
|7.000|25.926|
|11.000|40.741|
|5.000|18.519|

Statt mit allen Werten kann auch nur mit einer Auswahl davon gearbeitet werden, indem man die gewünschten Zeilen selektiert. So lassen sich die prozentualen Anteile der Summe einer beliebigen Teilmenge berechnen.

### Berechnungen in einzelnen Zeilen durchführen

Soll das Resultat von Berechnungen nur in eine bestimmte Zeile geschrieben werden, kann – statt umständlich mit Selektionen oder Zeilennummern zu arbeiten – auch in beliebigen weiteren Zeilen Programmcode gesetzt werden. Dazu ist ein Feld namens «Type» anzulegen (String, 4 Characters reichen), wo jede betreffende Zeile mit dem Eintrag «Code» gekennzeichnet wird. Für das Ergebnis ist eine weitere Zeile vorzusehen, die mit der Bezeichnung «Res» markiert wird.

**Aufgabe**
Spaltensumme berechnen

Das vorherige Beispiel lässt sich erweitern, indem geprüft wird, ob die ermittelten Prozentwerte in der Summe 100 ergeben.

Zum Aufsummieren wird die benutzerdefinierte Variable `_y` verwendet, die mit 0 initialisiert wird. In der Feldberechnung wird mit der Anweisung `_y=_y+_r` dafür gesorgt, dass die einzelnen Werte addiert werden. 

Um die Summe in die Tabelle zu schreiben, muss der Wert der Variable `_y` an die Systemvariable `_c` übergeben werden, was in derjenigen Zeile geschieht, die im Feld «Type» mit «Code» gekennzeichnet ist. Unmittelbar darunter befindet sich die Zeile mit der Bezeichnung «Res», die nach der Berechnung das Ergebnis aufnimmt. Wenn gewünscht, können zwischen der «Code»- und der «Res»-Zeile noch weitere normale Datenzeilen stehen:

Nach der Berechnung steht das Ergebnis in der mit «Res» gekennzeichneten Zeile:

|Type|A1|B1|
|----|--|--|
| |0.000|_a=2 _d=5 _x=0 _y=0|
| |0.000|if (_z=1)  then  _x=_x+_A1  else  _r=_A1*100/_x  _y=_y+_r  end|
| |1.000|3.70370|
| |3.000|11.11111|
| |7.000|25.92593|
| |11.000|40.74074|
| |5.000|18.51852|
|Code|0.000|_c=_y|
|Res|0.000|100.00000|

Auch für diese Codezeile gilt dabei die in der ersten Zeile festgelegte Formatierung, hier `_d=5`.

### Sprechende Datumsangaben ableiten

Datumsangeben werden in dbf-Tabellen grundsätzlich in der Formatierung <Jahr><Monat><Tag> gespeichert. Hinter diesen Angaben stecken aber wesentlich mehr Informationen, die sich z.B. mit einem einfachen Ausdruck abrufen und in ein neues Textfeld schreiben lassen:

Mittels der Methode `.SetFormat` werden hier die vom System zur Verfügung gestellten *date components* genutzt, welche folgende Bedeutung haben:
* `dddd`: Name des Tages
* `ddd`: Name des Tages in Kurzform
* `dd`: Tag des Monats als zweistellige Ganzzahl
* `MMMM`: Name des Monats
* `MM`: Monat als zweistellige Ganzzahl
* `d`: Der Tag als Ganzzahl
* `yyyy`: Komplette Jahreszahl

Weitere zur Verfügung stehende Formatierungszeichen sind im Buch «ArcView GIS Developer's Guide» auf den Seiten 39 und 40 aufgeführt. \[1\]

Auszugebende Sonderzeichen (hier diverse Kommas) sind direkt in den Formatierungsstring aufzunehmen.

|Date|DateTxt|
|----|-------|
| | |
| |_r=_Date.AsDate.SetFormat("dddd, MMMM d, yyyy")|
|20230517|Wednesday, May 17, 2023|
|20230518|Thursday, May 18, 2023|
|20230519|Friday, May 19, 2023|
|20230520|Saturday, May 20, 2023|
|20230521|Sunday, May 21, 2023|

Weiteres Beispiel

|Date|DateTxt|
|----|-------|
| | |
| |_r=_Date.AsDate.SetFormat("ddd. dd.MM.yyyy")|
|20230517|Wed. 17.05.2023|
|20230518|Thu. 18.05.2023|
|20230519|Fri. 19.05.2023|
|20230520|Sat. 20.05.2023|
|20230521|Sun. 21.05.2023|

Da die Inhalte der Feldvariablen intern in Strings umgewandelt werden, muss im Ausdruck die Feldvariable explizit als Typ «Date» deklariert werden. Hier: `_Date.AsDate`

### Fehlermeldungen

Fehlermeldungen entstehen meist durch nicht deklarierte Variablen oder durch fehlerhaft formulierte Ausdrücke. Im Falle eines Fehlers beicht das Tool die Ausführung idealerweise mit der Meldung «Fehlerhafte Anweisung Script 2» ab.

Nach dem Quittieren dieser Meldung erscheint ein weiteres Fenster mit dem aus der Variablendefinition und der Expression temporär generierten Avenue-Code. So lässt sich leichter feststellen, wo der Fehler passiert ist.

Da frei programmierbarer Avenue-Code verwendet wird, kann es aber durchaus auch zu nicht sauber abgefangenen Fehlern kommen, die sich dann meist mit einer allgemeinen Fehlermeldung bemerkbar machen (z.B. «Wrong class for parameter 1 of request +. Got a(n) String, expected a(n) Number»). Einfaches Quittieren der Meldung reicht in diesen Fällen dann meist nicht mehr, da der Fehler bei der folgenden Tabellenzeile sofort wieder auftritt. Da hilft nur noch das erzwungene Beenden von ArcView. Schon daher empfiehlt es sich, vor Benutzung des Tools, das ArcView Projekt zu sichern und eine laufende Edit-Session zu beenden.

## Anhang

### Speicherbedarfsabschätzung

**Gerüst einer Tabelle**
|Aktion|Bytes pro Aktion|Total Bytes|
|------|----------------|-----------|
|neue, leere Tabelle|34|34|
|erstes Feld|+31|65|
|zweites Feld|+32|97|
|drittes Feld|+32|129|
|pro weiteres Feld|+32| |

**Inhalt der Tabelle (zusätzliche Bytes)**
|Aktion|1 Zeichen|2 Zeichen|8 Zeichen|16 Zeichen|254 Zeichen|
|------|---------|---------|---------|----------|-----------|
|1. Zeile|+3|+4|+10|+18|+256|
|2. Zeile|+2|+3|+9|+17|+255|
|3. Zeile|+2|+3|+9|+17|+255|
|pro weitere Zeile|+2|+3|+9|+17|+255|

***

## Literature
\[1\] Razavi, Amir H.: ArcView GIS Developer's Guide, 1999

\[2\] dBASE description on Wikipedia: https://en.wikipedia.org/wiki/DBase

\[3\] datatool - Commodore 64 Spreadsheet: https://www.webnet.at/c64/datatool.htm
