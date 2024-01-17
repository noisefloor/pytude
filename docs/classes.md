# Klassen

Klassen bieten die Möglichkeit, Daten und Funktionen zu bündeln. Durch das Erstellen einer neuen Klasse wird ein neuer *Typ* eines Objekts erzeugt, so dass neue *Instanzen* dieses Typs erstellt werden können. Jede Klasseninstanz kann mit Attributen versehen werden, um den Zustand der Klasse zu verwalten. Klasseninstanzen können auch über von der Klasse definierte Methoden verfügen, um ihren Zustand zu ändern.

Im Vergleich zu anderen Programmiersprachen fügt der Klassenmechanismus von Python Klassen mit einem Minimum an neuer Syntax und Semantik hinzu. Er ist eine Mischung aus den Klassenmechanismen von C++ und Modula-3. Python-Klassen bieten alle Standardfunktionen der objektorientierten Programmierung: Der Klassenvererbungsmechanismus erlaubt mehrere Basisklassen, eine abgeleitete Klasse kann beliebige Methoden ihrer Basisklasse(n) außer Kraft setzen, und eine Methode kann die Methode einer Basisklasse mit demselben Namen aufrufen. Klassen können beliebige Mengen und Arten von Daten enthalten. Wie bei den Modulen sind auch die Klassen Teil der dynamischen Natur von Python: sie werden zur Laufzeit erstellt und können danach weiter verändert werden.

In der C++-Terminologie sind Klassenmitglieder (einschließlich der Datenmitglieder) normalerweise *öffentlich* (Ausnahmen werden weiter unten in diesem Kapitel erklärt) und alle Mitgliedsfunktionen sind *virtuell*. Wie in Modula-3 gibt es keine Abkürzungen für die Referenzierung von Objektmitgliedern in den Methoden: die Methodenfunktion wird mit einem expliziten ersten Argument deklariert, das das Objekt repräsentiert, das implizit durch den Aufruf bereitgestellt wird. Wie in Smalltalk sind die Klassen selbst Objekte. Dies bietet eine Semantik für das Importieren und Umbenennen. Im Gegensatz zu C++ und Modula-3 können eingebaute Typen als Basisklassen für Erweiterungen durch den Benutzer verwendet werden. Außerdem können, wie in C++, die meisten eingebauten Operatoren mit spezieller Syntax (arithmetische Operatoren, Indexierung usw.) für Klasseninstanzen neu definiert werden.

