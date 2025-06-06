# Tipps und Anmerkungen

Im Folgenden noch ein paar Anmerkung und Tipps, die vielleicht / hoffentlich bei den weiteren Schritten mit Python hilfreich sind.

## Das erste Projekt
Man hat jetzt die Grundlagen von Python gelernt und fühlt sich bereit für die ersten eigenen Programme und Projekte. Doch wie macht man jetzt am besten weiter? Für die Motivation ist es immer gut, wenn man etwas programmiert, woran man selbst Spaß hat und was im Idealfall für einen selber nützlich ist. Dabei ist es dann auch erstmal egal, ob es bereits andere, fertige Programme für die gleiche Anwendung gibt oder ob man selbst vielleicht der einzige Mensch auf der Welt ist, der dieses Programm später nutzt. Wenn man selber ein für sich selber nützliches Programmierziel hat, ist die Motivation höher, dieses auch erfolgreich und konsequent bis zum Abschluas zu bringen.

Prinzipiell können dies auch ambitioniertere Projekte sein, die länger dauern, wenn man dabei etwas lernt und sich weiterentwickelt. Natürlich sollte das Projekt nicht unrealistisch überambitioniert sein, wie dass man z.B. das nächste Blockbuster Spiel programmiert und damit Millionen verdient oder eine Forensoftware für das Internet erstellt, die alles andere in den Schatten stellt - so etwas kann man sich für später Zeiten aufsparen, wenn man weiter fortgeschritten ist ;-)

