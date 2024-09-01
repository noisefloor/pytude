# Fehler und Ausnahmen (Exceptions)

Bisher wurden Fehlermeldungen nur erwähnt. Aber wer schon die ersten eigenen Schritte mit Python gemacht hat, wird wahrscheinlich schon die ein oder andere Fehlermeldung "live" zu sehen bekommen haben. Es gibt mindestens zwei unterschiedlichen Arten von Fehlern: Syntaxfehler und Ausnahmen.

## Syntaxfehler

Syntaxfehler (auf Englisch: Syntax Error) werden auch "Parser Fehler" genannt. Diesen wird man gerade beim Lernen aber auch später immer wieder begegnen:

```pycon
>>> while True print('Hello world')
    File "<stdin>", line 1
    while True print('Hello world')
               ^^^^^
SyntaxError: invalid syntax
```

Der Parser moniert die Zeile mit dem Fehler und markiert diesen mit kleinen Pfeilen an der, wo der Fehler auftritt. Der Fehler wird durch das mit Pfeil markierte Token verursacht (oder zumindest erkannt). Es kann auch sein, dass der Fehler durch ein fehlendes Token vor dem markierten Token verursacht wird. In dem Beispiel wird der Fehler bei der Funktion `print` erkannt, da ein Doppelpunkt (`:`) vor dieser Funktion fehlt. Dateiname und Zeilennummer werden ausgegeben, damit man weiß, wo man suchen muss, falls die Eingabe von einem Skript stammt.

## Ausnahmen

Auch wenn eine Anweisung oder ein Ausdruck syntaktisch korrekt ist, kann es zu einem Fehler kommen, wenn man versucht, den Code auszuführen. Fehler, die während der Ausführung entdeckt werden, werden *Ausnahmen* (auf Englisch: Exception) genannt und sind nicht unbedingt fatal: Man kann, muss man aber nicht, Ausnahmen gezielt im Programm abfangen und darauf reagieren, sodass das Programm weiterläuft. Im Folgenden ein paar Beispiele für Ausnahmen:

```pycon
>>> 10 * (1/0)
Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
ZeroDivisionError: division by zero
>>> 4 + spam*3
Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
NameError: name 'spam' is not defined
>>> '2' + 2
Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
TypeError: can only concatenate str (not "int") to str
```

Die letzte Zeile der Fehlermeldung gibt an, was passiert ist. Ausnahmen gibt es in verschiedenen Typen und der Typ wird als Teil der Meldung ausgeben. Die Typen im Beispiel sind `ZeroDivisionError` (auf Deutsch: Division durch Null Fehler), `NameError` (frei übersetzt: Fehler durch unbekannten Namen) und `TypeError` (übersetzt: Typenfehler). Die als Ausnahmetyp ausgegebene Zeichenkette ist der Name der aufgetretenen eingebauten Ausnahme. Dies gilt für alle eingebauten Ausnahmen, muss aber nicht für benutzerdefinierte Ausnahmen gelten (obwohl es eine nützliche Konvention ist). Standardausnahmenamen sind eingebaute Bezeichner aber keine reservierten Schlüsselwörter. Der Rest der Zeile enthält detaillierte Informationen über die Art der Ausnahme und deren Ursache.

Der vorangehende Teil der Fehlermeldung zeigt den Kontext, in dem die Ausnahme aufgetreten ist, in Form eines Stack-Tracebacks (kurz: Stacktrace). Im Allgemeinen enthält der Stacktrace die Zeile des Quelltexts, die die Ausnahme ausgelöst hat. Der Stacktrace zeigt jedoch keine Zeilen an, die von der Standardeingabe gelesen wurden.