In Ermangelung einer allgemeingültigen Terminologie für Klassen werden hier gelegentlich Begriffe aus Smalltalk und C++ verwenden. Genauer wäre es, man würde die Begriffe von Modula-3 verwenden, da dessen objektorientierte Semantik näher an der von Python als an der von C++ ist - aber [Modula-3](https://en.wikipedia.org/wiki/Modula-3) gehört zu den - zumindest heutzutage - unbekannten und wenig verwendeten Programmiersprachen.

## ein paar Worte zu Namen und Klassen

Objekte haben Individualität, und mehrere Namen (in mehreren Bereichen) können an dasselbe Objekt gebunden werden. Dies ist in anderen Sprachen als "Aliasing" bekannt. Dies wird in der Regel nicht auf den ersten Blick in Python erkannt und kann beim Umgang mit unveränderlichen Grundtypen (Zahlen, Strings, Tupel) auch ignoriert werden. Das Aliasing hat jedoch eine möglicherweise überraschende Auswirkung auf die Semantik von Python-Code, der veränderliche Objekte wie z.B. Listen, Wörterbücher etc. enthält. Dies wird in der Regel zum Vorteil des Programms genutzt, da sich Aliase in mancher Hinsicht wie Zeiger verhalten. Zum Beispiel ist die Übergabe eines Objekts billig (billig im Sinne von mit minimalem Aufwand verbunden), da nur ein Zeiger von der Implementierung übergeben wird. Und wenn eine Funktion ein als Argument übergebenes Objekt ändert, sieht der Aufrufer die Änderung - dies eliminiert die Notwendigkeit für zwei verschiedene Mechanismen der Argumentübergabe wie in Pascal.

## Python Scopes und Namensräume

Bevor Klassen vorgestellt, muss zunächst etwas über die Scopes von Python erzählen. "Scope" heißt auf Deutsch so viel wie Geltungsbereich oder Anwendungsbereich. Aber auch im deutschen Sprachegebrauch zum Thema Python wird üblicherweise das englische Wort "scope" genutzt.  Klassendefinitionen kennen ein paar Tricks zu Namensräumen (auf Englisch: namespace), und man sollte wissen, wie Scopes und Namensräume funktionieren, um zu verstehen, was vor sich geht. Übrigens ist das Wissen über dieses Thema für jeden fortgeschrittenen Python-Programmierer nützlich.

Zu Beginn ein paar Definitionen:

Ein *Namensraum* (auf Englisch: namespace) ist eine Abbildung von Namen auf Objekte. Die meisten Namensräume sind derzeit bei Python intern als Python-Wörterbücher implementiert, aber ist für den Anwender erstmal sekundär (und könnte sich in zukünftigen Python eventuell ändern, da es ein internes Implementierungsdetail ist). Beispiele für Namensräume sind:

 * die Menge der eingebauten Namen (die Funktionen wie `abs` und eingebaute Ausnahmenamen enthalten)
 * die globalen Namen in einem Modul und die lokalen Namen in einem Funktionsaufruf.
 * In gewissem Sinne bildet auch die Menge der Attribute eines Objekts einen Namensraum.

Eine wichtige Dinge, die man über Namensräume wissen muss, ist, dass es absolut keine Beziehung zwischen Namen in verschiedenen Namensräumen gibt. Zum Beispiel könnten zwei verschiedene Module beide eine Funktion `maximize` definieren, ohne dass es zu Verwechslungen kommt - Benutzer der Module müssen ihr den Modulnamen voranstellen.

Übrigens wird hier das Wort *Attribut* für jeden Namen, der auf einen Punkt folgt genutzt - zum Beispiel in dem Ausdruck `z.real` ist `real` ein Attribut des Objekts `z`. Streng genommen sind Referenzen auf Namen in Modulen Attributreferenzen: in dem Ausdruck `modname.funcname` ist `modname` ein Modulobjekt und `funcname` ist ein Attribut davon. In diesem Fall gibt es eine direkte Zuordnung zwischen den Attributen des Moduls und den globalen Namen, die im Modul definiert sind: sie teilen sich denselben Namensraum. Bis auf eine Ausnahme: Modulobjekte haben ein geheimes Nur-Lese-Attribut namens `object.__dict__`, das das Wörterbuch zurückgibt, welches zur Implementierung des Namensraums des Moduls genutzt wird. Der Name `object.__dict__` ist ein Attribut, aber kein globaler Name. Offensichtlich verletzt die Verwendung dieses Namens die Abstraktion der Namespace-Implementierung und sollte auf Dinge wie Post-Mortem-Debugger beschränkt werden.

Attribute können nur-lesbar oder veränderbar sein. Im letzteren Fall ist eine Zuweisung auf Attributen möglich.  Modulattribute sind veränderbar: man kann z.B. `modname.the_answer = 42` schreiben. Änderbare Attribute können auch mit der Anweisung `del` gelöscht werden. Zum Beispiel wird `del modname.the_answer` das Attribut `the_answer` aus dem Objekt mit dem Namen `modname` entfernen.

Namensräume werden zu unterschiedlichen Zeitpunkten erstellt und haben unterschiedliche Lebensdauern. Der Namensraum, der die eingebauten Namen enthält, wird erstellt, wenn der Python-Interpreter startet, und wird im laufenden Interpreter nie gelöscht. Der globale Namensraum für ein Modul wird erstellt, wenn die Moduldefinition eingelesen wird. Normalerweise bleiben die Modulnamensräume auch bestehen, bis der Interpreter beendet wird. Die Anweisungen, die vom obersten Aufruf des Interpreters ausgeführt werden, entweder aus einer Skriptdatei gelesen oder interaktiv, werden als Teil eines Moduls namens `__main__` betrachtet, so dass sie ihren eigenen globalen Namensraum haben. Die eingebauten Namen befinden sich ebenfalls in einem Modul, dieses heißt `builtins`.

Der lokale Namensraum für eine Funktion wird erstellt, wenn die Funktion aufgerufen wird, und gelöscht, wenn die Funktion zurückkehrt oder eine Ausnahme auslöst, die nicht innerhalb der Funktion behandelt wird. Eigentlich wäre das Wort "Vergessen" genauer, um zu beschreiben, was tatsächlich passiert. Rekursive Aufrufe haben jeweils ihren eigenen lokalen Namensraum.

Ein *Scope* ist ein textueller Bereich (Anmerkung: "textuell" bedeutet hier so viel wie ein Text- / Code-mäßig zusammengehöriger Bereich, z.B ein Funktionskörper) eines Python-Programms, in dem ein Namensraum direkt zugänglich ist. "Direkt zugänglich" bedeutet hier, dass ein unqualifizierter Verweis reicht. "unqualifiziert" bedeutet hier so viel, dass der Name (der Variablen, der Funktion, ...) direkt angesprochen werden kann, ohne dass man das übergeordnete Objekt (wie den Funktionsname, Klassennamen, Modulnamen etc.) vorangestellt werden muss.

Obwohl die Scopes statisch festgelegt werden, werden sie dynamisch verwendet. Zu jedem Zeitpunkt der Ausführung gibt es drei oder vier verschachtelte Bereiche, auf deren Namensraum direkt zugegriffen werden kann:

 * der innerste Bereich, der zuerst durchsucht wird, enthält die lokalen Namen
 * die Bereiche aller umschließenden Funktionen, die beginnend mit dem nächstgelegenen Bereich durchsucht werden, enthalten nicht-lokale, aber auch nicht-globale Namen
 * der vorletzte Bereich enthält die globalen Namen des aktuellen Moduls
 * Der äußerste Bereich (der zuletzt durchsucht wird) ist der Namensraum, der die eingebauten Namen enthält.

Wenn ein Name als global deklariert ist, gehen alle Referenzen und Zuweisungen direkt in den vorletzten Bereich, der die globalen Namen des Moduls enthält. Um Variablen, die sich außerhalb des innersten Bereichs befinden, neu zu binden, kann die Anweisung `nonlocal` verwendet werden. Wenn Variablen nicht als nicht-lokal deklariert sind, sind diese sie schreibgeschützt. Ein Versuch, in eine solche Variable zu schreiben, erzeugt einfach eine neue lokale Variable im innersten Bereich, wobei die gleichnamige äußere Variable unverändert bleibt.

Normalerweise verweist der lokale Bereich auf die lokalen Namen der textuell aktuellen Funktion. Außerhalb von Funktionen verweist der lokale Bereich auf denselben Namensraum wie der globale Bereich: den Namensraum des Moduls. Klassendefinitionen legen einen weiteren Namensraum in den lokalen Bereich.

Es ist wichtig zu wissen, dass der Geltungsbereich textuell festgelegt wird: der globale Geltungsbereich einer in einem Modul definierten Funktion ist der Namensraum dieses Moduls, unabhängig davon, von wo oder durch welchen Alias die Funktion aufgerufen wird.  Andererseits erfolgt die eigentliche Suche nach Namen dynamisch, zur Laufzeit - die Sprachdefinition entwickelt sich jedoch in Richtung statischer Namensauflösung, zur "Kompilier"-Zeit, also verlassen Sie sich nicht auf dynamische Namensauflösung! Tatsächlich werden lokale Variablen bereits statisch bestimmt.

Eine besondere Eigenart von Python ist, dass - wenn keine `global` oder `nonlocal` Anweisung in Kraft ist - Zuweisungen an Namen immer in den innersten Bereich gehen. Zuweisungen kopieren keine Daten - sie binden nur Namen an Objekte. Das gleiche gilt für Löschungen: die Anweisung `del x` entfernt die Bindung von `x` aus dem Namensraum, der auf den lokalen Bereich verweist. Das Objekt im Speicher wird dadurch aber nicht automatisch mit gelöscht. Das erledigt zu einem späteren Zeitpunkt der Garbage Collector von Python. Tatsächlich verwenden alle Operationen, die neue Namen einführen, den lokalen Bereich: insbesondere `import` Anweisungen und Funktionsdefinitionen binden den Modul- oder Funktionsnamen im lokalen Bereich.

Die Anweisung `global` kann verwendet werden, um anzuzeigen, dass bestimmte Variablen im globalen Bereich liegen und dorthin zurückgebunden werden sollen. Die Anweisung `nonlocal` zeigt an, dass bestimmte Variablen in einem umschließenden Bereich liegen und dorthin zurückgebunden werden sollen. Allerdings ist die Verwendung von `global` und `nonlocal` in realem Programmcode sehr selten notwendig und wird oft falsch (falsch im Sinne von überflüssig und stilistisch schlecht) verwendet.

### Beispiele für Scopes und Namensräume

Das folgende Beispiel, das zeigt, wie man auf die verschiedenen Bereiche und Namensräume verweist und wie sich `global` und `nonlocal` auf die Variablenbindung auswirken:

```python
def scope_test():
    def do_local():
        spam = "local spam"

    def do_nonlocal():
        nonlocal spam
        spam = "nonlocal spam"

    def do_global():
        global spam
        spam = "global spam"

    spam = "test spam"
    do_local()
    print("After local assignment:", spam)
    do_nonlocal()
    print("After nonlocal assignment:", spam)
    do_global()
    print("After global assignment:", spam)

scope_test()
print("In global scope:", spam)
```

Die Ausgabe ist wie folgt:

```
After local assignment: test spam
After nonlocal assignment: nonlocal spam
After global assignment: nonlocal spam
In global scope: global spam
```

Zu beachten ist, dass die Zuweisung *local* (die Standardeinstellung) die Bindung von *scope_test* an *spam* nicht verändert hat.  Die `nonlocal` Zuweisung änderte die Bindung von *scope_test* an *spam*, und die `global` Zuweisung änderte die Bindung auf Modulebene. Man kann auch sehen, dass es vor der Zuweisung von `global` keine vorherige Bindung für *spam* gab.

Das folgende Beispiel zeigt, wann auf welchen Namensraum zugegriffen wird:

```python
a = 27
print("global", a)

class C:
    print("in C", a)   # a ist in C nicht definiert, Zugriff auf den höheren, globalen Namensraum
    a = 42
    print("in C", a)   # a ist jetzt im Scop von C definiert

    class Inner:
        print("in Inner", a)   # a ist in c.Inner nicht definiert, Zugriff auf den höheren Namensraum von C
        a = 99
        print("in Inner", a)

    print("in C", a)  # Zugriff au den Namensraum von C

print("global", a)   # Zugriff auf den globalen Namensraum
```

Lässt man den Code laufen, erhält man folgende Ausgabe:

```
global 27
in C 27
in C 42
in Inner 27
in Inner 99
in C 42
global 27
```

## Ein erster Blick auf Klassen

Klassen führen ein wenig neue Syntax, drei neue Objekttypen und einige neue Semantiken ein.

### Syntax zur Definition von Klassen

Die einfachste Definition einer Klasse sieht wiefolgt aus:

```python
class ClassName:
    <statement-1>
    .
    .
    .
    <statement-N>
```

Klassendefinitionen müssen vom Prinzip wie Funktionsdefinitionen ausgeführt werden. Dies geschieht mit dem Schlüsselwort `class` gefolgt vom Klassenamen. Klassennamen werden in Python per Konvention PascalCase Schreibweise geschrieben, als wie z.B. `Vehicle`, `ClassName`, `RequestDispatcher` oder `BarGraphCreator`. Grundsätzlich ist es möglich, eine Klassendefinition in einem Zweig einer `if`-Anweisung oder innerhalb einer Funktion zu platzieren- aber das ist extrem unüblich und wird selten benötigt.

In der Praxis sind die Anweisungen innerhalb einer Klassendefinition in der Regel Funktionsdefinitionen, aber auch andere Anweisungen sind erlaubt und manchmal nützlich - dazu später mehr. Die Funktionsdefinitionen innerhalb einer Klasse haben normalerweise eine besondere Form der Argumentliste, die von den Aufrufkonventionen für Methoden vorgegeben wird - auch dies wird später erklärt.

Wenn eine Klassendefinition eingegeben wird, wird ein neuer Namensraum erstellt und als lokaler Bereich verwendet - alle Zuweisungen an lokale Variablen gehen also in diesen neuen Namensraum. Insbesondere Funktionsdefinitionen binden hier den Namen der neuen Funktion.

Wenn eine Klassendefinition normal verlassen wird (über das Ende), wird ein *Klassenobjekt* erstellt. Dabei handelt es sich im Grunde um eine Hülle um den Inhalt des durch die Klassendefinition geschaffenen Namensraums. Im nächsten Abschnitt wird mehr über Klassenobjekte erklärt. Der ursprüngliche lokale Gültigkeitsbereich (derjenige, der unmittelbar vor der Eingabe der Klassendefinition in Kraft war) wird wiederhergestellt, und das Klassenobjekt wird hier an den im Kopf der Klassendefinition angegebenen Klassennamen gebunden (wie `ClassName` im obigen Beispiel).

### Klassenobjekte

Klassenobjekte unterstützen zwei Arten von Operationen: Attributreferenzen und Instanziierung.

*Attributreferenzen* verwenden die Standardsyntax, die für alle Attributreferenzen in Python verwendet wird: ``obj.name``.  Gültige Attributnamen sind alle Namen, die sich im Namensraum der Klasse befanden, als das Klassenobjekt erstellt wurde.  Wenn also die Klassendefinition wie folgt aussieht:

```python
class SomethingSimple:
    """A simple example class"""
    i = 12345

    def f(self):
           return 'hello world'
```

dann sind `SomethingSimple.i` und `SomethingSimple.f` gültige Attributreferenzen, die eine ganze Zahl bzw. ein Funktionsobjekt zurückgeben. Klassenattribute können auch zugewiesen werden, so dass Sie den Wert von `SomethingSimple.i` durch Zuweisung ändern können. `__doc__` ist ebenfalls ein gültiges Attribut, das den zur Klasse gehörenden docstring zurückgibt: "A simple example class".

Bei der *Instantiierung* von Klassen wird die Funktionsnotation verwendet. Man kann sich das einfach so vorstellen, dass das Klassenobjekt sei eine parameterlose Funktion ist, die eine neue Instanz der Klasse zurückgibt. Zum Beispiel unter Annahme der obigen Klasse:

```python
x = SomethingSimple()
```

Dies erzeugt eine neue *Instanz* der Klasse und weist dieses Objekt der lokalen Variablen `x` zu.

Die Instanziierungsoperation ("Aufruf" eines Klassenobjekts) erzeugt ein leeres Objekt. Klassen möchten oft ein Objekt mit Instanzen erzeugen, die an einen bestimmten Anfangszustand haben. Daher kann eine Klasse eine spezielle Methode mit dem Namen `object.__init__` definieren:

```python
   def __init__(self):
       self.data = []
```

Wenn eine Klasse eine Methode `object.__init__` definiert, wird bei der Instanziierung der Klasse automatisch `__init__` für die neu erzeugte Klasseninstanz aufgerufen. In diesem Beispiel kann also eine neue, initialisierte Instanz erzeugt werden durch:

```python
x = SomethingSimple()
```

und der Aufruf von `x.data` würde eine leere Liste zurückgeben.

Natürlich kann die Methode `object.__init__` mehrere Argumente haben und man diese auch beim Instanziieren der Klasse mit Werten versehen. Zum Beispiel:

```pycon
>>> class Complex:
...     def __init__(self, realpart, imagpart):
...         self.r = realpart
...         self.i = imagpart
...
>>> x = Complex(3.0, -4.5)   # repart wird der Wert 3.0, imagpart der Wert -4.5 zugewiesen
>>> x.r, x.i
(3.0, -4.5)
```

Man kann, ähnlich wie bei Funktionen, Argumente auch vorbelegten, so dass die Übergabe von Werten bei der Instanziierung der Klasse optional ist:

```pycon
>>> class Demo:
...     def __init__(self, number=5):
...         self.number=number
...     def square(self):
...         return self.number**2
...
>>> a = Demo()
>>> a.square()
25
>>> b = Demo(3)
>>> b.square()
9
```

Des Weiteren muss für in `__init__` definierte Variablen nicht zwingend ein Wert übergeben werden, es geht z.B. auch

```pycon
>>> class Demo:
...     def __init__(self):
...         self.number=5
...
```

Wie man `Demo.number` dann einen Wert zuweist wird im folgenden Abschnitt erklärt.

### Instanz Objekte

Was kann man nun mit Instanzobjekten tun? Die einzigen Operationen, die von Instanzobjekten verstanden werden, sind Attributreferenzen. Es gibt zwei Arten von gültigen Attributnamen: Datenattribute und Methoden.

*Datenattribute* entsprechen den "Instanzvariablen" in Smalltalk und den "Datenmitgliedern" in C++. Datenattribute müssen nicht deklariert werden, sie können einer Instanz nachträglich hinzugefügt werden. Wie lokale Variablen entstehen sie, wenn sie zum ersten Mal zugewiesen werden. Wenn zum Beispiel `a` die Instanz von `Demo` ist, die oben erstellt wurde, wird der folgende Code den Wert `16` ausgeben:

```pycon
>>> a.counter = 1
>>> while a.counter < 10:
        a.counter = a.counter * 2
>>> print(a.counter)
```

Zum Entfernen kann man `del a.counter` nutzen. Das nachträgliche Hinzufügen kommt in der realem Programmierpraxis eher sehr selten vor, da man in der Regel mit den in der Klasse bereits definierten Datenattributen arbeitet.

Die andere Art der Instanzattribut-Referenz ist eine *Methode*. Eine Methode ist eine Funktion, die zu einem Objekt gehört.  In Python ist der Begriff Methode nicht nur auf Klasseninstanzen beschränkt: auch andere Objekttypen können Methoden haben.  Zum Beispiel haben Listenobjekte Methoden wie `append`, `insert`, `remove` und so weiter. In der folgenden Diskussion wird der Begriff Methode jedoch ausschließlich für Methoden von Klasseninstanzobjekten verwenden, sofern nicht ausdrücklich anders angegeben.

Gültige Methodennamen eines Instanzobjekts hängen von seiner Klasse ab. Per Definition definieren alle Attribute einer Klasse, die Funktionsobjekte sind, entsprechende Methoden ihrer Instanzen. In dem Beispiel weiter oben mit der `SomethingSimple` Klasse ist also `x.f` eine gültige Methodenreferenz, da `SomethingSimple.f` eine Funktion ist, aber `x.i` ist keine, da `SomethingSimple.i` keine ist.  Aber `x.f` ist nicht dasselbe wie `SomethingSimple.f` - es ist ein *Methodenobjekt*, kein Funktionsobjekt.

### Methoden Objekte

Normalerweise wird eine Methode direkt nach ihrer Bindung aufgerufen:

```python
x.f()
```

Im Beispiel mit der Class `SomethingSimple` wird dies die Zeichenkette "hello world" zurückgeben. Es ist jedoch nicht notwendig, eine Methode sofort aufzurufen: `x.f` ist ein Methodenobjekt, das gespeichert und zu einem späteren Zeitpunkt aufgerufen werden kann. Zum Beispiel würde

```pycon
>>> xf = x.f
>>> while True:
...     print(xf())
```

"hello world" in einer Endlosschleife ausgeben.

Was genau passiert, wenn eine Methode aufgerufen wird?  Wie schon zu sehen war kann `x.f()` oben ohne ein Argument aufgerufen werden, obwohl die Funktionsdefinition für `f` ein Argument angibt, nämlich "self".  Was ist mit dem Argument passiert? Python löst ja bekanntlich eine Ausnahme aus, wenn eine Funktion, die ein Argument erwartet, ohne ein solches aufgerufen wird - auch wenn das Argument nicht wirklich genutzt wird...

Vielleicht wurde es bereits erahnt: Das Besondere an Methoden ist, dass das Instanzobjekt als erstes Argument der Funktion übergeben wird. In dem Beispiel oben ist der Aufruf `x.f()` genau äquivalent zu `SomethingSimple.f(x)`. Im Allgemeinen ist der Aufruf einer Methode mit einer Liste von *n* Argumenten äquivalent zum Aufruf der entsprechenden Funktion mit einer Argumentliste, die durch Einfügen des Instanzobjekts der Methode vor dem ersten Argument erstellt wird.

Im Allgemeinen funktionieren die Methoden wie folgt: Wenn ein Nicht-Daten-Attribut einer Instanz referenziert wird, wird die Klasse der Instanz durchsucht. Wenn der Name ein gültiges Klassenattribut angibt, das ein Funktionsobjekt ist, werden Verweise auf das Instanzobjekt und das Funktionsobjekt in ein Methodenobjekt gepackt. Wenn das Methodenobjekt mit einer Argumentliste aufgerufen wird, wird eine neue Argumentliste aus dem Instanzobjekt und der Argumentliste konstruiert, und das Funktionsobjekt wird mit dieser neuen Argumentliste aufgerufen.

Kurzfassung: Funktionen, die innerhalb einer Klasse definiert werden, benötigen grundsätzlich `self` als erstes Argument, auch wenn die Funktion an sich gar keine Argumente benötigt. Ein fehlendes `self` führt sonst zu einem Fehler. Dass das Argument `self` heißt ist übrigens eine Pythonkonvetion - an die man sich aber tunlist halten sollte! Grundsätzlich würde auch jeder andere Name wie z.B. "foo" oder "Python" oder "Wurstwasser" funktionieren, der Name muss innerhalb der Klasse nur einheitlich sein.

Es gibt zwei Ausnahmen, wo `self` nicht als Argument an eine Funktion in einer Klasse übergeben wird: wenn die Funktion mit dem `@classmethod` oder `@staticmethod` dekoriert wird. Darauf wird im Rahmen dieses Tutorials aber nicht weiter eingegangen.

### Klassen- und Instanzvariablen

Im Allgemeinen werden Instanzvariablen für Daten verwendet, die für jede Instanz einzigartig sind, und Klassenvariablen für Attribute und Methoden, die von allen Instanzen der Klasse gemeinsam genutzt werden:

```pycon
>>> class Dog:
...     kind = 'canine'         # Klassenvariable, die sich alle Instanzen teilen
...     def __init__(self, name):
...         self.name = name    # Instanzvariable, individuell für jede Instanz
...
>>> d = Dog('Fido')
>>> e = Dog('Buddy')
>>> d.kind                  # gleich für alle Instanzen von Dog
'canine'
>>> e.kind                  # gleich für alle Instanzen von Dog
'canine'
>>> d.name                  # individuell für d
'Fido'
>>> e.name                  # individuell für e
'Buddy'
```

Wie in im Abschnitt "ein paar Worte zu Namen und Klassen" besprochen, können gemeinsam genutzte Daten möglicherweise überraschende Auswirkungen haben, wenn man `mutable` Objekte wie Listen und Wörterbücher verwendet. Zum Beispiel sollte die ` Liste namens "tricks" im folgenden Code nicht als Klassenvariable verwendet werden, da nur eine einzige Liste von allen Dog-Instanzen gemeinsam genutzt werden würde:

```pycon
>>> class Dog:
...     tricks = []             # falsche Nutzung eine Klassenvariaale
...     def __init__(self, name):
...         self.name = name
...     def add_trick(self, trick):
            self.tricks.append(trick)
...
>>> d = Dog('Fido')
>>> e = Dog('Buddy')
>>> d.add_trick('roll over')
>>> e.add_trick('play dead')
>>> d.tricks                # wird zwischen allen Instanzen geteilt - was nicht richtig ist
['roll over', 'play dead']
```

Bei einem korrekten Entwurf der Klasse sollte stattdessen eine Instanzvariable verwendet werden:

```pycon
>>> class Dog:
...     def __init__(self, name):
...         self.name = name
...         self.tricks = []    # legt eine leere Liste für jeden Hund an
...     def add_trick(self, trick):
...         self.tricks.append(trick)
...
>>> d = Dog('Fido')
>>> e = Dog('Buddy')
>>> d.add_trick('roll over')
>>> e.add_trick('play dead')
>>> d.tricks
['roll over']
>>> e.tricks
['play dead']
```

## zusätzliche Hinweise

Wenn derselbe Attributname sowohl in einer Instanz als auch in einer Klasse vorkommt, dann hat das Instanzattribut Vorrang vor dem der Klasse:

```pycon
>>> class Warehouse:
...    purpose = 'storage'
...    region = 'west'
...
>>> w1 = Warehouse()
>>> print(w1.purpose, w1.region)
storage west
>>> w2 = Warehouse()
>>> w2.region = 'east'
>>> print(w2.purpose, w2.region)
storage east
```

Datenattribute können sowohl von Methoden als auch von normalen Benutzern (=den den Nutzern einer Instanz) eines Objekts referenziert werden. Mit anderen Worten: Klassen sind nicht geeignet, um rein abstrakte Datentypen zu implementieren. Tatsächlich gibt es in Python keine Möglichkeit, Daten zu verstecken - alles basiert auf Konventionen.  Andererseits kann die in C geschriebene Python-Implementierung Implementierungsdetails vollständig verbergen und den Zugriff auf ein Objekt kontrollieren, wenn dies erforderlich ist. Dies kann von in C geschriebenen Python-Erweiterungen genutzt werden.

Nutzer von Instanzen sollten Datenattribute mit Vorsicht verwenden - es können die Invarianten, die von den Methoden aufrechterhalten werden, durch Änderungen auf ihren Datenattributen durcheinanderbringen. Zu beachten ist, dass Nutzer eigene Datenattribute zu einem Instanzobjekt hinzufügen können, ohne die Gültigkeit der Methoden zu beeinträchtigen, solange Namenskonflikte vermieden werden - auch hier kann eine Namenskonvention eine Menge Kopfzerbrechen ersparen.

Es gibt keine Abkürzungen für den Verweis auf Datenattribute (oder andere Methoden) innerhalb von Methoden.  Dies erhöht die Lesbarkeit von Methoden: Es gibt keine Möglichkeit, lokale Variablen und Instanzvariablen zu verwechseln, wenn man eine Methode anschaut.

Jedes Funktionsobjekt, das ein Klassenattribut ist, definiert eine Methode für Instanzen dieser Klasse. Es ist nicht notwendig, dass die Funktionsdefinition textuell in die Klassendefinition eingeschlossen ist: die Zuweisung eines Funktionsobjekts an eine lokale Variable in der Klasse funktioniert ebenfalls. Zum Beispiel:

```python
# Function defined outside the class
def f1(self, x, y):
    return min(x, x+y)

class C:
    f = f1

    def g(self):
        return 'hello world'

    h = g
```

Nun sind `f`, `g` und `h` alles Attribute der Klasse `C`, die sich auf Funktionsobjekte beziehen, und folglich sind sie alle Methoden von Instanzen von `C` - `h` ist genau gleichwertig mit `g`.  Zu beachten ist, dass diese Praxis normalerweise nur dazu dient, den Leser eines Programms zu verwirren - oder anders ausgedrückt: einfach so nicht machen.

Methoden können andere Methoden aus derselben Klasse aufrufen, indem sie Methodenattribute des Arguments `self` verwenden:

```python
class Bag:
    def __init__(self):
        self.data = []

    def add(self, x):
        self.data.append(x)

    def addtwice(self, x):
        self.add(x)
        self.add(x)
```

Methoden können auf globale Namen in der gleichen Weise verweisen wie normale Funktionen. Der globale Bereich, der mit einer Methode verbunden ist, ist das Modul, das ihre Definition enthält. Eine Klasse wird nie als globaler Bereich verwendet. Es gibt zwar selten einen guten Grund für die Verwendung globaler Daten in einer Methode, aber es gibt viele legitime Verwendungszwecke für den globalen Bereich: Zum einen können Funktionen und Module, die in den globalen Bereich importiert wurden, von Methoden verwendet werden, ebenso wie Funktionen und Klassen, die in diesem Bereich definiert sind.  Normalerweise ist die Klasse, die die Methode enthält, selbst in diesem globalen Bereich definiert, und im nächsten Abschnitt werden einige gute Gründe zu sehen sein, warum eine Methode die eigene Klasse referenzieren sollte.

Jeder Wert ist ein Objekt und hat daher eine *Klasse*, auch *Typ* (auf Englisch: *type*) genannt. Sie wird als `Objekt.__Klasse__` gespeichert.

### @property für Methoden
Es gibt Anwendungsfälle, wo man in Klassen eine Methode definiert, aber in den Instanzen der Klasse darauf lieber wie auf ein Datenattribute zugreifen würde. Beispiel:

```pycon
>>> class Rectangle:
...     def __init__(self, a, b):
...         self.a = a
...         self.b = b
...     @property
...     def area(self):
...         return self.a * self.b
...
>>> r = Rectangle(4, 5)
>>> r.a, r.b
(4, 5)
>>> r.area
20
>>> r.area()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'int' object is not callable
>>>
```

Die Klasse `Rectangle` wird definiert und erwartet zwei Attribute (für die Länge und Breite des Rechtecks). Die Funktion `area` zur Berechnung des Flächeninhalts des Rechtecks wird mit dem `@property` Dekorator versehen. Dadurch kann man auf `area` wie ein Datenattribut zugreifen. Dies ist z.B. bei berechneten Werten wie im Beispiel sinnvoll, wo keine Änderungen an den Variablen innerhalb der Klasse vorgenommen werden. Versucht man auf `area` wie auf eine Methode zuzugreifen erhält man einen Fehler.

## Vererbung

Natürlich würde ein Sprachmerkmal den Namen "Klasse" nicht verdienen, wenn es nicht die Vererbung unterstützen würde. Die Syntax für eine abgeleitete Klassendefinition sieht wie folgt aus:

```python
class DerivedClassName(BaseClassName):
    <statement-1>
    .
    .
    .
    <statement-N>
```

Der Name `BaseClassName` muss in einem Namensraum definiert sein, der von dem Bereich aus zugänglich ist, der die Definition der abgeleiteten Klasse enthält. Anstelle eines Basisklassennamens sind auch andere beliebige Ausdrücke zulässig. Dies kann zum Beispiel nützlich sein, wenn die Basisklasse in einem anderen Modul definiert ist:

```python
class DerivedClassName(modname.BaseClassName):
```

Die Ausführung einer abgeleiteten Klassendefinition erfolgt auf die gleiche Weise wie bei einer Basisklasse. Wenn das Klassenobjekt konstruiert wird, wird die Basisklasse berücksichtig. Dies wird für die Auflösung von Attributreferenzen verwendet: Wenn ein angefordertes Attribut nicht in der Klasse gefunden wird, wird in der Basisklasse gesucht. Diese Regel wird rekursiv angewandt, wenn die Basisklasse selbst von einer anderen Klasse abgeleitet ist.

Die Instanziierung von abgeleiteten Klassen ist nichts Besonderes: `DerivedClassName()` erzeugt eine neue Instanz der Klasse. Methodenreferenzen werden wie folgt aufgelöst: Das entsprechende Klassenattribut wird durchsucht, gegebenenfalls absteigend in der Hierarchie der Basisklassen. Die Methodenreferenz ist gültig, wenn sie ein Funktionsobjekt ist.

Abgeleitete Klassen können Methoden ihrer Basisklassen außer Kraft setzen. Da Methoden keine besonderen Privilegien haben, wenn sie andere Methoden desselben Objekts aufrufen, kann eine Methode einer Basisklasse, die eine andere in derselben Basisklasse definierte Methode aufruft, am Ende eine Methode einer abgeleiteten Klasse aufrufen, die sie außer Kraft setzt.  (Für C++-Programmierer: alle Methoden in Python sind effektiv `virtuell`.)

Eine überschreibende Methode in einer abgeleiteten Klasse kann die gleichnamige Methode der Basisklasse nicht einfach ersetzen, sondern erweitern wollen. Es gibt eine einfache Möglichkeit, die Methode der Basisklasse direkt aufzurufen: Einfach `BaseClassName.methodname(self, arguments)` aufrufen.  Dies ist gelegentlich auch für Nutzer nützlich. Zu beachten ist, dass dies nur funktioniert, wenn die Basisklasse als `BaseClassName` im globalen Bereich zugänglich ist.

Beispiel. Der Code

```pythonclass BaseClass:
    def greet(self):
        print('Good day')

    def say_hello(self):
        print('Hello World')

class DerivedClass(BaseClass):
    def greet(self):
        print('Greeting of the day')

    def say_goodbye(self):
        print('Goodbye')

d = DerivedClass()
d.say_hello()
d.greet()
d.say_goodbye()
```

gibt folgenden Ausgabe:

```
Hello World
Greeting of the day
Goodbye
```

`d.say_hello` greift auf die Methode von `BaseClass` zu, weil diese dort definiert ist, aber nicht in `DerivedClass`. `greet` wird aus `DerivedClass` übernommen, weil die Methode die geerbte aus `BaseClass` überschreibt.

Python hat zwei eingebaute Funktionen, die mit Vererbung arbeiten:

 * `isinstance` wird verwendet, um den Typ einer Instanz zu prüfen: `isinstance(obj, int)` ist nur `True`, wenn `obj.__class__` `int` oder eine von `int` abgeleitete Klasse ist.
 * `issubclass` wird verwendet, um die Klassenvererbung zu prüfen: `issubclass(bool, int)` ist nur `True`, wenn `bool` eine Unterklasse von `int` ist.  Aber `issubclass(float, int)` ist `False`, da `float` keine Unterklasse von `int` ist.

### mehrfache Vererbung

Python unterstützt auch Mehrfachvererbung. Eine Klassendefinition mit mehreren Basisklassen sieht wie folgt aus:

```python
class DerivedClassName(Base1, Base2, Base3):
    <statement-1>
    .
    .
    .
    <statement-N>
```

Für die meisten Zwecke und in den einfachsten Fällen kann man sich die Suche nach Attributen, die von einer übergeordneten Klasse geerbt wurden, als eine Suche in der Tiefe von links nach rechts vorstellen, wobei nicht zweimal in derselben Klasse gesucht wird, wenn es eine Überschneidung in der Hierarchie gibt. Wenn also ein Attribut in `DerivedClassName` nicht gefunden wird, wird es in `Base1` gesucht, dann (rekursiv) in den Basisklassen von `Base1`, und wenn es dort nicht gefunden wurde, wurde es in `Base2` gesucht, und so weiter.

Tatsächlich ist es etwas komplexer als das. Die Reihenfolge der Methodenauflösung ändert sich dynamisch, um kooperative Aufrufe von `super` zu unterstützen. Dieser Ansatz ist in einigen anderen Sprachen mit Mehrfachvererbung als "call-next-method" bekannt und ist mächtiger als der Superaufruf in Sprachen mit Einfachvererbung.

Dynamische Ordnung ist notwendig, weil alle Fälle von Mehrfachvererbung eine oder mehrere rautenförmige Beziehungen aufweisen, bei denen auf mindestens eine der Elternklassen über mehrere Pfade von der untersten Klasse aus zugegriffen werden kann. Zum Beispiel erben alle Klassen von `object`, so dass jeder Fall von Mehrfachvererbung mehr als einen Pfad bietet, um `object` zu erreichen.  Um zu verhindern, dass auf die Basisklassen mehr als einmal zugegriffen wird, linearisiert der dynamische Algorithmus die Suchreihenfolge so, dass die in jeder Klasse angegebene Reihenfolge von links nach rechts erhalten bleibt, jede übergeordnete Klasse nur einmal aufgerufen wird und der Algorithmus monoton ist (d. h., dass eine Klasse untergeordnet werden kann, ohne die Rangfolge ihrer übergeordneten Klassen zu verändern). Zusammengenommen ermöglichen diese Eigenschaften die Entwicklung zuverlässiger und erweiterbarer Klassen mit Mehrfachvererbung.

## private Variablen

"Private" Instanzvariablen, auf die nur innerhalb eines Objekts zugegriffen werden kann, gibt es in Python nicht. Grundsätzlich man auf alles von außerhalb zugreifen. Es gibt jedoch eine Konvention, die von den meisten Python-Programmieren befolgt wird: Ein Name, dem ein Unterstrich vorangestellt ist (z.B. `_spam`), sollte als nicht-öffentlicher Teil einer Klasse / der API behandelt werden (egal ob es sich um eine Funktion, eine Methode oder ein Datenelement handelt). Es sollte als ein Implementierungsdetail betrachtet werden und könnte ohne Vorankündigung geändert werden oder auch verschwinden.

Da es einen gültigen Anwendungsfall für klassenprivate Mitglieder gibt - nämlich die Vermeidung von Namenskonflikten von Namen mit Namen, die von Unterklassen definiert werden - gibt es eine begrenzte Unterstützung für einen solchen Mechanismus, genannt `name mangling` (auf Deutsch frei übersetzt: Namensverunstaltung). Jeder Bezeichner der Form `__spam` (mindestens zwei führende Unterstriche, höchstens ein nachgestellter Unterstrich) wird textuell durch `_classname__spam` ersetzt, wobei `classname` der aktuelle Klassenname ohne führende(n) Unterstrich(e) ist. Diese Umwandlung erfolgt ohne Rücksicht auf die syntaktische Position des Bezeichners, solange er innerhalb der Definition einer Klasse auftritt.

Die Namensumwandlung ist hilfreich, damit Unterklassen Methoden überschreiben können, ohne klasseninterne Methodenaufrufe zu stören. Zum Beispiel:

```python
class Mapping:
    def __init__(self, iterable):
        self.items_list = []
        self.__update(iterable)

    def update(self, iterable):
        for item in iterable:
            self.items_list.append(item)

    __update = update   # als privat gekennzeichnete Kopie der Methode "update"

class MappingSubclass(Mapping):

    def update(self, keys, values):
        # neue Defintion von update()
        # steht nicht im Konflikt mit __init__()
        for item in zip(keys, values):
            self.items_list.append(item)
```

Das obige Beispiel würde auch funktionieren, wenn `MappingSubclass` einen Bezeichner `__update` einführen würde, da er in der Klasse `Mapping` durch `_Mapping__update` und in der Klasse `MappingSubclass__update` ersetzt wird.

Zu beachten ist, dass die Mangling-Regeln hauptsächlich dazu gedacht sind, ungewollte Konflikte zu vermeiden. Es ist immer noch möglich, auf eine Variable zuzugreifen oder sie zu verändern. Dies kann unter besonderen Umständen sogar nützlich sein, z.B. im Debugger.

Außerdem ist zu beachten, dass Code, der an `exec()` oder `eval()` übergeben wird, den Klassennamen der aufrufenden Klasse nicht als die aktuelle Klasse betrachtet. Dies ist vergleichbar mit der Wirkung der `global` Anweisung, deren Auswirkung ebenfalls auf Code beschränkt ist, der zusammen zu Bytecode kompiliert wird. Die gleiche Einschränkung gilt für `getattr()`, `setattr()` und `delattr()`, sowie bei der direkten Referenzierung von `__dict__`.

## andere Kleinigkeiten

Manchmal ist es nützlich, einen Datentyp ähnlich dem Pascal-"record" oder C-"struct" zu haben, der einige benannte Datenelemente zusammenfasst. Der idiomatische Ansatz ist die Verwendung des Moduls [dataclasses](https://docs.python.org/3/library/dataclasses.html) (welche mit Python 3.7 eingeführt wurden) für diesen Zweck:

```pycon
>>> from dataclasses import dataclass
>>> @dataclass
... class Employee:
...     name: str
...     dept: str
...     salary: int
...
>>> john = Employee('john', 'computer lab', 1000)
>>> john.dept
'computer lab'
>>> john.salary
1000
```

Python-Code, der einen bestimmten abstrakten Datentyp erwartet, kann oft eine Klasse übergeben werden, die stattdessen die Methoden dieses Datentyps emuliert. Wenn man zum Beispiel eine Funktion hat, die Daten aus einem Dateiobjekt formatiert, kann man eine Klasse mit den Methoden `io.TextIOBase.read` und `io.TextIOBase.readline` definieren, die die Daten stattdessen aus einem String-Puffer holen und als Argument übergeben.

Diese Technik hat aber ihre Grenzen: eine Klasse kann keine Operationen definieren, auf die mit einer speziellen Syntax zugegriffen wird, wie z.B. Indexzugriff auf Sequenzen oder arithmetische Operatoren, und die Zuweisung einer solchen "Pseudodatei" an `sys.stdin` führt nicht dazu, dass der Interpreter weitere Eingaben aus ihr liest.

Instanzmethodenobjekte haben ebenfalls Attribute: `m.__self__` ist das Instanzobjekt mit der Methode `m`, und `m.__func__` ist das Funktionsobjekt, das der Methode entspricht.

## Iteratoren

Es wurde schon im Rahmen dieses Tutorial gezeigt, dass über die meisten Containerobjekte mit einer `for`-Anweisung in einer Schleife iteriert werden kann:

```python
for element in [1, 2, 3]:
    print(element)
for element in (1, 2, 3):
    print(element)
for key in {'one':1, 'two':2}:
    print(key)
for char in "123":
    print(char)
for line in open("myfile.txt"):
    print(line, end='')
```

Diese Art des Zugriffs ist klar und bequem. Die Verwendung von Iteratoren finden man an vielen Stellen und ist vereinheitlicht für Python. Hinter den Kulissen ruft die Anweisung `for` die Funktion [iter](https://docs.python.org/3/library/functions.html#iter) für das Container-Objekt auf. Die Funktion gibt ein Iterator-Objekt zurück, das die Methode `iterator.__next__`, die auf ein Element im Container nach dem anderen zugreift. Wenn es keine weiteren Elemente mehr gibt, löst `iterator.__next__` eine `StopIteration` Ausnahme aus, die der `for`-Schleife mitteilt, dass sie beendet werden soll.  Man kann die Methode `iterator.__next__` mit der eingebauten Funktion [next](https://docs.python.org/3/library/functions.html#next) aufrufen. Dieses Beispiel zeigt, wie das Ganze funktioniert:

```pycon
>>> s = 'abc'
>>> it = iter(s)
>>> it
<str_iterator object at 0x10c90e650>
>>> next(it)
'a'
>>> next(it)
'b'
>>> next(it)
'c'
>>> next(it)
Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
    next(it)
StopIteration
```

Nachdem die Funktionsweise des Iterator-Protokolls gezeigt wurde, ist es einfach, Iterator-Verhalten zu eigenen Klassen hinzuzufügen.  Man definieren eine `container.__iter__` Methode, die ein Objekt mit einer `iterator.__next__` Methode zurückgibt. Wenn die Klasse `__next__` definiert, dann kann `__iter__` einfach `self` zurückgeben:

```pycon
>>> class Reverse:
...     """Iterator for looping over a sequence backwards."""
...     def __init__(self, data):
...         self.data = data
...         self.index = len(data)
...     def __iter__(self):
...         return self
...     def __next__(self):
...         if self.index == 0:
...             raise StopIteration
...         self.index = self.index - 1
...         return self.data[self.index]
...
>>> rev = Reverse('spam')
>>> iter(rev)
<__main__.Reverse object at 0x00A1DB50>
>>> for char in rev:
...     print(char)
...
m
a
p
s
```

## Generatoren

[Generatoren](https://docs.python.org/3/glossary.html#term-generator) sind ein einfaches und leistungsfähiges Werkzeug zur Erstellung von Iteratoren. Sie werden wie normale Funktionen geschrieben, verwenden aber die Anweisung [yield](https://docs.python.org/3/reference/simple_stmts.html#yield), wenn sie Daten zurückgeben wollen. Jedes Mal, wenn `next` aufgerufen wird, macht der Generator dort weiter, wo er aufgehört hat. Er merkt sich alle Datenwerte und welche Anweisung zuletzt ausgeführt wurde. Ein Beispiel zeigt, dass Generatoren trivial einfach zu erstellen sein können:

```pycon
>>> def reverse(data):
...     for index in range(len(data)-1, -1, -1):
...        yield data[index]
...
>>> for char in reverse('golf'):
...     print(char)
...
f
l
o
g
```

Alles, was mit Generatoren gemacht werden kann, kann auch mit klassenbasierten Iteratoren gemacht werden, wie im vorherigen Abschnitt beschrieben. Was Generatoren so kompakt macht, ist, dass die Methoden `iterator.__iter__` und `generator.__next__` automatisch erstellt werden.

Ein weiteres wichtiges Merkmal ist, dass die lokalen Variablen und der Ausführungsstatus zwischen Aufrufen automatisch gespeichert werden. Dies macht die Funktion einfacher zu schreiben und viel übersichtlicher als ein Ansatz, der Instanzvariablen wie `self.index` und `self.data` verwendet.

Zusätzlich zur automatischen Methodenerstellung und der Speicherung des Programmzustands lösen Generatoren bei Beendigung automatisch `StopIteration` aus. In Kombination machen diese Funktionen es einfach, Iteratoren zu erstellen, ohne mehr Aufwand als beim Schreiben einer normalen Funktion.

## Generatorausdrücke

Einige einfache Generatoren können kurz und bündig als Ausdrücke kodiert werden, wobei eine Syntax verwendet wird, die der von List Comprehensions ähnelt, jedoch mit Klammern anstelle von eckigen Klammern. Diese Ausdrücke sind für Situationen gedacht, in denen der Generator sofort von einer einschließenden Funktion verwendet wird. Generatorausdrücke sind kompakter, aber weniger vielseitig als vollständige Generatordefinitionen und tendenziell speicherfreundlicher als äquivalente Listenausdrücke.

Beispiele:

```pycon
>>> sum(i*i for i in range(10))                 # Summe der Quadrate
285
>>> xvec = [10, 20, 30]
>>> yvec = [7, 5, 3]
>>> sum(x*y for x,y in zip(xvec, yvec))         # dot product
260
>>> unique_words = set(word for line in page for word in line.split())
>>> valedictorian = max((student.gpa, student.name) for student in graduates)
>>> data = 'golf'
>>> list(data[i] for i in range(len(data)-1, -1, -1))
['f', 'l', 'o', 'g']
```

***

 * nächstes Kapitel: [kurze Tour durch die Standardbibliothek](stdlib.md)
 * vorheriges Kapitel: [Fehler und Ausnahmen](errors.md)
