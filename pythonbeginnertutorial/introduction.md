# eine lockere Einführung in Python

In den folgenden Beispielen werden Eingabe und Ausgabe durch das Vorhandensein oder Fehlen von Prompts `>>>` und `...` unterschieden.  Um das Beispiel zu wiederholen, muss alles nach dem Prompt eingeben werden, wenn der Prompt erscheint. Zeilen, die nicht mit einem Prompt beginnen, werden vom Interpreter ausgegeben. Zu beachten ist, dass ein zweiter Prompt in einer eigenen Zeile im Beispiel bedeutet, dass eine Leerzeile eingegeben werden muss. Dies wird verwendet, um einen mehrzeiligen Befehl zu beenden.

Viele der Beispiele in diesem Tutorial, auch solche, die in die interaktiven Eingabeaufforderung eingegeben werden, enthalten Kommentare.  Kommentare in Python beginnen mit dem Rautezeichen `#` und reichen bis physischen Zeilenende.  Ein Kommentar kann am Anfang einer Zeile oder nach einem Leerzeichen oder im Code stehen, aber nicht innerhalb eines Stringliterals.  Ein Rautenzeichen innerhalb eines Zeichenkettenliterales ist einfach ein Rautenzeichen. Da Kommentare zur Verdeutlichung des Codes dienen und von Python nicht interpretiert werden, können sie bei der Eingabe von Beispielen weggelassen werden.

Einige Beispiele:

```python
# das ist ein erster Kommentar
spam = 1  # und das ein zweiter Kommentar
          # ... und das ein dritter!
text = "# Das ist kein Kommentar, weil der Text in Anführungszeichen steht"
```

## Python als Taschenrechner

Es ist an der Zeit, ein paar einfache Pythonbefehle auszuprobieren. Dazu ruft man den Python-Interpreter auf und wartet kurz, bis der Prompt `>>>` erscheint.

### Zahlen

Der Interpreter funktioniert wie ein einfacher Taschenrechner: Man kann einen Ausdruck eingeben und der Interpreter berechnet den Wert.  Die Syntax des Ausdrucks ist einfach: die Operatoren `+`, `-`, `*` und `/` können zum Rechnen verwendet werden. Klammern (`()`) können zur Gruppierung verwendet werden - also alles, wie man es von der Mathematik her gewohnt ist.

Beispiel:

```pycon
>>> 2 + 2
4
>>> 50 - 5*6
20
>>> (50 - 5*6) / 4
5.0
>>> 8 / 5  # eine Division liefert immer das Ergebnis als floating point number = Gleitkommazahl zurück
1.6
```

