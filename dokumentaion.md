# Dokumentation Schulung LPIC-1

## Kommandos

### Aufbau von Kommandos:
```
<kommando> [-<kurzoption>]... [<argument>]...
<kommando> [--<langoption>]... [<argument>]...
```
Erklärung zur obigen Syntax (angelehnt an Manpages):

- `[  ]` was in eckigen Klammern steht, ist **optional** -> wir können also Optionen oder Argumente übergeben, **müssen** es aber nicht
- `...` die drei Punkte bedeuten, dass auch mehrere Optionen oder Argumente übergeben werden können

>[!NOTE]
> Es macht fast immer keinen Unterschied, ob wir die Option(en) vor oder nach den Argumenten schreiben:
> ```bash
> rm -r somedir
> rm somedir -r
>```

#### Beispiele
```
ls -l               # Übergabe der Option -l
ls /etc             # Übergabe des Arguments /etc
ls -la              # Übergabe mehrerer Optionen
ls -la /etc /home   # Übergabe mehrerer Optionen und Argumente
```
### Grundlegende Kommandos

- `whoami` gibt den aktuellen Benutzer aus
- `pwd` gibt das aktuelle Verzeichnis aus
- `cat` gibt den Inhalt einer Datei auf der Kommandozeile aus
- `less` ist ein sogenannter *Pager*: gibt den Inhalt einer Datei aus, mit der Möglichkeit zu scrollen, zu suchen etc. (Benutzung siehe Manpages)
- `history` zeigt alle bisher eingegebenen Kommandos inkl. eines Indexes an (s.u.)
- `ls` zeigt den Inhalt von Verzeichnissen an
- einige Optionen von `ls`:
  - `ls -a` ignoriert keine Einträge, die mit einem Punkt beginnen -> zeigt auch "versteckte" Dateien an
    - `ls -l` zeigt ausführliche Details zu den Dateien an
    - `su` *Substitue User* -> Benutzer wechseln
      - `su -l root` eine Shell mit Root-Rechten öffnen
      - `exit` beendet die aktuell laufende Shell 
      - `clear` *leert* die Seite, so dass unser *Prompt* sich ganz oben befindet, wir können aber auch die Tastenkombination `STRG+L` verwenden

## Hilfe auf der Kommandozeile

### Manpages

*Manual Pages*: eine Art Handbuch zu einzelnen Kommandos, mit Erklärungen zur Syntax, allen Optionen, teilweise Beispielen etc.

- `man <kommando>` 
- z.B. `man ls` Handbuchseite zum Kommando `ls`
- Suche in den Manpages: Eingabe von `/` gefolgt vom Suchbegriff, z.B. `/-l` sucht nach der Option `-l`
- zum nächsten Suchbegriff mit `n`
- zum vorherigen Suchbegriff mit `N`
- Manpage schliessen mit `q`
- zum Anfang der Manpage springen mit `g`
- ans Ende mit `G`

### Help

*Kurzhilfe* zu einem Kommando durch Übergabe der Option `--help` -> `ls --help`

## History

Eine Liste aller bisher eingegebenen Kommandos

- Blättern durch die History mit den Pfeiltasten oder `STRG+P` bzw. `STRG+N`
* gezielt ein bestimmtes Kommando aufrufen:
- nach Eingabe von `history` Aufruf von `!<index>` -> `<index>` ist der Index des Kommandos

## Aliase

Aliase sind selbstdefinierte Abkürzungen für Kommandos mit Optionen. Wir verwenden Aliase z.B. für häufig verwendete Kommandos mit Optionen oder auch Argumenten wie Pfadangaben.

Das Kommando `alias` an sich zeigt alle in der aktullen Shell gültigen Aliase an.

### Definition von Aliasen
```bash
alias <name-des-aliases>='<kommando> -<option> <argument>'
alias la='ls -a'
alias rm='rm -i'
alias somedir='cd ~/path/to/specific/dir/'
```
Wenn wir Aliase einfach so auf der Kommandozeile definieren, sind diese nur in der aktullen Shell gültig. Wollen wir Aliase persistent definieren (für alle neu geöffneten Shells bzw. auch nach einem Reboot), so müssen wir die Definition in dafür vorgesehene Dateien eintragen.

Dieses Konzept gilt nicht nur für Aliase, sondern generell für die Konfiguration unseres Systems.

Aliase werden z.B. direkt in der Datei `~/.bashrc` oder besser noch in der Datei `~/.bash_aliase` definiert (wenn wir als Shell die BASH verwenden).

> [!NOTE]
> Damit die neu eingerichteten Aliase gueltig werden, muss die entsprechende Datei neu eingelesen werden. Im Falle der `~/.bashrc` bzw. `~/.bash_aliases` kann dies folgende Art und Weise erfolgen:
> - neue BASH oeffnen
> - Kommando `source ~/.bashrc` bzw. `. ~/.bashrc`

## Variablen

### Umgebungsvariablen / Environment Variables

Sind systemweit gültig, enthalten wichtige Informationen, damit unser System wie gewünscht funktioniert, bestimmte Kommandos greifen auf diese Variablen zurück. Umgebungsvariablen werden nach Konvention komplett in Grossbuchstaben geschrieben.

Einige Beispiele:
```bash
$HOME       # Heimatverzeichnis des aktuellen Benutzers
$PWD        # absoluter Pfad des aktuellen Verzeichnisses
$USER       # Login Name des aktuellen Benutzers
$SHELL      # Shell des aktuellen Benutzers
$PATH       # Liste der Verzeichnisse, die nach ausführbaren Dateien durchsucht werden, so dass wir diese ohne eine Pfadangabe aufrufen können
```
Systemvariablen können unterschiedliche Werte enthalten, je nachdem welcher Benutzer angemeldet ist. 

### Shellvariablen / Shell Variables

Sind nur gültig in der aktuellen Shell, können vom Benutzer selbst definiert werden. Werden *nicht* automatisch in Subshells vererbt, es sei denn sie werden mit dem Kommando `export` exportiert.
```bash
foo=bar         # weist der Variablen foo den Wert bar zu
export foo      # macht die Variable foo auch in Subshells gültig
export hallo=huhu # weist der Variablen hallo den Wert huhu zu und macht diese in Subshells gültig
```
### Variablensubstitution

Bei der Variablensubstitution wird der Name der Variablen mit dem in ihr hinterlegten Wert ersetzt.

```bash
echo $foo       # gibt den Wert der Variablen foo aus
echo ${foo}     # gibt den Wert der Variablen foo aus
```
## Subshells

Innerhalb einer laufenden Shell können weitere Shells gestartet werden. Dies sind sogenannte *Subshell* oder *Kindshells*. Diese können entweder aktiv, z.B. durch die Eingabe des Kommandos `bash` gestartet werden, Subshells werden aber auch oft gestartet, ohne dass wir dies aktiv machen.

Es ist wichtig zu wissen, dass z.B. Aliase und Variablen *nicht* in Subshells vererbt werden!

Subshells begegnen uns ziemlich oft - z.B. werden Kommandos, Funktionen, Skripte usw. in Subshells ausgeführt, auch wenn wir davon direkt gar nichts mitbekommen.

Auch beim Wechsel in einen anderen Benutzeraccount wird eine Subshell mit den Berechtigungen dieses Benutzers gestartet.

Wir können uns einen Überblick über die momentan laufenden Shells bzw. Subshells mit dem Kommando `ps` verschaffen.

