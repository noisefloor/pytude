# Weitere Werkzeuge zur Steuerung des Programmflusses

Neben der im vorherigen Kapitel vorgestellten `while`-Anweisung gibt es Python noch einige weitere Anweisungen zur Steuerung des Programmflusses, die hier in diesem Kapitel erläutert werden.

## If-Bedingung

Die vielleicht bekannteste und oft genutzte Anweisung ist die [if](https://docs.python.org/3/reference/compound_stmts.html#if) Bedingung. Beispiel:

```pycon
>>> x = int(input("Bitte einen Integerwert eingeben: "))
Bitte einen Integerwert eingeben: 42
>>> if x < 0:
...     x = 0
...     print('Negative Zahl zu Null umgewandelt')
... elif x == 0:
...     print('Null')
... elif x == 1:
...     print('Eins')
... else:
...     print('Mehr')
...
Mehr
```

Es kann keinen, einen oder mehr `elif` Teile geben. Der `else` Teil ist optional. Das Schlüsselwort `elif` ist die Abkürzung für "else if" und ist nützlich, um eine übermäßige Einrückung zu vermeiden. Eine `if ... elif ... elif ...` Sequenz ist ein Ersatz für die `switch` oder `case` Anweisungen, die man in anderen Sprachen findet.

Der Ausdruck hinter `if` und `elif` muss immer einen Wahrheitswert ergeben, also [True oder False](https://docs.python.org/3/library/stdtypes.html#boolean-type-bool). Ist der Ausdruck wahr, wird der `if` bzw. `elif` Block betreteten und ausgeführt. Wenn nicht, wird er übersprungen und das nächste `if` oder `elif` überprüft. Ist ein Block mit `else` vorhanden wird dieser nur dann ausgeführt, wenn vorher der Wahrheitswert für den `if` und alle `elif` Ausdrücke falsch (=nicht wahr) waren.

Wenn man denselben Wert mit mehreren Konstanten vergleichen oder nach bestimmten Typen oder Attributen suchen will, kann auch die Anweisung `match` nützlich sein, welche weiter unten noch erläutert wird.

## For-Anweisung - Schleifen mit for

Die [for](https://docs.python.org/3/reference/compound_stmts.html#for) Anweisung von Python unterscheidet sich ein wenig von dem, was man vielleicht von anderen Programmiersprachen wie C oder Pascal gewohnt ist. Anstatt immer über eine arithmetische Folge von Zahlen zu iterieren (wie in Pascal) oder dem Benutzer die Möglichkeit zu geben, sowohl den Iterationsschritt als auch die Haltebedingung zu definieren (wie in C), iteriert Pythons `for` Anweisung über die Elemente einer beliebigen Sammlung (wie eine Liste oder eine Zeichenkette) in der Reihenfolge, in der sie in der Sammlung vorkommen. Zum Beispiel:

```pycon
>>> words = ['cat', 'window', 'defenestrate']
>>> for word in words:
...     print(word, len(w))
...
cat 3
window 6
defenestrate 12
```

Code, der eine Sammlung ändert, während er über dieselbe Sammlung iteriert, kann schwierig zu bewerkstelligen sein und zu unerwünschten Nebeneffekten führen. Im schlechtesten Fall kann man so eine Endlosschleife erzeugen. Stattdessen ist es normalerweise einfacher, eine Schleife über eine Kopie der Sammlung laufen zu lassen oder eine neue Sammlung zu erstellen:

```pycon
>>> # Create a sample collection
>>> users = {'Hans': 'active', 'Éléonore': 'inactive', '景太郎': 'active'}
# Strategy:  Iterate over a copy
>>> for user, status in users.copy().items():
...     if status == 'inactive':
...         del users[user]
>>> users
{'Hans': 'active', '景太郎': 'active'}
>>> # Strategy:  Create a new collection
>>> active_users = {}
>>> for user, status in users.items():
...     if status == 'active':
...         active_users[user] = status
>>> active_users
{'Hans': 'active', '景太郎': 'active'}
```

Das Iterieren über eine Sammlung, im Englischen auch als "iterable" bezeichnet, kommt sehr oft vor und ist "typisch pythonisch". Man iteriert dabei, wenn möglich, immer direkt über die Sammlung, und nicht per Indexzugriff. Auch wenn letzteres das gleiche Ergebnis liefert, gilt es als unpythonisch, als Anti-Pattern und schlechter Still. Beispiel:

```pycon
>>> primes = [2, 3, 5, 7]
>>> # richtiges iterieren:
>>> for prime in primes:
...     print(prime)
...
2
3
5
7
>>> # schlechtes iterieren, macht man so in der Regel nicht
>>> for i in range(len(primes)):
...     print(primes[i])
...
2
3
5
7
```

## Range-Funktion

Wenn über eine Zahlenfolge iteriert werden muss, ist die eingebaute Funktion [range](https://docs.python.org/3/library/stdtypes.html#range) sehr nützlich. Sie erzeugt arithmetische Progressionen:

```pycon
>>> for i in range(5):
...     print(i)
...
0
1
2
3
4
```

`range` beginnt standmäßig mit 0 (Null). Der angegebene Endpunkt ist nie Teil der erzeugten Folge. `range(10)` erzeugt 10 Werte, die Indizes für Elemente einer Folge der Länge 10 - also die Zahlen von 0 bis 9. Es ist möglich, den Bereich bei einer anderen Zahl beginnen zu lassen, oder eine andere Schrittweite anzugeben. Eine negative Schrittweite ist ebenfalls möglich:

```pycon
>>> list(range(5, 10))
[5, 6, 7, 8, 9]
>>> list(range(0, 10, 3))
[0, 3, 6, 9]
>>> list(range(-10, -100, -30))
[-10, -40, -70]
```

Das Iterieren über eine Sammlung kombiniert mit `range` und `len` geht auch, gilt aber als schlechter Stil:

```pycon
>>> a = ['Mary', 'had', 'a', 'little', 'lamb']
>>> for i in range(len(a)):
...     print(i, a[i])
...
0 Mary
1 had
2 a
3 little
4 lamb
```

Bei solchen Anwendungsfällen ist es besser, die eingebaute Funktion [enumerate](https://docs.python.org/3/library/functions.html#enumerate) zu nutzen:

```pycon
>>> for counter, word in enumerate(a):
...     print(counter, word)
...
0 Mary
1 had
2 a
3 little
4 lamb
```

Wenn man versucht, `print` auf `range` anzuwenden, passiert etwas Unerwartetes:

```pycon
>>> print(range(10))
range(0, 10)
```

In vielerlei Hinsicht verhält sich das von `range` zurückgegebene Objekt wie eine Liste, aber in Wirklichkeit ist es das nicht. Es ist ein Objekt, das die aufeinanderfolgenden Elemente der gewünschten Sequenz zurückgibt, wenn man darüber iterieren, aber es bildet nicht wirklich die Liste und spart so Platz. Die Werte werden erst generiert, wenn tatsächlich darauf zugegriffen wird.

Ein solches Objekt ist "iterable", auf Deutsch: iterierbar, d.h. es eignet sich als Ziel für Funktionen und Konstrukte, die etwas erwarten, von dem sie aufeinanderfolgende Elemente erhalten können, bis der Vorrat erschöpft ist. Die for-Schleife ist ebenfalls ein solches Konstrukt, während ein Beispiel für eine Funktion, die eine Iterable annimmt, die eingebaute Funktion [sum](https://docs.python.org/3/library/functions.html#sum) ist:

```pycon
>>> sum(range(4))  # 0 + 1 + 2 + 3
6
```

Später werden noch mehr Funktionen zu sehen sein, die Iterables zurückgeben und Iterables als Argumente annehmen. Im Kapitel über Datenstrukturen wird z.B. ausführlicher über Listen gesprochen.

## Break und continue Anweisungen, else Klauseln in Schleifen

Die Anweisung [break](https://docs.python.org/3/reference/simple_stmts.html#break) bricht aus der innersten umschließenden `for` oder `while` Schleife aus.

Eine `for` oder `while` Schleife kann eine `else` Klausel enthalten.

In einer `for` Schleife wird die `else` Klausel ausgeführt, nachdem die Schleife ihre letzte Iteration erreicht hat. In einer `while` Schleife wird sie ausgeführt, nachdem die Bedingung der Schleife falsch wird.

In beiden Arten von Schleifen wird die `else` Klausel *nicht* ausgeführt, wenn die Schleife durch ein `break` beendet wurde.

Dies wird in der folgenden `for` Schleife veranschaulicht, die nach Primzahlen sucht:

```pycon
>>> for n in range(2, 10):
...     for x in range(2, n):
...         if n % x == 0:
...             print(n, 'equals', x, '*', n//x)
...             break
...     else:
...         # loop fell through without finding a factor
...         print(n, 'is a prime number')
...
   2 is a prime number
   3 is a prime number
   4 equals 2 * 2
   5 is a prime number
   6 equals 2 * 3
   7 is a prime number
   8 equals 2 * 4
   9 equals 3 * 3
```

Ja, das ist valider Code. Die `else` Klausel gehört zur `for` Schleife, *nicht* zur `if`-Anweisung.

Wenn die `else` Klausel mit einer Schleife verwendet wird, hat sie mehr mit der `else` Klausel einer `try` Anweisung gemeinsam als mit der von der `if` Anweisungen: die `else` Klausel einer `try` Anweisung wird ausgeführt, wenn keine Ausnahme auftritt, und die `else` Klausel einer Schleife wird ausgeführt, wenn kein `break` auftritt. Mehr über die `try` Anweisung und Ausnahmen sind im Kapitel über Fehlerbehandlung zu finden.

Die ebenfalls aus C entlehnte Anweisung [continue](https://docs.python.org/3/reference/simple_stmts.html#continue) fährt mit der nächsten Iteration der Schleife fort:

```pycon
>>> for num in range(2, 10):
...     if num % 2 == 0:
...         print("Found an even number", num)
...         continue
...     print("Found an odd number", num)
...
Found an even number 2
Found an odd number 3
Found an even number 4
Found an odd number 5
Found an even number 6
Found an odd number 7
Found an even number 8
Found an odd number 9
```

## Pass-Anweisung

Die Anweisung [pass](https://docs.python.org/3/reference/simple_stmts.html#pass) bewirkt: nichts.
Sie kann verwendet werden, wenn eine Anweisung syntaktisch erforderlich ist, das Programm aber keine Aktion verlangt. Zum Beispiel:

```pycon
>>> while True:
...     pass  # Busy-wait Endlosschleife, mit STRG+C beenden
...
```

`pass` wird z.B. typischerweise verwendet, wenn man eine minimale, leere Klasse benötigt:

```pycon
>>> class MyEmptyClass:
...     pass
...
```

Ein anderer Fall, an dem `pass` verwendet werden kann, ist als Platzhalter für eine Funktion oder einen Bedingungskörper, wenn man an neuem Code arbeitet. Dies ermöglicht, auf einer abstrakteren Ebene zu denken. Das `pass` wird stillschweigend ignoriert:

```pycon
>>> def initlog(*args):
...     pass   # TODO: muss später noch implementiert werden.
...
```

## Match-Anweisung

Eine [match](https://docs.python.org/3/reference/compound_stmts.html#match) Anweisung nimmt einen Ausdruck und vergleicht dessen Wert mit aufeinanderfolgenden Mustern, die als ein oder mehrere `case` Blöcke angegeben sind. Oberflächlich betrachtet ähnelt dies einer switch-Anweisung in C, Java oder JavaScript (und vielen anderen Sprachen), aber es ähnelt eher dem Mustervergleich in Sprachen wie Rust oder Haskell. Nur das erste Muster, das passt, wird ausgeführt, und es können auch Komponenten (Sequenzelemente oder Objektattribute) aus dem Wert in Variablen extrahiert werden.

Die `match` Anweisung wurde mit Python 3.10 eingeführt.

Die einfachste Form vergleicht einen Wert mit einem oder mehreren Literalen:

```pycon
def http_error(status):
    match status:
        case 400:
            return "Bad request"
        case 404:
            return "Not found"
        case 418:
            return "I'm a teapot"
        case _:
            return "Something's wrong with the internet"
```

Zu beachten ist der letzte Block: Der "Variablenname" `_` fungiert als *Wildcard* und führt immer zu einer Übereinstimmung. Wenn kein Fall übereinstimmt würde, würde keine der Verzweigungen ausgeführt. Was natürlich nur der Fall sein kann, wenn kein `case _` verwendet würde.

Man kann mehrere Literale in einem einzigen Muster mit `|` ("oder") kombinieren:

```pycon
        case 401 | 403 | 404:
            return "Not allowed"
```

Muster können wie Entpackungszuweisungen aussehen und können zum Binden von Variablen verwendet werden:

```pycon
# point is an (x, y) tuple
match point:
    case (0, 0):
        print("Origin")
    case (0, y):
        print(f"Y={y}")
    case (x, 0):
        print(f"X={x}")
    case (x, y):
        print(f"X={x}, Y={y}")
    case _:
        raise ValueError("Not a point")
```

Lese dieses Muster sorgfältig! Das erste Muster besteht aus zwei Literalen und kann als Erweiterung des oben gezeigten Literalmusters betrachtet werden. Aber die nächsten beiden Muster kombinieren ein Literal und eine Variable, und die Variable *bindet* einen Wert aus dem Wert `point`. Das vierte Muster erfasst zwei Werte, was es konzeptionell der Entpackungszuweisung `(x, y) = point` ähnlich macht.

Wenn man Klassen verwendet, um Daten zu strukturieren, kann man den Klassennamen gefolgt von einer Argumentliste verwenden, die einem Konstruktor ähnelt, jedoch mit der Möglichkeit, Attribute in Variablen zu erfassen:

```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def where_is(point):
        match point:
            case Point(x=0, y=0):
                print("Origin")
            case Point(x=0, y=y):
                print(f"Y={y}")
            case Point(x=x, y=0):
                print(f"X={x}")
            case Point():
                print("Somewhere else")
            case _:
                print("Not a point")
```

Man kann Positionsparameter mit einigen eingebauten Klassen verwenden, die eine Reihenfolge für die Attribute vorgeben (z.B. Datenklassen). Man kann auch eine bestimmte Position für Attribute in Mustern definieren, indem man das spezielle Attribut `__match_args__` in der Klasse setzt. Wenn es auf ("x", "y") gesetzt ist, sind die folgenden Muster alle gleichwertig (und binden alle das Attribut `y` an die Variable `var`):

```python
Point(1, var)
Point(1, y=var)
Point(x=1, y=var)
Point(y=var, x=1)
```

Es empfiehlt sich, die Muster als eine erweiterte Form dessen zu betrachten, was man links von einer Zuweisung schreiben würde, um zu verstehen, welche Variablen auf was gesetzt werden würden. Nur die eigenständigen Namen (wie `var` oben) werden durch eine `match` Anweisung zugewiesen. Gepunktete Namen (wie `foo.bar`), Attributnamen (wie `x=` und `y=` oben) oder Klassennamen (erkennbar an dem "(...)" neben ihnen wie `Point` oben) werden niemals zugewiesen.

Muster können beliebig verschachtelt werden. Wenn man zum Beispiel eine kurze Liste von Punkten hat, die mit ``__match_args__`` ergänzt wurde, kann man sie wie folgt abgleichen:

```python
class Point:
    __match_args__ = ('x', 'y')

    def __init__(self, x, y):
        self.x = x
        self.y = y

match points:
    case []:
        print("No points")
    case [Point(0, 0)]:
        print("The origin")
    case [Point(x, y)]:
        print(f"Single point {x}, {y}")
    case [Point(0, y1), Point(0, y2)]:
        print(f"Two on the Y axis at {y1}, {y2}")
    case _:
        print("Something else")
```

Man kann eine `if`Klausel zu einem Muster hinzufügen, die als "guard" (auf Deutsch: Wächter) bekannt ist. Wenn die Schutzklausel falsch ist, fährt `match` mit dem nächsten Fallblock fort. Zu beachten ist, dass die Werteerfassung vor der Auswertung des Guards erfolgt:

```python
match point:
    case Point(x, y) if x == y:
        print(f"Y=X at {x}")
    case Point(x, y):
        print(f"Not on the diagonal")
```

Einige weitere Features dieser Anweisung sind:

 * Wie Entpackungszuweisungen haben Tupel- und Listenmuster genau dieselbe Bedeutung und passen tatsächlich zu beliebigen Sequenzen. Eine wichtige Ausnahme ist, dass sie nicht auf Iteratoren oder Zeichenketten passen.
 * Sequenzmuster unterstützen erweitertes Entpacken: `[x, y, *rest]` und `(x, y, *rest)` funktionieren ähnlich wie Entpackungszuweisungen. Der Name nach `*` kann auch `_` sein, sodass `(x, y, *_)` mit einer Folge von mindestens zwei Elementen übereinstimmt, ohne die restlichen Elemente zu binden.
 * Mapping patterns: `{"bandwidth": b, "latency": l}` erfasst die "bandwidth" und "latency" Werte aus einem Wörterbuch. Anders als bei Sequenzmustern werden zusätzliche Schlüssel ignoriert. Ein Entpacken wie `**rest` wird ebenfalls unterstützt (aber `**_` wäre redundant, daher ist es nicht erlaubt.)
 * Unterpattern (englisch: subpattern) können über das Schlüsselwort `as` erfasst werden. Im folgenden Beispiel wird das zweite Element der Eingabe als `p2` erfassen (solange die Eingabe eine Folge von zwei Punkten ist)

```python
    case (Point(x1, y1), Point(x2, y2) as p2): ...
```

 * Die meisten Literale werden durch Gleichheit verglichen, aber die Singletons `True`, `False` und `None` werden durch Identität verglichen.
 * Muster können benannte Konstanten verwenden. Diese müssen gepunktete Namen sein, damit sie nicht als Capture-Variable interpretiert werden können:

```python
from enum import Enum

class Color(Enum):
    RED = 'red'
    GREEN = 'green'
    BLUE = 'blue'

color = Color(input("Enter your choice of 'red', 'blue' or 'green': "))

match color:
    case Color.RED:
        print("I see red!")
    case Color.GREEN:
        print("Grass is green")
    case Color.BLUE:
        print("I'm feeling the blues :(")
```

Für weitere Details kann man die [PEP 636](https://peps.python.org/pep-0636/) lesen, welche viele weitere Beispiele und Erklärungen enthält.

## Funktionen definieren

Im Folgenden wird eine Funktion erstellt, die die [Fibonacci-Reihe](https://de.wikipedia.org/wiki/Fibonacci-Folge) bis zu einem beliebigen Wert berechnet:

```pycon
>>> def fib(n):    # write Fibonacci series up to n
...     """Print a Fibonacci series up to n."""
...     a, b = 0, 1
...     while a < n:
...         print(a, end=' ')
...         a, b = b, a+b
...     print()
...
>>> # Now call the function we just defined:
>>> fib(2000)
   0 1 1 2 3 5 8 13 21 34 55 89 144 233 377 610 987 1597
```

Das Schlüsselwort [def](https://docs.python.org/3/reference/compound_stmts.html#def) leitet eine *Funktionsdefinition* ein. Diese muss vom Funktionsnamen und der in Klammern gesetzten Liste der formalen Parameter gefolgt werden, wobei auch eine leere Liste (=keine Parameter) erlaubt ist. Die Anweisungen, die den Körper der Funktion bilden, beginnen in der nächsten Zeile und müssen eingerückt werden. Als Einrückung werden wie unter Python üblich vier Leerzeichen empfohlen.

Die erste Anweisung des Funktionskörpers kann optional ein Zeichenkettenliteral sein. Dieses Zeichenkettenliteral ist die Dokumentationszeichenkette der Funktion, genannt "docstring". Mehr über docstrings ist weiter unten in diesem Kapitel zu finden. Es gibt Werkzeuge, die docstrings verwenden, um automatisch Online- oder gedruckte Dokumentation zu erzeugen, oder um den Benutzer interaktiv durch den Code blättern zu lassen. Es gilt als gute Praxis, docstrings in den eigenen Code einzuschließen - sollte man sich am besten direkt zur Gewohnheit machen.

Die *Ausführung* einer Funktion führt eine neue Symboltabelle ein, die für die lokalen Variablen der Funktion verwendet wird. Genauer gesagt speichern alle Variablenzuweisungen in einer Funktion den Wert in der lokalen Symboltabelle, während Variablenreferenzen zuerst in der lokalen Symboltabelle, dann in den lokalen Symboltabellen der umschließenden Funktionen, dann in der globalen Symboltabelle und schließlich in der Tabelle der eingebauten Namen nachsehen. Globalen Variablen und Variablen von umschließenden Funktionen kann also innerhalb einer Funktion nicht direkt ein Wert zugewiesen werden - es sei denn, globale Variablen werden in einer `global`-Anweisung oder Variablen von umschließenden Funktionen in einer `nonlocal` aufgeführt, obwohl sie referenziert werden können. Die Verwendung von `global` ist aber in den allermeisten Fällen in einem sauber strukturierten Programm überflüssig und sollte von daher vermieden werden. Außerdem kann `global` den Zustand eines Programms schwer nachvollziehbar machen.

Die eigentlichen Parameter (Argumente) eines Funktionsaufrufs werden in die lokale Symboltabelle der aufgerufenen Funktion eingeführt, wenn diese aufgerufen wird. Argumente werden also mit *Aufruf durch Wert* übergeben, wobei der *Wert* immer ein Objekt *Referenz* ist, nicht der Wert des Objekts. Wenn eine Funktion eine andere Funktion aufruft oder sich selbst rekursiv aufruft, wird eine neue lokale Symboltabelle für diesen Aufruf erstellt.

Eine Funktionsdefinition verknüpft den Funktionsnamen mit dem Funktionsobjekt in der aktuellen Symboltabelle. Der Interpreter erkennt das Objekt, auf das dieser Name verweist, als eine benutzerdefinierte Funktion. Andere Namen können ebenfalls auf dasselbe Funktionsobjekt verweisen und für den Zugriff auf die Funktion verwendet werden:

```pycon
>>> fib
<function fib at 10042ed0>
>>> f = fib
>>> f(100)
0 1 1 2 3 5 8 13 21 34 55 89
```

Wenn man aus anderen Sprachen kommt, wird man vielleicht einwenden, dass `fib` keine Funktion, sondern eine Prozedur ist, da sie keinen Wert zurückgibt. In der Tat geben auch Funktionen ohne eine[return](https://docs.python.org/3/reference/simple_stmts.html#return) Anweisung einen Wert zurück, wenn auch einen ziemlich langweiligen. Dieser Wert ist `None`, welche in Python ein gültiges Objekt ist. Das Schreiben des Wertes `None` wird normalerweise vom Interpreter unterdrückt, wenn es der einzige Wert ist, der geschrieben wird. Dies kann man sehen, wenn man es wirklich will, indem man `print` verwenden:

```pycon
>>> fib(0)
>>> print(fib(0))
None
```

Es ist einfach, eine Funktion zu schreiben, die eine Liste der Zahlen der Fibonacci-Reihe zurückgibt, anstatt sie auszugeben:

```pycon
>>> def fib2(n):  # return Fibonacci series up to n
...     """Return a list containing the Fibonacci series up to n."""
...     result = []
...     a, b = 0, 1
...     while a < n:
...         result.append(a)
...         a, b = b, a+b
...     return result
...
>>> f100 = fib2(100)    # aufrufen von fib2 mit dem Parameter 100
>>> f100                # Ausgabe des Ergebnisses
[0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89]
```

Dieses Beispiel zeigt auch einige Features von Python, die bisher noch nicht behandelt wurden:

 * Die Anweisung `return` liefert einen Wert aus einer Funktion zurück. `return` ohne einen Wert gibt automatisch `None` zurück. Wird eine Funktion ohne abschließendes `return` beendet wird ebenfalls `None` zurück geliefert.
 * Die Anweisung `result.append(a)` ruft eine *Methode* des Listenobjekts `result` auf. Eine Methode ist eine Funktion, die zu einem Objekt "gehört" und den Namen `obj.Methodenname` trägt, wobei `obj` ein Objekt ist (dies kann ein Ausdruck sein) und `Methodenname` der Name einer Methode ist, die durch den Typ des Objekts definiert ist. Verschiedene Typen definieren verschiedene Methoden. Methoden verschiedener Typen können denselben Namen haben, ohne dass dies zu Mehrdeutigkeit führt. Es ist möglich, eigene Objekttypen und Methoden zu definieren, indem man *Klassen* verwendet, dazu später mehr im Kapitel über Klassen. Die im Beispiel gezeigte Methode `append` ist für Listenobjekte definiert. Sie fügt ein neues Element am Ende der Liste hinzu. In diesem Beispiel ist sie äquivalent zu `Ergebnis = Ergebnis + [a]`, aber effizienter.

### Hinweis zu Funktionsnamen

Bei der Benennung von Funktionen sollte man darauf achten, dass diese aussagekräftige Namen haben. Dies kommt der Verständlichkeit des geschriebenen Codes zugute. Des Weiteren sollte man darauf achten, dass der Funktionsname *nicht* einen Namen einer [built-in Funktion](https://docs.python.org/3/library/functions.html) oder einer importierten Funktion überschreibt. Dies kann zu unerwünschten Nebeneffekten und schwer nachzuvollziehenden Fehlern führen.

Im folgenden Beispiel wird die eingebaute Funktion [sum](https://docs.python.org/3/library/functions.html#sum) überschrieben:

```pycon
>>> data = [1, 2, 3]
>>> sum(data)
6
>>> def sum(name):
...     print(name + ' is summing things up')
...
>>> sum(data)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 2, in sum
TypeError: can only concatenate list (not "str") to list
>>>
```

Python besitzt keinen eingebauten Schutz, der verhindert, dass man bestehende Funktionsnamen überschreibt. Primär aus dem Grund, dass es Anwendungsfälle geben könnte, wo genau das gewollt ist (auch wenn diese Fälle rar sind).

Das Überschreiben von Python Schlüsselworten führt dagegen zu einem Fehler, wie zum Beispiel:

```pycon
>>> def if():
  File "<stdin>", line 1
    def if():
        ^^
SyntaxError: invalid syntax
>>> def break(name):
  File "<stdin>", line 1
    def break(name):
        ^^^^^
SyntaxError: invalid syntax
```

## Mehr zur Definition von Funktionen

Es ist auch möglich, Funktionen mit einer variablen Anzahl von Argumenten zu definieren. Es gibt drei Formen, die miteinander kombiniert werden können.

### Argumente mit Vorgabewerten

Die nützlichste Form ist die Angabe eines Standardwerts (Defaultwert) für ein oder mehrere Argumente. Dadurch wird eine Funktion geschaffen, die mit weniger Argumenten aufgerufen werden kann, als für sie definiert sind. Zum Beispiel:

```python
def ask_ok(prompt, retries=4, reminder='Please try again!'):
    while True:
        reply = input(prompt)
        if reply in {'y', 'ye', 'yes'}:
            return True
        if reply in {'n', 'no', 'nop', 'nope'}:
            return False
        retries = retries - 1
        if retries < 0:
            raise ValueError('invalid user response')
        print(reminder)
```

Diese Funktion kann mit verschiedenen Argumenten aufgerufen werden:

 * nur mit dem Pflichtargument `prompt`: `ask_ok('Do you really want to quit?')`
 * mit einem der optionalen Argumente: `ask_ok('OK to overwrite the file?', 2)`
 * mit allen Argumenten: `ask_ok('OK to overwrite the file?', 2, 'Come on, only yes or no!')`

In diesem Beispiel wird das Schlüsselwort [in](https://docs.python.org/3/reference/expressions.html#in) verwendet. Damit wird überprüft, ob eine Sequenz einen bestimmten Wert enthält.

Die Standardwerte werden an der Stelle der Funktionsdefinition im *definierenden* Bereich ausgewertet, sodass das folgende Beispiel 5 ausgibt:

```pycon
>>> i = 5
>>> def f(arg=i):
...     print(arg)
...
>>> i = 6
>>> f()
5
```

***Wichtiger Hinweis:*** Der Standardwert wird nur einmal ausgewertet. Es macht einen Unterschied, wenn der Standardwert ein veränderbares Objekt wie eine Liste, ein Wörterbuch oder Instanzen der meisten Klassen ist. Die folgende Funktion akkumuliert zum Beispiel die Argumente, die ihr bei nachfolgenden Aufrufen übergeben werden:

```pycon
>>> def f(a, L=[]):
...     L.append(a)
...     return L
...
>>> print(f(1))
>>> print(f(2))
>>> print(f(3))
```

Dies ergibt die Ausgabe:

```
[1]
[1, 2]
[1, 2, 3]
```

Wenn man nicht möchte, dass der Standardwert von nachfolgenden Aufrufen gemeinsam genutzt wird, kann die Funktion stattdessen wie folgt schreiben:

```pycon
>>> def f(a, L=None):
...     if L is None:
...         L = []
...     L.append(a)
...     return L
...
```

### Schlüsselwortargumente

Funktionen können auch mit [Schlüsselwortargumente](https://docs.python.org/3/glossary.html#term-keyword-argument) in der Form `kwarg=Wert` aufgerufen werden. Zum Beispiel:

```python
def parrot(voltage, state='a stiff', action='voom', type='Norwegian Blue'):
    print("-- This parrot wouldn't", action, end=' ')
    print("if you put", voltage, "volts through it.")
    print("-- Lovely plumage, the", type)
    print("-- It's", state, "!")
```

Die Funktion akzeptiert ein erforderliches Argument (`voltage`) und drei optionale Argumente (`state`, `action` und `type`). Diese Funktion kann auf eine der folgenden Arten aufgerufen werden:

```python
parrot(1000)                                          # 1 positional argument
parrot(voltage=1000)                                  # 1 keyword argument
parrot(voltage=1000000, action='VOOOOOM')             # 2 keyword arguments
parrot(action='VOOOOOM', voltage=1000000)             # 2 keyword arguments
parrot('a million', 'bereft of life', 'jump')         # 3 positional arguments
parrot('a thousand', state='pushing up the daisies')  # 1 positional, 1 keyword
```

Die folgenden Aufrufe sind nicht gültig und führen zu einem Fehler:

```python
parrot()                     # required argument missing
parrot(voltage=5.0, 'dead')  # non-keyword argument after a keyword argument
parrot(110, voltage=220)     # duplicate value for the same argument
parrot(actor='John Cleese')  # unknown keyword argument
```

In einem Funktionsaufruf müssen die Schlüsselwortargumente auf die Positionsargumente folgen. Alle übergebenen Schlüsselwortargumente müssen mit den Argumenten der Funktion übereinstimmen (z.B. `actor` ist kein gültiges Argument für die Funktion `parrot`) und ihre Reihenfolge ist egal. Dies schließt auch nicht-optionale Argumente ein (z.B. `parrot(voltage=1000)` ist ebenfalls gültig). Kein Argument darf mehr als einmal einen Wert erhalten. Hier ein Beispiel, das an dieser Einschränkung scheitert:

```pycon
>>> def function(a):
...     pass
...
>>> function(0, a=0)
   Traceback (most recent call last):
     File "<stdin>", line 1, in <module>
   TypeError: function() got multiple values for argument 'a'
```

Wenn ein endgültiger formaler Parameter der Form `**name` vorhanden ist, erhält er ein Wörterbuch (siehe [Datentyp Mapping](https://docs.python.org/3/library/stdtypes.html#typesmapping)), das alle Schlüsselwortargumente mit Ausnahme derjenigen enthält, die einem formalen Parameter entsprechen. Dies kann mit einem formalen Parameter der Form `*name` (beschrieben im nächsten Abschnitt) kombiniert werden, der ein [Tupel](https://docs.python.org/3/library/stdtypes.html#tuple) erhält, dass die Positionsargumente jenseits der formalen Parameterliste enthält. Dabei muss `*name` immer vor `**name` stehen. Im folgenden Beispiel ist eine Funktion wie diese definieren:

```python
def cheeseshop(kind, *arguments, **keywords):
    print("-- Do you have any", kind, "?")
    print("-- I'm sorry, we're all out of", kind)
    for arg in arguments:
        print(arg)
    print("-" * 40)
    for kw in keywords:
        print(kw, ":", keywords[kw])
```

Sie kann wie folgt aufgerufen werden:

```pycon
>>> cheeseshop("Limburger", "It's very runny, sir.",
 "It's really very, VERY runny, sir.",
 shopkeeper="Michael Palin",
 client="John Cleese",
 sketch="Cheese Shop Sketch")
```

Der Aufruf ergibt folgende Ausgabe:

```
-- Do you have any Limburger?
-- I'm sorry, we're all out of Limburger
It's very runny, sir.
It's really very, VERY runny, sir.
----------------------------------------
shopkeeper : Michael Palin
client : John Cleese
sketch : Cheese Shop Sketch
```

Zu beachten ist, dass die Reihenfolge, in der die Schlüsselwortargumente ausgegeben werden, garantiert der Reihenfolge entspricht, in der sie im Funktionsaufruf angegeben wurden.

### Spezielle Parameter

Standardmäßig können Argumente an eine Python-Funktion entweder nach Position oder explizit nach Schlüsselwort übergeben werden. Aus Gründen der Lesbarkeit und der Leistung ist es sinnvoll, die Art und Weise, wie Argumente übergeben werden können, einzuschränken, sodass ein Entwickler nur einen Blick auf die Funktionsdefinition werfen muss, um festzustellen, ob Elemente per Position, per Position oder Schlüsselwort oder per Schlüsselwort übergeben werden.

Eine Funktionsdefinition kann so aussehen:

```
def f(pos1, pos2, /, pos_or_kwd, *, kwd1, kwd2):
    -----------    ----------     ----------
        |             |                  |
        |        Positional or keyword   |
        |                                - Keyword only
        -- Positional only
```

Hierbei sind `/` und `*` optional. Falls verwendet, geben diese Symbole die Art des Parameters an, wie die Argumente an die Funktion übergeben werden können: nur Positionsparameter, Positions- oder Schlüsselwortparameter und nur Schlüsselwortparameter. Schlüsselwortparameter werden auch als "benannte Parameter" bezeichnet.

Wenn `/` und `*` in der Funktionsdefinition nicht vorhanden sind, können Argumente an eine Funktion durch Position oder durch Schlüsselwort übergeben werden.

#### Positions- oder Schlüsselwortargumente?

Bei näherer Betrachtung ist es möglich, bestimmte Parameter als *positional-only* zu kennzeichnen. Bei *positional-only* ist die Reihenfolge der Parameter wichtig, und die Parameter können nicht per Schlüsselwort übergeben werden. Nur-Positions-Parameter werden vor einem `/` (Schrägstrich) platziert. Das `/` wird verwendet, um die Nur-Positions-Parameter logisch von den übrigen Parametern zu trennen. Steht kein `/` in der Funktionsdefinition, so gibt es auch keine Parameter, die nur an einer bestimmten Stelle stehen.

Parameter, die dem `/` folgen, können *Positions- oder Schlüsselwort* oder *Nur-Schlüsselwort* sein.

#### Nur Schlüsselwortargumente

Um Parameter als *keyword-only* zu kennzeichnen, was bedeutet, dass die Parameter per Schlüsselwortargument übergeben werden müssen, setzt man ein `*` in die Argumentenliste direkt vor den ersten *keyword-only* Parameter.

#### Beispiele für Funktionen

Die folgenden Beispiele für Funktionsdefinitionen veranschaulichen dies. Man beachte dabei die Markierungen `/` und `*`:

```pycon
>>> def standard_arg(arg):
...     print(arg)
...
>>> def pos_only_arg(arg, /):
...     print(arg)
...
>>> def kwd_only_arg(*, arg):
...     print(arg)
...
>>> def combined_example(pos_only, /, standard, *, kwd_only):
...     print(pos_only, standard, kwd_only)
```

Die erste Funktionsdefinition `standard_arg` ist die bekannteste Form, enthält keine Einschränkungen bezüglich der Aufrufkonvention und Argumente können durch Position oder Schlüsselwort übergeben werden:

```pycon
>>> standard_arg(2)
2
>>> standard_arg(arg=2)
2
```

Die zweite Funktion `pos_only_arg` ist auf die Verwendung von Positionsparametern beschränkt, da sie ein `/` in der Funktionsdefinition enthält:

```pycon
>>> pos_only_arg(1)
1
>>> pos_only_arg(arg=1)
   Traceback (most recent call last):
     File "<stdin>", line 1, in <module>
   TypeError: pos_only_arg() got some positional-only arguments passed as keyword arguments: 'arg'
```

Die dritte Funktion `kwd_only_args` erlaubt nur Schlüsselwortargumente, die durch ein `*` in der Funktionsdefinition gekennzeichnet sind:

```pycon
>>> kwd_only_arg(3)
   Traceback (most recent call last):
     File "<stdin>", line 1, in <module>
   TypeError: kwd_only_arg() takes 0 positional arguments but 1 was given
>>> kwd_only_arg(arg=3)
3
```

Und die letzte verwendet alle drei Aufrufkonventionen in der gleichen Funktionsdefinition:

```pycon
>>> combined_example(1, 2, 3)
   Traceback (most recent call last):
     File "<stdin>", line 1, in <module>
   TypeError: combined_example() takes 2 positional arguments but 3 were given
>>> combined_example(1, 2, kwd_only=3)
1 2 3
>>> combined_example(1, standard=2, kwd_only=3)
1 2 3
>>> combined_example(pos_only=1, standard=2, kwd_only=3)
   Traceback (most recent call last):
     File "<stdin>", line 1, in <module>
   TypeError: combined_example() got some positional-only arguments passed as keyword arguments: 'pos_only'
```

Zu guter Letzt noch diese Funktionsdefinition, die eine potentielle Kollision zwischen dem Positionsargument `name` und `**kwds` aufweist, welche `name` als Schlüssel hat:

```pycon
>>> def foo(name, **kwds):
...    return 'name' in kwds
...
```

Es gibt keinen möglichen Aufruf, der `True` zurückgibt, da das Schlüsselwort `name` immer an den ersten Parameter gebunden ist. Zum Beispiel:

```pycon
>>> foo(1, **{'name': 2})
    Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
    TypeError: foo() got multiple values for argument 'name'
>>>
```

Aber mit `/` (nur Positionsargumente) ist es möglich, da es `name` als Positionsargument und `name` als Schlüssel in den Schlüsselwortargumenten erlaubt:

```pycon
>>> def foo(name, /, **kwds):
...     return 'name' in kwds
...
>>> foo(1, **{'name': 2})
True
```

Mit anderen Worten: die Namen von reinen Positionsparametern können in `**kwds` ohne Mehrdeutigkeit verwendet werden.

#### Zusammenfasung

Der Anwendungsfall bestimmt, welche Parameter in der Funktionsdefinition zu verwenden sind:

```python
def f(pos1, pos2, /, pos_or_kwd, *, kwd1, kwd2):
```

Als Richtlinie:

 * Man verwendet "positional-only", wenn man möchte, dass der Name der Parameter für den Benutzer nicht verfügbar ist. Dies ist nützlich, wenn Parameternamen keine wirkliche Bedeutung haben, wenn man die Reihenfolge der Argumente erzwingen will, wenn die Funktion aufgerufen wird oder wenn man einige Positionsparameter und beliebige Schlüsselwörter verwenden muss.
 * Man verwendet nur Schlüsselwörter, wenn die Namen eine Bedeutung haben und die Funktionsdefinition durch die explizite Angabe von Namen verständlicher ist oder man verhindern will, dass sich Benutzer auf die Position des übergebenen Arguments verlassen.
 * Für eine API sollte man nur Positionsangaben verwenden, um zu verhindern, dass die API kpautt ist, wenn der Name des Parameters in der Zukunft geändert werden sollte.

### Beliebige Argumentlisten

Die am seltensten verwendete Option ist schließlich die Angabe, dass eine Funktion mit einer beliebigen Anzahl von Argumenten aufgerufen werden kann. Diese Argumente werden in ein Tupel verpackt. Vor der variablen Anzahl von Argumenten können null oder mehr normale Argumente auftreten.

```python
def write_multiple_items(file, separator, *args):
    file.write(separator.join(args))
```

Normalerweise stehen diese *variablen* Argumente an letzter Stelle in der Liste der formalen Parameter, da sie alle verbleibenden Eingabeargumente, die an die Funktion übergeben werden, auffüllen. Alle formalen Parameter, die nach dem Parameter `*args` stehen, sind 'keyword-only'-Argumente, was bedeutet, dass sie nur als Schlüsselwörter und nicht als Positionsargumente verwendet werden können:

```pycon
>>> def concat(*args, sep="/"):
...     return sep.join(args)
...
>>> concat("earth", "mars", "venus")
'earth/mars/venus'
>>> concat("earth", "mars", "venus", sep=".")
'earth.mars.venus'
```

### Entpacken von Argumentlisten

Die umgekehrte Situation tritt ein, wenn die Argumente bereits in einer Liste oder einem Tupel vorliegen, aber für einen Funktionsaufruf, der separate Positionsargumente erfordert, ausgepackt werden müssen. Zum Beispiel erwartet die eingebaute Funktion `range` separate *start* und *stop* Argumente. Wenn sie nicht separat verfügbar sind, schreibt man den Funktionsaufruf mit dem `*` Operator, um die Argumente aus einer Liste oder einem Tupel auszupacken:

```pycon
>>> list(range(3, 6))            # normal call with separate arguments
[3, 4, 5]
>>> args = [3, 6]
>>> list(range(*args))            # call with arguments unpacked from a list
[3, 4, 5]
```

Auf die gleiche Weise können Wörterbücher Schlüsselwortargumente mit dem `**` Operator liefern:

```pycon
>>> def parrot(voltage, state='a stiff', action='voom'):
...     print("-- This parrot wouldn't", action, end=' ')
...     print("if you put", voltage, "volts through it.", end=' ')
...     print("E's", state, "!")
...
>>> d = {"voltage": "four million", "state": "bleedin' demised", "action": "VOOM"}
>>> parrot(**d)
-- This parrot wouldn't VOOM if you put four million volts through it. E's bleedin' demised !
```

### Lambda Ausdrücke

Kleine anonyme Funktionen können mit dem Schlüsselwort [lambda](https://docs.python.org/3/reference/expressions.html#lambda) erstellt werden. Diese Funktion gibt die Summe ihrer beiden Argumente zurück: `lambda a, b: a+b`. Lambda-Funktionen können überall dort verwendet werden, wo Funktionsobjekte benötigt werden. Sie sind syntaktisch auf einen einzigen Ausdruck beschränkt. Semantisch gesehen sind sie nur ein syntaktischer Zucker für eine normale Funktionsdefinition. Wie verschachtelte Funktionsdefinitionen können Lambda-Funktionen auf Variablen aus dem enthaltenden Bereich verweisen:

```pycon
>>> def make_incrementor(n):
...     return lambda x: x + n
...
>>> f = make_incrementor(42)
>>> f(0)
42
>>> f(1)
43
```

Im obigen Beispiel wird ein Lambda-Ausdruck verwendet, um eine Funktion zurückzugeben. Eine andere Verwendung ist die Übergabe einer kleinen Funktion als Argument. Im folgenden Beispiel wird `list.sort` den Schlüssel, nachdem sortiert werden soll, als Lambda-Funktion übergeben:

```pycon
>>> pairs = [(1, 'one'), (2, 'two'), (3, 'three'), (4, 'four')]
>>> pairs.sort(key=lambda pair: pair[1])
>>> pairs
[(4, 'four'), (1, 'one'), (3, 'three'), (2, 'two')]
```

### Docstrings

Im Folgenden werden einige Konventionen für den Inhalt und die Formatierung von docstrings (=Dokumentationsstrings) erläutert.

Die erste Zeile sollte immer eine kurze, prägnante Zusammenfassung des Zwecks des Objekts sein. Der Kürze halber sollte man nicht explizit den Namen oder den Typ des Objekts angeben, da dieser auf andere Weise verfügbar ist - es sei denn, der Name ist zufällig ein Verb, das die Funktion einer Funktion beschreibt. Diese Zeile sollte mit einem Großbuchstaben beginnen und mit einem Punkt enden.

Bei mehrzeiligen docstrings sollte die zweite Zeile leer sein, um die Zusammenfassung optisch vom Rest der Beschreibung zu trennen. Die folgenden Zeilen sollten aus einem oder mehreren Absätzen bestehen, die die Aufrufkonventionen des Objekts, seine Nebeneffekte usw. beschreiben.

Der Python-Parser entfernt keine Einrückung aus mehrzeiligen String-Literalen in Python, sodass Werkzeuge, die Dokumentation verarbeiten, die Einrückung auf Wunsch entfernen müssen. Dies geschieht nach der folgenden Konvention. Die erste nicht leere Zeile *nach* der ersten Zeile der Zeichenkette bestimmt den Grad der Einrückung für die gesamte Dokumentationszeichenkette. Man kann nicht die erste Zeile verwenden, da sie in der Regel an die öffnenden Anführungszeichen der Zeichenkette angrenzt, sodass ihre Einrückung im Zeichenfolgenliteral nicht ersichtlich ist. Leerzeichen, die dieser Einrückung "entsprechen", werden dann vom Anfang aller Zeilen der Zeichenkette entfernt. Zeilen, die weniger eingerückt sind, sollten nicht vorkommen, aber wenn sie vorkommen, sollten alle führenden Leerzeichen entfernt werden.

Im Folgenden ein Beispiel für einen mehrzeiligen docstring:

```pycon
>>> def my_function():
...     """Do nothing, but document it.
...
...     No, really, it doesn't do anything.
...     """
...     pass
...
>>> print(my_function.__doc__)
Do nothing, but document it.

No, really, it doesn't do anything.
```

### Annotation von Funktionen

[Funktionsannotationen](https://docs.python.org/3/reference/compound_stmts.html#function) sind vollständig optionale Metadaten-Informationen über die von benutzerdefinierten Funktionen verwendeten Typen (siehe [PEP3107](https://peps.python.org/pep-3107/) und [PEP484](https://peps.python.org/pep-0484) für weitere Informationen).

Annotationen werden im `__annotations__` Attribut der Funktion als Wörterbuch gespeichert und haben keine Auswirkung auf irgendeinen anderen Teil der Funktion. Parameter-Annotationen werden durch einen Doppelpunkt nach dem Parameternamen definiert, gefolgt von einem Ausdruck, der den Wert der Annotation auswertet. Rückgabe-Anmerkungen werden durch ein Literal `->`, gefolgt von einem Ausdruck, zwischen der Parameterliste und dem Doppelpunkt, der das Ende der `def` Anweisung bezeichnet, definiert. Das folgende Beispiel hat ein erforderliches Argument, ein optionales Argument und den Rückgabewert mit der Anmerkung:

```pycon
>>> def f(ham: str, eggs: str = 'eggs') -> str:
...     print("Annotations:", f.__annotations__)
...     print("Arguments:", ham, eggs)
...     return ham + ' and ' + eggs
...
>>> f('spam')
Annotations: {'ham': <class 'str'>, 'return': <class 'str'>, 'eggs': <class 'str'>}
Arguments: spam eggs
'spam and eggs'
```

## Coding Style - immer wichtig

Jetzt, wo man schon längere und komplexere Pythonprogramme schreiben kann, ist ein guter Zeitpunkt gekommen, um über den *Coding Style* zu sprechen. Die meisten Sprachen können in verschiedenen Stilen geschrieben (oder besser gesagt, *formatiert*) werden. Einige sind besser lesbar als andere. Es ist immer eine sehr gute Idee, es anderen leicht zu machen, den eigenen Code zu lesen - und ein guter Programmierstil hilft dabei enorm.

Für Python ist die [PEP8](https://peps.python.org/pep-0008/) der in der Python-Welt maßgebliche Coding Style (quasi der "heilige Gral" für gut lesbaren Code). Er fördert einen sehr lesbaren und angenehmen Programmierstil. Jeder Python-Entwickler sollte ihn irgendwann einmal lesen. Im Folgenden sind die wichtigsten Punkte aus der PEP8 aufgelistet:

 * Man verwendet zur Einrückung 4 Leerzeichen und *keine* Tabulatoren! 4 Leerzeichen sind ein guter Kompromiss zwischen kleinem Einzug (ermöglicht größere Verschachtelungstiefe) und großem Einzug (leichter zu lesen). Tabulatoren stiften Verwirrung und sollten daher weggelassen werden.
 * Die Zeilenlänge sollte nicht mehr als 79 Zeichen betragen. Dies hilft Benutzern mit kleinen Bildschirmen und ermöglicht es, mehrere Code-Dateien nebeneinander auf größeren Bildschirmen.
 * Man verwendet Leerzeilen, um Funktionen und Klassen sowie größere Codeblöcke innerhalb von Funktionen zu trennen.
 * Wenn möglich setzt man Kommentare in eine eigene Zeile.
 * Man verwendt Docstrings.
 * Man verwendet Leerzeichen um Operatoren und nach Kommas, aber nicht direkt innerhalb von Klammerkonstrukten: `a = f(1, 2) + g(3, 4)`.
 * Man benennt seine Klassen und Funktionen einheitlich. Die Konvention ist, `UpperCamelCase` für Klassen und `lowercase_with_underscores` für Funktionen und Methoden zu verwenden. Man verwendet immer `self` als Name für das erste Methodenargument (mehr dazu später im Kapitel über Klassen und Methoden).
 * Man verwendet keine ausgefallenen Kodierungen, wenn der Code in internationalen Umgebungen verwendet werden soll. Pythons Standard UTF-8 oder sogar einfaches ASCII funktionieren in jedem Fall am besten.
 * Man verwendet ebenfalls keine Nicht-ASCII-Zeichen in Bezeichnern, wenn auch nur die geringste Chance besteht, dass Menschen, die eine andere Sprache sprechen, den Code lesen oder pflegen werden.

***

 * Nächstes Kapitel: [Datenstrukturen und Datentypen](datastructures.md)
 * Vorheriges Kapitel: [Eine lockere Einführung in Python](introduction.md)
 * Zurück zur [Startseite](index.md)
