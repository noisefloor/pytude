# Module

Wird der Python-Interpreter verlassen und erneut aufgerufen, gehen die vorgenommenen Definitionen (Funktionen, Variablen
etc.) verloren. Schreibt man ein etwas längeres Programm, so ist es besser einen Texteditor zu verwenden, um die Eingabe
für den Interpreter vorzubereiten und diesen stattdessen mit der Programmdatei als Eingabe zu starten. Dies wird als
Erstellen eines Skripts bzw. Programms bezeichnet (Anmerkung zur Vermeidung von Missverständnissen: Python-Skript und
Python-Programm sind synonym, also dasselbe: eine Datei, die Python Code enthält). Wenn das Programm länger wird, möchte
man es vielleicht in mehrere Dateien aufteilen, um die Wartung und Pflege zu erleichtern. Vielleicht möchte man auch
eine praktische Funktion, die man geschrieben hat, in mehreren Programmen verwenden, ohne den Code in jedes Programm
hinein kopieren zu müssen.

Um dies zu unterstützen, bietet Python eine Möglichkeit, Definitionen in einer Datei abzulegen und sie in einem Skript
oder in einer interaktiven Instanz des Interpreters zu verwenden. Eine solche Datei wird als *Modul* bezeichnet.
Definitionen aus einem Modul können in anderen Modulen oder in das *Hauptmodul* (die Sammlung von Variablen, auf die man
in einem Skript, das auf der obersten Ebene und im Rechnermodus ausgeführt wird, Zugriff hat) *importiert* werden.

Ein Modul ist eine Datei, die Python-Definitionen und Anweisungen enthält. Der Dateiname ist der Modulname mit der
angehängten Endung `.py`. Innerhalb eines Moduls ist der Name des Moduls (als String) als Wert der globalen
Variable `__name__` verfügbar, welche automatisch von Python beim Import des Moduls angelegt wird.

Im Folgenden wird eine Datei namens `fibo.py` angelegt, welche dann als Modul importiert wird. Die Datei kann man mit
seinem bevorzugten Texteditor mit folgendem Inhalt füllen:

```python
# Fibonacci numbers module

def fib(n):
    '''write Fibonacci series up to n'''

    a, b = 0, 1
    while a < n:
        print(a, end=' ')
        a, b = b, a + b
    print()


def fib2(n):
    '''return Fibonacci series up to n'''

    result = []
    a, b = 0, 1
    while a < n:
        result.append(a)
        a, b = b, a + b
    return result
```

Jetzt öffnet man den Python Interpreter in demselben Verzeichnis, in dem die Datei liegt, und führt den folgenden Befehl
im Interpreter aus:

```python
>> > import fibo
>> >
```

Wenn der Import erfolgreich war, ist man einfach erneut am Prompt und könnte jetzt auf die importieren Funktionen
zugreifen.

