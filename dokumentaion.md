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
