# Datenstrukturen und Datentypen

In diesem Kapitel werden einige Dinge, die bereits in vorherigen Kapiteln beschrieben wurden, ausführlicher erklärt. Und es kommen auch einige neue Datenstrukturen und ergänzende Informationen hinzu.

## mehr zu Listen

Der Datentyp Liste hat einige weitere Methoden. Im Folgenden alle Methoden von Listenobjekten:

*Hinweis*: In den folgenden Beispielen - und vielen anderen Beispielen in der Python-Dokumentation - kennzeichnen eckige Klammern innerhalb eines Methodenaufrufs ein optionales Argument. So bedeutet z.B. `a.pop([i])`, dass die Methode `pop` von `a` entweder mit einem Argument wie `a.pop(4)` oder ohne `a.pop()` aufgerufen werden kann.

 * `list.append(x)`: fügt ein Element `x` am Ende der Liste hinzu. Äquivalent zu `a[len(a):] = x`.
 * `list.extend(iterable)`: Erweitert die Liste um alle Elemente aus der Sequenz `iterable`. Äquivalent zu `a[len(a):] = iterable`.
 * `list.insert(i, x)`: Fügt ein Element `x` vor dem Element an Position `i` der Liste hinzu. `list.insert(0, x)` würde also das Elemente `x` an den Anfang der Liste einsetzen. `list.insert(len(list), x)` ist äquivalent zu `list.append(x)`.
 * `list.remove(x)`: Entfernt das erste Element aus der Liste, dessen Wert gleich `x` ist. Wird kein Element gefunden, erhält man einen `ValueError`.
 * `list.pop([i])`: Entfernt das Element an der Position `i` aus der Liste und liefert es als Rückgabewert zurück. Wenn nur `list.pop()` ohne Index angegeben wird, wird das letzte Element aus der Liste entfernt.
 * `list.clear()`: Entfernt alle Elemente aus der Liste, sodass diese leer ist. Äquivalent zu `del list[:]`.
 * `list.index(x[, start[, end]])`: Gibt den Index des ersten Elements zurück, dessen Wert gleich `x` ist. Wird kein Element gefunden erhält man einen `ValueError`. Gibt man die optionalen Elemente `start` und `end` an wird nur in dem Abschnitt zwischen den Indizes `start` und `end` gesucht. Wird ein Element in dem Abschnitt gefunden wird dessen Index relativ zur gesamten Liste angegeben, nicht relativ zum Index des Starts.
 * `list.count(x)`: Liefert die Anzahl der Vorkommen von `x` in der Liste zurück.
 * `list.sort(*, key=None, reverse=False)`: Sortiert die Liste in-situ, d.h. es wird keine neue Liste zurückgeliefert, sondern die Liste an sich wieder sortiert. Die möglichen Argumente und Optionen sind identisch mit der Built-In Funktion [sorted](https://docs.python.org/3/library/functions.html#sorted).
 * `list.sort(*, key=None, reverse=False)`: Sortiert die Liste in-situ, d.h. es wird keine neue Liste zurückgeliefert, sondern die Liste an sich wieder sortiert. Die möglichen Argumente und Optionen sind identisch mit der der Built-In Funktion [sorted](https://docs.python.org/3/library/functions.html#sorted).
 * `list.reverse()`: Kehrt die Reihenfolge der Elemente in der Liste um. Die Liste wird direkt umgekehrt, es wird keine neue Liste zurückgegeben.
 * `list.copy()`: Liefert eine flache Kopie der Liste zurück. Äquivalent zu `list[:]`.

Ein Beispiel, das die meisten der Listenmethoden verwendet:

```pycon
>>> fruits = ['orange', 'apple', 'pear', 'banana', 'kiwi', 'apple', 'banana']
>>> fruits.count('apple')
2
>>> fruits.count('tangerine')
0
>>> fruits.index('banana')
3
>>> fruits.index('banana', 4)  # Find next banana starting at position 4
6
>>> fruits.reverse()
>>> fruits
['banana', 'apple', 'kiwi', 'banana', 'pear', 'apple', 'orange']
>>> fruits.append('grape')
>>> fruits
['banana', 'apple', 'kiwi', 'banana', 'pear', 'apple', 'orange', 'grape']
>>> fruits.sort()
>>> fruits
['apple', 'apple', 'banana', 'banana', 'grape', 'kiwi', 'orange', 'pear']
>>> fruits.pop()
'pear'
```

Wie oben bereits erwähnt sind Methoden wie `insert`, `remove` oder `sort` solche, die die Liste verändern und keinen Rückgabewert ausgeben - sie liefern den Standardwert `None`. Dies ist grundsätzliches Designprinzip für alle veränderbaren Datenstrukturen in Python.

Eine andere erwähenenswerte Sache ist, dass nicht alle Daten sortiert oder verglichen werden können. Zum Beispiel kann `[None, 'hello', 10]` nicht sortiert werden, weil Ganzzahlen nicht mit Zeichenketten und `None` nicht mit anderen Typen verglichen werden können. Außerdem gibt es einige Typen, die keine definierte Ordnungsbeziehung haben. Zum Beispiel komplexe Zahlen. So ist `3+4j < 5+7j` kein gültiger Vergleich.

### Listen als Stacks

Die Listenmethoden machen es sehr einfach, eine Liste als Stack (auf Deutsch: [Stapelspeicher](https://de.wikipedia.org/wiki/Stapelspeicher) oder kurz: Stapel) zu verwenden, wobei das zuletzt hinzugefügte Element das erste Element ist, welches abgerufen wird ("last-in, first-out", kurz: LiFo). Um ein Element auf dem Stack abzulegen wird `list.append` verwendet. Um ein Element vom Stack zu nehmen wird `list.pop` ohne expliziten Index verwendet. Zum Beispiel:

```pycon
>>> stack = [3, 4, 5]
>>> stack.append(6)
>>> stack.append(7)
>>> stack
[3, 4, 5, 6, 7]
>>> stack.pop()
7
>>> stack
[3, 4, 5, 6]
>>> stack.pop()
6
>>> stack.pop()
5
>>> stack
[3, 4]
```

### Listen als Queues

Es ist auch möglich, eine Liste als Queue (auf Deutsch: [Warteschlange](https://de.wikipedia.org/wiki/Warteschlange_(Datenstruktur)) zu verwenden, bei der das erste hinzugefügte Element auch das erste abgefragte Element ist ("first-in, first-out", kurz: FiFo). Allerdings sind Listen für diesen Zweck nicht effizient. Während `append` und `pop` am Ende einer Liste schnell sind, sind `insert` oder `pop` am Anfang einer Liste vergleichsweisen langsam. Dies ist durch die interne Art und Weise bedingt, wie Python Listen intern speichert. Bei einem `insert` oder `pop` am Anfang muss die komplette Liste intern neu geschrieben werden.

Um eine Queue zu implementieren, verwendet man besser [collections.deque](https://docs.python.org/3/library/collections.html#collections.deque), die für schnelles `append` und `pop` von beiden Enden aus konzipiert wurde. "deque" (gesprochen wie das deutsche Wort "Deck") ist die Kurzform für "double-ended queue". Beispiel:

```pycon
>>> from collections import deque
>>> queue = deque(["Eric", "John", "Michael"])
>>> queue.append("Terry")           # Terry arrives
>>> queue.append("Graham")          # Graham arrives
>>> queue.popleft()                 # The first to arrive now leaves
'Eric'
>>> queue.popleft()                 # The second to arrive now leaves
'John'
>>> queue                           # Remaining queue in order of arrival
deque(['Michael', 'Terry', 'Graham'])
```

### List Comprehensions

List Comprehensions bieten eine kompakte Möglichkeit Listen zu erstellen. Übliche Anwendungen sind die Erstellung neuer Listen bei denen jedes Element das Ergebnis einer oder einiger Operation(en) ist, die auf jedes Element einer anderen Sequenz oder Iterables angewandt werden. Eine weitere Anwendung ist die Erstellung einer Teilfolge von Elementen, die eine bestimmte Bedingung erfüllen.

Wenn z.B. eine Liste mit den Quadratzahlen von 1 bis 9 angelegt werden soll, geht das konventionell so:

```pycon
>>> squares = []
>>> for i in range(1, 10):
...     squares.append(i**2)
...
>>> squares
[1, 4, 9, 16, 25, 36, 49, 64, 81]
```

Zu beachten ist, dass dies eine Variable mit dem Namen `i` erzeugt (oder überschreibt), die auch nach Beendigung der Schleife noch existiert. Man kann die Liste der Quadrate ohne jegliche Nebeneffekte berechnen, und zwar wie folgt:

```pycon
>>> squares = list(map(lambda i: i**2, range(1, 10)))
```

Mittels List Comprehension funktioniert das wie folgt:

```pycom
>>> squares = [x**2 for x in range(1, 10)]
```

List Comprehensions sind im Vergleich zu den beiden zuvor genannten Wegen kompakter und besser lesbar.

Eine List Comprehension besteht aus Klammern die einen Ausdruck enthalten, gefolgt von einer `for` Klausel, dann null oder mehr `for` oder `if` Klauseln. Das Ergebnis ist eine neue Liste, die sich aus der Auswertung des Ausdrucks im Kontext der nachfolgenden `for` und `if` Klauseln ergibt. Zum Beispiel kombiniert dieser List Comprehension die Elemente zweier Listen, wenn sie nicht gleich sind:

```pycon
>>> [(x, y) for x in [1,2,3] for y in [3,1,4] if x != y]
[(1, 3), (1, 4), (2, 3), (2, 1), (2, 4), (3, 1), (3, 4)]
```
Konventionell geschrieben sähe dies so aus:

```pycon
>>> combs = []
>>> for x in [1,2,3]:
...     for y in [3,1,4]:
...         if x != y:
...             combs.append((x, y))
...
>>> combs
[(1, 3), (1, 4), (2, 3), (2, 1), (2, 4), (3, 1), (3, 4)]
```

Zu beachten ist, dass die Reihenfolge der `for` und `if` Anweisungen in diesen beiden Ausschnitten gleich ist.

Wenn der Ausdruck ein Tupel ist (z.B. `(x, y)` wie im vorherigen Beispiel), muss er in Klammern gesetzt werden:

```pycon
>>> vec = [-4, -2, 0, 2, 4]
>>> # neue Liste generieren, in der alle Elemente von vec verdoppelt sind
>>> [x*2 for x in vec]
[-8, -4, 0, 4, 8]
>>> # filtern der Liste zum Ausschluss negativer Zahlen
>>> [x for x in vec if x >= 0]
[0, 2, 4]
>>> # eine Funktion, hier die Built-In Funktion abs() aus alle Elemente anwenden
>>> [abs(x) for x in vec]
[4, 2, 0, 2, 4]
>>> # für jedes Element die strip() Methode von Strings anwenden
>>> freshfruit = ['  banana', '  loganberry ', 'passion fruit  ']
>>> [weapon.strip() for weapon in freshfruit]
['banana', 'loganberry', 'passion fruit']
>>> # eine Liste aus 2er-Tupel der Struktur (number, square) generieren
>>> [(x, x**2) for x in range(6)]
[(0, 0), (1, 1), (2, 4), (3, 9), (4, 16), (5, 25)]
>>> # das Tupel mit in Klammern stehen, sonst gibt es einen Fehler
>>> [x, x**2 for x in range(6)]
File "<stdin>", line 1
    [x, x**2 for x in range(6)]
     ^^^^^^^
SyntaxError: did you forget parentheses around the comprehension target?
>>> # eine flache Liste mit List Comprehension erzeugen unter Verwendung von zwei 'for'
>>> vec = [[1,2,3], [4,5,6], [7,8,9]]
>>> [num for elem in vec for num in elem]
[1, 2, 3, 4, 5, 6, 7, 8, 9]
```

List Comprehensions können komplexe Ausdrücke und verschachtelte Funktionen enthalten:

```pycon
>>> from math import pi
>>> [str(round(pi, i)) for i in range(1, 6)]
['3.1', '3.14', '3.142', '3.1416', '3.14159']
```

List Comprehensions gelten im Allgemeinen als "pythonisch" und werden entsprechend häufig verwendet bzw. es gilt allgemein als empfehlenswert, List Comprehensions an passenden Stellen im Code zu verwenden.

### verschachtelte List Comprehensions

Der Anfangsausdruck einer List Comprehension kann ein beliebiger Ausdruck sein, einschließlich einer anderen List Comprehension. Im folgenden Beispiel wird eine 3x4-Matrix, die als eine Liste von 3 Listen der Länge 4 implementiert ist, generiert:

```pycon
>>> matrix = [
...     [1, 2, 3, 4],
...     [5, 6, 7, 8],
...     [9, 10, 11, 12],
... ]
```

Die folgende List Comprehension transformiert die Liste, d.h. es werden Spalten und Reihen vertauscht:

```pycon
>>> [[row[i] for row in matrix] for i in range(4)]
[[1, 5, 9], [2, 6, 10], [3, 7, 11], [4, 8, 12]]
```

Wie im vorigen Abschnitt gezeigt wird die innere List Comprehension im Kontext des nachfolgenden `for` ausgewertet. Dieses Beispiel ist äquivalent zu:

```pycom
>>> transposed = []
>>> for i in range(4):
...     transposed.append([row[i] for row in matrix])
...
>>> transposed
[[1, 5, 9], [2, 6, 10], [3, 7, 11], [4, 8, 12]]
```
Was wiederum das gleich ist wie:

```pycon
>>> transposed = []
>>> for i in range(4):
...     # the following 3 lines implement the nested listcomp
...     transposed_row = []
...     for row in matrix:
...         transposed_row.append(row[i])
...     transposed.append(transposed_row)
...
>>> transposed
[[1, 5, 9], [2, 6, 10], [3, 7, 11], [4, 8, 12]]
```

In der realen Programmierwelt sollte man integrierte Funktionen komplexen Ablaufanweisungen vorziehen. Die Funktion [zip](https://docs.python.org/3/library/functions.html#zip) würde sich für diesen Anwendungsfall sehr gut eignen:

```pycon
>>> list(zip(*matrix))
[(1, 5, 9), (2, 6, 10), (3, 7, 11), (4, 8, 12)]
```

## die del Anweisung

Es gibt eine Möglichkeit, ein Element aus einer Liste zu entfernen. Dazu dient die [del](https://docs.python.org/3/tutorial/datastructures.html) Anweisung in Kombination mit der Angabe des Indexes des Elements, das man entfernen möchte. `del` steht für `delete`, auf Deutsch: entfernen, löschen. Dies unterscheidet sich von der Methode `list.pop`, die einen Wert zurückgibt. Die Anweisung `del` kann auch verwendet werden, um Slices aus einer Liste zu entfernen oder die gesamte Liste zu löschen (was zuvor durch Zuweisung einer leeren Liste an den Slice gemacht wurde). Zum Beispiel:

```pycon
>>> a = [-1, 1, 66.25, 333, 333, 1234.5]
>>> del a[0]
>>> a
[1, 66.25, 333, 333, 1234.5]
>>> del a[2:4]
>>> a
[1, 66.25, 1234.5]
>>> del a[:]
>>> a
[]
```

`del` kann auch dazu verwendet werden, um Variablen zu löschen:

```pycon
>>> del a
```

Den Namen `a` im Folgenden zu referenzieren liefert einen Fehler - zumindest bis ihm ein anderer Wert zugewiesen wird. Später werden noch andere Anwendungen von `del` gezeigt.

***Hinweis***: `del` löscht die Referenz auf ein Objekt aber *nicht* das Objekt selber! `del` kann keine Objekte löschen. Dies ist ein subtiler Unterschied, der zwar im Rahmen diese Tutorials keine Rolle spielt, aber später bei fortgeschrittener Python-Programmierung gegebenenfalls einmal relevant werden könnte. Hat man so etwas wie

```pycon
>>> a = 'Python'
>>> del a
```

würde `del a` die Referenz von `a` auf den String "Python" aus dem aktuellen Namensraum entfernen, das String-Objekt "Python" ist aber noch im Speicher des Rechners. Wenn es sonst keine weiteren Referenzen auf `a` im aktuellen Namensraum gibt, würde der nächste Durchlauf des [Garbage Collectors](https://docs.python.org/3/library/gc.html) von Python das String-Objekt löschen.

## Tupel und Sequenzen

Es wurde schon gezeigt, dass Listen und Zeichenketten viele gemeinsame Eigenschaften haben, z. B. Index- und Slicing-Operationen. Dies sind zwei Beispiele für *Sequenz*-Datentypen (siehe [Sequenz Typen — list, tuple, range](https://docs.python.org/3/library/stdtypes.html#typesseq)). Da Python eine sich entwickelnde Sprache ist, könnten weitere Sequenz-Datentypen hinzugefügt werden. Es gibt auch einen anderen Standard-Sequenz-Datentyp: das *Tupel* (auf Englisch: tuple).

Ein Tupel besteht aus einer Reihe von Werten, die durch Kommas getrennt sind, zum Beispiel:


```pycon
>>> t = 12345, 54321, 'hello!'
>>> t[0]
12345
>>> t
(12345, 54321, 'hello!')
>>> # Tupels können verschachtelt sein:
... u = t, (1, 2, 3, 4, 5)
>>> u
((12345, 54321, 'hello!'), (1, 2, 3, 4, 5))
>>> # Tupels sind immutable (unveränderlich):
... t[0] = 88888
Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
TypeError: 'tuple' object does not support item assignment
>>> # but they can contain mutable objects:
... v = ([1, 2, 3], [3, 2, 1])
>>> v
([1, 2, 3], [3, 2, 1])
```

Wie zu sehen ist, werden Tupel bei der Ausgabe immer in Klammern eingeschlossen, damit verschachtelte Tupel korrekt interpretiert werden. Sie können mit oder ohne umgebende Klammern eingegeben werden, obwohl Klammern oft ohnehin notwendig sind, z.B. wenn das Tupel Teil eines größeren Ausdrucks ist. Es ist nicht möglich, den einzelnen Elementen eines Tupels Zuweisungen zuzuweisen. Es ist jedoch möglich, Tupel zu erstellen, die veränderbare Objekte enthalten, z. B. Listen.

Obwohl Tupel auf den ersten Blick ähnlich wie Listen aussehen, werden sie oft in unterschiedlichen Situationen und für unterschiedliche Zwecke verwendet. Tupel sind "immutable" (=unveränderlich) und enthalten in der Regel eine heterogene Folge von Elementen, auf die durch Entpacken (siehe später in diesem Abschnitt) oder Indizierung (oder sogar durch Attribute im Fall von [namedtuples](https://docs.python.org/3/library/collections.html#collections.namedtuple)) zugegriffen wird. Listen sind "mutable" (=veränderlich), und ihre Elemente sind normalerweise homogen und werden durch Iteration über die Liste angesprochen.

Ein spezielles Problem ist die Konstruktion von Tupeln, die 0 oder 1 Elemente enthalten: Die Syntax hat einige zusätzliche Eigenheiten, um diese zu berücksichtigen. Leere Tupel werden durch ein leeres Klammerpaar konstruiert. Ein Tupel mit einem Element wird konstruiert, indem einem Wert ein Komma folgt - es genügt nicht, einen einzelnen Wert in Klammern zu setzen! Hässlich, aber effektiv. Zum Beispiel:

```pycon
>>> empty = ()
>>> singleton = 'hello',    # <-- note trailing comma
len(empty)
0
>>> len(singleton)
1
>>> singleton
('hello',)
```

Die Anweisung `t = 12345, 54321, 'hallo!'` ist ein Beispiel für *tuple packing*: die Werte `12345`, `54321` und `'hallo!'` werden zu einem Tupel verpackt. Die umgekehrte Operation ist ebenfalls möglich:

```pycom
>>> x, y, z = t
```

Dies wird passenderweise *sequnce unpacking* (auf Deutsch: Entpacken einer Sequenz) genannt und funktioniert für jede beliebige Sequenz auf der rechten Seite. Das Entpacken von Sequenzen setzt voraus, dass sich auf der linken Seite des Gleichheitszeichens so viele Variablen befinden, wie es Elemente in der Sequenz gibt. Zu beachten ist, dass die Mehrfachzuweisung eigentlich nur eine Kombination aus Tupel-Packing und Sequence-Unpacking ist.

## Sets

Python enthält auch einen Datentyp für *Sets* (auf Deutsch: Menge). Ein Set ist eine ungeordnete Sammlung, die keine doppelten Elemente enthält. Zu den grundlegenden Verwendungszwecken gehören Zugehörigkeitstests und das Eliminieren doppelter Einträge. Sets unterstützen auch mathematische Operationen wie Vereinigung, Schnittmenge, Differenz und symmetrische Differenz.

Geschweifte Klammern oder die Funktion [set](https://docs.python.org/3/library/stdtypes.html#set) können verwendet werden, um Mengen zu erzeugen.

*Hinweis*: Um eine leere Menge zu erzeugen, muss `set()` verwendet werden, nicht `{}`! Letzteres erzeugt ein leeres Wörterbuch, eine Datenstruktur, die im nächsten Abschnitt behandelt wird.

Eine kurze Demonstration zu Sets:

```pycon
>>> basket = {'apple', 'orange', 'apple', 'pear', 'orange', 'banana'}
>>> print(basket)                      # doppelt Einträge wurden automatisch entfernt
{'orange', 'banana', 'pear', 'apple'}
>>> 'orange' in basket                 # schneller Test auf Zugehörigkeit
True
>>> 'crabgrass' in basket
False
>>> # Set Operationen für einzigartige Buchstaben von zwei Worten
>>> a = set('abracadabra')
>>> b = set('alacazam')
>>> a                                  # einzigartige Buchstaben in a
{'a', 'r', 'b', 'c', 'd'}
>>> a - b                              # Buchstaben in a aber nicht in b
{'r', 'd', 'b'}
>>> a | b                              # Buchstaben in a oder b oder beiden
{'a', 'c', 'r', 'd', 'b', 'm', 'z', 'l'}
>>> a & b                              # Buchstaben in a und b
{'a', 'c'}
>>> a ^ b                              # Buchstaben in a oder b aber nicht in beiden
{'r', 'd', 'b', 'm', 'z', 'l'}
```

Analog zu List Comprehensions gibt es auch Set Comprehensions:

```pycon
>>> a = {x for x in 'abracadabra' if x not in 'abc'}
>>> a
{'r', 'd'}
```

## Dictionaries - Wörterbücher


Ein weiterer nützlicher Datentyp, der in Python eingebaut und sehr oft verwendet wird, ist das *Dictionary* (auf Deutsch: Wörterbuch). Wörterbücher sind vom Typ [mapping](https://docs.python.org/3/library/stdtypes.html#typesmapping). Wörterbücher sind gegenwärtig die einzige Datenstruktur vom Typ mapping. Für Wörterbücher ist als Bezeichnung auch der Begriff "dict" (Kurzform für Dictionary) gebräuchlich.

Wörterbücher sind in anderen Sprachen manchmal als "assoziative Speicher" oder "assoziative Arrays" bekannt. Im Gegensatz zu Sequenzen, die durch einen Zahlenbereich indiziert werden, werden Wörterbücher durch *Schlüssel* indiziert, die jeder unveränderliche Typ sein können. Strings und Zahlen können ein Schlüssel sein. Tupel können als Schlüssel verwendet werden, wenn sie nur Strings, Zahlen oder Tupel enthalten. Wenn ein Tupel direkt oder indirekt ein veränderbares Objekt enthält, kann es nicht als Schlüssel verwendet werden. Listen können nicht als Schlüssel verwendet werden, da Listen mit Indexzuweisungen, Slice-Zuweisungen oder Methoden `list.append` und `list.extend` an Ort und Stelle verändert werden können.

Am besten stellt man sich ein Wörterbuch als eine Menge von *Schlüssel-Wert*-Paaren vor, wobei die Schlüssel innerhalb eines Wörterbuchs eindeutig sein müssen. Ein Paar geschweifte Klammern erzeugt ein leeres Wörterbuch: `{}`. Eine kommagetrennte Liste von Schlüssel-Wert Paaren innerhalb der geschweiften Klammern fügt dem Wörterbuch erste Schlüssel-Wert Paare hinzu. Dies ist auch die Art und Weise, wie Wörterbücher bei der Ausgabe geschrieben werden.

Die wichtigsten Operationen mit einem Wörterbuch sind das Speichern eines Wertes mit einem Schlüssel und das Extrahieren des Wertes mithilfe des Schlüssels. Es ist auch möglich, ein Schlüssel-Wert-Paar mit `del` zu löschen. Wenn man einen Schlüssel speichert, der bereits in Gebrauch ist, wird der alte Wert, der mit diesem Schlüssel verbunden war, überschrieben. Greift man auf einen Schlüssel zu, der nicht existiert, erhält man einen `KeyError`.

Wenn man ``list(d)`` auf ein Wörterbuch anwendet, erhält man eine Liste aller Schlüssel, die in dem Wörterbuch verwendet werden - und zwar in der Reihenfolge, in der die Schlüssel hinzugefügt wurden. Wenn man die SChlüssel aplhapbetisch sortiert haben will, verwendet man stattdessen `sorted(d)`. Um zu prüfen, ob ein einzelner Schlüssel im Wörterbuch enthalten ist, verwendet man das Schlüsselwort `in`.

Im Folgenden ein kleines Beispiel für die Nutzung als Wörterbuch:

```pycon
>>> tel = {'jack': 4098, 'sape': 4139}   # Anlegen von tel mit zwei Schlüssel-Werte Paaren
>>> tel['guido'] = 4127   # Hinzufügen eines Werten Paars
>>> tel
{'jack': 4098, 'sape': 4139, 'guido': 4127}
>>> tel['jack']   # Abfrage des Schlüssels "jack", der Rückgabewert ist der zugehörige Wert
4098
>>> del tel['sape']   # Entfernen des Schlüssels "sape" und damit auch des zugehörigen Werts
>>> tel['irv'] = 4127   # Hinzufügen des SChlüssel "irv"
>>> tel
{'jack': 4098, 'guido': 4127, 'irv': 4127}
>>> list(tel)
['jack', 'guido', 'irv']    # Ausgabe aller Schlüssel als Liste
>>> sorted(tel)   # Sortieren des Dicts nach Schlüsseln
['guido', 'irv', 'jack']
>>> 'guido' in tel   # Prüfen, ob der Schlüssel "guido" enthalten ist
True
>>> 'jack' not in tel  # Prüfen, ob der Schlüssel "jack" nicht enthalten ist
False
```

Der [dict](https://docs.python.org/3/library/stdtypes.html#dict) Konstruktor erstellt Wörterbücher direkt aus Sequenzen von Schlüssel-Werte-Paaren:

```pycon
>>> dict([('sape', 4139), ('guido', 4127), ('jack', 4098)])
{'sape': 4139, 'guido': 4127, 'jack': 4098}
```

Darüber hinaus gibt es auch Dict Comprehensions, um Wörterbücher aus beliebigen Schlüssel- und Wertausdrücken zu erstellen. Das Ganze funktioniert analog zu List Comprehensions, nur das man bei Dict Comprehensions Schlüssel-Wert Paare schreibt. Dict Comprehensions werden ebenfalls in geschweifte Klammer eingeschlossen.

Das folgende Beispiel erzeigt ein Wörterbuch, wo der Schlüssel eine Ganzzahl ist und der zugehörige Wert dessen Quadrat:

```pycon
>>> {x: x**2 for x in (2, 4, 6)}
{2: 4, 4: 16, 6: 36}
```

Wenn die Schlüssel einfache Zeichenketten sind, ist es manchmal einfacher, Paare mit Schlüsselwortargumenten anzugeben:

```pycon
>>> dict(sape=4139, guido=4127, jack=4098)
{'sape': 4139, 'guido': 4127, 'jack': 4098}
```

### weitere Methoden von Wörterbüchern

Iteriert man über ein Wörterbuch erhält man die Schlüssel als Rückgabewert. Um Schlüssel und Wert zu erhalten nutzt man die `dict.items()` Methode:

```pycon
>>> tel = {'jack': 4098, 'sape': 4139, 'guido': 4127}
>>> for x in tel:
...     print(x)
...
jack
sape
guido
>>> for key, value in tel.items():
...     print(key, value)
...
jack 4098
sape 4139
guido 4127
```

Die Methode `dict.len()` gibt die Länge des Wörterbuchs zurück, also die Anzahl der Einträge. Versucht man, auf einem Schlüssel zuzugreifen, der nicht existiert, bekommt man wie oben bereits erwähnt einen `KeyError`. Wenn man stattdessen einen Vorgabewert zurückerhalten möchte nutzt man der `dict.get(Schlüssel[, Defaultwert)` Methode:

```pycon
>>> tel['peter']
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
KeyError: 'peter'
>>> tel.get('peter', 0)
0
>>> tel.get('jack', 0)
4098
```

Gibt man bei `dict.get()` keinen Defaultwert an erhält man stattdessen `None` als Rückgabewert.

Wörterbücher kennen noch eine ganze Reihe weiterer Optionen, welche [in der Dokumentation](https://docs.python.org/3/library/stdtypes.html#mapping-types-dict) nachgelesen werden können.

## mehr zur Iteration über Datenstrukturen

Das Iterieren über Schlüssel-Wert Paare eines Wörterbuchs wurde bereits im vorherigen Abschnitt gezeigt.

Beim Durchlaufen einer Schleife durch eine Sequenz können der Positionsindex und der entsprechende Wert gleichzeitig mit der Funktion `enumerate` abgefragt werden. Das funktioniert mit jeder Sequenz, über die iteriert werden kann, also auch Tupeln, Sets und Wörterbüchern:

```pycon
>>> for index, item in enumerate(['tic', 'tac', 'toe']):
...     print(index, item)
...
0 tic
1 tac
2 toe
```

Um zwei oder mehr Sequenzen gleichzeitig zu durchlaufen, können die Einträge mit der Funktion [zip](https://docs.python.org/3/library/functions.html#zip) zusammengezogen werden:

```pycon
>>> questions = ['name', 'quest', 'favorite color']
>>> answers = ['lancelot', 'the holy grail', 'blue']
>>> for question, answer in zip(questions, answers):
...     print('What is your {0}?  It is {1}.'.format(question, answer))
...
What is your name?  It is lancelot.
What is your quest?  It is the holy grail.
What is your favorite color?  It is blue.
```

Um eine Sequenz in umgekehrter Reihenfolge zu durchlaufen, ruft man die Sequenz mit der [reversed](https://docs.python.org/3/library/functions.html#reversed) Funktion auf:

```pycon
>>> for i in reversed(range(1, 10, 2)):
...     print(i)
...
9
7
5
3
1
```

Um eine Sequenz in sortierter Reihenfolge in einer Schleife zu durchlaufen, verwendet man die Funktion [sorted](https://docs.python.org/3/library/functions.html#sorted), die eine neue sortierte Liste zurückgibt, während die Sequenz an sich unverändert bleibt:

```pycon
>>> basket = ['apple', 'orange', 'apple', 'pear', 'orange', 'banana']
>>> for i in sorted(basket):
...     print(i)
...
apple
apple
banana
orange
orange
pear
```

Die Verwendung von `set` auf eine Sequenz eliminiert doppelte Elemente. Die Verwendung von `sorted` in Kombination mit `set` über einer Sequenz ist ein idiomatischer Weg, eine Schleife über eindeutige Elemente der Sequenz in sortierter Reihenfolge zu bringen:

```pycon
>>> basket = ['apple', 'orange', 'apple', 'pear', 'orange', 'banana']
>>> for f in sorted(set(basket)):
...     print(f)
...
apple
banana
orange
pear
```

Manchmal ist es verlockend, eine Liste zu ändern, während man sie in einer Schleife durchläuft. Es ist jedoch oft einfacher und vor allem sicherer, stattdessen eine neue Liste zu erstellen:

```pycon
>>> import math
>>> raw_data = [56.2, float('NaN'), 51.7, 55.3, 52.5, float('NaN'), 47.8]
>>> filtered_data = []
>>> for value in raw_data:
...     if not math.isnan(value):
...         filtered_data.append(value)
...
>>> filtered_data
[56.2, 51.7, 55.3, 52.5, 47.8]
```

## mehr über Bedingungen für if und while

Die in `while` und `if` Anweisungen verwendeten Bedingungen können beliebige Operatoren enthalten, nicht nur einfache Vergleiche.

Die Vergleichsoperatoren `in` und `not in` sind Zugehörigkeitstests, die feststellen, ob ein Wert in einem Container wie z.B. einer Sequenz oder einem Wörterbuch ist (oder nicht). Die Operatoren `is` und `is not` vergleichen, ob zwei Objekte wirklich dasselbe Objekt ist. Alle Vergleichsoperatoren haben die gleiche Priorität, die niedriger ist als die aller numerischen Operatoren.

Vergleiche können verkettet werden. Zum Beispiel testet `a < b == c`, ob `a` kleiner als `b` ist und außerdem `b` gleich `c` ist.

Vergleiche können mit den booleschen Operatoren `and` und `or` kombiniert werden, und das Ergebnis eines Vergleichs (oder jedes anderen booleschen Ausdrucks) kann mit `not` negiert werden. Diese haben eine niedrigere Priorität als Vergleichsoperatoren. Zwischen ihnen hat `not` die höchste Priorität und `or` die niedrigste, sodass `A and not B or C` äquivalent zu `(A and (not B)) or C` ist. Wie immer können Klammern verwendet werden, um die gewünschte Zusammensetzung auszudrücken.

Beispiel dazu: Im Folgenden Bespiel spielt der Wert von `C` für den Vergleich beim `if` keine Rolle, weil bereits `A and not B` wahr ist und dadurch das `... or C` nicht mehr relevant ist:

```pycon
>>> A = 'irgendwas'
>>> B = None
>>> C = None
>>> if A and not B or C:
...     print('ist wahr')
...
ist wahr
>>> C = 'auch was'
>>> if A and not B or C:
...     print('ist wahr')
...
ist wahr
```

Die booleschen Operatoren `and` und `or` sind sogenannte *Kurzschluss*-Operatoren: Ihre Argumente werden von links nach rechts ausgewertet, und die Auswertung endet, sobald das Ergebnis feststeht. Wenn zum Beispiel `A` und `C` wahr sind, aber `B` falsch ist, wertet `A and B and C` den Ausdruck `C` nicht aus. Wenn er als allgemeiner Wert und nicht als Boolescher Wert verwendet wird, ist der Rückgabewert eines Kurzschlussoperators das zuletzt ausgewertete Argument.

Es ist möglich, das Ergebnis eines Vergleichs oder eines anderen booleschen Ausdrucks einer Variablen zuzuweisen. Zum Beispiel:

```pycon
>>> string1, string2, string3 = '', 'Trondheim', 'Hammer Dance'
>>> non_null = string1 or string2 or string3
>>> non_null
'Trondheim'
```

Zu beachten ist, dass in Python, anders als in C, Zuweisungen innerhalb von Ausdrücken explizit mit dem [walrus](https://docs.python.org/3/faq/design.html#why-can-t-i-use-an-assignment-in-an-expression) Operator `:=` erfolgen müssen. Dies vermeidet eine häufige Form von Problemen, die in C-Programmen auftreten: die Eingabe von ``=`` in einem Ausdruck, obwohl ``==`` beabsichtigt war.

## Vergleiche von Sequenzen und anderen Datentypen

Sequenzobjekte können normalerweise mit anderen Objekten desselben Sequenztyps verglichen werden. Der Vergleich verwendet eine *lexikografische* Reihenfolge: zuerst werden die ersten beiden Elemente verglichen, und wenn sie sich unterscheiden, bestimmt dies das Ergebnis des Vergleichs. Wenn sie gleich sind, werden die nächsten beiden Elemente verglichen, und so weiter, bis eine der beiden Sequenzen erschöpft ist. Sind zwei zu vergleichende Elemente selbst Sequenzen desselben Typs, wird der lexikografische Vergleich rekursiv durchgeführt. Wenn alle Elemente zweier Sequenzen gleich sind, werden die Sequenzen als gleich betrachtet. Ist eine Sequenz eine anfängliche Untersequenz der anderen, so ist die kürzere Sequenz die kleinere (geringere). Die lexikografische Ordnung für Zeichenketten verwendet die Unicode-Codepunktnummer, um die einzelnen Zeichen zu ordnen. Einige Beispiele für Vergleiche zwischen Sequenzen desselben Typs:

```
   (1, 2, 3)              < (1, 2, 4)
   [1, 2, 3]              < [1, 2, 4]
   'ABC' < 'C' < 'Pascal' < 'Python'
   (1, 2, 3, 4)           < (1, 2, 4)
   (1, 2)                 < (1, 2, -1)
   (1, 2, 3)             == (1.0, 2.0, 3.0)
   (1, 2, ('aa', 'ab'))   < (1, 2, ('abc', 'a'), 4)
```

Zu beachten ist, dass der Vergleich von Objekten unterschiedlichen Typs mit `<` oder `>` zulässig ist, sofern die Objekte über geeignete Vergleichsmethoden verfügen. Zum Beispiel werden gemischte numerische Typen entsprechend ihrem numerischen Wert verglichen, sodass 0 gleich 0.0 ist, usw. Andernfalls wird der Interpreter eine `TypeError` Ausnahme auslösen, anstatt eine beliebige Reihenfolge zu liefern.

***

 * nächstes Kapitel: [Module](modules.md)
 * vorheriges Kapitel: [weitere Werkzeuge zur Steuerung des Programmflusses](controlflow.md)
