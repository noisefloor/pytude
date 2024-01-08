# eine (kurze) Tour durch die Standardbibliothek

Wie an anderer Stelle bereits erwähnt, bringt die Python Standardinstallation eine sehr große Anzahl an Modulen für alle möglichen Aufgaben mit. Eine Übersicht gruppiert nach Themengebieten ist auf der Dokumentationsseite [Library Index](https://docs.python.org/3/library/index.html) zu finden.

Im Folgenden werden einige Module aus der Standardbibliothek (sehr) kurz vorgestellt. Häufig genutzte externe Module sind auf der Seite [externe Module](externallibs.md) kurz vorgestellt.

Man muss (und realistischer Weise kann) sicherlich nicht alle Module aus der Standardbibliothek in- und auswendig kennen. Und das ist auch nicht wirklich notwendig. Aber es ist durchaus hilfreich, wenn man sich mehr mit Python beschäftigt und mehr damit programmiert, eine Idee zu haben, für welche Aufgaben es bereits ein fertiges Modul in der Standardbibliothek gibt. Das reduziert den eigenen Programmieraufwand und man ist schneller mit der Erstellung seines Programms fertig.

## Kommandozeilenargumente
### sys.argv

Skripte müssen häufig Befehlszeilenargumente verarbeiten. Diese Argumente werden im argv-Attribut des [sys](https://docs.python.org/3/library/sys.html#module-sys) Moduls als Liste gespeichert. Nimmt man zum Beispiel die folgende Datei "demo.py":

```python
# File demo.py
import sys
print(sys.argv)
for counter, item in enumerate(sys.argv):
    print(f'{counter}: {item}')
```
und ruft diese im Terminal mit `python3 demo.py one 2 three` auf, erzeugt diese die folgende Ausgabe:

```
['demo.py', 'one', '2', 'three']
0: demo.py
1: one
2: 2
3: three
```

Die Liste `sys.argv` enthält an erster Position, also am Index Null, immer den Namen des Skripts, an den folgenden Positionen in chronologischer Reihenfolge die Argumente, die übergeben wurden.

Die Verwendung von `sys.argv` ist für das schnelle Testen oder einfache Argumente gut genug. Alle Argumente werden als String in die Liste aufgenommen. Auch wenn es wie im Falle der 2 eigentlich ein Integerwert ist.

### argparse

Das [argparse](https://docs.python.org/3/library/argparse.html#module-argparse) Modul bietet einen ausgefeilteren Mechanismus zur Verarbeitung von Befehlszeilenargumenten. Das folgende Skript extrahiert einen oder mehrere Dateinamen und eine optionale Anzahl von Zeilen, die angezeigt werden sollen:

```python
#file: top.py
import argparse

parser = argparse.ArgumentParser(
    prog='top',
    description='Show top lines from each file')
parser.add_argument('filenames', nargs='+')
parser.add_argument('-l', '--lines', type=int, default=10)
args = parser.parse_args()
print(args)
```

Wenn das Skript auf der Kommandozeile mit `python3 top.py --lines=5 alpha.txt beta.txt` ausgeführt wird, setzt es `args.lines` auf `5` (wobei die 5 als Integer Wert vorliegt) und `args.filenames` auf `['alpha.txt', 'beta.txt']`.

Das argparse-Modul ist immer die bessere Wahl als `sys.argv`, wenn man ein paar mehr und nicht-trivial Argumente übergeben will. Außerdem bietet argparse noch viel mehr Möglichkeiten wie die Ausgabe von Hilfstexten, Beschränkungen von Argumenten auf bestimmte Werte, sich gegenseitig ausschließende Argumente und vieles mehr.

## String Pattern Matching - reguläre Ausdrücke

Das Modul [re](https://docs.python.org/3/library/re.html#module-re) bietet Werkzeuge für [reguläre Ausdrücke](https://de.wikipedia.org/wiki/Regul%C3%A4rer_Ausdruck) zur erweiterten Verarbeitung von Zeichenketten. Für komplexe Übereinstimmungen und Manipulationen bieten reguläre Ausdrücke prägnante und optimierte Lösungen:

```pycon
>>> import re
>>> re.findall(r'\bf[a-z]*', 'which foot or hand fell fastest')
['foot', 'fell', 'fastest']
>>> re.sub(r'(\b[a-z]+) \1', r'\1', 'cat in the the hat')
'cat in the hat'
```

Mit dem re-Modul lässt sich Text nach (Teil-) Ausdrücken auch mit (sehr) komplexen Suchmustern durchsuchen.

## Mathematik
### math

Das Modul [math](https://docs.python.org/3/library/math.html) ermöglicht den Zugriff auf die zugrunde liegenden C-Bibliotheksfunktionen für Mathematik:

```pycon
>>> import math
>>> math.cos(math.pi / 4)
0.70710678118654757
>>> math.log(1024, 2)
10.0
```

## random
Das Modul [random](https://docs.python.org/3/library/random.html) stellt Werkzeuge zur Verfügung, um Zufallsauswahlen zu generieren:

```pycon
>>> importiere random
>>> random.choice(['Apfel', 'Birne', 'Banane'])
'Apfel'
>>> random.sample(bereich(100), 10)   # Stichprobe ohne Ersetzung
[30, 83, 16, 4, 8, 81, 41, 50, 18, 33]
>>> random.random() # zufälliger Float aus dem Intervall [0.0, 1.0)
0.17970987693706186
>>> random.randrange(6) # zufällige ganze Zahl aus dem Bereich(6)
4
```

### statistics

Das Modul [statistics](https://docs.python.org/3/library/statistics.html) berechnet die grundlegenden statistischen Eigenschaften (Mittelwert, Median, Varianz, etc.) von numerischen Daten:

```pycon
>>> import statistics
>>> data = [2.75, 1.75, 1.25, 0.25, 0.5, 1.25, 3.5]
>>> statistics.mean(data)
1.6071428571428572
>>> statistics.median(data)
1.25
>>> statistics.variance(data)
1.3720238095238095
```

## Internet
Es gibt eine Reihe von Modulen für den Zugriff auf das Internet und die Verarbeitung von Internetprotokollen. Zwei der einfachsten sind [urllib.request](https://docs.python.org/3/library/urllib.request.html#module-urllib.request) zum Abrufen von Daten von URLs und [smtplib](https://docs.python.org/3/library/smtplib.html#module-smtplib) für das Versenden von Mails:

```pycon
>>> from urllib.request import urlopen
>>> with urlopen('http://worldtimeapi.org/api/timezone/etc/UTC.txt') as response:
...     for line in response:
...         line = line.decode()             # Convert bytes to a str
...         if line.startswith('datetime'):
...             print(line.rstrip())         # Remove trailing newline
...
datetime: 2022-01-01T01:36:47.689215+00:00
```

```pycon
>>> import smtplib
>>> server = smtplib.SMTP('localhost')
>>> server.sendmail('soothsayer@example.org', 'jcaesar@example.org',
... """To: jcaesar@example.org
... From: soothsayer@example.org
...
... Beware the Ides of March.
... """)
>>> server.quit()
```

Hinweis: Das zweite Beispiel setzt einen auf localhost laufenden Mail-Server voraus.

## Datum und Zeit

Das [datetime](https://docs.python.org/3/library/datetime.html#module-datetime) Modul stellt Klassen zur Verfügung, mit denen Datums- und Zeitangaben sowohl auf einfache als auch auf komplexe Weise bearbeitet werden können. Während die Datums- und Zeitarithmetik unterstützt wird, liegt der Schwerpunkt der Implementierung auf der effizienten Extraktion von Daten für die Formatierung und Manipulation der Ausgabe. Das Modul unterstützt auch Objekte, die die Zeitzonen berücksichtigen.

```pycon
# dates are easily constructed and formatted
>>> from datetime import date, dateime
>>> today = date.today()
>>> today
datetime.date(2024, 1, 5)
>>> today.strftime("%m-%d-%y. %d %b %Y is a %A on the %d day of %B.")
'01-05-24. 05 Jan 2024 is a Friday on the 05 day of January.'
>>> # dates support calendar arithmetic
>>> birthday = date(1964, 7, 31)
>>> age = today - birthday
>>> age.days
21707
>>> now = datetime.now()
>>> now
datetime.datetime(2024, 1, 5, 21, 29, 30, 238507)
```

Hinweis: Das Modul heißt "datetime" und enthält ein Submodul mit dem Namen "datetime" - was zu Verwirrung führen kann. Um z.B. das aktuelle Datum und die aktuelle Uhrzeit zu erhalten, lautet der Aufruf je nach dem, was man importiert:

```pycon
>>> # wenn man nur das Modul datetime importiert
>>> import datetime
>>> now = datetime.datetime.now()
>>> # wenn man datetime aus datetime importiert
>>> from datetime import datetime
>>> now = datetime.now()
```

## pathlib - Umgang mit Pfaden und Verzeichnissen
Das Modul [pathlib](https://docs.python.org/3/library/pathlib.html) bietet eine Reihe von Klassen und Methoden zum Umgang mit Dateipfaden. Eine zentralle Klasse ist `Path`, welche einen Dateipfad darstellt.

Das Modul bietet unter anderem das Zusammensetzen von neuen Pfaden aus mehreren Path-Objekte, die Extraktion von Pfad, Dateiname und Dateiendung aus einem Path-Objekt sowie das Iterieren über den Inhalt eines Path-Ojekts, sofern es sich dabei um einen Verzeichnis handelt.

`pathlib` ist das Standardmodul für die Umgang mit Dateipfaden und hat die älteren Methoden aus dem `os` Modul weitesgehend ersetzt.

## Performance Messung

Einige Python-Benutzer sind sehr daran interessiert, die relative Leistung verschiedener Ansätze für dasselbe Problem zu kennen. Python bietet ein Messwerkzeug, das diese Fragen sofort beantwortet. So kann es zum Beispiel von Interesse sein, die Funktion zum Packen und Entpacken von Tupeln anstelle des traditionellen Ansatzes zum Vertauschen von Argumenten zu verwenden. Das [timeit](https://docs.python.org/3/library/timeit.html#module-timeit) Modul kann zur Messung der Ausführungsdauer eingesetzt werden:

```pycon
>>> from timeit import Timer
>>> Timer('t=a; a=b; b=t', 'a=1; b=2').timeit()
0.57535828626024577
>>> Timer('a,b = b,a', 'a=1; b=2').timeit()
0.54962537085770791
```

Im Gegensatz zur feinen Granularität von timeit bieten die Module [profile](https://docs.python.org/3/library/profile.html#module-profile) und [pstats](https://docs.python.org/3/library/profile.html#module-pstats) Werkzeuge zur Identifizierung zeitkritischer Abschnitte in größeren Codeblöcken.

## Tests zur Qualitätskontolle

Ein Ansatz für die Entwicklung qualitativ guter Software ist das Schreiben von Tests für jede Funktion während der Entwicklung und die häufige Ausführung dieser Tests während des Entwicklungsprozesses.

Das [doctest](https://docs.python.org/3/library/doctest.html#module-doctest) Modul bietet ein Werkzeug zum Scannen eines Moduls und zur Validierung von Tests, die in den Docstrings eines Programms eingebettet sind. Die Testkonstruktion ist so einfach wie das Ausschneiden und Einfügen eines typischen Aufrufs zusammen mit seinen Ergebnissen in den Docstring. Dies verbessert die Dokumentation, indem dem Benutzer ein Beispiel zur Verfügung gestellt wird, und es ermöglicht dem doctest-Modul, sicherzustellen, dass der Code der Dokumentation entspricht:

```python
def average(values):
    """Computes the arithmetic mean of a list of numbers.

    >>> print(average([20, 30, 70]))
    40.0
    """
    return sum(values) / len(values)

import doctest
doctest.testmod()   # führt die im Docstring enthaltenen Tests automatisch aus
```

Das [unittest](https://docs.python.org/3/library/unittest.html#module-unittest) Modul erfordert mehr Aufwand als das doctest-Modul, aber es ermöglicht die Pflege eines umfangreicheren und detaillierten Satzes von Tests in einer separaten Datei:

```python
import unittest

class TestStatisticalFunctions(unittest.TestCase):

    def test_average(self):
        self.assertEqual(average([20, 30, 70]), 40.0)
        self.assertEqual(round(average([1, 5, 7]), 1), 4.3)
        with self.assertRaises(ZeroDivisionError):
            average([])
        with self.assertRaises(TypeError):
            average(20, 30, 70)

unittest.main()  # Calling from the command line invokes all tests
```

## locale - lokale Einstellungen für Zahlen und Währung

Das Modul [locale](https://docs.python.org/3/library/locale.html#module-locale) greift auf eine Datenbank mit kulturspezifischen Datenformaten zu. Das Attribut `grouping` der locale-Formatfunktion bietet eine direkte Möglichkeit, Zahlen mit Gruppentrennzeichen zu formatieren:

```pycon
>>> import locale
>>> locale.setlocale(locale.LC_ALL, 'de_DE.utf-8')   # auf deutschen Standard setzen
'de_DE.utf-8'
>>> conv = locale.localeconv()
>>> conv
{'int_curr_symbol': 'EUR', 'currency_symbol': '€', 'mon_decimal_point': ',', 'mon_thousands_sep': '.', 'mon_grouping': [3, 0], 'positive_sign': '', 'negative_sign': '-', 'int_frac_digits': 2, 'frac_digits': 2, 'p_cs_precedes': 0, 'p_sep_by_space': 1, 'n_cs_precedes': 0, 'n_sep_by_space': 1, 'p_sign_posn': 1, 'n_sign_posn': 1, 'decimal_point': ',', 'thousands_sep': '.', 'grouping': [3, 0]}
>>> x = 1234567.8
>>> >>> locale.format_string("%d", x, grouping=True)   # x auf Ganzzahl mit Tausender-Separator formatieren
'1.234.567'
>>> locale.format_string("%s%.*f", (conv['currency_symbol'],
... conv['frac_digits'], x), grouping=True)   # x auf Betrag in Euro mit zwei Nachkommastellen formatieren
>>> '$1,234,567.80'
```

## Logging

Das [logging](https://docs.python.org/3/library/logging.html#module-logging) Modul bietet ein vollwertiges und flexibles Logging-System. Im einfachsten Fall werden die Protokollmeldungen in eine Datei oder an sys.stderr gesendet:

```python
import logging
logging.debug('Debugging information')
logging.info('Informational message')
logging.warning('Warning:config file %s not found', 'server.conf')
logging.error('Error occurred')
logging.critical('Critical error -- shutting down')
```

Das generiert die folgende Ausgabe:

```WARNING:root:Warning:config file server.conf not found
ERROR:root:Error occurred
CRITICAL:root:Critical error -- shutting down
```

Standardmäßig werden Informations- und Debugging-Meldungen unterdrückt und die Ausgabe wird an den stderr gesendet. Weitere Ausgabeoptionen sind neben dem Logging in eine Datei die Weiterleitung von Nachrichten über E-Mail, Datagramme, Sockets oder an einen HTTP-Server. Neue Filter können je nach Nachrichtenpriorität eine andere Weiterleitung auswählen. Das logging-Modul kennt fünf verschiedene Stufen der Wichtigkeit eines Logeintrags: DEBUG, INFO, WARNING, ERROR und CRITICAL.

Das Logging-System kann direkt von Python aus konfiguriert oder aus einer vom Benutzer bearbeitbaren Konfigurationsdatei geladen werden, um die Protokollierung ohne Änderung der Anwendung anzupassen.

## Dezimalzahlen

Das [decimal](https://docs.python.org/3/library/decimal.html#module-decimal) Modul bietet einen [Decimal](https://docs.python.org/3/library/decimal.html#decimal.Decimal) Datentyp für dezimale Fließkomma-Arithmetik. Im Vergleich zur eingebauten float-Implementierung der binären Fließkommadarstellung ist die Klasse besonders hilfreich für:

 * Finanzanwendungen und andere Anwendungen, die eine exakte Dezimaldarstellung erfordern
 * Kontrolle über die Genauigkeit
 * Kontrolle über die Rundung, um gesetzliche oder regulatorische Anforderungen zu erfüllen
 * Verfolgung signifikanter Dezimalstellen
 * Anwendungen, bei denen der Benutzer erwartet, dass die Ergebnisse mit von Hand durchgeführten Berechnungen übereinstimmen.

Die Berechnung einer 5 %igen Steuer auf eine 70-Cent-Telefongebühr ergibt beispielsweise unterschiedliche Ergebnisse in dezimaler und binärer Fließkommadarstellung. Der Unterschied wird signifikant, wenn die Ergebnisse auf den nächsten Cent gerundet werden:

```pycon
from decimal import Decimal, round
>>> round(Decimal('0.70') * Decimal('1.05'), 2)
Decimal('0.74')
>>> round(.70 * 1.05, 2)
0.73
```

Das Dezimal-Ergebnis behält eine nachgestellte Null und schließt automatisch von Multiplikatoren mit zweistelliger Wertigkeit auf eine vierstellige Wertigkeit. Decimal reproduziert Mathematik, wie sie von Hand gemacht wird, und vermeidet Probleme, die entstehen können, wenn binäre Fließkommazahlen dezimale Größen nicht exakt darstellen können.

Die exakte Darstellung ermöglicht es der Decimal-Klasse, Modulo-Berechnungen und Gleichheitstests durchzuführen, die für binäre Fließkommazahlen ungeeignet sind:

```pycon
>>> Decimal('1.00') % Decimal('.10')
Decimal('0.00')
>>> 1.00 % 0.10
0.09999999999999995
>>> sum([Decimal('0.1')]*10) == Decimal('1.0')
True
>>> 0.1 + 0.1 + 0.1 + 0.1 + 0.1 + 0.1 + 0.1 + 0.1 + 0.1 + 0.1 == 1.0
False
```

Wer mehr zur systembedingten Ungenauigkeit von Rechnungen mit floats (Gleitkommazahlen) erfahren möchte, der sollte den Artikel [Floating Point Arithmetic: Issues and Limitations](https://docs.python.org/3/tutorial/floatingpoint.html) in der Pythondokumentation lesen. Das Problem betrifft übrigens alle Programmiersprachen, weil es durch die Art und Weise bedingt ist, wie Computer intern arbeiten

Das Dezimalmodul ermöglicht das Rechnen mit der benötigten Genauigkeit:

```pycon
>>> getcontext().prec = 36
>>> Decimal(1) / Decimal(7)
Decimal('0.142857142857142857142857142857142857')
```

## Nebenläufige Programmierung

Threading ist eine Technik zur Entkopplung von Aufgaben, die nicht sequenziell voneinander abhängig sind. Mit Threads kann die Reaktionsfähigkeit von Anwendungen verbessert werden, die Benutzereingaben akzeptieren, während andere Aufgaben im Hintergrund laufen. Ein verwandter Anwendungsfall ist die parallele Ausführung von Eingabe / Ausgabe Operation (kurz I/O genannt, vom englischen "Input / Output") mit Berechnungen in einem anderen Thread.

Der folgende Code zeigt, wie das High-Level [threading](https://docs.python.org/3/library/threading.html#module-threading)  Modul Aufgaben im Hintergrund ausführen kann, während das Hauptprogramm weiterläuft:

```python
import threading, zipfile

class AsyncZip(threading.Thread):
    def __init__(self, infile, outfile):
        threading.Thread.__init__(self)
        self.infile = infile
        self.outfile = outfile

    def run(self):
        f = zipfile.ZipFile(self.outfile, 'w', zipfile.ZIP_DEFLATED)
        f.write(self.infile)
        f.close()
        print('Finished background zip of:', self.infile)

background = AsyncZip('mydata.txt', 'myarchive.zip')
background.start()
print('The main program continues to run in foreground.')
background.join()    # Wait for the background task to finish
print('Main program waited until background was done.')
```

Die größte Herausforderung bei Anwendungen mit mehreren Threads ist die Koordinierung von Threads, die Daten oder andere Ressourcen gemeinsam nutzen. Zu diesem Zweck bietet das Threading-Modul eine Reihe von Synchronisationsprimitiven, darunter Locks, Events, Condition Variables und Semaphorem.

Diese Werkzeuge sind zwar sehr leistungsfähig, aber kleine Designfehler können zu Problemen führen, die schwer zu reproduzieren sind. Daher besteht der bevorzugte Ansatz für die Aufgabenkoordination darin, den gesamten Zugriff auf eine Ressource in einem einzigen Thread zu konzentrieren und dann das [queue](https://docs.python.org/3/library/queue.html#module-queue) Modul zu verwenden, um diesen Thread mit Anfragen von anderen Threads zu versorgen. Anwendungen, die [Queue](https://docs.python.org/3/library/queue.html#queue.Queue) Objekte für die Inter-Thread-Kommunikation und -Koordination verwenden, sind einfacher zu entwerfen, besser lesbar und zuverlässiger.

*Hinweis*: Threads, über das threading Modul in einem Python-Skript gestartet werden, laufen im Interpreter von CPython (welches hier, wie ganz am Anfang des Tutorials gesagt, behandelt wird) nicht parallel, es kann nur ein Thread gleichzeitig laufen. Dies ist durch Interna des Python-Interpreter an sich bedingt, nämlich den [GIL](https://wiki.python.org/moin/GlobalInterpreterLock) (ausgeschrieben: great interpreter lock). Wer mehr zu dem Thema wissen möchte, kann z.B. im Internet nach "python gil" suchen.

### weitere Module für nebenläufige Programmierung

Python hat standardmäßig noch drei weitere Module für nebenläufige Programmierung an Bord:

 * [multiprocessing](https://docs.python.org/3/library/multiprocessing.html): erlaubt es mehrere eigenständige Prozesse auf einem Python-Skript heraus zu starten. Die Prozesse können dann auch parallel laufen.
 * [concurrent.futures](https://docs.python.org/3/library/concurrent.futures.html): Das Modul `concurrent.futures` bietet eine High-Level-Schnittstelle für die asynchrone Ausführung von Aufgaben. Die Schnittstell ist für Threads und Multiprocessing identisch. Zentrales Element des Moduls die die `Executer` Klasse, die sich um die Aufgabenverteilung kümmert.
 * [asyncio](https://docs.python.org/3/library/asyncio.html): Nebenläufigkeit mit `async`/`await` Syntax. Besonders geeignet für asyncio für I/O-lastigem und hochstrukturierten Netzwerkcode.

## weitere Module
 * [iterttools](https://docs.python.org/3/library/itertools.html): Das Modul `itertools` bietet eine größere Sammlung von Funktionen, die verschiedene Iterator zum Iterieren über Sequenzen bereitstellen
 * [SQLite Datenbank](https://docs.python.org/3/library/sqlite3.html): Das Modul `sqlite3` ist ein Wrapper für die SQLite-Datenbankbibliothek und stellt eine persistente Datenbank zur Verfügung, die aus Python heraus genutzt werden kann.
 * [email](https://docs.python.org/3/library/email.html#module-email): Das E-Mail-Paket ist eine Bibliothek zur Verwaltung von E-Mail-Nachrichten, einschließlich MIME- und anderer RFC 2822-basierter Nachrichtendokumente. Im Gegensatz zu smtplib und poplib, die tatsächlich Nachrichten senden und empfangen, verfügt das E-Mail-Paket über ein komplettes Toolset zum Erstellen oder Dekodieren komplexer Nachrichtenstrukturen (einschließlich Anhängen) und zur Implementierung von Internet-Kodierungs- und Header-Protokollen.
 * XML: Die XML-Verarbeitung wird durch die Pakete [xml.etree.ElementTree](https://docs.python.org/3/library/xml.etree.elementtree.html#module-xml.etree.ElementTree), [xml.dom](https://docs.python.org/3/library/xml.dom.html#module-xml.dom) und [xml.sax](https://docs.python.org/3/library/xml.sax.html#module-xml.sax) unterstützt. Zusammen vereinfachen diese Module und Pakete den Datenaustausch zwischen Python-Anwendungen und anderen Tools erheblich
 * [tkinter](https://docs.python.org/3/library/tkinter.html): Mit Hilfe des tkinter-Moduls können grafische Benutzeroberflächen erstellt werden. tkinter ist nicht so umfangreich wie "große" Frameworks wie z.B. Qt, GTK oder Kivy, ist aber relativ einfach zu Lernen und ist durchaus für nicht allzu komplexe GUIs geeignet. Um sauber und strukturiert mit tkinter programmieren zu können sollte man Klassen und deren Nutzung verstanden haben, da man für die GUI in der Regel immer in einer Klasse kapseln möchte (und nicht also lose Sammlung von Funktionen erstellt).

 * nächstes Kapitel: [venv - virtuelle Umgebungen für Python](venv.md)
 * vorheriges Kapitel: [Klassen](classes.md)