Die ganzen Zahlen wie z.B. `2`, `4`, `20` haben den Typ [int](https://docs.python.org/3/library/functions.html#int) (ausgeschrieben: Integer), die mit Nachkommastellen wie z.B. `5.0`, `1.6` den Typ [float](https://docs.python.org/3/library/functions.html#float) (= Gleitkommazahlen). Später werden in diesem Tutorial noch mehr Informationen über numerische Typen zu sehen sein. Die normale Division mit `/` liefert immer einen Float.  Um *floor divison* durchzuführen und ein ganzzahliges Ergebnis zu erhalten, kann man den Operator `//` benutzen. Um den Rest zu berechnen, kann man `%` benutzen:

```pycon
>>> 17 / 3  # normale Division, liefert eine Kommazahl
5.666666666666667
>>>
>>> 17 // 3  # floor division, verwirft die Nachkommastellen
5
>>> 17 % 3  # der % Operator liefert den Rest der Division
2
>>> 5 * 3 + 2  # floored Quotient * Divisor + Rest
17
```

Um Potenzen zu berechnet wird in Python der `**` Operator benutzt:

```pycon
>>> 5 ** 2  # 5 zum Quadrat
25
>>> 2 ** 7  # 2 hoch 7
128
>>> 9 ** 0.5 # Wurzel aus 9
3.0
```

Potenzieren hat in Python Vorrang vor dem Vorzeichen, d.h. möchte man eine negative Zahl Potenzieren muss man diese in Klammern setzen, sonst erhält man nicht das erwartete Ergebnis:

```pycon
>>> -3**2   # -3 zum Quadrat ergibt 9 - da Python hier aber zuerst potenziert und dann das - anwendet erhält man -9
-9
>>> (-3)**2   # liefert das erwachtete Ergebnis
9
```

Das Gleichheitszeichen (`=`) wird verwendet, um einer Variablen einen Wert zuzuweisen. Danach wird vor der nächsten interaktiven Eingabeaufforderung kein Ergebnis mehr angezeigt:

```pycon
>>> width = 20
>>> height = 5 * 9
>>> width * height
900
```

Wenn eine Variable nicht definiert ist, d.h. ihr kein Wert zugewiesen wurde, führt der Versuch, sie zu verwenden, zu einer Fehlermeldung:


```pycon
>>> n  # try to access an undefined variable
   Traceback (most recent call last):
     File "<stdin>", line 1, in <module>
   NameError: name 'n' is not defined
```

Es gibt volle Unterstützung für Gleitkommazahl. Operatoren mit gemischten Operanden konvertieren den Integer-Operanden automatisch in eine Gleitkommazahl:

```pycon
>>> 4 * 3.75 - 1
14.0
```

Im interaktiven Modus wird der zuletzt gedruckte Ausdruck der Variablen ``_`` zugewiesen.  Das bedeutet, dass es z.B. bei der Verwendung von Python als Taschenrechner etwas einfacher ist, Berechnungen fortzusetzen:

```pycon
>>> tax = 12.5 / 100
>>> price = 100.50
>>> price * tax
12.5625
>>> price + _
113.0625
>>> round(_, 2)
113.06
```

Diese Variable sollte vom Benutzer als schreibgeschützt behandelt werden.  Es sollte nie explizit ein Wert zugewiesen werden - es würden eine unabhängige lokale Variable mit demselben Namen erstellt, die die eingebaute Variable mit ihrem magischen Verhalten maskiert!

Zusätzlich zu `int` und `float` unterstützt Python auch andere Zahlentypen, wie [decimal.Decimal](https://docs.python.org/3/library/decimal.html#decimal.Decimal) (=Dezimalzahlen mit einer festen Anzahl von Nachkommastellen) und [fractions.Fraction](https://docs.python.org/3/library/fractions.html#fractions.Fraction) (zur Darstellung von Brüchen). Python hat auch eine eingebaute Unterstützung für [komplexe Zahlen](https://docs.python.org/3/library/stdtypes.html#typesnumeric) und verwendet dafür das Suffix `j` oder `J`, um den Imaginärteil anzugeben (z.B. `3+5j`).

### Text

Python kann sowohl Text, dargestellt durch den Typ [str](https://docs.python.org/3/library/stdtypes.html#str) (=Strings, auf Deutsch: Zeichenkette) als auch Zahlen verarbeiten.  Dazu gehören Zeichen "`!`", Wörter "`Kaninchen``, Namen "`Paris`", Sätze "`Ich halte dir den Rücken frei :-) Juhu!`" etc. Strings werden entweder in einfache Anführungszeichen `'...'` oder doppelte Anführungszeichen `"..."` eingeschlossen. Beide sind werden von Python gleichwertig behandelt.

```pycon
>>> 'spam eggs'  # single quotes
'spam eggs'
>>> "Ich halte dir den Rücken frei :-) Juhu!"  # double quotes
'Ich halte dir den Rücken frei :-) Juhu!'
>>> '1975'  # digits and numerals enclosed in quotes are also strings
   '1975'
```

Um Anführungszeichen auszugeben, müssen diese entweder mit dem Backslash `\` maskiert werden oder man benutzt den anderen Typ Anführungszeichen:

```pycon
>>> 'doesn\'t'  # nutzt \' zum Escapen des einfachen Anführungszeichens
"doesn't"
>>> "doesn't"  # ...oder stattdessen doppelt Anführungszeichnen verwenden
"doesn't"
>>> '"Yes," they said.'
'"Yes," they said.'
>>> "\"Yes,\" they said."
'"Yes," they said.'
>>> '"Isn\'t," they said.'
'"Isn\'t," they said.'
```

Im interaktiven Python-Interpreter können die Stringdefinition und der Ausgabestring unterschiedlich aussehen. Die [print Funktion](https://docs.python.org/3/library/functions.html#print) erzeugt eine lesbarere Ausgabe, indem sie die einschließenden Anführungszeichen weglässt und Sonderzeichen sowie escapete Zeichen ausgibt:

```pycon
>>> s = 'First line.\nSecond line.'  # \n erzeugt einen Zeilenumbruch
>>> s  # ohne print() werden Sonderzeichen mit ausgegeben
'First line.\nSecond line.'
>>> print(s)  # mit print() werden Sonderzeichen interpretiert und \n erzeugt einen Zeilenumbruch
First line.
Second line.
```

Wenn nicht gewollt ist, dass Zeichen, die mit `\` eingeleitet werden, als Sonderzeichen interpretiert werden, kann man *raw strings* verwenden. Diese erzeugt man, indem man ein `r` vor das erste Anführungszeichen setzt:

```pycon
>>> print('C:\some\name')  # \n wird als Zeilenumbruch interpretiert!
C:\some
ame
>>> print(r'C:\some\name')  # beachte das r vor dem Anführungsstrich!
C:\some\name
```

Es gibt einen subtilen Aspekt bei raw strings, der zu beachten ist: ein raw string darf nicht mit einer ungeraden Anzahl von Zeichen enden, siehe [diesen FAQ-Eintrag](https://docs.python.org/3/faq/programming.html#faq-programming-raw-string-backslash) für weitere Informationen und Umgehungsmöglichkeiten.

String-Literale können sich über mehrere Zeilen erstrecken.  Eine Möglichkeit ist die Verwendung von dreifachen Anführungszeichen: `"""..."""` oder `'''...'''`.  Zeilenenden werden automatisch in die Zeichenkette aufgenommen. Aber es ist möglich, dies zu verhindern, indem man ein `\` am Ende der Zeile hinzufügt. Das folgende Beispiel:

```python
print("""\
Usage: thingy [OPTIONS]
    -h                        Display this usage message
    -H hostname               Hostname to connect to
""")
```

erzeugt folgende Ausgabe. Der ersten Zeilenumbruch nach den öffnenden `"""` ist nicht mehr enthalten:

```
Usage: thingy [OPTIONS]
    -h                        Display this usage message
    -H hostname               Hostname to connect to
```

Strings können mit `+` zusammengefügt und mit `*` wiederholt werden:

```pycon
>>> # 3 times 'un', followed by 'ium'
>>> 3 * 'un' + 'ium'
'unununium'
```

Zwei oder mehr Stringeiterale (d.h. solche, die in Anführungszeichen eingeschlossen sind) nebeneinander werden automatisch verkettet:

```pycon
>>> 'Py' 'thon'
'Python'
```

Diese Funktion ist besonders nützlich, wenn man lange Zeichenfolgen umbrechen will:

```pycon
>>> text = ('Put several strings within parentheses '
...         'to have them joined together.')
>>> text
'Put several strings within parentheses to have them joined together.'
```

Dies funktioniert allerdings nur mit zwei Literalen, nicht mit Variablen oder Ausdrücken:

```pycon
>>> prefix = 'Py'
>>> prefix 'thon'  # can't concatenate a variable and a string literal
  File "<stdin>", line 1
    prefix 'thon'
           ^^^^^^
   SyntaxError: invalid syntax
>>> ('un' * 3) 'ium'
  File "<stdin>", line 1
   ('un' * 3) 'ium'
              ^^^^^
  SyntaxError: invalid syntax
```

Wenn man Variablen oder eine Variable und ein Literal aneinanderhängen will kann man `+` verwenden:

```pycon
>>> prefix + 'thon'
'Python'
```

*Hinweis*: Das Zusammenfügen von Strings mit `+` gilt aber als schlechter Stil und sollte normalerweise nicht verwendet werden. Zur Formatierung von Strings gibt es die später in diesem Tutorial auch noch behandelten f-Strings oder alternativ die format-Methode von Strings.

Zeichenketten können auch über einen *Index* angesprochen werden, wobei das erste Zeichen den Index 0 hat, das zweite Zeichen den Index 1 usw. Der Indexzugriff funktioniert wie folgt:

```pycon
>>> word = 'Python'
>>> word[0]  # Zeichen an Position 0
'P'
>>> word[5]  # Zeichen an Position 5
'n'
```

Der Index kann auch eine negative Zahl sein, dann beginnt die Zählung von der rechten Seite (=dem Ende des Strings):

```pycon
>>> word[-1]  # letztes Zeichen
'n'
>>> word[-2]  # zweitletztes Zeichen
'o'
>>> word[-6]
'P'
```

Da 0 dasselbe wie 0 ist beginnen negative Indizes bei -1.

Außerdem wird *Slicing* unterstützt. "Slicing" heißt auf Deutsch frei übersetzt so viel wie "in Scheiben schneiden" oder "in Stücke aufteilen". Mittels Slicing kann man einen Bereich eines Strings auswählen, wie im folgenden Beispiel gezeigt:

```pycom
>>> word[0:2]  # Zeichen von Position 0 (inklusive) bis Position 2 (exklusive)
'Py'
>>> word[2:5]  # Zeichen von Position 2 (inklusive) bis Position 5 (exklusive)
'tho'
```
Die Indizes beim Slicing haben sinnvolle Vorgabewerte. Wenn der Startindex weggelassen wird, wird dieser automatisch als `0` (Null) angekommen. Wird der Endindex angenommen, wird dieser automatisch als die Länge des Strings angenommen. Beispiel:

```pycon
>>> word[:2]   # Zeichen vom Beginn bis Position 2 (exklusive)
'Py'
>>> word[4:]   # Zeichen von Position 4 (inklusive) bis zum Ende
'on'
>>> word[-2:]  # zweitletztes Zeichen bis zum Ende (inklusive)
'on'
```

Zu beachten ist, dass der Endindex standardmäßig nicht inklusive ist! So liefert z.B. das Slicing `word[2:5]` das zweite, dritte und vierte Zeichen zurück, aber nicht mehr das fünfte - auch wenn der Endindex `5` lautet. Dadurch ist auch sichergestellt, dass `string[:i] + string[i:]` immer dem String `string` entspricht:

```pycon
>>> word[:2] + word[2:]
'Python'
>>> word[:4] + word[4:]
'Python'
```

Eine Möglichkeit, sich an die Funktionsweise von Slicing zu merken, besteht darin, sich die Indizes so vorzustellen, als würden sie zwischen Zeichen zeigen, wobei der linke Rand des ersten Zeichens mit 0 nummeriert ist. Dann hat der rechte Rand des letzten Zeichens einer Zeichenkette mit n Zeichen beispielsweise den Index n:

```
+---+---+---+---+---+---+
| P | y | t | h | o | n |
+---+---+---+---+---+---+
  0   1   2   3   4   5   6
  6  -5  -4  -3  -2  -1
```

Die erste Zahlenreihe gibt die Position der Indizes 0..6 in der Zeichenkette an; die zweite Reihe gibt die entsprechenden negativen Indizes an. Der Abschnitt von i bis j besteht aus allen Zeichen zwischen den mit i bzw. j gekennzeichneten Ränder.

Bei nicht-negativen Indizes ist die Länge eines Slice die Differenz der Indizes, wenn beide innerhalb der Grenzen liegen. Zum Beispiel ist die Länge von `word[1:3]` 2.

Der Versuch, einen zu großen Index außerhalb des Rands zu verwenden, führt zu einem Fehler:

```pycon
>>> word[42]  # the word only has 6 characters
  Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
  IndexError: string index out of range
```

Allerdings werden Slice-Indizes, die außerhalb des Bereichs liegen, bei der Verwendung für das Slicing korrekt behandelt:

```pycon
>>> word[4:42]
'on'
>>> word[42:]
''
```

Python-Strings können nicht geändert werden - sie sind "immutable", auf Deutsch: unveränderlich. Von daher führt die Zuweisung an eine indizierte Position in der Zeichenkette zu einem Fehler:

```pycon
>>> word[0] = 'J'
  Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
  TypeError: 'str' object does not support item assignment
>>> word[2:] = 'py'
  Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
  TypeError: 'str' object does not support item assignment
```

"Immutable" bedeutet aber nicht, dass der Variablen `word` nicht trotzdem ein neuer Wert zugewiesen werden kann. So würde eine neue Zuweisung wie `word = 'JavaScript` ohne Probleme funktionieren. Das unveränderlich bezieht sich darauf, dass mittels Slicing und Zuweisung nicht Teile des Strings ausgetauscht werden können. Wenn man einen anderen String benötigt, kann man diesen erzeugen:

```pycon
>>> 'J' + word[1:]
'Jython'
>>> new_word = word[:2] + 'py'
>>> print(new_word)
Pypy
>>>
```

Mittels Slicing lassen sich Strings auch ganz einfach umdrehen:

```pycon
>>> word[::-1]
'nohtyP'
>>>
```

Die in Python eingebaute Funktion [len](https://docs.python.org/3/library/functions.html#len) liefert die Länge eines Strings zurück:

```pycon
>>> s = 'supercalifragilisticexpialidocious'
>>> len(s)
34
```

Weiterführende Informationen hierzu:

 * [Dokumention zu Strings](https://docs.python.org/3/library/stdtypes.html#textseq)
 * [Methoden von Strings](https://docs.python.org/3/library/stdtypes.html#string-methods), wie in Klein- oder Großbuchstaben umwandeln, Strings durchsuchen, Teile ersetzen usw.
 * [f-Strings](https://docs.python.org/3/reference/lexical_analysis.html#f-strings) - String-Literale, die eingebettete Ausdrücke enthalten
 * [format-Methode](https://docs.python.org/3/library/string.html#formatstrings) von Strings

### Listen

Python kennt eine Reihe von *zusammengesetzten* Datentypen, die dazu dienen, andere Werte zusammenzufassen.  Der vielseitigste ist *list* (auf Deutsch: Liste), der als eine Liste von durch Komma getrennten Werten ("items") zwischen eckigen Klammern geschrieben werden kann. Listen können Elemente unterschiedlichen Typs enthalten, aber in der Regel ist es sinnvoll, dass die Elemente alle denselben Typ haben. Außerdem können Listen verschachtelt werden (dazu später weiter unten mehr):

```pycon
>>> squares = [1, 4, 9, 16, 25]
>>> squares
[1, 4, 9, 16, 25]
>>> mixed_data = ['pi', 3.14, 1, [1, 2, 3]]
>>> mixed_data
['pi', 3.14, 1, [1, 2, 3]]
```

***Hinweis***: Das, was in Python eine Liste ist, wird in einigen anderen Programmiersprache als "Array" bezeichnet. In Python ist eine Liste und ein Array *nicht* dasselbe und nicht das gleiche! Ein Array ist ein eigener Datentyp in Python, in dem alle Elemente den gleichen Typ haben müssen und der Typ beim Anlegen des Arrays festgelegt werden muss - ein fundamentaler Unterschied zu einer Python Liste! Python Arrays benötigt man eher selten, mehr Informationen dazu findet man bei Bedarf [in der Dokumentation](https://docs.python.org/3/library/array.html).

Wie Strings (und alle anderen eingebauten Datenstrukturen vom Typ [sequence](https://docs.python.org/3/glossary.html#term-sequence) können Listen per Index angesprochen werden und unterstützen Slicing:

```pycon
>>> squares[0]  # der Zugriff per Index liefer das entsprechende Element zurück
1
>>> squares[-1]
25
>>> squares[-3:]  # beim Slicing erhält man eine neue Liste
[9, 16, 25]
```

Alle Slice-Operationen geben eine neue Liste zurück, die die angeforderten Elemente enthält.  Das bedeutet, dass das folgende Slice eine [flache Kopie](https://docs.python.org/3/library/copy.html#shallow-vs-deep-copy) der Liste ist:

```pycon
>>> squares[:]
[1, 4, 9, 16, 25]
```

Listen können miteinander verbunden werden:

```pycon
>>> squares + [36, 49, 64, 81, 100]
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```

Im Gegensatz zu Strings sind Listen [mutable](https://docs.python.org/3/glossary.html#term-mutable) (auf Deutsch: veränderlich), d.h.man kann den Inhalt einer Liste ändern:

```pycon
>>> cubes = [1, 8, 27, 65, 125]  # hier ist wohl etwas falsch...
>>> 4 ** 3  #  4 hoch 3 ist 64, nicht 65!
64
>>> cubes[3] = 64  # Ersetzen des falschen Werts
>>> cubes
[1, 8, 27, 64, 125]
```

Man kann auch neue Elemente am Ende der Liste hinzufügen, indem man die Methode `list.append` verwenden (später gibt es mehr Informationen über Methoden):

```pycom
>>> cubes.append(216)  # Hinzufügen von 6 hoch 3
>>> cubes.append(7 ** 3)  # und 7 hoch 3
>>> cubes
[1, 8, 27, 64, 125, 216, 343]
```

Es ist auch möglich, Slices neue Werte zuzuweisen, und so die Größe der Liste zu verändern oder sie ganz zu löschen kann:

```pycon
>>> letters = ['a', 'b', 'c', 'd', 'e', 'f', 'g']
>>> letters
['a', 'b', 'c', 'd', 'e', 'f', 'g']
>>> # einige Werte ersetzen
>>> letters[2:5] = ['C', 'D', 'E']
>>> letters
['a', 'b', 'C', 'D', 'E', 'f', 'g']
>>> # und jetzt entfernen
>>> letters[2:5] = []
>>> letters
['a', 'b', 'f', 'g']
>>> # Liste leeren, indem alle Werte durch eine leere Liste ersetzt werden
>>> letters[:] = []
>>> letters
[]
```

Die eingebaute Funktion [len](https://docs.python.org/3/library/functions.html#len) lässt sich auch auf Listen anwenden:

```pycon
>>> letters = ['a', 'b', 'c', 'd']
>>> len(letters)
4
```

Es ist möglich, Listen zu verschachteln, also Listen zu erstellen, die andere Listen enthalten, wie zum Beispiel:

```pycon
>>> a = ['a', 'b', 'c']
>>> n = [1, 2, 3]
>>> x = [a, n]
>>> x
[['a', 'b', 'c'], [1, 2, 3]]
>>> x[0]
['a', 'b', 'c']
>>> x[0][1]
'b'
```

## Erste Schritte Richtung Programmierung

Natürlich kann man Python auch für kompliziertere Aufgaben verwenden, als zwei und zwei zusammenzuzählen.  Zum Beispiel kann man eine anfängliche Teilfolge der [Fibonacci-Folge](https://de.wikipedia.org/wiki/) wie folgt schreiben:

```pycon
>>> # Fibonacci series:
... # the sum of two elements defines the next
... a, b = 0, 1
>>> while a < 10:
...     print(a)
...     a, b = b, a+b
...
0
1
1
2
3
5
8
```

Dieses Beispiel zeigt einige neue Features:

 * Die erste Zeile enthält eine *Mehrfachzuweisung*: die Variablen `a` und `b` erhalten gleichzeitig die neuen Werte 0 und 1. In der letzten Zeile wird dies erneut verwendet, um zu zeigen, dass die Ausdrücke auf der rechten Seite alle zuerst ausgewertet werden, bevor eine der Zuweisungen stattfindet. Die Ausdrücke der rechten Seite werden von links nach rechts ausgewertet.
 * Die [while-Schleife](https://docs.python.org/3/reference/compound_stmts.html#while) wird so lange ausgeführt, wie die Bedingung (hier: `a < 10`) wahr bleibt. In Python ist, wie in C, jeder Ganzzahlwert ungleich Null wahr und Null ist falsch. Die Bedingung kann auch eine Zeichenkette oder eine Liste sein, eigentlich eine beliebige Sequenz. Alles mit einer Länge ungleich Null ist wahr, leere Sequenzen sind falsch. Der in diesem Beispiel verwendete Test ist ein einfacher Vergleich. Die Standard-Vergleichsoperatoren sind die gleichen wie in C: `<` (kleiner als), `>` (größer als), `==` (gleich), `<=` (kleiner oder gleich), `>=` (größer oder gleich) und `!=` (ungleich).
 * Der *Körper* der Schleife ist *eingerückt*: Die Einrückung ist Pythons Art, Anweisungen zu gruppieren. An der interaktiven Eingabeaufforderung muss man für jede eingerückte Zeile einen Tabulator oder ein Leerzeichen eingeben. In der Praxis wird man kompliziertere Eingaben für Python mit einem Editor oder eine IDE machen. Alle guten Texteditoren haben eine automatische Einrückungsfunktion. Wenn eine zusammengesetzte Anweisung interaktiv eingegeben wird, muss ihr eine Leerzeile folgen, um die Fertigstellung anzuzeigen (da der Parser nicht erraten kann, wann die letzte Zeile eingegeben wird). Zu beachten ist, dass jede Zeile innerhalb eines Basisblocks um die gleiche Tiefe eingerückt werden muss. Es gilt als "best practice" für Python, dass die Einrückung pro Ebene immer mit vier Leerzeichen gemacht wird.
 * Die Funktion `print` schreibt den Wert des Arguments /der Argumente, die sie erhält. Sie unterscheidet sich von der einfachen Ausgabe des gewünschten Ausdrucks (wie in den Taschenrechner-Beispielen zuvor) durch die Art und Weise, wie sie mit mehreren Argumenten, Fließkommazahlen und Zeichenketten umgeht. Zeichenketten werden ohne Anführungszeichen gedruckt, und zwischen den Elementen wird ein Leerzeichen eingefügt, so dass man die Dinge schön formatieren können, etwa so:

```pycon
>>> i = 256*256
>>> print('The value of i is', i)
The value of i is 65536
```

Das Schlüsselwortargument *end* kann verwendet werden, um den Zeilenumbruch nach der Ausgabe zu vermeiden oder die Ausgabe mit einer anderen Zeichenkette zu beenden:

```pycon
>>> a, b = 0, 1
>>> while a < 1000:
...     print(a, end=',')
...     a, b = b, a+b
...
0,1,1,2,3,5,8,13,21,34,55,89,144,233,377,610,987,
```

 * nächstes Kapitel: [weitere Werkzeuge zur Steuerung des Programmflusses](controlflow.md)
 * vorheriges Kapitel: [den Python-Interpreter nutzen](interpreter.md)
