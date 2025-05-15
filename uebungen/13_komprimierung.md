# Übungen zur Kompression unter Linux

## Vorbereitung

Folgende Kommandos werden für die Übungen benötigt:
```bash
dd
tar
gzip
bzip2
xz
time
```
1. Erstellt ein Verzeichnis `uebungen-kompression` für die Übungen und wechselt hinein.
2. Erstellt für die Übungen drei Testdateien mit dem Kommando `dd` wie folgt:
```bash
dd if=/dev/zero of=bigfile1.data bs=1M count=1024
dd if=/dev/zero of=bigfile2.data bs=1M count=1024
dd if=/dev/zero of=bigfile3.data bs=1M count=1024
```
**Erklärung:** `dd` liest hier von der Gerätedatei `/dev/zero/` 1024 Blöcke mit Nullbytes zu je 1MB ein und erstellt daraus die Datei `bigfile1.data` im aktuellen Verzeichnis. Das Resultat ist eine 1 GB grosse Datei.

### 1. Komprimieren einzelner Dateien ohne Tar
Verwendet `gzip`, `bzip2` und `xz`, um eine einzelne Datei direkt zu komprimieren. 

1. Komprimiert zuerst `bigfile1.data` mit `gzip`, dann `bigfile2.data` mit `bzip2` und `bigfile3.data` mit `xz`. Was fällt euch auf? Was ist mit den Originaldateien passiert? Schaut vielleicht in den Manpages mal nach der Option `-k`.
2. Messt mit dem Kommando `time` wie lange die einzelenen Komprimierungsalgorithmen zur Komprimierung.
```bash
time <kommando>
```
3. Wie gross sind die resultierenden Dateien? 
```bash
ls -lh <datei>
du -h <datei>
```
4. Was schliesst ihr daraus bezüglich der einzelenen Algorithmen?
5. Ist `xz` am langsamsten, also am "schlechtesten"? Oder solltet ihr euch vielleicht auch noch das Verhalten beim Dekomprimieren anschauen? Testet doch auch das mal mit dem Kommando `time`.
6. Mit welchem Hintergrund wurde also `xz` entwickelt? Es ist wie gesagt der neuste der drei Komprimierungsalgorithmen.
7. Löscht vor den anderen Übungen die komprimierten Dateien, so dass nur noch die drei unkomprimierten Dateien mit der Endung `.data` im Verzeichnis übrig bleiben.

### 2. Erstellen eines Tar-Archivs ohne Kompression
Erstellt nun ein Tar-Archiv `backup.tar`, das alle Dateien aus dem aktuellen Verzeichnis enthält, also `bigfile1.data`, `bigfile2.data` und `bigfile3.data`.

>[!NOTE]
> Wenn euch die Komprimierung und Dekomprimierung mit den drei 1GB grossen Dateien zu lange dauert, könnt ihr auch z.B. zwei andere Dateien mit jeweils 512MG erstellen und diese für die folgenden Übungen nutzen:
```bash
dd if=/dev/zero of=mediumfile1.data bs=1M count=512
dd if=/dev/zero of=mediumfile2.data bs=1M count=512
```

### 3. Erstellen eines komprimierten Tar-Archivs
Erstellt ein gzip-komprimiertes Archiv `backup.tar.gz` aus dem aktuellen Verzeichnis und überprüfe die Dateigröße. Dazu könnt ihr natürlich das in Übung 2 erstellte Archiv einfach mit `gzip` komprimieren. Das sind aber zwei Schritte, die durchgeführt werden müssen. Vielleicht geht das ja auch in einem Schritt. Kann `tar` das etwa direkt? Sucht doch in der Manpage von `tar` mal nach dem Stichwort `Compression`.

Dekomprimiert das komprimierte Archiv anschliessend wieder mit `tar`.

Führt die beiden Schritte Komprimierung und Dekomprimierung auch für die anderen beiden Komprimierungsalgorithmen `bzip2` und `xz` durch. Notiert euch die jeweiligen Optionen. 

Wie könnt ihr euch diese merken? Gibt es vielleicht eine Art Eselsbrücke dafür?

### 4. Dekomprimieren der komprimierten Archive mit `tar`
Muss bei der Dekomprimierung mit `tar` zwingend der jeweilige Algorithmus als Option angegeben werden? Oder gibt es auch andere Möglichkeiten?

### 5. Falsche Dateiendung
Was passiert, wenn ihr eine Datei mit `bzip2` komprimierst, ihr aber die Dateiendung `.xz` verpasst (durch Umbenennen mit `mv`) und sie dann einfach mit `tar -xf` entpacken möchtet?

### 6. Entpacken ohne ursprüngliches Format zu kennen
Wie könnt ihr eine komprimierte Datei entpacken, ohne zu wissen, welches Kompressionsformat verwendet wurde? Stellt euch vor, es ist keine Dateiendung angegeben. Welches Kommando könnte hier hilfreich sein?

### 7. Fehlersuche: Doppelte Kompression
Was passiert, wenn ihr eine Datei mit `gzip` komprimierst und anschließend das komprimierte Ergebnis erneut mit `gzip` bearbeitest?

Was passiert, wenn ihr eine mit `gzip` komprimierte Datei zusätzlich mit z.B. `xz` komprimiert?

### 8. Versuch, ein Verzeichnis mit gzip, bzip2 oder xz zu komprimieren
Versucht, ein Verzeichnis direkt mit `gzip`, `bzip2` oder `xz` zu komprimieren, ohne `tar`. Funktioniert das? Warum bzw. warum nicht?

### 9. Komprimierung mit zip
Erstellt ein ZIP-Archiv `backup.zip`, das alle Dateien eines Verzeichnisses enthält (also z.B. alle `bigfile` Dateien). 

Wie unterscheidet sich das Verhalten von `zip` im Vergleich zu `tar` mit `gzip`? Ihr könnt auch hier die Kompressionsgeschwindigkeit und resultierende Dateigrösse vergleichen.

Hierzu müsst ihr `zip` aber erst einmal installieren: Als `root` mit `apt install zip`.

### 11. Komprimierungsalgorithmen 
Erstellt eine weitere Testdatei `bigfile_random.data` mit folgendem Kommando:
```bash
dd if=/dev/random of=bigfile_random.data bs=1M count=1024
```
Ihr verwendet hier die Gerätesdatei `/dev/random`. Diese produziert keine Nullbytes, sondern "Zufall".

Komprimiert diese Datei nun mit einem Algorithmus eurer Wahl oder auch mit allen drei. Was fällt euch auf? Was könnt ihr daraus über das generelle Vorgehen bzw. die Arbeitsweise eines Komprimierungsalgorithmus schliessen?
