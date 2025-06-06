# Eingabe und Ausgabe

Es gibt mehrere Wege, die Ausgabe eines Programms zu gestalten. Diese kann in einer gut lesbaren Form auf einem Bildschirm ausgegeben oder zur Weiterverwendung in eine Datei geschrieben werden. In diesem Kapitel werden einige Möglichkeiten vorgestellt.

## Schönere Formatierung der Ausgabe

Bis jetzt sind in diesem Tutorial zwei Wege zur Ausgabe genutzt worden: die `print` Funktion sowie die Ausgabe von Rückgabewerten von Methoden und ähnlichem. Es gibt noch einen dritten Weg über die [write](https://docs.python.org/3/library/io.html#io.TextIOBase.write) Methoden von "Datei-ähnlichen Objekten", wobei die Standardausgabe als Datei-ähnliches Objekt namens `sys.stdout` genutzt wird. Darauf wird im Rahmen dieses Tutorials aber nicht weiter eingegangen.

Man benötigt aber öfters eine bessere Kontrolle über die Formatierung als die simple, durch Leerzeichen getrennte Ausgabe. Dafür gibt es mehrere Wege.

Die Nutzung von [f-Strings](https://docs.python.org/3/reference/lexical_analysis.html#f-strings), welche mit Python 3.6 eingeführt wurde. Dazu setzt man ein `f` oder `F` vor die öffnenden Anführungszeichen des Strings. Im String selber können dann Pythonausdrücke (wie z.B. Variablennamen) in geschweifte Klammer `{}` eingeschlossen werden.

```pycon
>>> year = 2016
>>> event = 'Referendum'
>>> f'Results of the {year} {event}'
'Results of the 2016 Referendum'
```

Die [format-Methode](https://docs.python.org/3/library/stdtypes.html#str.format) von Strings ist äquivalent, bedingt aber mehr Tipparbeit. Innerhalb des Strings werden wie bei f-Strings Python-Ausdrücke (wie z.B. Variablennamen) in geschweifte Klammern `{}` eingeschlossen werden, aber man muss zusätzliche Informationen hinterlegen, womit diese dann gefüllt werden sollen.

```pycon
>>> yes_votes = 42_572_654
>>> total_votes = 85_705_149
>>> percentage = yes_votes / total_votes
>>> '{:-9} YES votes  {:2.2%}'.format(yes_votes, percentage)
' 42572654 YES votes  49.67%'
```

Im Beispiel wird `yes_votes` mit Leerstellen aufgefüllt und eine Minus-Zeichen vorangestellt, falls die Zahl negativ ist. Außerdem wird `percentage` mit 100 multipliziert und mit zwei Nachkommastellen sowie folgendem Prozentzeichen ausgegeben

Schließlich kann man die gesamte String-Verarbeitung selbst übernehmen, indem man String-Slicing und Verkettungsoperationen verwenden, um jedes erdenkliche Layout zu erstellen. Der String-Typ verfügt über einige Methoden, die nützliche Operationen zum Auffüllen von Strings auf eine bestimmte Spaltenbreite durchführen. Diese Methode ist die unüblichste und wird - im Vergleich zu f-Strings und der format-Methode - eher selten angewendet.

Wenn man keine ausgefallene Ausgabe benötigt, sondern nur eine schnelle Anzeige einiger Variablen zu Debugging-Zwecken, kann man jeden Wert mit den Funktionen [repr](https://docs.python.org/3/library/functions.html#repr) oder [str](https://docs.python.org/3/library/functions.html#str) in eine Zeichenkette umwandeln.

Die Funktion `str` soll Darstellungen von Werten zurückgeben, die für den Menschen passabel lesbar sind, während `repr` Darstellungen erzeugen soll, die vom Interpreter gelesen werden können (oder einen `SyntaxError` erzwingt, wenn es keine entsprechende Syntax gibt). Für Objekte, die keine besondere Darstellung für das Lesen durch Menschen haben, gibt `str` den gleichen Wert zurück wie `repr`. Viele Werte, wie z.B. Zahlen oder Datenstrukturen wie Listen und Wörterbücher, können mit beiden Funktionen gleich dargestellt werden. Insbesondere Zeichenketten haben zwei unterschiedliche Darstellungen.

Einige Beispiele:

```pycon
>>> s = 'Hello, world.'
>>> str(s)
'Hello, world.'
>>> repr(s)
"'Hello, world.'"
>>> str(1/7)
'0.14285714285714285'
>>> x = 10 * 3.25
>>> y = 200 * 200
>>> s = 'The value of x is ' + repr(x) + ', and y is ' + repr(y) + '...'
>>> print(s)
The value of x is 32.5, and y is 40000...
>>> # repr() für einen String, fügt Anführungsstriche und Backslashes hinzu
>>> hello = 'hello, world\n'
>>> hellos = repr(hello)
>>> print(hellos)
'hello, world\n'
>>> # repr() kann ein Python Objekt als Argument übergeben werden.
... repr((x, y, ('spam', 'eggs')))
"(32.5, 40000, ('spam', 'eggs'))"
```

Das [string-Modul](https://docs.python.org/3/library/string.html#module-string) enthält eine [string.Template Klasse](https://docs.python.org/3/library/string.html#string.Template), die eine weitere Möglichkeit bietet, Werte in Strings zu ersetzen, indem sie Platzhalter wie `$x` verwendet und diese durch Werte aus einem Wörterbuch ersetzt, dafür aber viel weniger Kontrolle über die Formatierung bietet.

### Formatierte String-Literale - f-Strings

Formatierte String-Literale, kurz f-Strings genannt, erlauben es, den Wert von Python-Ausdrücken in eine Zeichenkette einzuschließen, indem man der Zeichenkette ein `f` oder `F` voranstellt und Ausdrücke als `{Ausdruck}` schreibt.

Ein optionaler Formatspezifizierer kann dem Ausdruck folgen. Dies erlaubt eine genauere Kontrolle darüber, wie der Wert formatiert wird. Das folgende Beispiel rundet Pi auf drei Stellen nach dem Komma:

```pycon
>>> import math
>>> print(f'The value of pi is approximately {math.pi:.3f}.')
The value of pi is approximately 3.142.
```

Die Übergabe einer Ganzzahl nach dem `':'` bewirkt, dass das Feld eine Mindestanzahl von Zeichen breit ist. Dies ist nützlich, um Spalten bündig auszurichten:

```pycon
>>> table = {'Sjoerd': 4127, 'Jack': 4098, 'Dcab': 7678}
>>> for name, phone in table.items():
...     print(f'{name:10} ==> {phone:10d}')
...
Sjoerd     ==>       4127
Jack       ==>       4098
Dcab       ==>       7678
```

Andere Modifikatoren können verwendet werden, um den Wert zu konvertieren, bevor er formatiert wird. `'a'` wendet `ascii` an, `'s'` wendet `str` an und `'r'` wendet `repr` an:

```pycon
>>> animals = 'eels'
>>> print(f'My hovercraft is full of {animals}.')
My hovercraft is full of eels.
>>> print(f'My hovercraft is full of {animals!r}.')
My hovercraft is full of 'eels'.
```

Der `=` Spezifizierer kann verwendet werden, um einen Ausdruck auf den Text des Ausdrucks, ein Gleichheitszeichen und dann die Darstellung des ausgewerteten Ausdrucks zu erweitern:

```pycon
>>> bugs = 'roaches'
>>> count = 13
>>> area = 'living room'
>>> print(f'Debugging {bugs=} {count=} {area=}')
Debugging bugs='roaches' count=13 area='living room'
```

Weitere Informationen über den `=` Spezifizierer sind [in der Python-Dokumentation](https://docs.python.org/3/whatsnew/3.8.html#bpo-36817-whatsnew) zu finden. Eine Referenz zu diesen Formatspezifikationen finden man im Referenzhandbuch unter [Format Specification Mini-Language](https://docs.python.org/3/library/string.html#formatspec).

### Die str.format-Methode

Die grundsätzliche Nutzung der `str.format` Methode sieht wie folgt aus:

```pycon
>>> print('We are the {} who say "{}!"'.format('knights', 'Ni'))
We are the knights who say "Ni!"
```

Die eckigen Klammern und die Zeichen darin (die sogenannten Formatfelder) werden durch die Objekte ersetzt, die an die Methode `str.format` übergeben werden. Eine Zahl in den Klammern kann verwendet werden, um auf die Position des Objekts zu verweisen, das an die Methode `str.format` übergeben wird:

```pycon
>>> print('{0} and {1}'.format('spam', 'eggs'))
spam and eggs
>>> print('{1} and {0}'.format('spam', 'eggs'))
eggs and spam
```

Wenn Schlüsselwortargumente in der Methode `str.format` verwendet werden, wird auf ihre Werte mit dem Namen des Arguments verwiesen:

```pycon
>>> print('This {food} is {adjective}.'.format(
...       food='spam', adjective='absolutely horrible'))
This spam is absolutely horrible.
```

Positions- und Schlüsselwortargumente können beliebig kombiniert werden:

```pycon
>>> print('The story of {0}, {1}, and {other}.'.format('Bill', 'Manfred',
...                                                    other='Georg'))
The story of Bill, Manfred, and Georg.
```

Wenn man einen sehr langen Formatierungsstring hat, den man nicht aufteilen will, kann man die zu formatierenden Variablen über den Namen statt über die Position referenzieren. Dazu übergibt man ein Wörterbuch und eckige Klammern `'[]'`, um auf die Schlüssel zuzugreifen:

```pycon
>>> table = {'Sjoerd': 4127, 'Jack': 4098, 'Dcab': 8637678}
>>> print('Jack: {0[Jack]:d}; Sjoerd: {0[Sjoerd]:d}; '
...       'Dcab: {0[Dcab]:d}'.format(table))
Jack: 4098; Sjoerd: 4127; Dcab: 8637678
```

Dies kann auch durch die Übergabe des `table` Wörterbuchs als Schlüsselwortargument mit der `**`-Notation machen:

```
>>> table = {'Sjoerd': 4127, 'Jack': 4098, 'Dcab': 8637678}
>>> print('Jack: {Jack:d}; Sjoerd: {Sjoerd:d}; Dcab: {Dcab:d}'.format(**table))
Jack: 4098; Sjoerd: 4127; Dcab: 8637678
```

Dies ist besonders nützlich in Kombination mit der eingebauten Funktion [vars](https://docs.python.org/3/library/functions.html#vars), die ein Wörterbuch mit allen lokalen Variablen zurückgibt. Als Beispiel erzeugen die folgenden Zeilen einen ordentlich ausgerichteten Satz von Spalten mit ganzen Zahlen und ihren Quadraten und Würfeln:

```pycon
>>> for x in range(1, 11):
...     print('{0:2d} {1:3d} {2:4d}'.format(x, x*x, x*x*x))
...
1   1    1
2   4    8
3   9   27
4  16   64
5  25  125
6  36  216
7  49  343
8  64  512
9  81  729
10 100 1000
```

Eine vollständige Übersicht über die `string.format` Methode ist in der Python-Dokumentation unter [Format String Syntax](https://docs.python.org/3/library/string.html#formatstrings) zu finden.

### Manuellele String-Formatierung

Das folgende Beispiel ist identisch mit dem vorherigen, nutzt aber eine vollständig manuelle Formatierung:

```pycon
>>> for x in range(1, 11):
...     print(repr(x).rjust(2), repr(x*x).rjust(3), end=' ')
...     # Note use of 'end' on previous line
...     print(repr(x*x*x).rjust(4))
...
1   1    1
2   4    8
3   9   27
4  16   64
5  25  125
6  36  216
7  49  343
8  64  512
9  81  729
10 100 1000
```

Zu beachten ist, dass da ein Leerzeichen zwischen den Spalten durch die Funktionsweise `print` hinzugefügt wurde: es fügt immer Leerzeichen zwischen seinen Argumenten ein.

Die Methode `str.rjust` von String-Objekten richtet eine Zeichenkette in einem Feld mit einer bestimmten Breite rechts aus, indem sie links mit Leerzeichen aufgefüllt wird. Es gibt ähnliche Methoden `str.ljust` und `str.center`. Diese Methoden schreiben nichts, sie geben nur eine neue Zeichenkette zurück. Wenn die Eingabezeichenkette zu lang ist, wird sie nicht abgeschnitten, sondern unverändert zurückgegeben. Das bringt zwar das Spaltenlayout durcheinander, ist aber normalerweise besser als die Alternative, bei der man über einen Wert lügen würde. Wenn man wirklich ein Abschneiden will, kann man immer eine Slice-Operation hinzufügen, wie in z.B. `x.ljust(n)[:n]`.

Es gibt eine weitere Methode, `str.zfill`, die eine numerische Zeichenkette auf der linken Seite mit Nullen auffüllt. Die Methode versteht auch Plus- und Minuszeichen:

```pycon
>>> '12'.zfill(5)
'00012'
>>> '-3.14'.zfill(7)
'-003.14'
>>> '3.14159265359'.zfill(5)
'3.14159265359'
```

### Die alte String-Formatierung

Der %-Operator (modulo) kann auch für die Formatierung von Zeichenketten verwendet werden. In `format % values` (wobei `format` ein String ist) werden Instanzen von `%` in `format` durch null oder mehr Elemente von `values` ersetzt. Diese Operation ist allgemein als String-Interpolation bekannt. Zum Beispiel:

```pycon
   >>> import math
   >>> print('The value of pi is approximately %5.3f.' % math.pi)
   The value of pi is approximately 3.142.
```

Mehr Informationen dazu sind in der Python-Dokumentation [printf-style string formatting](https://docs.python.org/3/library/stdtypes.html#old-string-formatting) zu finden.


## Dateien Lesen und Schreiben

[open](https://docs.python.org/3/library/functions.html#open) gibt ein [Dateiobjekt](https://docs.python.org/3/glossary.html#term-file-object) (auf Englisch: "file object") zurück. Es wird am häufigsten mit zwei Positionsargumenten und einem Schlüsselwortargument verwendet: `open(Dateiname, mode, encoding=none)`

```pycon
>>> f = open('workfile', 'w', encoding="utf-8")
.. XXX str(f) is <io.TextIOWrapper object at 0x82e8dc4>
>>> print(f)
<open file 'workfile', mode 'w' at 80a0960>
```

Das erste Argument ist ein String, der den Dateinamen enthält. Das zweite Argument ist eine weitere Zeichenkette, die ein oder mehrere Zeichen enthält, die die Art und Weise beschreibt, wie die Datei verwendet werden soll bzw. kann. *mode* ist `'r'`, wenn die Datei nur gelesen werden soll. `'r'` steht für "read", auf Deutsch: lesen. `'w'` wird verwendet, wenn in die Datei geschrieben werden soll. `'w'` steht für "write", auf Deutsch "schreiben". Eine existierende Datei mit demselben Namen wird dabei gelöscht. `'a'` öffnet die Datei zum Anhängen, alle Daten, die in die Datei geschrieben werden, werden automatisch am Ende angefügt. `'a'` steht für "append", auf Deutsch: anhängen, anfügen. `'r+'` öffnet die Datei sowohl zum Lesen als auch zum Schreiben. Das Argument *mode* ist optional. Wird es weggelassen, wird automatsisch `'r'`, also nur Lesen, angenommen.

Üblicherweise werden Dateien im Textmodus geöffnet, d.h. man liest und schreibt Zeichenketten von und in die Datei, die in einem bestimmten *encoding* (auf Deutsch: Zeichensatzkodierung) kodiert ist. Wenn *encoding* nicht angegeben wird, ist der Standard plattformabhängig (siehe `open`). Da UTF-8 der moderne de-facto Standard ist, wird `encoding="utf-8"` empfohlen, es sei denn, mann weiß, dass man eine andere Kodierung verwenden muss oder möchte. Beim Lesen muss man das Encoding der Datei korrekt angeben, sonst erhält man eventuell beim Lesen falsche und sonderbare Zeichen. Wenn man eine andere Kodierung als die der Datei angibt, wie z.B. cp-1252 für eine utf-8 kodierte Datei, nimmt Python keine automatische Umwandlung der Kodierung beim Lesen vor.

Das Anhängen eines `'b'` an den Modus öffnet die Datei im Binärmodus. Daten im Binärmodus werden als [bytes Objekte](https://docs.python.org/3/library/stdtypes.html#bytes) gelesen und geschrieben. Sie können keine *Kodierung* angeben, wenn eine Datei im Binärmodus geöffnet wird.

Im Textmodus werden beim Lesen plattformspezifische Zeilenenden (`\n` unter Unix, `\r\n` unter Windows) standardmäßig in einfaches `\n` umgewandelt. Beim Schreiben im Textmodus wird das Vorkommen von `\n` standardmäßig wieder in plattformspezifische Zeilenenden umgewandelt. Diese Modifikation der Dateidaten im Hintergrund ist für Textdateien in Ordnung, beschädigt aber binäre Daten wie JPEG oder EXE Dateien. Von daher muss man vorsichtig sein, wenn man solche Dateien im Binärmodus lesen und schreiben will.

Es ist eine empfohlene und gängige Praxis, das Schlüsselwort [with](https://docs.python.org/3/reference/compound_stmts.html#with) zu verwenden, wenn man mit Dateiobjekten arbeitet. Der Vorteil ist, dass die Datei nach Beendigung des Lese- / Schreibvorgangs wieder ordnungsgemäß geschlossen wird, auch wenn irgendwann eine Ausnahme ausgelöst wird. Die Verwendung von `with` ist auch viel kürzer als das Schreiben entsprechender `try`  \ `finally` Blöcke (mehr zu `try` im Kapitel zur Fehlerbehandlung). Beispiel für die Verwendung von `with`:

```pycon
>>> with open('workfile', encoding="utf-8") as f:
...     read_data = f.read()
...
>>> # We can check that the file has been automatically closed.
>>> f.closed
True
```

Wenn man das Schlüsselwort `with` nicht verwendet, dann sollte man unbedingt `f.close()` aufrufen, um die Datei zu schließen und sofort alle von ihr verwendeten Systemressourcen freizugeben. Ruft man `close()` nicht auf besteht außerdem das Risiko, dass bei Schreibvorgängen nicht alle Daten in die Datei geschrieben werden.

Nachdem ein Dateiobjekt geschlossen wurde, entweder durch eine `with` Anweisung oder durch den Aufruf von `f.close()`, schlagen Versuche das Dateiobjekt zu benutzen fehl:

```pycon
>>> f.close()
>>> f.read()
Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
ValueError: I/O operation on closed file.
```

### Methoden von Dateiobjekten

Die restlichen Beispiele in diesem Abschnitt gehen davon aus, dass ein Dateiobjekt namens `f` bereits erstellt wurde.

Um den Inhalt einer Datei zu lesen, ruft man `f.read(size)` auf, was eine bestimmte Menge an Daten liest und sie als String (im Textmodus) oder Byte-Objekt (im Binärmodus) zurückgibt. *size* ist ein optionales numerisches Argument. Wenn *size* weggelassen wird oder negativ ist, wird der gesamte Inhalt der Datei gelesen und zurückgegeben. Es ist dann das Problem des Nutzers, wenn die Datei z.B. doppelt so groß ist wie der Speicher des Rechners ist. Ansonsten werden höchstens *size* Zeichen (im Textmodus) oder *size* Bytes (im Binärmodus) gelesen und zurückgegeben. Wenn das Ende der Datei erreicht ist, gibt `.read()` eine leere Zeichenkette (`''`) zurück:

```pycon
>>> f.read()
'This is the entire file.\n'
>>> f.read()
''
```

`f.readline()` liest eine einzelne Zeile aus der Datei. Ein Zeilenumbruch `\n` wird am Ende der Zeichenkette stehen gelassen und nur in der letzten Zeile der Datei ausgelassen, wenn die Datei nicht mit einem Zeilenumbruch endet. Dies macht den Rückgabewert eindeutig. Wenn `f.readline()` eine leere Zeichenkette zurückgibt, wurde das Ende der Datei erreicht, während eine leere Zeile durch `'\n'` dargestellt wird:

```pycon
>>> f.readline()
'This is the first line of the file.\n'
>>> f.readline()
'Second line of the file\n'
>>> f.readline()
''
```

Um Zeilen aus einer Datei zu lesen, kann man über das Dateiobjekt iterieren. Dies ist speichereffizient, schnell und führt zu gut lesbarem Code:

```pycon
>>> for line in f:
...     print(line, end='')
...
This is the first line of the file.
Second line of the file
```

Wenn man alle Zeilen einer Datei in eine Liste einlesen will, nutzt man `list(f)` oder `f.readlines()`.

`f.write(string)` schreibt den Inhalt von string in die Datei und gibt die Anzahl der geschriebenen Zeichen zurück:

```pycon
>>> f.write('This is a test\n')
15
```

Andere Objekttypen müssen vor dem Schreiben entweder in eine Zeichenkette (im Textmodus) oder in ein Byte-Objekt (im Binärmodus) umgewandelt werden:

```pycon
>>> value = ('the answer', 42)
>>> s = str(value)  # konvertiert das Tupel zu einem String
>>> f.write(s)
18
```

`f.tell()` gibt eine ganze Zahl zurück, die die aktuelle Position des Zeigers des Dateiobjekts in der Datei angibt. Im Binärmodus ist das die Anzahl von Bytes, gezählt vom Anfang der Datei. Im Textmodus ist es die Anzahl der Zeichen - was aber *nicht* der Anzahl der Buchstaben entsprechen muss, da manche Kodierungen wie z.B. UTF-8 je nach darzustellendem Buchstaben mehr als ein Byte verwenden. Man kann mit `tell()` als nicht gezielt zum X-ten Buchstaben in der Datei springen, wenn man deren Inhalt inkl. Kodierung und Zeichendarstellung nicht 100% exakt kennt.

Um die Position des Dateiobjekts zu ändern, verwendet man `f.seek(offset, whence)`. Die Position wird durch Addition von *offset* zu einem Referenzpunkt berechnet. Der Referenzpunkt wird durch das Argument *whence* ausgewählt. Ein *whence* Wert von 0 geht vom Anfang der Datei aus, 1 verwendet die aktuelle Dateiposition und 2 verwendet das Ende der Datei als Referenzpunkt. *whence* kann weggelassen werden und hat den Standardwert 0, wobei der Anfang der Datei als Referenzpunkt verwendet wird:

```pycon
>>> f = open('workfile', 'rb+')
>>> f.write(b'0123456789abcdef')
16
>>> f.seek(5)      # geht zum 6. Byte
5
>>> f.read(1)
b'5'
>>> f.seek(-3, 2)  # gehe zum 2. Byte vor dem Dateiende
13
>>> f.read(1)
b'd'
```

In Textdateien (also nicht im Binärmodus geöffnete Dateien) sind nur Suchvorgänge relativ zum Anfang der Datei erlaubt. Die Ausnahme ist das Suchen zum Ende der Datei mit `seek(0, 2)`. Die einzigen gültigen Offset-Werte sind die, die von `f.tell()` zurückgegeben werden, oder Null. Jeder andere Offset-Wert führt zu undefiniertem Verhalten.

Datei-Objekte haben einige zusätzliche Methoden, wie [io.IOBase.isatty](https://docs.python.org/3/library/io.html#io.IOBase.isatty) und [io.IOBase.truncate](https://docs.python.org/3/library/io.html#io.IOBase.truncate), was man aber sehr selten wirklich benötigt. Weitere Informationen sind in der verlinkten Dokumentation zu finden.

### JSON zum Speichern von strukturierten Daten

Zeichenketten lassen sich leicht in eine Datei schreiben und aus ihr lesen. Zahlen erfordern etwas mehr Aufwand, da die Methode `io.TextIOBase.read` nur Zeichenketten zurückgibt, die an eine Funktion wie `int` übergeben werden müssen, die eine Zeichenkette wie `'123'` nimmt und ihren numerischen Wert `123` zurückgibt. Wenn man komplexere Datentypen wie verschachtelte Listen und Dictionaries speichern will, wird das Parsen und Serialisieren in eine Textdatei von Hand kompliziert.

Anstatt dass die Benutzer ständig Code schreiben und debuggen müssen, um komplizierte Datentypen in Dateien zu speichern, ermöglicht Python die Verwendung des beliebten und sehr gängigen Datenaustauschformats namens JSON (= "JavaScript Object Notation"). Details dazu [https://json.org](https://json.org). Das Standardmodul [json](https://docs.python.org/3/library/json.html#module-json) kann Python-Datenhierarchien in String-Darstellungen umwandeln. Dieser Prozess wird "serializing" (auf Deutsch: Serialisierung) genannt. Die Rekonstruktion der Daten aus der String-Repräsentation wird "deserializing" (auf Deutsch: Deserialisierung) genannt. Zwischen der Serialisierung und der Deserialisierung kann die Zeichenkette, die das Objekt repräsentiert, in einer Datei oder in Daten gespeichert oder über eine Netzwerkverbindung an einen entfernten Rechner gesendet werden.

JSON stammt zwar, wie im Namen bereits enthalten, aus dem JavaScript Umfeld, ist aber in der Welt der Programmierung sehr verbreitet und gängig. Quasi alle modernen Programmiersprachen haben Module zum Lesen und Schreiben von JSON, weswegen es sich sehr gut für den Datenaustausch eignet. Im Python-Kontext ist außerdem praktisch, dass die beiden Hauptdatenstrukturen von JSON quasi 1:1 auf Python übertragbar sind. Ein JSON-Objekt entsprich in Python einem Wörterbuch und ein JSON-Array einer Python Liste.

Wenn man ein Objekt ``x`` hat, kann man seine JSON-String-Repräsentation mit einer einfachen Codezeile anzeigen:

```pycon
>>> import json
>>> x = [1, 'simple', 'list']
>>> json.dumps(x)
'[1, "simple", "list"]'
```

Eine andere Variante der Funktion `json.dumps`, genannt `json.dump`, serialisiert das Objekt einfach in eine Textdatei. Wenn also `f` ein Textdatei-Objekt ist, das zum Schreiben geöffnet wurde, können man folgendes tun:

```python
json.dump(x, f)
```

Um das Objekt wieder zu dekodieren, wenn `f` eine Binärdatei oder Textdatei-Objekt ist, das zum Lesen geöffnet wurde:

```python
x = json.load(f)
```

JSON-Dateien sind standardmäßig Textdateien die mit UTF-8 kodiert sein müssen. Also muss man `encoding="utf-8"` beim Öffnen nutzen.

Diese einfache Serialisierungstechnik kann mit Listen und Wörterbüchern umgehen, aber die Serialisierung beliebiger Klasseninstanzen in JSON erfordert ein wenig zusätzlichen Aufwand. Die Referenz für das Modul `json` enthält eine Erklärung dazu.

### Weitere Methoden

Im Gegensatz zu JSON ist [pickle](https://docs.python.org/3/library/pickle.html) ein Protokoll, das die Serialisierung beliebig komplexer Python-Objekte ermöglicht. Als solches ist es spezifisch für Python und kann nicht für die Kommunikation mit Anwendungen verwendet werden, die in anderen Sprachen geschrieben sind. Außerdem ist es standardmäßig unsicher. Die Deserialisierung von Pickle-Daten, die aus einer nicht vertrauenswürdigen Quelle stammen, kann beliebiger Code ausgeführt werden, wenn die Daten von einem geschickten Angreifer manipuliert wurden.

CSV-Dateien erlauben es, Daten strukturiert abzulegen mit einer Struktur ähnlich einer Tabellenkalkulation. Allerdings sind CSV-Dateien auch nur normale Textdateien, d.h. in der Datei ist alles ein String. CSV kennt keine Datentypen wie Zahlen etc. Je nach zu speichernden Daten kann die Nutzung von CSV aber sinnvoll sein. Python hat ein Standardmodul zum Lesen und Schreiben von CSV an Bord. Details stehen [in der Python-Dokumentation](https://docs.python.org/3/library/csv.html).

***

 * Nächstes Kapitel: [Fehler und Ausnahmen (Exceptions)](errors.md)
 * Vorheriges Kapitel: [Module](modules.md)
 * Zurück zur [Startseite](index.md)