#### Ausgabe aller Prozesse des Benutzers, die mit einem Terminal verbunden sind
```bash
ps -a
```

### Rechnen mir Variablen / Arithmetic Operations

Wir können auch einfache Rechenoperationen in der BASH durchführen:
```bash
zahl1=3
zahl2=4
summe=$(( zahl1 + zahl2 ))
summe=$((zahl1+zahl2))
let summe = $zahl1 + $zahl2 
```

## Escaping / Maskieren von Sonderzeichen
Bestimmte Zeichen haben eine Sonderbedeutung für die BASH. Das wohl wichtigste Sonderzeichen ist das *Leerzeichen*: 

> Das Leerzeichen ist ein Sonderzeichen. Das Leerzeichen ist das **Trennzeichen**.

Weitere Sonderzeichen sind:
```bash
*       # Asterisk
?       
#       # Kommentarzeichen
$       # Subsitution
!       # History Expansion
\       # Backslash
'
''
```
Sonderzeichen können durch das sogenannte *Escaping* oder *Maskieren* ihrer Sonderbedeutung entledigt werden, so dass sie von der Shell wie reguläre Satzzeichen behandelt werden.

Zum Maskieren gibt es drei verschiedene Wege:

1. Maskieren mit dem Backslash `\`: Der Backslash maskiert (nur) das **direkt darauffolgende** Zeichen

2. Maskieren mit einfachen Hochkommata `'`: Einfache Hochkommata maskieren **jedes** in ihnen eingeschlossene Zeichen.

3. Maskieren mit doppelten Hochkommata `"`: Doppelte Hochkommata maskieren **fast** alle in ihnen eingeschlossene Zeichen, nicht aber das Dollarzeichen `$`, Backticks `\`` für die Kommandosubstitution und der Backslash `/` vor bestimmten Zeichen.

## Dateioperationen

### Verzeichnisse erstellen

Mit dem Kommando `mkdir` (*make directory*) können wir Verzeichnisse erstellen.
```bash
mkdir <name-des-verzeichnisses>
mkdir <pfad-zum-verzeichniss>
```
### Dateien erstellen

Dateien können auf vielfältige Art und Weise erstellt werden. Ein einfacher Weg, leere Dateien zu erstellen, ist mit dem Kommando `touch`.
```bash
touch <name-der-datei>
touch <name-der-datei-1> <name-der-datei-2> ...
touch <pfad-zur-datei>
```
Dateien können aber auch z.B. mit einem Editor wie `nano` erstellt werden.
```bash
nano <name-der>
```
### Editor nano

`nano` ist ein einfacher Editor, der auf den meisten Linux Distributionen vorinstalliert ist. Als Hilfe zur Bedienung wird unten ein Menü mit Tastenkürzeln angezeigt. Hier bedeutet das Zeichen `^` die Taste `STRG`.

Einige wichtige Tastenkombinationen:

- `STRG+O` Datei speichern unter...  (Name kann/muss angegeben werden)
- `STRG+S` Datei speichern (unter dem gleichen Namen)
- `STRG+X` Editor verlassen (bei ungespeicherten Änderungen werden wir gefragt, ob wir diese speichern möchten)

### Dateien und Verzeichnisse kopieren

Dateien und Verzeichnisse können mit dem Kommando `cp` (*copy*) kopiert werden.
```bash
cp <quelle> <ziel>
cp <pfad-zur-quelle> <pfad-zum-ziel>
```
**Vorsicht:** Wenn wir eine bestehende Datei als Ziel angeben, wird die Zieldatei **ohne Nachfrage** ersetzt und nicht etwas der Inhalt der Quelldatei an die Zieldatei angefügt.

Beim Kopieren von Verzeichnissen müssen wir an die Option `-r` (*rekursiv*) denken. 
```bash
cp -r <quellverzeichnis> <zielverzeichnis>
```
Der Grund ist, dass ein Verzeichnis nicht leer ist, die Kopieraktion also wiederholt/rekursiv ausgeführt werden muss.

### Dateien und Verzeichnisse löschen

Dateien und Verzeichnisse können mit dem Kommando `rm` (*remove*) gelöscht werden.

Analog zum Kopieren von Verzeichnissen müssen wir auch beim Löschen von Verzeichnissen die Option `-r` angeben.

> [!NOTE] 
> Dies gilt übrigens für sehr viele Kommandos: funktioniert die Anwendung eines Kommandos auf eine Datei, nicht aber auf ein Verzeichnis, so feht oft einfach nur die Option `-r`.

```bash
rm <pfad-zur-datei>
rm -r <pfad-zum-verzeichniss>
```
>[!NOTE]
> Wenn wir eine Datei löschen, so löschen wir nicht die Datei an sich. Wir entfernen lediglich den Dateinamen bzw. Pointer auf die Daten der Datei auf dem Speichermedium. Dieser Bereich im Speicher wird dann als wieder überschreibbar gemeldet.
>
> Die Daten könnten also solange keine weitere Schreiboperation auf diesen Speicherbereich erfolgt ist wiederhergestellt werden.

### Dateien und Verzeichnisse verschieben / umbenennen

Dateien und Verzeichnisse können mit dem Kommando `mv` (*move*) verschoben und umbenannt werden.

Beim Verschieben von Verzeichnissen dürfen wir die Option `-r` *nicht* angeben. Der Grund dafür ist, dass beim Verschieben nicht wie vielleicht angenommen eine Art *ausschneiden* und *einfügen* stattfindet, sondern wie beim Löschen lediglich der Dateiname ersetzt wird. 

Es muss also keine rekursive Operation auf dem Speichermedium stattfinden. Das ist auf unten stehendender Illustration vielleicht besser zu erkennen.
```bash
mv <quelle> <ziel>
mv <quellverzeichnis> <zielverzeichnis>
mv <alter-name> <neuer-name>
```
### Illustration kopieren, löschen, verschieben

![illustration-cp-rm-mv](./images/cp-mv-rm.png)

### relative und absolute Pfadangaben

Immer wenn wir eine Dateioperation durchführen, müssen wir den Pfad (eine Art *Wegbeschreibung*) zu der jeweiligen Datei angeben. Diese Angabe können wir auf zwei unterschiedliche Arten und Weisen machen: *relativ* oder *absolut*.

#### relative Pfadangaben

Eine *relative Pfadangabe* beschreibt den Weg ausgehend vom **aktuellen Standort** (aktuelles Verzeichnis) im Dateisystem.
```bash
cp somefile Somedir/
```
##### spezielle Verzeichniseinträge (Special Directory Entries) . und ..

- Sie gehören zur Kategorie der relativen Pfadangaben (Relative Pathnames)
- Sie werden auch als *Pseudodirektoren* (Pseudo-Directories) bezeichnet
- Manchmal nennt man sie auch *Implizite Links* oder *Selbstreferenzierende Einträge*
- Formal sind sie aber einfach reguläre Einträge im Dateisystem, die bei jedem Verzeichnis automatisch vorhanden sind.

- `.` (Punkt) symbolisiert das aktuelle Verzeichnis
- `..` (doppelter Punkt) symbolisiert das übergeordnete Verzeichnis (Parent Directory)

#### absolute Pfadangaben

