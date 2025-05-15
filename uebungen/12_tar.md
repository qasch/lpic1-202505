# Übungen zu `tar` unter Linux

## Übung 1: Einfache Archivierung
Erstelle ein Archiv aus mehreren Dateien.

1. Erstelle drei Textdateien: `file1.txt`, `file2.txt` und `file3.txt`.
2. Erstelle ein Archiv `backup.tar`, das diese drei Dateien enthält.
3. Überprüfe, ob das Archiv erfolgreich erstellt wurde.

## Übung 2: Archivinhalt anzeigen
Zeige den Inhalt eines bestehenden `tar`-Archivs an.

1. Verwende das zuvor erstellte `backup.tar`.
2. Liste die enthaltenen Dateien auf.

## Übung 3: Archiv erweitern
Füge eine Datei zu einem bestehenden Archiv hinzu.

1. Erstelle eine neue Datei `file4.txt`.
2. Füge diese Datei zum `backup.tar`-Archiv hinzu.
3. Überprüfe, ob die Datei nun im Archiv enthalten ist.

## Übung 4: Archiv extrahieren
Entpacke ein `tar`-Archiv.

1. Lösche die ursprünglichen `file1.txt`, `file2.txt`, `file3.txt` und `file4.txt`.
2. Extrahiere alle Dateien aus `backup.tar`.
3. Prüfe, ob die Dateien korrekt wiederhergestellt wurden.

## Übung 5: Einzelne Datei extrahieren
Extrahiere nur eine bestimmte Datei aus einem `tar`-Archiv.

1. Lösche nun die Dateien `file1.txt`, `file2.txt`, `file3.txt` und `file4.txt`.
2. Extrahiere nur `file2.txt` aus `backup.tar`.
3. Stelle sicher, dass die anderen Dateien nicht erneut extrahiert wurden.

## Übung 6: Archiv mit Verzeichnis erstellen
Erstelle ein Archiv, das ein ganzes Verzeichnis enthält. Es soll also ein gesamtes Verzeichnis und nicht nur der gesamte Inhalt des Verzeichnisses zum Archiv hinzugefügt werden.

1. Erstelle ein Verzeichnis `mydir` und darin einige Textdateien.
2. Erstelle ein `tar`-Archiv, das das gesamte Verzeichnis `mydir` enthält. Verwende einen **relativen** Pfad zu diesem Verzeichnis.
2. Erstelle ein weiteres `tar`-Archiv, das das gesamte Verzeichnis `mydir` enthält. Verwende diesmal einen **absoluten** Pfad zu diesem Verzeichnis.
3. Überprüfe den Inhalt der beiden Archive.

## Übung 7: Archiv mit Dateiattributen erstellen
Erstelle ein `tar`-Archiv unter Beibehaltung der Dateiattribute.

1. Setze spezielle Berechtigungen oder Zeitstempel auf einige Dateien.
2. Erstelle ein Archiv, das diese Dateien mit ihren Attributen speichert.
3. Extrahiere das Archiv und prüfe, ob die Attribute erhalten bleiben.

## Übung 8: Differentielles Archiv erstellen
Erstelle ein Archiv von Dateien, die seit einer bestimmten Zeit geändert wurden. Nutze dafür vielleicht das Archiv aus Übung 6.

1. Erstelle einige Dateien und ändere einige davon nach einer gewissen Zeit.
2. Erstelle ein `tar`-Archiv, das nur die geänderten Dateien enthält.
3. Überprüfe den Archivinhalt.

## Übung 9: Beschädigtes Archiv überprüfen
Überprüfe ein `tar`-Archiv auf Fehler.

1. Erstelle ein `tar`-Archiv und Simuliere eine Beschädigung, indem du einige Bytes entfernst:
```sh
truncate -s -10 backup.tar
```
2. Versuche, es auf Fehler zu überprüfen.

## Übung 10: Archiv ohne bestimmte Dateien erstellen
Erstelle ein Archiv, das bestimmte Dateien oder Dateitypen ausschließt.

1. Erstelle verschiedene Dateitypen (`.txt`, `.log`, `.cfg`).
2. Erstelle ein `tar`-Archiv, das alle Dateien außer `.log`-Dateien enthält.
3. Überprüfe den Inhalt des Archivs, um sicherzustellen, dass `.log`-Dateien fehlen.

## Übung 11: Archiv an einem bestimmten Ort extrahieren
Extrahiere das in der vorigen Übung erstellte Archiv in einem bestimmten Verzeichnis, ohne vorher dorthin zu navigieren.

1. Erstelle ein Verzeichnis in deinem Heimatverzeichnis mit dem Namen `some-dir-for-tar`.
2. Wechsel **nicht** dorthinein, sondern extrahiere den Inhalt des Archivs in dieses Verzeichnis von dort aus, wo du dich gerade befindest.
3. Überprüfe, ob alles wie gewünscht funktioniert hat.