Was beim Import passiert: Es wird der Modulname `fibo` im aktuelle Namensraum des Python Interpreter eingefügt. 
Die beiden Funktionsnamen aus dem `fibo` Modul werden nicht eingefügt, jedenfalls nicht direkt. 
Mehr zum Thema Namensräume (auf Englisch: namespace) sind in der Python Dokumentation im
Abschnitt [namespace](https://docs.python.org/3/glossary.html#term-namespace) zu finden. Mehr Informationen zu
Namensräumen und Gültigkeiten in Namensräumen sind im Kapitel [Klassen](classes.md) in diesem Tutorial zu finden.

Über den Modulnamen kann man auf die Funktionen zugreifen:

```pycon
>>> fibo.fib(1000)   #ruft die Funktion fib auf dem fibo Modul mit einem Wert von aus 1000 auf
0 1 1 2 3 5 8 13 21 34 55 89 144 233 377 610 987
>>> fibo.fib2(100)   #ruft die Funktion fib2 auf dem fibo Modul mit einem Wert von aus 100 auf
[0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89]
>>> fibo.__name__    # gibt den Name des Modul fibo aus
'fibo'
```

Wenn man eine Funktion häufig verwenden möchten, kann man ihr einen lokalen Namen zuweisen - was aber eher selten
notwendig ist:

```pycon
>>> fib = fibo.fib
>>> fib(500)
0 1 1 2 3 5 8 13 21 34 55 89 144 233 377
```

Ohne zu weit in die Tiefe zu gehen sei hier noch erwähnt, dass man nicht nur Funktionen wie im obigen Beispiel
importieren kann, sondern alle Objekte, die auf oberster Ebene in der Datei stehen. Hätte man z.B. eine Datei `demo.py`
mit folgendem Inhalt

```python
THE_ANSWER = 42


def say_hello_world():
    print('Hello World')


class EmptyClass:
    pass
```

kann man nach einem Import der Datei auf die Variable `THE_ANSWER`, die Funktion `say_hello_world` und die
Klasse `EmptyClass` zugreifen:

```pycon
>>> import demo
>>> print(demo.THE_ANSWER)
42
>>> demo.say_hello_world()
Hello World
>>> nothing = EmptyClass()
>>> type(nothing)
<class 'demo.EmptyClass'>
```

## mehr zu Modulen

Ein Modul kann sowohl ausführbare Anweisungen als auch Funktionsdefinitionen enthalten. Diese Anweisungen dienen der
Initialisierung des Moduls. Sie werden nur beim *ersten* Auftreten des Modulnamens in einer Import-Anweisung ausgeführt.
Sie werden immer ausgeführt, auch wenn die Datei als Skript ausgeführt wird (dazu später in diesem Kapitel mehr).

Jedes Modul hat seinen eigenen privaten Namensraum, der von allen im Modul definierten Funktionen als Namensraum
verwendet wird. Auf diese Weise kann der Autor eines Moduls globale Variablen im Modul verwenden, ohne sich Gedanken
über versehentliche Konflikte mit den globalen Variablen eines Benutzers zu machen. Andererseits kann man, wenn man
weiß, was man tut, die globalen Variablen eines Moduls mit der gleichen Notation ansprechen, mit der man auf die
Funktionen des Moduls verweist: `Modulname.Funktionsname`.

Module können andere Module importieren. Es ist üblich und gilt als "best practice" (ist aber nicht zwingend
erforderlich), alle `import` Anweisungen an den Anfang eines Moduls (oder Skripts) zu schreiben. Die importierten
Modulnamen werden, wenn sie auf der obersten Ebene eines Moduls (außerhalb von Funktionen oder Klassen) stehen, dem
globalen Namensraum des Moduls hinzugefügt.

Es gibt eine Variante der `import` Anweisung, die Namen aus einem Modul direkt in den Namensraum des importierenden
Moduls importiert. Im Folgenden Beispiel werden die Funktionen `fib` und `fib2` aus dem Moduel `fibo` importiert:

```pycon
>>> from fibo import fib, fib2
>>> fib(500)
0 1 1 2 3 5 8 13 21 34 55 89 144 233 377
```

Dadurch wird der Modulname, von dem die Importe übernommen werden, nicht in den lokalen Namensraum eingefügt. Im
Beispiel ist also `fibo` nicht definiert. Ein vorangestelltes `fibo` um auf die Funktionen `fib` und `fib2` zuzugreifen
ist also nicht notwendig.

Es gibt auch eine Variante, um alle Namen zu importieren, die ein Modul definiert:

```pycon
>>> from fibo import *
>>> fib(500)
>>> 0 1 1 2 3 5 8 13 21 34 55 89 144 233 377
```

Dies importiert alle Namen außer denen, die mit einem Unterstrich (`_`) beginnen. In den meisten Fällen benutzen
Python-Programmierer diese Möglichkeit nicht, da sie eine unbekannte Menge von Namen in Namensraum einbinden. Dadurch
besteht das Risiko, dass bereits bestehend Objekte mit gleichem Namen überschrieben werden. Außerdem ist dann oft
unklar, wo ein Name herkommt, was zu schlecht lesbarem Code führt. Von daher gelten diese *Sternchen Importe* als
schlechter Stil und werden in vielen Fällen vermieden.

Folgt auf dem Modulnamen das Schlüsselwort `as`, wird der Name, der auf `as` folgt, direkt an das importierte Modul
gebunden. Oder anders ausgedrückt: im Namenraum, in den das Modul importiert wird, wird das Modul umbenannt (=an einen
anderen Namen gebunden):

```pycon
>>> import fibo as fib
>>> fib.fib(500)
0 1 1 2 3 5 8 13 21 34 55 89 144 233 377
```

Damit wird das Modul auf die gleiche Weise importiert wie mit `import fibo`. Der einzige Unterschied ist, dass es
als `fib` verfügbar ist. Dies kann auch verwendet werden, wenn man `from` benutzt:

```pycon
>>> from fibo import fib as fibonacci
>>> fibonacci(500)
0 1 1 2 3 5 8 13 21 34 55 89 144 233 377
```

Aus Effizienzgründen wird jedes Modul nur einmal pro Interpreter-Sitzung importiert. Wenn man also sein Modul ändert,
muss man den Interpreter neu starten - oder, wenn es nur ein Modul ist, das man interaktiv testen will, verwendet
man `importlib.reload`, z.B. `import importlib;importlib.reload(modulname)`.

### Module als Skript ausführen

Wenn man ein Python-Modul wie folgt aufruft

```shell
python3 scriptname.py <arguments>
```

wird - sofern vorhanden - ausführbarer Code auf oberster Ebene des Moduls ausgeführt. Beispiel am folgenden Skript
names `square.py`:

```python
import sys


def square(number):
    '''Returns the square of a given number'''

    return number ** 2


if len(sys.argv) > 1:
    number = sys.argv[1]
else:
    number = 10
    print(f'No argument provided - using {number} as default')
result = square(number)
print(f'The square of {number} is {result}')
```

Wird das Skript wie folgt aufgerufen

```shell
python square.py 5
```

liefert es das Quadrat von 5 zurück. Würde man keine Zahl als Argument angeben, würde das Skript mit 10 als Defaultwert
rechnen.

Der Code ab `if len(...)` steht auf oberster Ebene des Moduls und wird somit auch beim Import des Skripts ausgeführt:

```pycon
>>> import square
No argument provided - using 10 as default
The square of 10 is 100
>>>
```

Die will man in der Regel aber nicht. Verhindern kann man es, in dem man den Code wie folgt ergänzt:

```python
import sys


def square(number):
    '''Returns the square of a given number'''

    return number ** 2


if __name__ == "__main__":
    if len(sys.argv) > 1:
        number = sys.argv[1]
    else:
        number = 10
        print(f'No argument provided - using {number} as default')
    result = square(number)
    print(f'The square of {number} is {result}')
```

Führt man ein Skript aus, wird `__name__` auf `"__main__"` gesetzt (zur Erinnerung: beim Import als Modul ist `__name__`
gleich dem Dateinamen des Skripts). Dies wird mit der `if` Bedingung geprüft. Wenn `__name__` gleich `"__main__"` ist,
wird der folgende Code ausgeführt. Bei einem Import ist das dann nicht der Fall. So kann man die Datei sowohl als Skript
als auch als importierbares Modul verwenden:

```pycon
>>> import square
>>>
```

Dies wird häufig verwendet, um eine Benutzeroberfläche für ein Modul bereitzustellen oder zu Testzwecken (wenn das Modul
als Skript ausgeführt wird, wird eine Testsuite ausgeführt).

### Suchpfad für Module

Wenn ein Modul namens `spam` importiert wird, sucht der Interpreter zuerst nach einem eingebauten Modul mit diesem
Namen. Diese Modulnamen sind in `sys.builtin_module_names` aufgelistet (*Hinweis*: dies sind *nicht* alle Module, die
eine Python Standardinstallation mitbringt, sondern deutlich weniger!). Wenn es dort nicht gefunden wird, sucht der
Interpreter nach einer Datei mit dem Namen `spam.py` in einer Liste von Verzeichnissen, die durch die
Variable `sys.path` angegeben wird. `sys.path` wird von den folgenden Orten aus initialisiert:

* Das Verzeichnis, das das Eingabeskript enthält (oder das aktuelle Verzeichnis, wenn keine Datei angegeben ist).
* Die Umgebungsvariable [PYTHONPATH](https://docs.python.org/3/using/cmdline.html#envvar-PYTHONPATH) (eine Liste von
  Verzeichnisnamen, mit der gleichen Syntax wie die Shell-Variable `PATH`).
* Die installationsabhängige Voreinstellung (per Konvention einschließlich eines `site-packages`-Verzeichnisses, das
  vom [site](https://docs.python.org/3/library/site.html#module-site) Modul verwaltet wird).

Weiterführende Informationen sind in der Python-Dokumentation
unter [The initialization of the sys.path module search path](https://docs.python.org/3/library/sys_path_init.html#sys-path-init)
zu finden.

*Hinweis*: Auf Dateisystemen, die Symlinks unterstützen, wird das Verzeichnis, das das Eingabeskript enthält, bestimmt,
nachdem der Symlink verfolgt wurde. Mit anderen Worten: Das Verzeichnis, das den Symlink enthält, wird *nicht* zum
Modulsuchpfad hinzugefügt.

Nach der Initialisierung können Python-Programme `sys.path` ändern. Das Verzeichnis, das das auszuführende Skript
enthält, wird an den Anfang des Suchpfades gesetzt, noch vor dem Pfad der Standardbibliothek. Das bedeutet, dass Skripte
in diesem Verzeichnis anstelle von gleichnamigen Modulen im Bibliotheksverzeichnis geladen würden. Das führt in der
Regel zu verwirrenden Fehlermeldungen, gerade bei Einsteigern. Von daher sollte man seinen Skripten immer eigenständige
Namen geben, die nicht identisch mit den Namen von Standardmodulen bzw. zu importierenden Modulen sind.

### kompilierte Python Dateien

Um das Laden von Modulen zu beschleunigen, speichert Python ein zu Python Bytecode kompilierte Version jedes Moduls
im `__pycache__` Verzeichnis unter dem Namen `module.{version}.pyc`, wobei die Version das Format der kompilierten Datei
kodiert. Sie enthält im Allgemeinen die Python-Versionsnummer. Zum Beispiel würde in CPython 3.9 die kompilierte Version
von spam.py als `__pycache__/spam.cpython-39.pyc` zwischengespeichert werden. Diese Namenskonvention ermöglicht die
Koexistenz von kompilierten Modulen aus verschiedenen Releases und verschiedenen Python-Versionen.

Python prüft das Änderungsdatum des Quelltextes mit der kompilierten Version, um festzustellen, ob sie veraltet ist und
neu kompiliert werden muss. Dies ist ein völlig automatischer Prozess. Außerdem sind die kompilierten Module
plattformunabhängig, sodass dieselbe Bibliothek von Systemen mit unterschiedlichen Architekturen gemeinsam genutzt
werden kann.

Python überprüft den Cache in zwei Fällen nicht. Erstens kompiliert es immer neu und speichert das Ergebnis nicht für
das Modul, das direkt von der Kommandozeile geladen wird. Zweitens prüft es den Cache nicht, wenn es kein Quellmodul
gibt. Um eine Nicht-Quellcode-Distribution (nur kompiliert) zu unterstützen, muss sich das kompilierte Modul im
Quellcode-Verzeichnis befinden, und es darf kein Quellcode-Modul vorhanden sein.

Einige Expertentipps:

* Man kann die Optionen `-O` oder `-OO` auf dem Python-Befehl verwenden, um die Größe eines kompilierten Moduls zu
  reduzieren. Der Schalter `-O` entfernt `assert` Anweisungen, der Schalter `-OO` entfernt sowohl Assert-Anweisungen als
  auch `__doc__` Zeichenketten. Da einige Programme darauf angewiesen sind, dass diese zur Verfügung stehen, sollte man
  diese Option nur verwenden, wenn man weiß, was man tut. "Optimierte" Module haben ein `opt-` Tag und sind
  normalerweise kleiner. Zukünftige Versionen könnten die Auswirkungen der Optimierung ändern.
* Ein Programm läuft nicht schneller, wenn es aus einer `.pyc` Datei anstelle einer `.py` Datei gelesen wird. Das
  Einzige, was an `.pyc` Dateien schneller ist, ist die Geschwindigkeit, mit der sie geladen werden.
* Das Modul [compileall](https://docs.python.org/3/library/compileall.html#module-compileall) kann .pyc Dateien für alle
  Module in einem Verzeichnis erstellen.
* Es gibt mehr Details über diesen Prozess, einschließlich Entscheidungsbaums, in
  der [PEP3147](https://peps.python.org/pep-3147/).

## Standard Module

Python wird mit einer relativ umfangreichen Bibliothek von Standardmodulen ausgeliefert, die in
der [Python Library Reference](https://docs.python.org/3/library/index.html) beschrieben sind. Einige Module sind in
dem Interpreter eingebaut. Diese ermöglichen den Zugriff auf Operationen, die nicht Teil des Kerns der Sprache sind, 
aber dennoch eingebaut sind, entweder aus Gründen der Effizienz oder um den Zugriff auf Betriebssystemprimitive wie
Systemaufrufe zu ermöglichen.

Die Menge solcher Module ist eine Konfigurationsoption, die auch von der zugrunde liegenden Plattform abhängt. Das
Modul `winreg` ist beispielsweise nur auf Windows-Systemen verfügbar. Ein Modul verdient besondere
Aufmerksamkeit: [sys](https://docs.python.org/3/library/sys.html#module-sys), das in jeden Python-Interpreter eingebaut
ist. Die Variablen `sys.ps1` und `sys.ps2` definieren beispielsweise die Zeichenketten, die als primäre und sekundäre
Prompts verwendet werden. Diese beiden Variablen sind nur allerdings definiert, wenn sich der Interpreter im
interaktiven Modus befindet.

```pycon
>>> import sys
>>> sys.ps1
'>>> '
>>> sys.ps2
'... '
>>> sys.ps1 = 'C> '
C> print('Yuck!')
Yuck!
C>
```

Die Variable `sys.path` ist eine Liste von Strings, die den Suchpfad des Interpreters für Module bestimmt. Sie wird mit
einem Standardpfad initialisiert, der aus der Umgebungsvariablen `PYTHONPATH` entnommen wird, oder aus einer eingebauten
Vorgabe, wenn `PYTHONPATH` nicht gesetzt ist. Man kann ihn mit Standard-Listenoperationen ändern:

```pycon
>>> import sys
>>> sys.path.append('/ufs/guido/lib/python')
```

## die dir() Funktion

Die eingebaute Funktion [dir](https://docs.python.org/3/library/functions.html#dir) wird verwendet, um herauszufinden,
welche Namen ein Modul definiert. Sie gibt eine sortierte Liste von Namen innerhalb des Moduls zurück:

```pycon
>>> import fibo, sys
>>> dir(fibo)
['__name__', 'fib', 'fib2']
>>> dir(sys)  # doctest: +NORMALIZE_WHITESPACE
['__breakpointhook__', '__displayhook__', '__doc__', '__excepthook__',
'__interactivehook__', '__loader__', '__name__', '__package__', '__spec__',
'__stderr__', '__stdin__', '__stdout__', '__unraisablehook__',
'_clear_type_cache', '_current_frames', '_debugmallocstats', '_framework',
'_getframe', '_git', '_home', '_xoptions', 'abiflags', 'addaudithook',
'api_version', 'argv', 'audit', 'base_exec_prefix', 'base_prefix',
'breakpointhook', 'builtin_module_names', 'byteorder', 'call_tracing',
'callstats', 'copyright', 'displayhook', 'dont_write_bytecode', 'exc_info',
'excepthook', 'exec_prefix', 'executable', 'exit', 'flags', 'float_info',
'float_repr_style', 'get_asyncgen_hooks', 'get_coroutine_origin_tracking_depth',
'getallocatedblocks', 'getdefaultencoding', 'getdlopenflags',
'getfilesystemencodeerrors', 'getfilesystemencoding', 'getprofile',
'getrecursionlimit', 'getrefcount', 'getsizeof', 'getswitchinterval',
'gettrace', 'hash_info', 'hexversion', 'implementation', 'int_info',
'intern', 'is_finalizing', 'last_traceback', 'last_type', 'last_value',
'maxsize', 'maxunicode', 'meta_path', 'modules', 'path', 'path_hooks',
'path_importer_cache', 'platform', 'prefix', 'ps1', 'ps2', 'pycache_prefix',
'set_asyncgen_hooks', 'set_coroutine_origin_tracking_depth', 'setdlopenflags',
'setprofile', 'setrecursionlimit', 'setswitchinterval', 'settrace', 'stderr',
'stdin', 'stdout', 'thread_info', 'unraisablehook', 'version', 'version_info',
'warnoptions']
```

Ohne Argumente listet `dir` die Namen auf, die derzeit definiert sind:

```pycon
>>> a = [1, 2, 3, 4, 5]
>>> import fibo
>>> fib = fibo.fib
>>> dir()
['__builtins__', '__name__', 'a', 'fib', 'fibo', 'sys']
```

Zu beachten ist, dass hier alle Arten von Namen aufgeführt sind: Variablen, Module, Funktionen usw. `dir(Modulname)`
kann auch praktisch sein, wenn man ein Modul importiert hat, aber vergessen hat, welche Funktionen, Variablen etc darin
eigentlich entahlten sind.

In `dir` sind die Namen der eingebauten Funktionen und Variablen nicht aufgeführt. Wenn man eine Liste dieser Funktionen
und Variablen wünscht, muss man das Standardmodul `builtins` importieren und dafür `dir()` aufrufen:

```pycon
>>> import builtins
>>> dir(builtins)  # doctest: +NORMALIZE_WHITESPACE
['ArithmeticError', 'AssertionError', 'AttributeError', 'BaseException',
'BlockingIOError', 'BrokenPipeError', 'BufferError', 'BytesWarning',
'ChildProcessError', 'ConnectionAbortedError', 'ConnectionError',
'ConnectionRefusedError', 'ConnectionResetError', 'DeprecationWarning',
'EOFError', 'Ellipsis', 'EnvironmentError', 'Exception', 'False',
'FileExistsError', 'FileNotFoundError', 'FloatingPointError',
'FutureWarning', 'GeneratorExit', 'IOError', 'ImportError',
'ImportWarning', 'IndentationError', 'IndexError', 'InterruptedError',
'IsADirectoryError', 'KeyError', 'KeyboardInterrupt', 'LookupError',
'MemoryError', 'NameError', 'None', 'NotADirectoryError', 'NotImplemented',
'NotImplementedError', 'OSError', 'OverflowError',
'PendingDeprecationWarning', 'PermissionError', 'ProcessLookupError',
'ReferenceError', 'ResourceWarning', 'RuntimeError', 'RuntimeWarning',
'StopIteration', 'SyntaxError', 'SyntaxWarning', 'SystemError',
'SystemExit', 'TabError', 'TimeoutError', 'True', 'TypeError',
'UnboundLocalError', 'UnicodeDecodeError', 'UnicodeEncodeError',
'UnicodeError', 'UnicodeTranslateError', 'UnicodeWarning', 'UserWarning',
'ValueError', 'Warning', 'ZeroDivisionError', '_', '__build_class__',
'__debug__', '__doc__', '__import__', '__name__', '__package__', 'abs',
'all', 'any', 'ascii', 'bin', 'bool', 'bytearray', 'bytes', 'callable',
'chr', 'classmethod', 'compile', 'complex', 'copyright', 'credits',
'delattr', 'dict', 'dir', 'divmod', 'enumerate', 'eval', 'exec', 'exit',
'filter', 'float', 'format', 'frozenset', 'getattr', 'globals', 'hasattr',
'hash', 'help', 'hex', 'id', 'input', 'int', 'isinstance', 'issubclass',
'iter', 'len', 'license', 'list', 'locals', 'map', 'max', 'memoryview',
'min', 'next', 'object', 'oct', 'open', 'ord', 'pow', 'print', 'property',
'quit', 'range', 'repr', 'reversed', 'round', 'set', 'setattr', 'slice',
'sorted', 'staticmethod', 'str', 'sum', 'super', 'tuple', 'type', 'vars',
'zip']
```

## Pakete

Pakete sind eine Möglichkeit, den Modul-Namensraum von Python durch die Verwendung von "gepunkteten Modulnamen" zu
strukturieren. Zum Beispiel bezeichnet der Modulname `A.B` ein Submodul namens `B` in einem Paket namens `A`. Genauso
wie die Verwendung von Modulen die Autoren verschiedener Module davor bewahrt, sich um die Namen der globalen Variablen
des jeweils anderen kümmern zu müssen, erspart die Verwendung von gepunkteten Modulnamen den Autoren von Paketen mit
mehreren Modulen wie NumPy oder Pillow, sich um die Namen der Module des jeweils anderen kümmern zu müssen.

Angenommen, man will eine Sammlung von Modulen (ein "Paket") für die einheitliche Handhabung von Tondateien und Tondaten
entwerfen. Es gibt viele verschiedene Tondateiformate (in der Regel an der Dateierweiterung zu erkennen, z.
B. `.wav`, `.aiff`, `.mp3`), sodass man möglicherweise eine wachsende Sammlung von Modulen für die Konvertierung
zwischen den verschiedenen Dateiformaten erstellen und pflegen muss. Außerdem gibt es viele verschiedene Operationen,
die man mit den Klangdaten durchführen möchte (z. B. Abmischen, Hinzufügen von Echo, Anwenden einer Equalizer-Funktion,
Erzeugen eines künstlichen Stereoeffekts usw.), sodass man zusätzlich eine nicht enden wollenden Anzahl von Modulen zur
Durchführung dieser Operationen schreiben müsste. Hier ist eine mögliche Struktur für das Paket (ausgedrückt in Form
eines hierarchischen Dateisystems):

```
   sound/                          Top-level package
         __init__.py               Initialize the sound package
         formats/                  Subpackage for file format conversions
                 __init__.py
                 wavread.py
                 wavwrite.py
                 aiffread.py
                 aiffwrite.py
                 auread.py
                 auwrite.py
                 ...
         effects/                  Subpackage for sound effects
                 __init__.py
                 echo.py
                 surround.py
                 reverse.py
                 ...
         filters/                  Subpackage for filters
                 __init__.py
                 equalizer.py
                 vocoder.py
                 karaoke.py
                 ...
```

Beim Importieren des Pakets durchsucht Python die Verzeichnisse in `sys.path` auf der Suche nach dem Unterverzeichnis
des Pakets.

Die `__init__.py`-Dateien werden benötigt, damit Python Verzeichnisse, die die Datei enthalten, als Pakete behandelt (es
sei denn, es wird ein "Namensraumpaket" verwendet, eine relativ fortgeschrittene Funktion). Dies verhindert, dass
Verzeichnisse mit einem gemeinsamen Namen, wie `string`, ungewollt gültige Module verstecken, die später auf dem
Modulsuchpfad auftauchen. Im einfachsten Fall kann `__init__.py` einfach eine leere Datei sein, aber sie kann auch
Initialisierungscode für das Paket ausführen oder die Variable `__all__` setzen, die später beschrieben wird.

Benutzer des Pakets können einzelne Module aus dem Paket importieren, wie zum Beispiel:

```python
import sound.effects.echo
```

Dies lädt das Submodul `sound.effects.echo`. Es muss mit seinem vollen Namen referenziert werden:

```python
sound.effects.echo.echofilter(input, output, delay=0.7, atten=4)
```

Ein alternativer Weg zum Importieren von Submodulen ist:

```python
from sound.effects import echo
```

Dies lädt auch das Submodul `echo` und macht es ohne sein Paketpräfix verfügbar, sodass es wie folgt verwendet werden
kann:

```python
echo.echofilter(input, output, delay=0.7, atten=4)
```

Eine andere Variante ist, die gewünschte Funktion oder Variable direkt zu importieren:

```python
from sound.effects.echo import echofilter
```

Dies lädt wiederum das Submodul `echo`, macht aber dessen Funktion `echofilter` direkt verfügbar:

```python
echofilter(input, output, delay=0.7, atten=4)
```

Zu beachten ist, dass bei der Verwendung von `from package import item` das Element entweder ein Untermodul (oder
Unterpaket) des Pakets sein kann oder ein anderer im Paket definierter Name, wie eine Funktion, Klasse oder Variable.
Die `import` Anweisung testet zuerst, ob das Element im Paket definiert ist. Wenn nicht, nimmt sie an, dass es ein Modul
ist und versucht es zu laden. Wenn es nicht gefunden wird, wird eine `ImportError` Ausnahme ausgelöst.

Im Gegensatz dazu muss bei einer Syntax wie `import item.subitem.subsubitem` jedes Element außer dem letzten ein Paket
sein. Das letzte Element kann ein Modul oder ein Paket sein, aber keine Klasse, Funktion oder Variable, die im
vorherigen Element definiert ist.

### Sternchen-Importe aus einem Paket

Was passiert nun, wenn der Benutzer `from sound.effects import *` schreibt? Idealerweise würde man hoffen, dass Python
irgendwie das Dateisystem durchsucht und herausfindet, welche Untermodule im Paket vorhanden sind und sie alle
importiert. Dies könnte sehr lange dauern und der Import von Untermodulen könnte unerwünschte Nebeneffekte haben, die
nur auftreten sollten, wenn das Untermodul explizit importiert wird.

Die Lösung besteht darin, dass der Paketautor einen expliziten Index des Pakets bereitstellt. Die `import` Anweisung
verwendet die folgende Konvention: Wenn der `__init__.py` Code eines Pakets eine Liste namens `__all__` definiert, wird
diese als die Liste der Modulnamen angesehen, die importiert werden sollen, wenn `from Package import *` aufgerufen
wird. Es ist Sache des Paketautors, diese Liste aktuell zu halten, wenn eine neue Version des Pakets veröffentlicht
wird. Paketautoren können auch entscheiden, dies nicht zu unterstützen, wenn sie keinen Nutzen für den Import von
Sternchen-Importe aus ihrem Paket sehen. Zum Beispiel könnte die Datei `sound/effects/__init__.py` den folgenden Code
enthalten:

```python
__all__ = ["echo", "surround", "reverse"]
```

Das würde bedeuten, dass `from sound.effects import *` die drei genannten Untermodule des `sound.effects` Pakets
importieren würde.

Zu beachten ist, dass Untermodule durch lokal definierte Namen überschattet werden können. Wenn man zum Beispiel
eine `reverse` Funktion in die Datei `sound/effects/__init__.py` hinzufügt, würde der
Aufruf `from sound.effects import *` nur die beiden Submodule `echo` und `surround` importieren, aber *nicht*
das `reverse` Submodul, da es von der lokal definierten `reverse` Funktion überschattet wird:

```python
__all__ = [
    "echo",  # referenziert die 'echo.py' Datei
    "surround",  # referenziert die 'surround.py' Datei
    "reverse",  # ! referenziert jetzt hier die 'reverse' Funktion !
]


def reverse(msg: str):  # <-- dieser Name überschattet das 'reverse.py' Submodul
    return msg[::-1]  # wenn 'from sound.effects import *' aufgerufen wird
```

Wenn ``__all__`` nicht definiert ist, importiert die Anweisung `from sound.effects import *` *nicht* alle Untermodule
aus dem Paket `sound.effects` in den aktuellen Namensraum. Sie stellt nur sicher, dass das Paket `sound.effects`
importiert wurde (und führt möglicherweise jeglichen Initialisierungscode in `__init__.py` aus) und importiert dann alle
Namen, die im Paket definiert sind. Dies schließt alle Namen ein, die durch `__init__.py` definiert (und Untermodule
explizit geladen) wurden. Es schließt auch alle Untermodule des Pakets ein, die explizit durch vorherige `import`
Anweisungen geladen wurden. Betrachtet man den folgenden Code:

```python
import sound.effects.echo
import sound.effects.surround
from sound.effects import *
```

In diesem Beispiel werden die Module `echo` und `surround` in den aktuellen Namespace importiert, weil sie im
Paket `sound.effects` definiert sind, wenn die Anweisung `from...import` ausgeführt wird. Dies funktioniert auch,
wenn `__all__` definiert ist.

Obwohl bestimmte Module so konzipiert sind, dass sie nur Namen exportieren, die bestimmten Mustern folgen, wenn
Sie `import *` verwenden, wird `import *` nach wie vor als schlechter Stil angesehen und sollte von daher im eigenen
Code vermieden werden.

Aber es ist nicht falsch, `from package import specific_submodule` zu verwenden! Dies ist sogar die empfohlene
Schreibweise, es sei denn, das importierende Modul muss Submodule mit demselben Namen aus verschiedenen Paketen
verwenden.

### Intra-Paket Referenzen

Wenn Pakete in Unterpakete strukturiert sind (wie das Paket `sound` im Beispiel oben), kann man absolute Importe
verwenden, um auf Untermodule von Geschwisterpaketen zu verweisen. Wenn zum Beispiel das Modul `sound.filters.vocoder`
das Modul `echo` aus dem Paket `sound.effects` verwenden muss, kann es `from sound.effects import echo` für den Import
verwenden.

Man kann auch relative Importe schreiben, mit der Form der Import-Anweisung "from module import name". Diese Importe
verwenden führende Punkte, um die aktuellen und übergeordneten Pakete anzuzeigen, die am relativen Import beteiligt
sind. Aus dem Modul `surround` heraus könnte man z.B. verwenden

```python
from . import echo
from .. import formats
from ..filters import equalizer
```

Zu beachten ist, dass sich relative Importe auf den Namen des aktuellen Moduls beziehen. Da der Name des Hauptmoduls
immer `"__main__"` ist, müssen Module, die als Hauptmodul einer Python-Anwendung verwendet werden sollen, immer absolute
Importe verwenden.

### Pakete in mehreren Verzeichnissen

Pakete unterstützen ein weiteres spezielles Attribut, `__path__`. Dieses wird als Liste initialisiert, die den Namen des
Verzeichnisses enthält, das die `__init__.py` des Pakets enthält, bevor der Code in dieser Datei ausgeführt wird. Diese
Variable kann geändert werden. Dies beeinflusst die zukünftige Suche nach Modulen und Unterpaketen, die in dem Paket
enthalten sind.

Obwohl diese Funktion nicht oft benötigt wird, kann sie verwendet werden, um die Menge der in einem Paket gefundenen
Module zu erweitern.

***

* nächstes Kapitel: [Eingabe und Ausgabe](inputoutput.md)
* vorheriges Kapitel: [Datenstrukturen und Datentypen](datastructures.md)