Eine *absolute Pfadangabe* beschreibt den Weg ausgehend von der Wurzel `/` (bzw. `\` in Windows) des Dateisystembaums.
```bash
cp /home/tux/somefile /home/tux/Somedir
```
Absolute Pfadangaben können wir immer daran erkennen, dass das erste Zeichen des Pfades ein Slash `/` ist.

Einzige Ausnahme ist die Tilde `~`, welche den absoluten Pfad zum Heimatverzeichnis des aufrufenden Benutzers symbolisiert. Folgende Pfadangaben sind identisch:
```bash
cd ~/Somedir
cd /home/tux/Somedir
```
### Alles ist eine Datei

"Alles ist eine Datei" ist ein zentrales Konzept in Unix/Linux, das bedeutet, dass nahezu alle Ressourcen im System – einschließlich regulärer Dateien, Verzeichnisse, Geräte, Prozesse und Sockets – über das Dateisystem als Datei abstrahiert werden. Dadurch können viele Systemkomponenten über einheitliche Schnittstellen (z. B. Dateioperationen wie `open`, `read`, `write`) angesprochen werden.

### Pattern Matching

Ein *Pattern* ist ein *Muster*, bzw. ein *Platzhalter* welches auf eine Zeichenfolge passt, so dass wir damit z.B. nach Dateien bzw. Pfadangaben suchen können (mit entsprechenden Kommandos).

Wir können in einem *Pattern* bestimmte Sonderzeichen verwenden, um dieses allgemeingültiger zu machen:

*Globbing Characters:*

- `*` (*Asterisk*) -> Steht für beliebige Zeichen, welche beliebig oft vorkommen können (auch keinmal)
- `?` -> Steht für jedes beliebige Zeichen welches, **exakt** einmal vorkommt

Weitere Möglichkeiten für Pattern Matching:

- `!(pattern)` Exkludiert das angegebene Pattern (in dem Pattern dürfen auch wieder die oben angegebenen *Globbing Characters* vorkommen

Beispiele:
```bash
rm *.jpg       # löscht alle Dateien mit der Endung .jpg
ls datei?.txt  # zeigt nur Dateien an, bei denen nach der Zeichenfolge datei noch ein weiteres beliebiges Zeichen folgt und die die Endung .txt haben
mv !(o*) ../somdir/    # verschiebt alle Dateien des aktullen Verzeichnisses nach ../somedir, ausser Dateien, die mit einem o beginnen
```

## Textströme und Standardkanäle

Es gibt drei Standardkanäle unter Linux:

| Kanalbezeichnung | Filedescriptor | Nummer |
| ---------------- | -------------- | ------ |
| *Standareingabekanal*  | `stdin` | 0 |
| *Standardausgabekanal*|  `stdout` | 1 |
| *Standardfehlerkanal*  | `stderr` | 2 |

Jeder Prozess der gestartet wird, wird mit diesen drei Standardkanälen verbunden. Über diese Kanäle erhält der Prozess Daten und gibt sie auch wieder aus. So können Ein- und Ausgaben unabhängig voneinander verarbeitet und auch umgeleitet werden.

Die Kanäle jedes Prozesses, der in einer Shell gestartet wird, sind automatisch mit der Shell verbunden.

Durch dieses Konzept können wir durch die Kombination simpler Kommandos komplexe Aufaben lösen (-> *Kommandopipelines*) 

Wir könne so z.B. auch Ausgaben von Kommandos in Dateien umleiten (-> *Redirects*).

### Redirects

Mit Redirects kann die der Standardausgabekanal oder der Standardfehlerkanal in eine **Datei** umgeleitet werden. Es gibt zwei Arten von Redirects:

- `>` - einfacher Redirect: Erstellt eine Datei falls nicht vorhanden, **leert** eine bereits vorhandene Datei
- `>>` - doppelter Redirect: Erstellt eine Datei falls nicht vorhanden, **hängt Ausgabe an**

#### Umleitung des Standareingabekanals
```bash
echo huhu 1> hallo.txt   # die 1 gibt hier die Kanalnummer an
echo huhu 1>> hallo.txt  # die 1 gibt hier die Kanalnummer an
echo huhu > hallo.txt    # kann bei stdout auch weggelassen werden

ls -l /etc > ls-ausgabe.txt
ls -l /etc >> ls-ausgabe.txt
```
#### Umleitung des Standardfehlerkanals
```bash
ls mich-gibts-nicht  2> ls-fehler.txt     # hier muss die 2 stehen, da wir stderr umleiten
ls mich-gibts-nicht  2>> ls-fehler.txt    # hier muss die 2 stehen, da wir stderr umleiten
```
#### Umleitung beider Kanäle
##### in separate Dateien
```bash
ls mich-gibts/ mich-gibts-nicht/ > ergebnis.txt 2>fehler.txt
```
##### in die gleiche Datei
```bash
ls mich-gibts/ mich-gibts-nicht/ > ausgabe-und-fehler.txt 2>&1
```
>[!NOTE]
> Das `&` gibt hier an, dass wir einen *Kanal*/*Filedescriptor* meinen, ansonsten würden die Fehler in eine Datei mit dem Namen `1` umgeleitet werden.

### /dev/null

`/dev/null` ist soetwas wie das *Schwarze Loch* eines Linux Systems. Alles was wir dorthin leiten, verschwindet. Wir nutzen einen Redirect nach `/dev/null` ganz bewusst, um z.B. Fehlermeldungen eines Kommandos zu unterdrücken. Oder auch in Skripten, um den normalen Output eines Kommandos zu unterdrücken.

#### Beispiel find und 2>/dev/null
Wir wollen als regulärer Benutzer mit `find` unser gesamtes Dateisystem nach `.tar` Dateien durchsuchen. Mit folgendem Kommando erhalten wir aber sehr viele `Permission Denied` Fehlermeldungen:
```bash
find / -name "*.tar"
```
Wir leiten also ganz bewusst alle Fehler nach `/dev/null` um die Ergebnisse klarer sehen zu können:
```bash
find / -name "*.tar* 2>/dev/null
```
## UNIX Philosophie
Die Unix-Philosophie ist ein Satz von Prinzipien für Software-Design, die ursprünglich in den 1970er Jahren mit dem Unix-Betriebssystem entwickelt wurden. Sie betont Einfachheit, Modularität und Wiederverwendbarkeit.

Douglas McIlroy, der Erfinder der Unixpipes, fasste die Philosophie folgendermaßen zusammen:

- Schreibe Computerprogramme so, dass sie nur eine Aufgabe erledigen und diese gut machen.
- Schreibe Programme so, dass sie zusammenarbeiten.
- Schreibe Programme so, dass sie Textströme verarbeiten, denn das ist eine universelle Schnittstelle.

> "Write programs that do one thing and do it well."

## KISS Prinzip

- "Keep it stupid simple"
- "Keep it super simple"
- "Keep it simple, stupid!"

## Kommandopipelines
Kommandopipelines sind ein mächtiges Werkzeug, mit dem sich erst die ganze Stärke der Kommandozeile nutzen lässt.

Syntax:
```bash
<kommando1> | <kommando2>
```
Mit der *Pipe* (`|`) wird `stdout` von `<kommando1>` mit `stdin` von `<kommando2>` verbunden, so dass `<kommando2>` die Ausgabe von `<kommando1>` entgegenehmen und weiterverarbeiten kann.

### Beispiele:
```bash
ls -l /etc/ | less       # der Output von ls -l wird an den Pager less geleitet
cat /etc/passwd | wc -l  # Anzahl der Benutzer ausgeben
```
## Filterkommandos
Durch die Kombination von Filterkommandos und Pipelines extrahieren wir genau die Informationen aus Textdateien, die wir benötigen. Und zwar im Speicher, ohne dazu weitere Textdateien anlegen zu müssen.

### grep

Mit `grep` können wir Textströme zeilenweise filtern. 

`grep` nimmt ein `PATTERN`, also einen Suchbegriff bzw. genauer gesagt einen *Regulären Ausdruck* entgegen und gibt nur die Zeilen eines Textstroms aus, in denen dieses `PATTERN` vorkommt.

#### Beispiele:
##### nur die Benutzerkonfiguration von root ausgeben
```bash
grep "root" /etc/passwd     
```
##### -c --count 
`grep` zählt die Ergebnisse, hier also die Anzahl der Benutzer, die BASH als Login-Shell nutzen, es wird nur die Anzahl ausgegeben, nicht die Zeilen, die PATTERN enthalten
```bash
grep -c bash /etc/passwd    
```
##### -v --invert 
`grep` gibt all das aus, was *nicht* auf das PATTERN passt.

hier: Ausgabe aller Benutzerkonfigurationen, die *nicht* die BASH nutzen
```bash
grep -v bash /etc/passwd
```
##### -i --ignore-case 
`grep` ignoriert Gross-und Kleinschreibung

hier: sowohl alias als auch Alias wird gefunden
```bash
grep -i alias ~/.bashrc
```
##### -n --line-number 
`grep` zeigt zusätzlich die Zeilennummer an, in der PATTERN gefunden wurde. Sehr praktisch.
```bash
grep -n alias ~/.bashrc
```
##### -r --recursive 
`grep` kann so auch ganze Verzeichnisse durchsuchen

hier: gesamtes Home-Verzeichnis von tux nach etwas zu aliasen durchsuchen
```bash
grep -r alias /home/tux 
```
### cut

Mit `cut` können wir Textströme **spaltenweise** filtern. 

Für `cut` können wir ein Trennzeichen / *Delimiter* definieren, anhand dessen `cut` einen Textstrom in Spalten / *Fields* unterteilt und nur das oder die gewünschten Felder ausgibt.

#### Beispiele
```bash
cut -d: -f1 /etc/passwd     # nur die Login-Namen der Benutzer ausgeben
cut -d: -f7 /etc/passwd     # nur die Shells der Benutzer ausgeben

cat /etc/passwd | cut -d: -f1 # geht auch, sog. Useless Use of Cat
```
### tail

`tail` gibt standardmässig die letzten 10 Zeilen einer Datei aus. 

Die Anzahl der Zeilen kann über die Option `-n` angegeben werden.

Eine sehr nützliche Option von `tail` ist die Option `-f` / `--follow`. Damit können wir Änderungen an einer Datei sozusagen *live* beobachten. Wird oft in Verbindung mit Log-Dateien genutzt, um zu verfolgen, was gerade passiert. 

#### Beispiele
```bash
tail -n 5 ~/.bashrc     # die letzten 5 Zeilen der bashrc ausgeben
tail -f /var/log/apache2/access.log   # Zugriffe auf den Webserver verfolgen
```
### head

`head` gibt standardmässig die ersten 10 Zeilen einer Datei aus. 

Die Anzahl der Zeilen kann über die Option `-n` angegeben werden.

#### Beispiele
```bash
head -n 5 ~/.bashrc     # die ersten 5 Zeilen der bashrc ausgeben
```
### sort

Mit `sort` können wir Textströme (nach bestimmten Kriterien) sortiert ausgeben.

### uniq

Mit `uniq` könne wir direkt aufeinanderfolgende gleiche Zeilen zu einer einzigen zusammenfassen.

Um z.B. alle Dublikate/gleiche Zeilen aus einem Textstrom zu entfernen, kombinieren wir (KISS Prinzip) `uniq` und `sort`

>[!NOTE] 
> `sort` hat übrigens die Option `-u` eingebaut, mit der wir gleiches erreichen können.

#### Beispiele
Anzahl der verschiedenen Shells in /etc/passwd zählen
```bash
cut -d: -f7 /etc/passwd | sort | uniq | wc -l 
```

== Prozesse

Ein Prozess ist ein sich in der Auführung befindliches Programm. Ein Programm resultiert immer in mindestens einem Prozess. Prozesse laufen jeweils in einem von anderen unabhängigen "Resourcenraum", haben eine eigene _PID_, kennen nur die _PID_ des Prozesses, von dem sie gestartet wurden (Elternprozess). Prozesse können mit dem Kommando `kill` über _Signale_ beeinflusst werden.

Wird der Elternprozess beendet, so werden (in der Regel) gleichzeitig alle Kindprozesse mit beendet.

Auf der Shell kann immer nur ein einzelner Prozess im _Vordergrund_ ausgeführt werden. Prozesse können mit der Tastenkomnination `STRG+Z` angehalten und in den Hintergrund geschickt werden. Mit dem Kommando `bg` kann dieser Prozess dann im Hintergund fortgesetzt werden, `fg` holt den Prozess in den Vordergrund zurück. 

Wir können einen Prozess beim Start aber auch direkt in den Hintergrund schicken und starten (duch Anhängen eines `&`):
```
 kommando &
```
 - `ps -aux`: Anzeige aller laufende Prozessez
 - `ps -ef`: auch Anzeige aller laufenden Prozesse
 - `ps --forest`: Prozesshirarchie (Baumstruktur) anzeigen
 - `jobs`: Anzeigen der Hintergrundprozesse
 - `fg`: letzten/aktuellen/default Job in den Vordergrund holen
 - `fg %<jobnummer>`: Job mit Jobnummer `<jobnummer>` in den Vordergrund holen
 - `bg`: Hintergrundprozess fortsetzen
 - `bg %<jobnummer>`: Hintergrundprozess mit Jobnummer `<jobnummer>` in fortsetzen

 == kill

 `kill` sendet Signale an Prozesse. Es muss die PID des Prozesses angegeben werden, Prozessname funktioniert nicht.

 - `kill -s <signal> <PID>`: sendet <signal> an den Prozess mit der PID <PID>
 - `kill -<signal> <PID>`: sendet <signal> an Prozess mit der PID <PID>

 Die PID eines Prozesses kann auf mehrere Arten ermittelt werden:
```
  ps -ef | grep <prozessname>
   pgrep <prozessname>
    ...
```
=== einige wichtige Signale

    - `SIGTERM` (15): Standard, falls kein bestimmtes Signal angegeben wird. Sendet eine "freundliche" Aufforderung an den Prozess, sich doch bitte zu beenden. Im Prozess selbst ist festgelegt, wie er sich beendet, z.B. werden noch gewisse Aufräumarbeiten durchgeführt etc.
    - `SIGINT` (2): sendet eine etwas deutlichere Aufforderung an den Prozess, sich zu beenden, wird bei der Tastenkomnination `STRG+C` (_Cancel_) gesendet
    - `SIGKILL` (9): rabiateste Methode, Signal wird nicht an den Prozess, sondern direkt an den Scheduler gesendet, der daraufhin den entsprechenden Prozess aus seiner Liste löscht, der Prozess somit keine CPU Zeit mehr zur Verfügung gestellt bekommt und somit zwangsläufig beendet wird.
    - `SIGSTOP` (19): hält Prozess an und schickt ihn in den Hintergrund (`STRG+Z`)
    - `SIGCONT` (18): startet angehaltene Prozesse

=== pkill und killall

    - `pkill`: analog zu oben, `pkill` erwartet aber den Namen bzw. einen Teil des Namens eines Prozesses anstatt der PID. Falls mehere Prozesse auf den Namen passen, wird das Signal an *alle* diese Prozesse gesendet.
    - `killall` auf neueren Distributionen nicht mehr vorhanden, ähnliches Verhalten wie `pkill`

## Berechtigungen

Berechtigungen in Linux steuern den Zugriff auf Dateien und Verzeichnisse für *Benutzer* und *Gruppen*. Sie legen fest, wer lesen (`r`), schreiben (`w`) und ausführen (`x`) darf.

Der folgende Auszug von `ls -l file1.txt` sagt folgendes aus:
```bash
  u  g  o
  -rw-r--r-- 1 tux tux 5 Feb 12 13:09 file1.txt
  ```
  - Der User/Besitzer (`u`) darf den Inhalt der Datei lesen und ändern (`rw`)
  - Mitglieder der Gruppe (`g`) dürfen den Inhalt der Datei nur lesen (`r`)
  - Alle anderen Benutzer, die weder der Besitzer, noch Mitglieder der Gruppe sind, dürfen den Inhalt der Datei lesen (`r`)

  ### Bedeutung der Berechtigungen für Dateien

  `r` (*read*) -> Dateiinhalt lesen
  `w` (*write*) -> Dateiinhalt ändern -> aber **nicht** Datei löschen
  `x` (*eXecute*) -> Datei ausführen

  ### Bedeutung der Berechtigungen für Verzeichnisse

  `r` (*read*) -> Verzeichnisinhalt lesen bzw. das Auflisten der Namen der Dateien
  `w` (*write*) -> Verzeichnisinhalt ändern -> Dateien hinzufügen und löschen
  `x` (*eXecute*) -> Verzeichnis betreten 

  >[!IMPORTANT] 
  > Es macht **keinen wirkliche Sinn** wenn das Execute Bit bei Verzeichnissen **nicht** gesetzt ist. Dann wird alles etwas seltsam... Wir brauchen dieses Bit, damit Verzeichnisse wie gewünscht funktionieren.

  ### Symbolische Rechtevergabe
  Hierbei werden *Symbole* für die Berechtigungen verwendet. Diese Art der Rechtevergabe ist sehr intuitiv und eignet sich besonders dafür, einzelne Berechtigungen hinzuzufügen oder zu entfernen, ohne die bestehenden Berechtigungen zu verändern. Es ist auch einfach, diese Vorgänge wieder rückgängig zu machen.

  #### Symbole

  `r` -> read
  `w` -> write
  `x` -> eXecute

  `u` -> user/owner
  `g` -> group
  `o` -> others (weder owner noch group)
  `a` -> all

  `+` -> hinzufügen
  `-` -> entziehen
  `=` -> setzten

  Der Gruppe Schreibrechte hinzufügen:
  ```bash
  chmod g+w file1.txt
  ```
  Dem Besitzer Leserechte entfernen:
  ```bash
  chmod o-r file1.txt
  ```
  Besitzer und Gruppe Schreib- und Leserechte
  ```bash
  chmod ug+rw file1.txt
  ```
  Dem Besitzer Ausführungsrechte hinzufügen, der Gruppe Schreibrechte entziehen, allen anderen Leserechte hinzufügen.
  ```bash
  chmod u+x,g-w,o+r file1.txt
  ```
  ### Numerische/Oktale Rechtevergabe
  Die numerische oder oktale Rechtevergabe verwendet Zahlen des Oktalsystems, um Berechtigungen gleichzeitig für Besitzer, Gruppe und Others zu vergeben. 

  Es eignet sich besonders für Situationen, in denen wir eine Datei oder Verzeichnis in einen expliziten Zustand versetzten wollen.

  Interessant ist die Herkunft der Zahlen für die Berechtigungen. Übersetzen wir sie doch einmal ins Binärsystem:

  | Symbol | Okal | Binär |
  | ------ | ---- | ----- |
  | `r` | `4` | `100` |
  | `w` | `2` | `010` |
  | `x` | `1` | `001` |
  | `-` | `0` | `000` |

  Wir sehen, dass das gesetzte Bit *wandert* bzw. sich immer um eine Position verschiebt. Sehen wir uns das einmal im Listing von `ls -l` an:
  ```bash
    7  6  4
     111110100
     -rwxr--r-- 1 tux tux 5 Feb 12 13:09 file1.txt

     111 -> 7
     110 -> 6
     100 -> 4
     ```
     Ist also ein bestimmtes Recht gesetzt, bedeutet dass, das hier binär auch eine `1` steht. Wir sehen sozusagen ein Abbild dessen, was wirklich im Speicher passiert. Toll, oder?

     ### Sonderbits
     Zusätzlich gibt es noch drei Sonderbits, die gewisse Dinge ermöglichen, damit unser System funktioniert:

     #### SUID Bit

     Auf eine **ausführbare Binärdatei** gesetzt, bewirkt das SUID Bit, dass die Datei mit den Berechtigungen des **Besitzers** der Datei ausgeführt wird und **nicht** mit den Berechtigungen des aufrufenden Users.

     ##### Beispiel `/etc/passwd`
     ```bash
     ls -l /usr/bin/passwd

     -rwsr-xr-x 1 root root 68248 Feb 24  2025 /usr/bin/passwd

     ls -l /etc/shadow

     -rw-r----- 1 root shadow 1619 Feb 24 2025 /etc/shadow
     ```
     Das Kommando kann also von einem regulären Benutzer ausgeführt werden, läuft dann aber mit Root-Rechten und kann ja auch nur so den Inhalt der Datei `/etc/shadow` ändern.

     #### SGID Bit

     Auf eine **ausführbare Binärdatei** gesetzt, bewirkt das SGID Bit, dass die Datei mit den Berechtigungen der **Gruppe** der Datei ausgeführt wird und **nicht** mit den Berechtigungen des aufrufenden Users.

     Auf ein Verzeichnis gesetzt, bewirkt es, dass neu darin erstellte Dateien der Gruppe zugewiesen werden, der das Verzeichnis gehört und nicht der Gruppe des erstellenden Users.

     ```bash
     ls -ld /var/mail

     drwxrwsr-x 2 root mail 4096 Feb 20 09:26 /var/mail/
     ```
     So werden alle E-Mails der Gruppe `mail` zugeordnet und können vom Mailserver hinzugefügt und auch gelöscht werden.

     #### Sticky Bit

     Auf ein Verzeichnis gesetzt, bewirkt es, dass darin enthaltene Dateien nur noch vom Besitzer der Datei genändert oder gelöscht werden dürfen.
     ```bash
     ls -ld tmp

     drwxrwxrwt 8 root root 4096 Feb 20 09:30 /tmp
     ```
     So ist es einem regulären Benutzer nicht möglich, Dateien eines anderen Benutzers zu ändern oder zu löschen.

 ## Links

 ### Symbolische Links / Symlinks
 Sind im Prinzip das Gleiche wie Verknüpfungen unter Windows. Ein Symlink verweist auf eine andere Datei oder gesamtes Verzeichnis, genauer gesagt *auf den Pfad* zu einer Datei oder Verzeichnis. Es ist eine zusätzliche Inode, die auf ein Ziel verweist. Symlinks können auch über Partitionsgrenzen hinaus bestehen. Wird das Original gelöscht, bleibt ein sog. *toter* oder *verwaister* Link zurück, der nicht mehr funktioniert.
 ```bash
 ln -s original.txt symlink.txt
 ln -s originalverzeichnis symlinkverzeichnis
 ```
 Symlinks werden z.B. von Webservern genutzt: 
 - `/etc/apache2/sites-available` -> alle vorhandenen Konfigurationsdateien für Websites (Original)
 - `/etc/apache2/sites-enabled` -> nur die *aktiven* Konfigurationsdateien für Websites (Symlink)

 ### Hardlinks
 Hardlinks sind eigentlich lediglich Dateinamen. Mehrere Dateinamen bzw. Hardlinks können auf den gleichen Bereich im Speicher bzw. die gleiche Datei zeigen. Ein Hardlink ist nicht mehr vom Original zu unterscheiden, er hat die selbe Inode wie das Original. Wird das "Original" gelöscht, ist die Datei weiterhin über den "Link" bzw. jetzt neuen Dateinamen zu erreichen.

 Hardlinks funktionieren daher natürlich **nicht** auf Verzeichnisse oder über Partitionsgrenzen hinaus.
 ```bash
 ln original.txt hardlink.txt
 ```
 Man kann die Anzahl der Hardlinks mit `ls -l` sehen (Spalte direkt nach den Berechtigungen) bzw. in der Ausgabe des Kommandos `stat`:
 ```bash
 ln file1.txt hardlink-file1
 ls -l file1.txt

 -rw-r--r-- 2 tux tux 5 Feb 12 13:09 file1.txt

 stat file1.txt

   File: file1.txt
     Size: 5         	Blocks: 8          IO Block: 4096   regular file
     Device: 254,1	Inode: 1177496     Links: 2
     Access: (0644/-rw-r--r--)  Uid: ( 1000/     tux)   Gid: ( 1000/     tux)
     Access: 2025-02-17 08:55:39.987803189 +0100
     Modify: 2025-02-12 13:09:04.146349786 +0100
     Change: 2025-02-24 17:31:49.220713220 +0100
      Birth: 2025-02-12 11:45:38.078409807 +0100
      ```
      Hardlinks werden im System verwendet, um Speicherplatz zu sparen z.B. für bestimmte Systemkommandos. Auch nutzen manche Backup Lösungen Hardlinks um inkrementelle Backups zu erstellen.

## Archivierung und Komprimierung

### Archivierung mit `tar`

*Archivierung* bezeichnet das Zusammenfassen **mehrerer** Dateien und Verzeichnisse in eine **einzige** Datei, ohne zwingende Kompression. Dadurch bleibt die ursprüngliche Struktur der Dateien erhalten, so können mehrere Dateien einfacher gespeichert oder übertragen werden.

Unter Linux wird das Kommando `tar` (*Tape Archiver*) zur Archivierung verwendet. `tar` ist ein sehr altes Programm und die Syntax (freundlich ausgedrückt) etwas gewöhnungsbedürftig. Kurzoptionen haben oft keine direkte Entsprechung zu den Langoptionen.

Ein `tar`-Archiv kann man sich mit dem Kommando `cat` anzeigen lassen:

![Ausgabe tar Archiv mit cat](./images/ausgabe-tar-archiv-mit-cat.png)

> [!NOTE]
> Alle numerischen Angaben hier sind im *Oktalformat*. Dies hat historische Gründe. Möchte man den Zeitstempel umrechnen, kann man sich von der BASH helfen lassen:
> ```bash
> echo $(( 8#14757403716 ))
> ```

Einige wichtige Optioenen zu `tar`:
#### Archiv aus Dateien erstellen
```bash
tar -cf archive.tar file1.txt file2.txt file3.txt 
tar --create --file archive.tar file1.txt file2.txt file3.txt 
```
#### Dateien aus Archiv extrahieren
```bash
tar -xf archive.tar
tar --extract --file archive.tar
```
#### Die Option -v / --verbose gibt eine Rückmeldung darüber, was tar macht
```bash
tar -xvf archive.tar
tar --extract --verbose --file archive.tar
```
#### Einzelne Datei aus Archiv extrahieren
```bash
tar -xf archive.tar file_to_extract.txt
```
#### Inhalt eines Archivs anzeigen/auflisten
```bash
tar -tf archiv.tar
tar --list --file archive.tar
```
#### Datei einem bestehenden Archiv hinzufügen
```bash
tar -rf archive.tar other_file.txt
tar --append --file archive.tar other_file.txt
```
#### Archiv aus einem Verzeichnis erstellen
```bash
tar -cf archive.tar /absolute/path/to/dir
tar -cf archive.tar relativ/path/to/dir
```
#### Differentielles Archiv erstellen

#### Bestimmte Dateien *nicht* hinzufügen

#### Archiv an einem bestimmten Ort extrahieren

#### Teile des Pfades beim entpacken weglassen
```bash
--strip-components=NUMBER
```
> [!NOTE] 
> Pfadangaben werden immer mit archiviert! Wir müssen uns also im Vorhinein Gedanken machen, ob wir z.B. einen relativen oder absoluten Pfad angeben.

> [!IMPORTANT]
> Die Option `-f` erfordert **zwingend** ein direkt darauffolgendes Argument - den Namen des Archivs. Insofern ist die Reihenfolge der Optionen hier **nicht** egal.

#### Unix Epoch Time

Die *Unix Epoch Time* (auch *POSIX-Zeit* oder *Unix-Zeitstempel*) ist die Anzahl der Sekunden, die seit dem 1. Januar 1970, 00:00:00 UTC vergangen sind. Sie wird oft in Betriebssystemen und Programmiersprachen verwendet, um Zeitangaben als einfache Ganzzahlen zu speichern und zu verarbeiten.

So ist es möglich, Daten in unterschiedlichen Formaten (z.B. Zeitzonen) anzugeben oder Differenzen zwischen zwei Zeitpunkten zu berechnen.

### Komprimierung

Durch die Komprimierung können wir **eine einzelne** Datei mit Hilfe bestimmter Algorithmen verlustfrei in ihrer Grösse verkleinern.

*Analogie Komprimierung:* getrocknete Handtücher, die im Wasser wieder gross werden

*Analogie Archivierung:* einzelne Blumen werden zu einem Strauss gebunden

Um **mehrere** Dateien oder ganze Verzeichnisse zu komprimieren, müssen wir zusätzlich im Vorfeld die *Archivierung* anwenden. Bestimmte Programme in Windows-Systemen vereinen diese beiden Konzepte unter einer Haube. Wir müssen uns jedoch merken, dass dies grundsätzlich zwei komplett verschiedene Konzepte sind.

Auch unter Linux ist es möglich beide Schritte auf einmal mit dem Kommando `tar` durchzuführen, dabei ruft `tar` im Hintergrund jedoch die jeweiligen Kommandos zur Komprimierung auf.

Unter Linux nutzen wir standardmässig drei verschiedene Tools zur Komprimierung: `gzip`, `bzip2` und `xz`.

Die Optionen der drei Kommandos sind mehr oder weniger identisch.
#### Datei komprimieren:
```bash
gzip somefile
bzip2 somefile
xz somefile
```
#### Datei dekomprimieren:
```bash
gzip -d somefile.gz
bzip2 -d somefile.bz2
xz -d somefile.xz

gunzip somefile.gz
bunzip2  somefile.bz2
unxz -d somefile.xz
```
>[!NOTE]
> Sowohl bei der Komprimierung als auch bei der Dekomprimierung wird die jeweilige Originaldatei nicht behalten, sondern ersetzt.
> Dies können wir mit der Option `-k` (`--keep`) umgehen.

>[!NOTE]
> Es gibt für alle drei Algorithmen auch die Option `-l` bzw.  `--list`. Hiermit können wir uns einen Überblick/Vergleich der komprimierten und unkomprimierten Datei und die Kompressionsrate anzeigen lassen.
> 
> Interessanterweise ist dies nur ein *Listing*, es findet zwar eine Dekomprimierung der Datei statt, allerdings nur im RAM, selbst wenn wir zusätzlich die Option `-d` zur Dekomprimierung angeben.

### Vergleich der drei Komprimierungsalgorithmen

Vergleich der Geschwindigkeiten und resultierenden Grössen beim Komprimieren:

![vergleich-komprimierung](./images/vergleich-komprimierung.png)
Vergleich der Geschwindigkeiten beim Dekomprimieren:

![vergleich-dekomprimierung](./images/vergleich-dekomprimierung.png)
Zusammenfassend lassen sich folgende Aussagen über die drei Komprimierungsalgorithmen treffen:

- `gzip` ist am schnellsten bei der Komprimierung, die komprimierte Datei ist aber nicht besonders klein
- `bzip2` braucht lange für die Komprimierung und die Dekomprimierung, erzeugt aber eine ziemlich kleine Datei
- `xz` braucht ziemlich lange bei der Komprimierung, erzeugt liegt bei der Kompressionsrate zwischen den beiden anderen, ist aber sehr schnell bei der Dekomprimierung

Es gibt also für alle drei bestimmte Anwendungsfälle, in denen sie ihre Stärken ausspielen können.

### Erstellen und Entpacken eines komprimierten Archivs direkt mit `tar`

#### gzip komprimiertes Archiv erstellen
```bash
tar -czf archiv.tar.gz somdir/
tar -czvf archiv.tar.gz somdir/     # verboser Output
```
#### bzip2 komprimiertes Archiv erstellen
```bash
tar -cjf archiv.tar.bz2 somdir/
tar -czjf archiv.tar.bz2 somdir/     # verboser Output
```
#### xz komprimiertes Archiv erstellen
```bash
tar -cJf archiv.tar.xz somdir/
tar -cJvf archiv.tar.xz somdir/     # verboser Output
```
#### Komprimiertes Archiv entpacken
##### mit gzip komprimiertes Archiv entpacken
```bash
tar -xzf archiv.tar.gz
tar -xzvf archiv.tar.gz
```
##### mit bzip2 komprimiertes Archiv entpacken
```bash
tar -xjf archiv.tar.bz2
tar -xzjf archiv.tar.bz2
```
##### mit xz komprimiertes Archiv entpacken
```bash
tar -xJf archiv.tar.xz
tar -xJvf archiv.tar.xz
```
##### automatisch jeweiligen Algorithmus anhand der Dateiendung anwenden lassen (Dateiendung muss stimmen!)
```bash
tar -xf archiv.tar.gz
tar -xf archiv.tar.bz2
tar -xf archiv.tar.xz
```

== Paketmanagement Debian

Unter Debian basierten Distributionen wird der Paketmanager `apt` bzw. `apt-get` verwendet. Im Hintergund nutzen beide aber das Low-Level-Tool `dpkg`. Dieses hat unter anderem folgende Einschränkungen:

- kein Zugriff aufs Internet, Pakete müssen manuell heruntergeladen werden
- Abhängigkeiten können also nicht automatisch aufgelöst werden
- `dpkg` gibt aber Hinweise auf fehlende Abhängigkeiten

=== Debian Paketmanagement

- `dpkg -i <datei>.deb`: Paket `<datei>` installieren (oder Update falls Paket schon installiert)
- `dpkg -I <datei>.deb`: Informationen über Paket `<datei>.deb`
- `dpkg --info <datei>.deb`: Informationen über Paket `<datei>.deb`
- `dpkg -r <paket>`: Paket `<paket>` entfernen/deinstallieren (Konfigurationsdateien bleiben erhalten)
- `dpkg -P <paket>`: Paket `<paket>` entfernen/deinstallieren (_purge_) (Konfigurationsdateien werden mit entfernt)
- `dpkg -l`: Liste aller auf dem System installierten Pakete inkl. Status (installiert, entfernt, teilweise installiert/entfernt ...)
- `dpkg --list`: Liste aller auf dem System installierten Pakete inkl. Status (installiert, entfernt, teilweise installiert/entfernt ...)
`dpkg -l <glob-pattern>`: Liste aller Pakete, die auf `<glob-pattern>` passen 
- `dpkg --get-selections`: übersichtliche Liste aller installierten Pakete
- `dpkg -L <paket>`: Liste aller im Paket `<paket>` enthaltener Dateien
- `dpkg --list-files <paket>`: Liste aller im Paket `<paket>` enthaltener Dateien
- `dpkg -s`: Statusinformationen aller auf dem System installierten Pakete
- `dpkg -s <paket>`: Statusinformationen von `<paket>`
- `dpkg -S /path/to/file`: Angabe, zu welchem Paket `/path/to/file/` gehört

=== Troubleshooting

Bei fehlerhaften oder nur teilweise installierten Paketen können wir folgende Option von `apt` nutzen:

 apt install -f
  apt install --fix-broken

  === Paketquellen

  `Archive type`:: A repository may contain packages with ready-to-run software (binary packages, type deb) or with the source code to this software (source packages, type deb-src). The example above provides binary packages.

  `URL`:: The URL for the repository.

  `Distribution`:: The name (or codename) for the distribution for which packages are provided. One repository may host packages for multiple distributions. In the example above, disco is the codename for Ubuntu 19.04 Disco Dingo.

  `Components`:: Each component represents a set of packages. These components may be different on different Linux distributions. For example, on Ubuntu and derivatives, they are:

  `main`:: contains officially supported, open-source packages.

  `restricted`:: contains officially supported, closed-source software, like device drivers for graphic cards, for example.

  `universe`:: contains community maintained open-source software.

  `multiverse`:: contains unsupported, closed-source or patent-encumbered software.

  On Debian, the main components are:

  `main`:: consists of packages compliant with the Debian Free Software Guidelines (DFSG), which do not rely on software outside this area to operate. Packages included here are considered to be part of the Debian distribution.

  `contrib`:: contains DFSG-compliant packages, but which depend on other packages that are not in main.

  `non-free`:: contains packages that are not compliant with the DFSG.

  `security`:: contains security updates.

  `backports`:: contains more recent versions of packages that are in main. The development cycle of the stable versions of Debian is quite long (around two years), and this ensures that users can get the most up-to-date packages without having to modify the main core repository.

  === apt-get

  - `apt-get update`: Paketindex auf den neuesten Stand bringen
  - `apt-get upgrade`: alle Pakete aktualisieren. Es werden aber keine neuen Pakete installiert oder vorhandene entfernt.
  - `apt-get dist-upgrade`: wie `upgrade`, es werden aber ggf. neue Pakete installiert oder vorhandene entfernt.
  - `apt-get remove`: Paket entfernen, Konfigurationsdateien bleiben erhalten
  - `apt-get remove --purge`: Paket entfernen, Konfigurationsdateien werden mit entfernt
  - `apt-get purge`: Paket entfernen, Konfigurationsdateien werden mit entfernt
  - `apt-get autoremove`: alle Pakete entfernen, die automatisch installiert wurden (als Abhängigkeit) und von keinem weiteren Paket benötigt werden.
  - `apt-get clean`: `.deb` Pakete löschen
  - `apt-cache search`: nach Paketen suchen, akzeptiert RegEx, durchsucht Paketnamen und Beschreibung
  - `apt-cache show`: Informationen über ein Paket
  - `apt-file`: Muss zusätzlich installiert werden 
  - `apt-file search `: Kann herausfinden, welches Paket `<datei>` bereitstellt 
  - `apt-file update `: Paketindex von `apt-file` aktualiseren

  == Paketmanagement Red hat

  === rpm

  - kann Programme installieren, die lokal auf dem System vorliegen
  - die Pakete müssen dafür in einem bestimmten Binärformat vorliegen (`.rpm`)
  - Abhängigkeiten können nicht automatisch aufgelöst werden
  - fehlende Abhängigkeiten werden allerdings aufgelistet (jedoch nur die Dateien
    an sich, nicht die fehlenden _Pakete_)
    - `-v` verboser Output
    - `-h` Hashtags (`#`) als Fortschrittsanzeige

    Installation eines Pakets:

    ----
    rpm -i PACKAGENAME
    rpm -i gimp-2.8.22-1.el7.x86_64.rpm
    ----

    === Update

    Ein Paket aktualisieren

    ----
    rpm -U PACKAGENAME
    ----

    - ist kein Paket mit diesem Namen vorhanden, wird es trotzdem die neues Version installiert.
    - dies kann mit der Option `-F` verhindert werden

    === Deinstallation

    ----
    rpm -e PACKAGE    # erase
    rpm -e wget
    ----

    === Installierte Pakete anzeigen

    Alle installierten Pakete anzeigen

    ----
    rpm -qa      # query all
    ----

    === Informationen über Pakete erhalten

    Informationen über ein installiertes Paket:

    ----
    rpm -qi unzip       # query information
    ----

    Informationen darüber, welche `Dateien` in einem Paket enthalten sind:

    ----
    rpm -ql unzip      # query list
    ----

    Informationen über ein `noch nicht` installiertes Paket, hier kann eine FTP-Adresse oder URL angegeben werden:

    ----
    rpm -qip atom.x86_64.rpm           # query information not installed
    ----

    ----
    rpm -qlp atom.x86_64.rpm     # query list not installed
    ----


    === Zu welchem Paket gehört eine Datei

    ----
    rpm -qf /usr/bin/unzip       # query file
    > unzip-6.0-19.el7.x86_64
    ----

    == YellowDog Updater Modified (yum)

    Unterschied zu Debian: Der Paketindex wird automatisch aktualisiert. 

    === nach Paketen suchen

    ----
    yum search PATTERN

    yum search 7zip
    ----

    === Installation, Upgrade, Removal

    Ohne die Angabe eines Paketnamens wird das gesamte System aktualisiert. Es gibt *kein* `yum upgrade`!

    ----
    yum update
    ----

    ----
    yum install p7zip
    ----

    ----
    yum update wget
    ----

    ----
    yum check-update PACKAGENAME
    yum check-update                # gesamtes System
    ----

    === Pakete nur herunterladen

    ----
    yum install --downloadonly <package>
    yum install --downloadonly --downloaddir=<directory> <package>
    ----

    === zu welchem Paket gehört eine Datei

    ----
    yum whatprovides DATEI
    yum whatprovides libgimpui-2.0.so.0
    ----

    Funktioniert auch mit Dateien:

    ----
    yum whatprovides /etc/hosts
    ----

    === Informationen über ein Paket erhalten

    ----
    yum info firefox
    ----

    === Software Repositories

    - Repos sind in `/etc/yum.repos.d` zu finden, z.B. `CentOS-Base.repo`
    - Repos können hinzugefügt werden, indem eine `.repo` Datei dort abgelegt wird (oder am Ende von `/etc/yum.conf`)
    - besser aber mit dem Tool `yum-config-manager`

    Um den `yum-config-manager` nutzen zu können muss das Paket `yum-utils` installiert werden.

    ----
    yum-config-manager --add-repo https://rpms.remirepo.net/enterprise/remi.repo

    yum-config-manager --enable-repo baseos-source
    ----

    Liste der verfügbaren Repositories:

    ----
    yum repolist all
    ----

    Repos disablen und enablen:

    ----
    yum-config-manager --disable <reponame>
    yum-config-manager --disable updates
    yum-config-manager --enable <reponame>
    yum-config-manager --enable updates
    ----

    === Cache leeren

    ----
    # yum clean packages
    # yum clean metadata
    ----

    == Paketmanagement bei OpenSUSE

    - OpenSUSE verwednet `zypper` als Paketmanagement Tool
    - ähnlich wie `apt` und `yum`

    === Paketindex aktualisieren

    ----
    zypper refresh      # Paketindex aktualisieren
    ----

    - pro Repo kann eingestellt werden, ob der Index automatisch aktualiert werden soll

    === nach Paketen suchen

    ----
    zypper search <paketname>      # nach Paketen suche
    zypper se <paketname>
    zypper se -i              # Liste der installierten Pakete
    zypper se -i <string>     # prüfen, ob Pakete installiert mit <string> im Namen installiert sind
    zypper se -u <string>     # Suche nach nicht-installierten Paketen
    ----

    === Installation von Paketen

    ----
    zypper install <paketname>
    zypper in <paketname>
    ----

    - mit `zypper` können auch lokal vorhandene `.rpmr` Pakete installiert werden
    - Abhängigkeiten werden dabei aufgelöst
    - hier muss der absolute Pfad angegeben werden

    === Upgrade von Paketen / des Systems

    ----
    zypper update        # alle installierten Pakete aktualisieren
    zypper list-updates  # alle Aktualisierungen anzeigen
    ----

    === Löschen von Paketen

    - beim Entfernen von Paketen werden automatisch alle Pakete mit entfernt, die von dem zu entfernenden Paket abhängen

    ----
    zypper remove <paket>
    zypper re <paket>
    ----

    === Suche nach dem Paket, welches eine Datei enthält

    - hierzu wird der Suchparameter `se` mit der Option `--provides` erweitert
    - es folgt der absolute Pfad zur Datei

    ----
    # zypper se --provides /usr/lib64/libgimpmodule-2.0.so.0
    ----

    === Informationen über installierte Pakete

    ----
    # zypper info gimp
    ----

    === Software Repositories

    - Anzeige aller auf dem System registrierten Repositories mit `zypper repos`
    - mit der Option `modifyrepo` können Repos konfiguriert werden
    - Repos aktivieren und deaktivieren mit `-e` und `-d`:

    ----
    # zypper modifyrepo -d repo-non-oss
    Repository 'repo-non-oss' has been successfully disabled.

    # zypper modifyrepo -e repo-non-oss
    Repository 'repo-non-oss' has been successfully enabled.
    ----

    - _autorefresh_ für ein Repo ein- bzw. ausschalten mit `-f` und `-F`:

    ----
    # zypper modifyrepo -F repo-non-oss
    Autorefresh has been disabled for repository 'repo-non-oss'.

    # zypper modifyrepo -f repo-non-oss
    Autorefresh has been enabled for repository 'repo-non-oss'.
    ----

    - Repos können mit `addrepo` hinzugefügt und mit `removerepo` wieder entfernt werden:

    ----
    # zypper addrepo http://packman.inode.at/suse/openSUSE_Leap_15.1/ packman
    ----

    - beim Hinzufügen können mit `-f` Autoupdates eingeschaltet werden
    - wird beim Hinzufügen die Option `-d` mit angegeben, wird das Repo hinzugefügt aber nicht aktiviert

    ----
    # zypper removerepo packman
    ----