Die Seite [Built-In Exceptions](https://docs.python.org/3/library/exceptions.html#bltin-exceptions) in der Python Dokumentation zeigt alle eingebauten Ausnahmen inklusiver Erklärung.

## Umgang mit Ausnahmen

Es ist möglich, Programme zu schreiben, die Ausnahmen gezielt behandeln. Im folgenden Beispiel wird der Benutzer zu einer Eingabe aufgefordert, bis eine gültige ganze Zahl eingegeben wurde. Aber dem Benutzer ist erlaubt, das Programm zu unterbrechen (mit der Tastenkombination STRG+C oder was auch immer das Betriebssystem unterstützt). Zu beachten ist, dass eine vom Benutzer erzeugte Unterbrechung durch das Auslösen der Ausnahme `KeyboardInterrupt` signalisiert wird:

```pycon
>>> while True:
...     try:
...         x = int(input("Please enter a number: "))
...         break
...     except ValueError:
...         print("Oops!  That was no valid number.  Try again...")
...
```
Das Schlüsselwort [try](https://docs.python.org/3/reference/compound_stmts.html#try) funktioniert wie folgt:

 * Als Erstes wird der *try Block*, also der auf das `try` folgende, eingerückte Codeblock ausgeführt. Das ist der gesamte Code bis zum folgenden `except` Block.
 * Wenn dieser Code ohne das eine Ausnahme auftritt, fehlerfrei durchläuft, wird der `except` Block ignoriert und übersprungen. Der Code wird mit der nächsten regulären Zeile fortgesetzt.
 * Wenn im `try` Block eine Ausnahme auftritt wird die Ausführung des Codes im `try` Block abgebrochen und in den `except` Block gesprungen, sofern ein `except` für die aufgetretene Ausnahme definiert ist. Nach Abarbeitung des `except` Blocks wird der darauffolgende Code regulär ausgeführt.
 * Wenn im `try` Block eine Ausnahme auftritt, für die kein `except` Block definiert ist, gibt es eine *unhandled exception* (auf Deutsch: unbehandelte Ausnahme) und die Ausführung des Codes wird mit einer Fehlermeldung abgebrochen.

Eine `try` Anweisung kann mehr als eine except Klausel enthalten, um Handler für verschiedene Ausnahmen anzugeben. Es wird höchstens ein Handler ausgeführt. Handler behandeln nur Ausnahmen, die in der entsprechenden `try` Klausel vorkommen, nicht in anderen Handlern der gleichen `try` Anweisung. Eine `except` Klausel kann mehrere Ausnahmen als eingeklammertes Tupel benennen. Zum Beispiel:

```pycon
... except (RuntimeError, TypeError, NameError):
...     pass
```

Eine Klasse in einer `except` Klausel ist mit einer Ausnahme kompatibel, wenn es sich um dieselbe Klasse oder eine Basisklasse davon handelt. Aber nicht umgekehrt: eine `except` Klausel, die eine abgeleitete Klasse aufführt, ist nicht mit einer Basisklasse kompatibel. Zum Beispiel wird der folgende Code B, C, D in dieser Reihenfolge ausgeben:

```python
class B(Exception):
    pass

class C(B):
    pass

class D(C):
    pass

for cls in [B, C, D]:
    try:
        raise cls()
    except D:
        print("D")
    except C:
        print("C")
    except B:
        print("B")
```

Wären die `except` Klauseln mit `Ausnahme B` an erster Stelle vertauscht, würde B, B, B gedruckt - die erste passende `except` Klausel wird ausgelöst.

Wenn eine Ausnahme auftritt, kann sie mit Werten verbunden sein, die auch als *Argumente* der Ausnahme bezeichnet werden. Das Vorhandensein und die Art der Argumente hängen von der Art der Ausnahme ab.

Die `except` Klausel kann eine Variable nach dem Namen der Ausnahme angeben. Die Variable ist an die Exception-Instanz gebunden, die typischerweise ein `args` Attribut hat, das die Argumente speichert. Der Einfachheit halber definieren die eingebauten Ausnahmetypen `object.__str__`, um alle Argumente ohne expliziten Zugriff auf `.args` auszugeben:

```pycon
>>> try:
...     raise Exception('spam', 'eggs')
... except Exception as inst:
...     print(type(inst))    # Der Ausnahmetyp
...     print(inst.args)     # Argumente gespeichert in .args
...     print(inst)          # __str__ erlaubt args direkt auszugeben, könnte
...                          # aber in Subklassen überschrieben werden
...     x, y = inst.args     # args entpacken
...     print('x =', x)
...     print('y =', y)
...
<class 'Exception'>
('spam', 'eggs')
('spam', 'eggs')
x = spam
y = eggs
```

Die Ausgabe der Ausnahme `object.__str__` wird bei unbehandelten Ausnahmen als letzter Teil ('Detail') der Meldung ausgegeben.

`BaseException` ist die gemeinsame Basisklasse für alle Ausnahmen. Eine ihrer Unterklassen, `Exception`, ist die Basisklasse für alle nicht-fatalen Ausnahmen. Ausnahmen, die keine Unterklassen von `Exception` sind, werden normalerweise nicht behandelt, da sie dazu dienen, anzuzeigen, dass das Programm beendet werden sollte. Dazu gehören `SystemExit`, das von `sys.exit` ausgelöst wird, und `KeyboardInterrupt`, das ausgelöst wird, wenn ein Benutzer das Programm unterbrechen möchte.

`Exception` kann als Platzhalter verwendet werden, der (fast) alles abfängt. Es ist gilt jedoch als gute Praxis, die Arten von Ausnahmen, die behandelt werden sollen, so genau wie möglich festzulegen und zuzulassen, dass unerwartete Ausnahmen weitergegeben werden.

Das gebräuchlichste Muster für die Behandlung von `Exception` ist, die Ausnahme auszugeben oder zu protokollieren und sie dann erneut auszulösen (damit ein Aufrufer die Ausnahme ebenfalls behandeln kann):

```python
import sys

try:
    file = open('myfile.txt')
    line = file.readline()
    number = int(line.strip())
except OSError as err:
    print("OS error:", err)
except ValueError:
    print("Could not convert data to an integer.")
except Exception as err:
    print(f"Unexpected {err=}, {type(err)=}")
    raise
```

Die `try...except` Anweisung hat eine optionale *else-Klausel*, die, wenn vorhanden, allen `except` Klauseln folgen muss. Dies ist nützlich für Code, der ausgeführt werden muss, wenn die `try` Klausel keine Ausnahme auslöst. Zum Beispiel:

```python
for arg in sys.argv[1:]:
    try:
        file = open(arg, 'r')
    except OSError:
       print('cannot open', arg)
    else:
        print(arg, 'has', len(file.readlines()), 'lines')
        file.close()
```

Die Verwendung der `else` Klausel ist besser als das Hinzufügen von zusätzlichem Code zur `try` Klausel, weil so verhindert wird, dass versehentlich eine Ausnahme abgefangen wird, die nicht durch den Code ausgelöst wurde, der durch die `try...except`-Anweisung geschützt ist.

Ausnahmebehandler behandeln nicht nur Ausnahmen, die unmittelbar in der `try` Klausel auftreten, sondern auch solche, die innerhalb von Funktionen auftreten, die (auch indirekt) in der `try` Klausel aufgerufen werden. Zum Beispiel:

```pycon
>>> def this_fails():
...     x = 1/0
...
>>> try:
...     this_fails()
... except ZeroDivisionError as err:
...     print('Handling run-time error:', err)
...
Handling run-time error: division by zero
```

## Ausnahmen auslösen

Mit der Anweisung `raise` kann der Programmierer das Auftreten einer bestimmten Ausnahme erzwingen. Zum Beispiel:

```pycon
>>> raise NameError('HiThere')
Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
NameError: HiThere
```

Das einzige Argument von `raise` gibt die Ausnahme an, die ausgelöst werden soll. Dies muss entweder eine Ausnahme-Instanz oder eine Ausnahme-Klasse sein (eine Klasse, die von `BaseException` abgeleitet ist, wie `Exception` oder eine ihrer Unterklassen). Wenn eine Ausnahmeklasse übergeben wird, wird sie implizit instanziiert, indem ihr Konstruktor ohne Argumente aufgerufen wird:

```python
raise ValueError  # Kurzform für 'raise ValueError()'
```

Wenn man feststellen muss, ob eine Ausnahme ausgelöst wurde, aber nicht beabsichtigen, sie zu behandeln, kann man mit einer einfacheren Form der `raise`-Anweisung die Ausnahme erneut auslösen:

```pycon
>>> try:
...     raise NameError('HiThere')
... except NameError:
...     print('An exception flew by!')
...     raise
...
An exception flew by!
Traceback (most recent call last):
    File "<stdin>", line 2, in <module>
NameError: HiThere
```

## Verkettung von Ausnahmen

Wenn eine unbehandelte Ausnahme innerhalb eines `except` Abschnitts auftritt, wird die behandelte Ausnahme an diesen Abschnitt angehängt und in die Fehlermeldung aufgenommen:

```pycon
>>> try:
...     open("database.sqlite")
... except OSError:
...     raise RuntimeError("unable to handle error")
...
Traceback (most recent call last):
    File "<stdin>", line 2, in <module>
FileNotFoundError: [Errno 2] No such file or directory: 'database.sqlite'

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
    File "<stdin>", line 4, in <module>
RuntimeError: unable to handle error
```

Um anzuzeigen, dass eine Ausnahme eine direkte Folge einer anderen ist, erlaubt die Anweisung `raise` eine optionale Klausel `from`:

```python
# exc must be exception instance or None.
raise RuntimeError from exc
```

Dies kann nützlich sein, wenn man Ausnahmen umwandeln will. Zum Beispiel:

```pycon
>>> def func():
...     raise ConnectionError
...
>>> try:
...     func()
... except ConnectionError as exc:
...     raise RuntimeError('Failed to open database') from exc
...
Traceback (most recent call last):
    File "<stdin>", line 2, in <module>
    File "<stdin>", line 2, in func
ConnectionError

The above exception was the direct cause of the following exception:

Traceback (most recent call last):
      File "<stdin>", line 4, in <module>
RuntimeError: Failed to open database
```

Es erlaubt auch die Deaktivierung der automatischen Ausnahmeverkettung mit Hilfe des `from None` Idioms:

```pycon
>>> try:
...     open('database.sqlite')
... except OSError:
...     raise RuntimeError from None
...
Traceback (most recent call last):
    File "<stdin>", line 4, in <module>
RuntimeError
```

Mehr Information über Verkettungsmechanismen findet man in der Python-Dokumentation unter [Built-in Exceptions](https://docs.python.org/3/library/exceptions.html#bltin-exceptions)

## benutzerdefinierte Ausnahmen

Programme können ihre eigenen Ausnahmen definieren, indem sie eine neue Ausnahmeklasse erstellen (mehr zu Klassen ist im entsprechenden Kapitel dieses Tutorial zu finden). Ausnahmen sollten normalerweise von der Klasse `Exception` abgeleitet werden, entweder direkt oder indirekt.

Es können Ausnahmeklassen definiert werden, die alles tun können, was jede andere Klasse auch tun kann, aber sie sind normalerweise einfach gehalten und bieten oft nur eine Reihe von Attributen, die es ermöglichen, Informationen über den Fehler durch Handler für die Ausnahme zu extrahieren.

Die meisten Ausnahmen werden mit Namen definiert, die auf "Error" enden, ähnlich wie die Namen der Standardausnahmen.

Viele Standardmodule definieren ihre eigenen Ausnahmen, um Fehler zu melden, die in den von ihnen definierten Funktionen auftreten können.

## Aufräumarbeiten nach Ausnahmen definieren

Die Anweisung `try` hat eine weitere optionale Klausel, die dazu dient, Aufräumaktionen zu definieren, die unter allen Umständen ausgeführt werden müssen. Zum Beispiel:

```pycon
>>> try:
...     raise KeyboardInterrupt
... finally:
...     print('Goodbye, world!')
...
Goodbye, world!
Traceback (most recent call last):
    File "<stdin>", line 2, in <module>
KeyboardInterrupt
```

Wenn eine `finally` Klausel vorhanden ist, wird die `finally` Klausel als letzte Aufgabe ausgeführt, bevor die `try` Anweisung abgeschlossen wird. Die `finally` Klausel wird unabhängig davon ausgeführt, ob die `try` Anweisung eine Ausnahme erzeugt oder nicht. Die folgenden Punkte behandeln komplexere Fälle, in denen eine Ausnahme auftritt:

 * Tritt während der Ausführung der `try`-Klausel eine Ausnahme auf, kann diese durch eine `except`-Klausel behandelt werden. Wenn die Ausnahme nicht durch eine `except`-Klausel behandelt wird, wird die Ausnahme nach Ausführung der `finally`-Klausel erneut ausgelöst.
 * Eine Ausnahme kann während der Ausführung einer `except` oder `else`-Klausel auftreten. Auch hier wird die Ausnahme nach der Ausführung der `finally`-Klausel erneut ausgelöst.
 * Wenn die `finally` Klausel eine `break`, `continue` oder `return` Anweisung ausführt, werden die Ausnahmen nicht erneut ausgelöst.
 * Wenn die `try`-Anweisung eine `break`, `continue` oder `return` Anweisung erreicht, wird die `finally`-Klausel unmittelbar vor der Ausführung der `break`, `continue` oder `return`-Anweisung ausgeführt.
 * Wenn eine `finally`-Klausel eine `return` Anweisung enthält, ist der zurückgegebene Wert derjenige aus der `finally` Klausel und nicht derjenige aus der `try` Klausel und der `return`-Anweisung.

Zum Beispiel:

```pycon
>>> def bool_return():
...     try:
...         return True
...     finally:
...         return False
...
>>> bool_return()
False
```

Ein komplizierteres Beispiel:

```pycon
>>> def divide(x, y):
...     try:
...         result = x / y
...     except ZeroDivisionError:
...         print("division by zero!")
...     else:
...         print("result is", result)
...     finally:
...         print("executing finally clause")
...
>>> divide(2, 1)
result is 2.0
executing finally clause
>>> divide(2, 0)
division by zero!
executing finally clause
>>> divide("2", "1")
executing finally clause
Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
    File "<stdin>", line 3, in divide
TypeError: unsupported operand type(s) for /: 'str' and 'str'
```

Wie man sehen kann, wird die `finally` Klausel auf jeden Fall ausgeführt. Der `TypeError`, der durch die Teilung zweier Strings ausgelöst wird, wird von der `except` Klausel nicht behandelt und wird daher nach Ausführung der `finally` Klausel erneut ausgelöst.

In realen Anwendungen ist die `finally` Klausel nützlich, um externe Ressourcen (wie Dateien oder Netzwerkverbindungen) freizugeben, unabhängig davon, ob die Verwendung der Ressource erfolgreich war.

## vordefinierte Ausräumaktionen

Einige Objekte definieren standardmäßige Aufräumaktionen, die durchgeführt werden, wenn das Objekt nicht mehr benötigt wird, unabhängig davon, ob die Operation, die das Objekt verwendet, erfolgreich war oder nicht. Im folgenden Beispiel wird versucht, eine Datei zu öffnen und ihren Inhalt auf den Bildschirm auszugeben:

```python
for line in open("myfile.txt"):
    print(line, end="")
```

Das Problem bei diesem Code ist, dass er die Datei für eine unbestimmte Zeit offenlässt, nachdem dieser Teil des Codes ausgeführt wurde. Dies ist bei einfachen Skripten in der Regel kein Problem, kann aber bei größeren Anwendungen ein Problem darstellen. Mit der Anweisung `with` (mehr dazu gab es schon im Kapitel "Eingabe und Ausgabe" dieses Tutorials) können Objekte wie Dateien auf eine Weise verwendet werden, die sicherstellt, dass sie immer rechtzeitig und korrekt aufgeräumt werden, in diesem Fall, dass die Datei geschlossen wird:

```python
with open("myfile.txt") as file:
    for line in file:
        print(line, end="")
```

Nach Ausführung der Anweisung wird die Datei immer geschlossen, auch wenn bei der Verarbeitung der Zeilen ein Problem aufgetreten ist. Mehr Informationen dazu, wie das funktioniert, findet man in der Python Dokumentation im Abschnitt [with-statement context managers](https://docs.python.org/3/reference/datamodel.html#with-statement-context-managers) sowie zum Modul [contextlib](https://docs.python.org/3/library/contextlib.html), wenn man dies selber im eigenen Code nutzen möchte.

## Auslösen und Behandeln mehrerer unabhängiger Ausnahmen

Es gibt Situationen, in denen es notwendig ist, mehrere aufgetretene Ausnahmen zu behandeln. Dies ist oft der Fall, wenn mehrere Programmteile nebenläufig sind, wenn also mehrere Aufgaben parallel fehlgeschlagen sein können. Aber es gibt auch andere Anwendungsfälle, in denen es wünschenswert ist, die Ausführung fortzusetzen und mehrere Fehler zu sammeln, anstatt die erste Ausnahme auszulösen.

Das Built-In `ExceptionGroup` umschließt eine Liste von Exception-Instanzen, sodass sie zusammen ausgelöst werden können. Es ist selbst eine Ausnahme, die wie jede andere Ausnahme abgefangen werden kann:

```pycon
>>> def f():
...     excs = [OSError('error 1'), SystemError('error 2')]
...     raise ExceptionGroup('there were problems', excs)
...
>>> f()
    + Exception Group Traceback (most recent call last):
    |   File "<stdin>", line 1, in <module>
    |   File "<stdin>", line 3, in f
    | ExceptionGroup: there were problems
    +-+---------------- 1 ----------------
      | OSError: error 1
      +---------------- 2 ----------------
      | SystemError: error 2
      +------------------------------------
>>> try:
...     f()
... except Exception as e:
...     print(f'caught {type(e)}: e')
...
caught <class 'ExceptionGroup'>: e
>>>
```

Durch die Verwendung von `except*` anstelle von `except` kann man selektiv nur die Ausnahmen in der Gruppe behandeln, die einem bestimmten Typ entsprechen. Im folgenden Beispiel, das eine verschachtelte Ausnahmegruppe zeigt, extrahiert jede `except*` Klausel Ausnahmen eines bestimmten Typs aus der Gruppe, während alle anderen Ausnahmen an andere Klauseln weitergegeben und schließlich erneut ausgelöst werden:

```pycon
>>> def f():
...     raise ExceptionGroup(
...         "group1",
...         [
...             OSError(1),
...             SystemError(2),
...             ExceptionGroup(
...                 "group2",
...                 [
...                     OSError(3),
...                     RecursionError(4)
...                 ]
...             )
...         ]
...     )
...
>>> try:
...     f()
... except* OSError as e:
...     print("There were OSErrors")
... except* SystemError as e:
...     print("There were SystemErrors")
...
There were OSErrors
There were SystemErrors
    + Exception Group Traceback (most recent call last):
    |   File "<stdin>", line 2, in <module>
    |   File "<stdin>", line 2, in f
    | ExceptionGroup: group1
    +-+---------------- 1 ----------------
      | ExceptionGroup: group2
      +-+---------------- 1 ----------------
        | RecursionError: 4
        +------------------------------------
>>>
```

Zu beachten ist, dass die in einer Ausnahmegruppe verschachtelten Ausnahmen Instanzen und keine Typen sein müssen. Dies liegt daran, dass in der Praxis die Ausnahmen typischerweise diejenigen sind, die bereits ausgelöst und vom Programm abgefangen wurden, nach dem folgendem Muster:

```pycon
>>> excs = []
... for test in tests:
...     try:
...         test.run()
...     except Exception as e:
...         excs.append(e)
...
>>> if excs:
...    raise ExceptionGroup("Test Failures", excs)
...
```

## Notizen zu Ausnahmen hinzufügen

Wenn eine Ausnahme erstellt wird, um ausgelöst zu werden, wird sie normalerweise mit Informationen initialisiert, die den aufgetretenen Fehler beschreiben. Es gibt Fälle, in denen es nützlich ist, Informationen hinzuzufügen, nachdem die Ausnahme abgefangen wurde. Zu diesem Zweck haben Ausnahmen eine Methode `add_note(note)`, die eine Zeichenkette akzeptiert und sie zur Liste der Notizen der Ausnahme hinzufügt. Die Standard-Traceback-Wiedergabe enthält alle Notizen in der Reihenfolge, in der sie nach der Exception hinzugefügt wurden:

```pycon
>>> try:
...     raise TypeError('bad type')
... except Exception as e:
...     e.add_note('Add some information')
...     e.add_note('Add some more information')
...     raise
...
Traceback (most recent call last):
    File "<stdin>", line 2, in <module>
TypeError: bad type
Add some information
Add some more information
>>>
```

Wenn man zum Beispiel Ausnahmen in einer Ausnahmegruppe sammelt, möchten man vielleicht Kontextinformationen zu den einzelnen Fehlern hinzufügen. Im Folgenden wird zu jeder Ausnahme in der Gruppe ein Hinweis gegeben, der angibt, wann dieser Fehler aufgetreten ist:

```pycon
>>> def f():
...     raise OSError('operation failed')
...
>>> excs = []
>>> for i in range(3):
...     try:
...         f()
...     except Exception as e:
...         e.add_note(f'Happened in Iteration {i+1}')
...         excs.append(e)
...
>>> raise ExceptionGroup('We have some problems', excs)
    + Exception Group Traceback (most recent call last):
    |   File "<stdin>", line 1, in <module>
    | ExceptionGroup: We have some problems (3 sub-exceptions)
    +-+---------------- 1 ----------------
      | Traceback (most recent call last):
      |   File "<stdin>", line 3, in <module>
      |   File "<stdin>", line 2, in f
      | OSError: operation failed
      | Happened in Iteration 1
      +---------------- 2 ----------------
      | Traceback (most recent call last):
      |   File "<stdin>", line 3, in <module>
      |   File "<stdin>", line 2, in f
      | OSError: operation failed
      | Happened in Iteration 2
      +---------------- 3 ----------------
      | Traceback (most recent call last):
      |   File "<stdin>", line 3, in <module>
      |   File "<stdin>", line 2, in f
      | OSError: operation failed
      | Happened in Iteration 3
      +------------------------------------
>>>
```

***

 * nächstes Kapitel: [Klassen](classes.md)
 * vorheriges Kapitel: [Eingabe und Ausgabe](inputoutput.md)