Im Laufe des Programmierprozesses kann es auch normal sein, dass man nochmals von vorne beginnt und alles unter Beibehaltung des Funktionalität neu programmiert, weil man erkannt hat, dass die aktuelle Programmstruktur eine Sackgasse ist. Das ist per se auch überhaupt nicht schlimm und gehört zum Programmieren dazu. Und sowas kommt auch in großen Projekten und bei professionellen Programmierern vor. Der Begriff dafür ist [Refactoring](https://de.wikipedia.org/wiki/Refactoring).

Des Weiteren sollte man sich nicht scheuen, seinen Code bzw. auch Zwischenstände seines Codes in einem Python Supportforum (wie z.B. [www.python-forum.de](https://www.python-forum.de)) zu zeigen, um Verbesserungsvorschläge, Kritik und eventuell neue Ideen zu bekommen. Genau so wenig sollte man sich scheuen, bei Problemen, die man partout nicht selber gelöst bekommt, sich an ein Supportforum zu wenden. Egal, für wie schlecht (oder gut) man den eigenen Code hält. Wichtig dabei ist: *nicht* das Problem wortreich beschreiben, sondern a) den relevanten, realen Code zeigen, b) beschreiben, was man gerne hätte / erwartet und c) beschreiben, was man aktuell stattdessen bekommt. Wenn man beim Ausführen des Codes eine Fehlermeldung bekommt, diese *immer* vollständig posten und nicht nur die letzte Zeile.

## GUI-Programmierung - für Einsteiger?

Als Computernutzer ist man es gewohnt, eine grafische Benutzeroberfläche zu haben und Programme mit grafischer Benutzeroberfläche zu nutzen. Egal ob am PC, am Smartphone oder am Tablet. Im Rahmen dieses Tutorials wurde ausschließlich ein textbasierter Terminal genutzt, um Python interaktiv zu nutzen und Python Programme auszuführen. Grundsätzlich sollte man keine Angst für dem Terminal haben. Auch rein textbasierte Programme können Nutzereingaben entgegennehmen und Ergebnisse ausgeben. Auch wenn dies für viele Nutzer eher ungewohnt ist, in der Windows-Welt vielleicht noch mehr als in der MacOS und vor allem der Linux-Welt.

Jedenfalls hat man deswegen vielleicht die Idee, ein Programm mit grafischer Benutzeroberfläche zu erstellen. Geht das als Anfänger, nachdem man die in diesem Tutorial vermittelten Grundlagen beherrscht? Im Prinzip: ja. Aber...

Das Erstellen selbst einer einfachen GUI erfordert relativ viel Code. Was per se nicht nicht schlimm ist, aber der "Aufwand", eine GUI zu erstellen, kann ggf. deutlich höher sein als das Programm an sich. Wenn man z.B. ein Programm erstellt, dass eine Prüfung macht, ob eine eingegebene Zahl eine Primzahl ist, braucht (bei einer simplen, nicht sonderlich effizienten) Implementierung 13 Zeilen Code für eine entsprechende Funktion. Wenn man eine passendes GUI dazu programmiert, z.B. mit dem in Python enthaltenen tkinter, die nur ein Eingabefeld, ein Ergebnisfeld und eine *"jetzt berechnen"* Schaltfläche hat, dann sind das ca. 2-3 mal so viele Codezeilen zusätzlich. Dies ist natürlich kein Grund, es nicht zu machen - aber es ist halt deutlich mehr Aufwand.

Um typische Fehler, die man zu Anfang eventuell macht, zu vermeiden, im Folgenden noch ein paar Tipps für die Erstellung einer eigenen GUI:

 * die GUI in eine Klasse packen: Damit der Code für die GUI strukturiert und später ggf. noch erweiterbar ist sollte man diesen in eine Klasse packen. Das gilt prinzipiell für alle GUI Frameworks und jede GUI, die über sehr trivial (= mehr als zwei Elemente) hinausgeht.
 * GUI und Anwendungslogik trennen: Es ist grundsätzlich immer eine gute Idee, den Code von der GUI an sich und den der Anwendungslogik zu trennen. Dies verbessert deutlich die Wartbarkeit und Wiederverwendbarkeit. Um beim obigen Beispiel zu bleiben: der Code zum Testen, ob eine Zahl prim ist (oder nicht) packt man in ein eigenes Modul, importiert dieses im Code mit der GUI und ruft dann von dort die Funktion aus dem Prim-Modul auf.
 * nicht den `mainloop()` blockieren: Alle GUI-Frameworks haben eine Methode, die zum Starten der GUI an sich aufgerufen wird, die die GUI kontrolliert und steuert. Bei tkinter ist dies `mainloop`, bei anderen Frameworks kann der Name anders lauten, aber es wird das gleich gemacht. "Kontrolliert und steuert" bedeutet, dass der Mainloop Eingabefelder auf Eingaben überwacht, auf Tastendrücke oder Mausklicks in der GUI reagiert, Ausgaben aktualisiert usw. Oder anders gesagt: der Mainloop hat und braucht die Kontrolle, damit die GUI benutzbar ist und reagiert. Was dann im Umkehrschluss auch heißt, dass man den Mainloop nicht blockieren sollte. Dies würde z.B. passieren, wenn man eine länger laufende while-Schleife in der GUI hat. Diese würde, solange der Code in der Schleife abgearbeitet wird, den Mainloop blockieren. Möchte man Funktionen innerhalb einer GUI periodisch ausführen, aber die GUI-Frameworks dafür eigene Methoden, die sich in den Mainloop integrieren und diesen nicht blockieren. Bei tkinter ist dies z.B. die `after` Methode.
 * keine feste Größe für Elemente und das Fenster festlegen: Manchmal ist man vielleicht dazu geneigt, eine GUI pixelgenau designen zu wollen, also Fenstergröße z.B. 1200x900px, den eigenen Button genau an Position X,Y, das Eingabefeld an Position Q,P, usw. Das geht auch mit allen GUI-Frameworks, aber: sollte man nicht machen. Grund: Man kann Fenster minimieren, maximieren bzw. beliebig in Länge und Breite ändern. Starre Layouts passen dann ggf. nicht mehr, man sieht nicht mehr alle Elemente oder ähnliches. Deswegen ist es sinnvoll, die GUI die Elemente platzieren zu lassen, in Abhängig von der aktuellen Fenstergröße. Das nennt man "relatives Layout". Hierfür bieten alle GUI-Framework auch Möglichkeiten zur Positionierung, z.B. ein "Grid Layout", wo Elementen in einem Raster / (virtuellen) Gitter platziert werden. So ist die GUI wesentlich flexibler, was den Umgang mit verschiedenen Fenster- und Bildschirmgrößen angeht.

## Nebenläufige Programmierung - ist das kompliziert?

Hat man ein Programm geschrieben, was z.B. umfangreiche Berechnungen durchführt und vielleicht lange braucht, um fertig zu werden, kommt man vielleicht auf die Idee, die Berechnung aufzuteilen und in mehreren Teilen parallel zu machen. Und das geht grundsätzlich auch. Mit nebenläufiger Programmierung.

[Nebenläufige Programmierung](https://de.wikipedia.org/wiki/Nebenl%C3%A4ufigkeit) bezeichnet beim Programmieren bzw. in der Informatik allgemein, wenn ein Programm mehrere Dinge gleichzeitig (oder quasi gleichzeitig) macht. Python hat verschiedene Module für nebenläufige Programmierung in der Standardbibliothek, siehe [Kapitel zur Standardbibliothek, Abschnitt "Nebenläufige Programmierung"](stdlib.md#nebenläufige-programmierung), inklusive den Hinweisen / Anmerkungen zu den Modulen. Kann man sowas auch als Einsteiger oder fortgeschrittener Einsteiger nutzen? Sicherlich, aber...

Nebenläufige Programmierung kann alles von relativ einfach bis sehr komplex sein. Einer der Hauptpunkte ist, dass es - auch für fortgeschritten Programmierer - schwierig(er) sein kann, sich vorzustellen, was wann passiert - weil eben der Programmablauf nicht mehr linear ist. Dies wird umso komplexer, je mehr Dinge man parallel machen will oder muss. Und es wird nochmals komplexer, wenn mehrere nebenläufige Programmteile voneinander abhängen oder aufeinander warten.

Wenn man mit nebenläufiger Programmierung beginnt sollte man erst einmal "klein" anfangen, d.h. nicht direkt dutzende von Threads oder Prozessen starten, sondern vielleicht erst mal einen Hauptprozess, der dann einen weiteren Thread oder Prozess startet. Wenn das alles klappt und man ein bisschen Routine hat, kann man das dann hochskalieren. Des Weiteren sollte man, zumindest am Anfang, immer eine [Queue](https://docs.python.org/3/library/queue.html#queue.Queue) zum Austausch von Daten zwischen zwei Threads oder Prozessen benutzen und niemals versuchen, dies über globale Variablen zu machen.

## Wie funktioniert der Python-Interpreter eigentlich?

Solange man keine eigenen low-level Module für Python entwickeln will, sind die Internas von Python - bzw. genau genommen von CPython, der hier in diesem Tutorial behandelten Referenzimplementierung - egal. Egal im Sinne von, dass man diese nicht kennen muss.

Nichtsdestotrotz schadet es nicht, wenn man weiß, wie Python grundsätzlich arbeitet, wenn es ein Programm abarbeitet. Python ist eine interpretierende Programmiersprache, d.h. ein Programm wird von Interpreter Befehl für Befehl abgearbeitet. Dies ist ein Unterschied zu kompilierenden Programmiersprachen wie z.B. C/C++, Go oder Rust, wo immer das komplette Programm in Maschienenbefehle vom Compiler übersetzt und dann erst ausgeführt wird.

Bei Python führt der Interpreter aber nicht den Quellcode direkt aus, die Ausführung des Programms erfolgt in zwei Schritten. Im ersten Schritt übersetzt ein in Python enthaltender Compiler den kompletten Quelltext in Bytecode und dieser Bytecode wird dann Anweisung für Anweisung ausgeführt. Der Bytecode ist plattformunabhängig und kann sich auch ggf. zwischen zwei verschiedenen Python Versionen unterschieden. Wie Bytecode aussieht, ist im folgenden Beispiel gezeigt:

```pycon
>>> def func():
...     a = [1, 2, 3]
...     return len(a)
...
>>> import dis   # das dis Modul dient zum Disassemblieren von Bytecode
>>> dis.dis(func)
  2           0 BUILD_LIST               0
              2 LOAD_CONST               1 ((1, 2, 3))
              4 LIST_EXTEND              1
              6 STORE_FAST               0 (a)

  3           8 LOAD_GLOBAL              0 (len)
             10 LOAD_FAST                0 (a)
             12 CALL_FUNCTION            1
             14 RETURN_VALUE
```

Python selber ist übrigens in der Programmiersprache C geschrieben. Ein Teil der in der Python Standardbibliothek enthaltenen Module ebenfalls, andere sind direkt in Python implementiert.

## andere Python-Implementierungen

Wie zu Beginn dieses Tutorials bereits erwähnt ist Python an sich eine Sprachdefinition der Programmiersprache, und das hier behandelte CPython ist die Referenzimplementierung der Python Software Foundation.

Und es gibt noch diverse anderen Implementierung. Diese Unterscheiden sich in der Regel in Internas (erheblich) von CPython. Zwei der gängigeren / bekannteren Implementierungen sind: PyPy und Micropython.

[PyPy](https://www.pypy.org/) ist eine Python-Implementierung, die sich vor allem (aber nicht nur) von CPython darin unterscheidet, dass intern ein Just-in-Time Compiler zum Ausführen von Python-Code genutzt wird. Dadurch laufen manche Programme (deutlich) schneller. Dieser Vorteil wird in der Regel am deutlichsten sichtbar, wenn man langlaufende, CPU-intensive Programme ausführt. Bei kurzen Programmen oder Programmen, die viel auf I/O warten ist der Vorteil geringer bis nicht vorhanden. Jedenfalls ist der Einsatz von PyPy immer einen Versuch wert, wenn man das Gefühl hat, dass das eigene Programm schneller laufen könnte. PyPy und CPython lassen sich problemlos und konfliktfrei parallel installieren und nutzen.

[Micropython](https://micropython.org/) ist eine schlankere, reduzierte Python-Implementierung, die speziell auf Microcontroller wie z.B. den ESP32 oder Raspberry RP2 ausgelegt ist. Micropython kommt mit (deutlich) weniger Speicher aus, hat (deutlich) weniger Module in der Standardbibliothek, hat dafür aber zusätzliche Module (wie z.B. das `machine` Modul) zur Ansteuerung der jeweiligen Hardware des Microcontrollers (wie z.B. GPIO Pins) mit an Bord.

***

 * Nächstes Kapitel: [Externe Python Module](externallibs.md)
 * Zurück zur [Startseite](index.md)
