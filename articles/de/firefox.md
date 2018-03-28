Wenn sich Firefox nicht ordnungsgemäß beendet, kann es passieren, dass Firefox nicht mehr startet. Es erscheint folgende Fehlermeldung:

```
Close Firefox: A copy of Firefox is already open. Only one copy of Firefox can be open at a time.
```

Damit Firefox wieder startet müsst ihr die Datei `.parentlock` in folgendem Pfad löschen: `~/Library/Application Support/Firefox/Profiles/.default/.parentlock`.
Den Pfad könnt ihr im Terminal mit `find ~/ -iname .parentlock` herausfinden.

Folgender Terminal-Befehl löst also das Problem:
```
rm ~/Library/Application Support/Firefox/Profiles/.default/.parentlock
```
