# der Python-Interpreter

## Installation
Je nach verwendetem Betriebssystem ist der Python-Interpreter bereits vorinstalliert und kann direkt verwendet werden. Ansonsten lässt dieser sich einfach für eine Vielzahl von Plattformen nachinstallieren.

Um nachzuschauen, ob Python bereits installiert ist, öffnet man einen Terminal / eine Kommandozeile und führt doch den Befehl

```shell
python
#oder
python3
```

aus.

Ob der Interpreter mit `python` oder `python3` aufgerufen wird hängt vom Betriebssystem und dessen Einstellungen ab. Wenn der Interpreter vorhanden ist sollte man eine Ausgabe wie

```shell
Python 3.10.11 (tags/v3.10.11:7d4cc5a, Apr  5 2023, 00:38:17) [MSC v.1929 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

erhalten. Die erste Zeile, welche mit "Python 3. ..." beginnt kann je nach installierter Python-Version und Betriebssystem variieren. Hinter `>>>` sollte der Cursor blinken.

### Installation Linux
Bei den meisten Linux-Distributionen ist Python bereits in in der Standardinstallation enthalten. Ansonsten muss man in der Regel das Paket **python3** installieren.

*Hinweis*: Einige Linux-Distributionen haben eventuell noch ein Paket namens **python** in den Paketquellen, welches Python 2.7 installiert. Python 2.7 ist seit dem 1.1.2020 ohne Support durch die Entwickler und sollte deshalb nicht mehr verwendet werden.

### Installation Windows
Unter Windows lässt sich Python einfach über das Microsoft Store installieren. Dazu öffnet man dieses und sucht nach "Python". Man sollte mehrere Suchtreffer für verschiedene Python-Version erhalten, welche alle von der Python Software Foundation bereitgestellt werden. Sofern man nicht eine bestimmte Version nutzen muss, sollte man hier immer die aktuellste Python-Version zur Installation auswählen.

Weitere Information zur Nutzung unter Windows sind in der [Python Dokumentation](https://docs.python.org/3/using/windows.html) zu finden.

### Installation MacOS
Python lässt sich für MacOS entweder über HomeBrew installieren `brew install python` oder man lädt von der [Python Downloadseite für MacOS](https://www.python.org/downloads/macos/) den Installer für die aktuelle Version heruntern.

Weitere Informationen zur Nutzung unter MacOS sind in der [Python Dokumentation](https://docs.python.org/3/using/mac.html) zu finden.

## Aufruf des Interpreters

Zum Aufruf des Python-Interpreters öffnet man einen Terminal / eine Eingabeaufforderung und führt den Befehl

```shell
python3
```

oder

```shell
python
```

aus. Der Befehl, also ob `python3` oder `python`, hängt vom Betriebssystem und der Konfiguration an. Für dieses Tutorial wird im weiteren Verlauf `python3` genutzt.

Um den Interpreter wieder zu verlassen drückt man unter Linux und MacOS die Tasten <kbd>STRG</kbd>+<kbd>d</kbd>, unter Windows <kbd>STRG</kbd>+<kbd>z</kbd>. Alternativ kann man am Prompt des Interpreters auf `quit()` eintippen und dann die Eingabetaste drücken.

Zu den Zeilenbearbeitungsfunktionen des Interpreters gehören die interaktive Bearbeitung, die Substitution der Historie und die Code-Vervollständigung auf Systemen, die die Bibliothek [GNU Readline](https://tiswww.case.edu/php/chet/readline/rltop.html) unterstützen. Der vielleicht schnellste Test, ob die Kommandozeilenbearbeitung unterstützt wird, ist die Eingabe von <kbd>STRG</kbd>+<kbd>p</kbd> bei der ersten Python-Eingabeaufforderung, die man erhält. Wenn es piept, kann mna die Befehlszeile bearbeiten. Wenn nichts passiert oder `^P` als Echo ausgegeben wird, ist die Befehlszeilenbearbeitung nicht verfügbar. Dann kann man dann nur die Rücktaste verwenden, um Zeichen aus der aktuellen Zeile zu entfernen.

Der Interpreter arbeitet ähnlich wie die Unix-Shell: Wenn er mit einer an ein tty-Gerät angeschlossenen Standardeingabe aufgerufen wird (oder einfacher ausdrückt: Monitor und Tastatur), liest er Befehle und führt sie interaktiv aus. Wenn er mit einem Dateinamenargument oder mit einer Datei als Standardeingabe aufgerufen wird, liest er ein *Skript* aus dieser Datei und führt es aus.

Eine zweite Möglichkeit, den Interpreter zu starten, ist `python3 -c command [arg] ...`, was die Anweisung(en) in *command* ausführt, analog zur `-c`-Option der Shell. Da Python-Anweisungen oft Leerzeichen oder andere für die Shell spezielle Zeichen enthalten, ist es normalerweise ratsam, *command* vollständig in Anführungszeichen zu setzen, also z.B. `python3 "dies ist mein erstes skript.py"`.

Einige Python-Module sind auch als Skripte nützlich. Diese können mit `python -m module [arg] ...` aufgerufen werden, was die Quelldatei für *module* so ausführt, als ob Sie den vollen Namen auf der Kommandozeile geschrieben hätten.

Wenn eine Skriptdatei verwendet wird, ist es manchmal nützlich, das Skript auszuführen und danach in den interaktiven Modus zu wechseln.  Dies kann durch die Übergabe von `-i` vor dem Skript erreicht werden.

Alle Kommandozeilenoptionen sind im Abschnitt [Command line and environment](https://docs.python.org/3/using/cmdline.html) in der Python-Dokumentation zu finden.

## Argumente übergeben

Wenn der Skriptname und die zusätzlichen Argumente dem Interpreter bekannt sind, werden sie in eine Liste von Zeichenketten umgewandelt und der Variable `argv` im Modul `sys` zugewiesen.  Man kann auf diese Liste zugreifen, indem man `import sys` ausführt. Die Länge der Liste ist mindestens eins. Wenn kein Skript und keine Argumente angegeben werden, ist `sys.argv[0]` ein leerer String.  Wenn der Skriptname als `'-'` angegeben wird (was Standard-Eingabe bedeutet), wird `sys.argv[0]` auf `'-'` gesetzt.  Wenn `-c` *Befehl* verwendet wird, wird `sys.argv[0]` auf `'-c'` gesetzt.  Wenn `-m` *Modul* verwendet wird, wird `sys.argv[0]` auf den vollen Namen des gefundenen Moduls gesetzt.  Optionen, die nach `-c` *Befehl* oder `-m` *Modul* gefunden werden, werden nicht von der Optionsverarbeitung des Python-Interpreters verbraucht, sondern in `sys.argv` belassen, damit der Befehl oder das Modul sie verarbeiten kann.

## Interaktiver Modus

Wenn Befehle von einem tty (Terminal) gelesen werden, befindet sich der Interpreter im *interaktiven Modus*.  In diesem Modus fordert er mit dem *primären Prompt*, normalerweise drei Größer-als-Zeichen (`>>>`), zum nächsten Befehl auf. Bei Fortsetzungszeilen fordert er mit dem *sekundären Prompt*, standardmäßig drei Punkte (`...`), zu weiteren Eingaben auf. Der Interpreter gibt eine Willkommensnachricht mit seiner Versionsnummer und einem Copyright-Hinweis aus bevor er den ersten Prompt ausgibt, wie z.B.

```shell
$ python3
Python 3.13 (default, April 4 2023, 09:25:04)
[GCC 10.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

Fortsetzungszeilen werden benötigt, wenn ein mehrzeiliges Konstrukt eingegeben wird. Wie im folgenden Beispiel die `if`-Anweisung:

```pycon
>>> the_world_is_flat = True
>>> if the_world_is_flat:
...     print("Be careful not to fall off!")
...
Be careful not to fall off!
```

## der Interpreter und seine Umgebung

### Quellcode Encoding

Als "Encoding" bezeichnet man im Kontext der Programmierung, welche Kodierung für Zeichen (Buchstaben, Zahlen etc.) verwendet wird. Standardmäßig werden Python-Quelldateien als in [UTF-8](https://de.wikipedia.org/wiki/UTF-8) kodiert behandelt. In dieser Kodierung können Zeichen der meisten Sprachen der Welt gleichzeitig in Stringliteralen, Bezeichnern und Kommentaren verwendet werden - obwohl die Standardbibliothek nur [ASCII-Zeichen](https://de.wikipedia.org/wiki/American_Standard_Code_for_Information_Interchange) für Bezeichner verwendet. Dies ist eine Konvention, die jeder portable Code befolgen sollte. Um all diese Zeichen richtig darzustellen, muss der verwendete Editor erkennen, dass es sich um eine UTF-8-Datei handelt. Und er muss eine Schriftart verwenden, die alle Zeichen in der Datei unterstützt.

Um eine andere Kodierung als die Standardkodierung zu deklarieren, sollte eine spezielle Kommentarzeile als *erste* Zeile der Datei hinzugefügt werden. Die Syntax lautet wie folgt:

```python
# -*- coding: encoding -*-
```

wobei *encoding* einer der für Python gültigen [Codecs](https://docs.python.org/3/library/codecs.html) sein muss.

Um z.B. das Window-1252 Encoding zu verwenden, muss die Zeile wiefolgt lauten:

```python
# -*- coding: cp1252 -*-
```

Eine Ausnahme von der ersten Zeile Regel besteht, wenn der Quellcode mit einer UNIX [Shebang](https://de.wikipedia.org/wiki/Shebang) beginnt. In diesem Fall sollte die Kodierungserklärung in der zweiten Zeile der Datei hinzugefügt werden. Zum Beispiel:

```python
#!/usr/bin/env python3
# -*- coding: cp1252 -*-
```

Sofern es keinen triffigen Grund gibt, eine anderen Codierung als UTF-8 zu nutzen, sollte man immer bei diesem Standard-Encoding von Python bleiben.

***

 * nächste Seite: [eine ungezwungene Einführung in Python](introduction.md)
 * vorherige Seite: [ein kleiner Appetitanreger auf Python](appetite.md)
