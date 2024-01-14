# Ein kleiner Appetitanreger auf Python

Wer viel am Computer arbeiten, wird irgendwann feststellen, dass man eine Aufgabe automatisieren könnte oder möchte.  Vielleicht möchte man zum Beispiel bei einer großen Anzahl von Textdateien eine Suchen und Ersetzen Operation durchführen oder eine Reihe von Fotodateien auf komplizierte Weise umbenennen und neu anordnen. Vielleicht möchte man auch eine kleine benutzerdefinierte Datenbank, eine spezielle GUI-Anwendung oder ein einfaches Spiel schreiben.

Wer ein professioneller Softwareentwickler ist, muss vielleicht mit mehreren C/C++/Java-Bibliotheken arbeiten, findet aber den üblichen Zyklus Schreiben/Kompilieren/Testen/Rekompilieren zu langsam. Vielleicht schreibt man gerade eine Test-Suite für eine solche Bibliothek und findet das Schreiben des Testcodes eine langwierige Aufgabe. Oder vielleicht hat man ein Programm geschrieben, das eine Erweiterungssprache verwenden könnte, und man möchte keine völlig neue Sprache für die Anwendung entwerfen und implementieren.

Python dafür die geeignete und richtige Programmiersprache.

Man könnte für einige dieser Aufgaben ein Unix-Shell-Skript oder Windows-Batch-Dateien schreiben, aber Shell-Skripte eignen sich am besten für das Verschieben von Dateien und das Ändern von Textdaten und sind nicht so gut für GUI-Anwendungen oder Spiele geeignet. Man könnte ein C/C++/Java-Programm schreiben, aber es kann sehr viel Entwicklungszeit in Anspruch nehmen, um auch nur einen ersten Programmentwurf zu erstellen. Python ist einfach(er) zu verwenden, auf Windows-, MacOS-, Linux- und Unix-Betriebssystemen verfügbar und wird helfen, die Arbeit schneller zu erledigen.

Python ist einfach zu benutzen, aber es ist eine echte Programmiersprache, die viel mehr Struktur und Unterstützung für große Programme bietet, als Shell-Skripte oder Batch-Dateien es können. Andererseits bietet Python auch viel mehr Fehlerprüfung als beispielsweise C. Und da es sich um eine *Hochsprache* handelt, verfügt es über eingebaute Hochdatentypen, wie z.B. flexible Listen (in anderen Programmiersprache Arrays genannt) und Wörterbücher. Aufgrund seiner allgemeineren Datentypen ist Python auf einen viel größeren Problembereich anwendbar als zum Beispiel [Awk](https://de.wikipedia.org/wiki/Awk) oder [Perl](https://www.perl.org/), dennoch sind viele Dinge in Python mindestens genauso einfach wie in diesen Sprachen.

Mit Python kann man sein Programm in Module aufteilen, die in anderen Python-Programmen wiederverwendet werden können. Es wird mit einer großen Sammlung von Standardmodulen geliefert, die man als Grundlage für eigene Programme verwenden kann - oder als Beispiele, um mit dem Programmieren in Python zu beginnen. Einige dieser Module bieten Dinge wie Dateieingabe/Ausgabe, Systemaufrufe, Sockets und sogar Schnittstellen zu grafischen Benutzeroberflächen-Toolkits wie [Tk](https://www.tcl.tk/). Aufgrund der Popularität von Python gibt es auch eine enorm große Anzahle von Python-Modulen, die durch andere Python-Programmierer bereit gestellt werden. Der primäre Anlaufpunkt dafür ist die Webseite [The Python Package Index](https://pypi.org/) (kurz: PyPI).

Python ist eine interpretierte Sprache, mit der man bei der Programmentwicklung viel Zeit sparen kann, da keine Kompilierung und kein Linken erforderlich sind. Der Interpreter kann interaktiv verwendet werden, was das Experimentieren mit Funktionen der Sprache, das Schreiben von Wegwerfprogrammen oder das Testen von Funktionen während der Bottom-up-Programmentwicklung erleichtert. Der Interpreter ist auch ein praktischer Taschenrechner.

Mit Python lassen sich Programme kompakt und gut lesbar schreiben. In Python geschriebene Programme sind in der Regel (viel) kürzer als entsprechende C-, C++- oder Java-Programme, und zwar aus mehreren Gründen:

 * die High-Level-Datentypen ermöglichen es, komplexe Operationen in einer einzigen Anweisung auszudrücken
 * die Gruppierung von Anweisungen erfolgt durch Einrückung anstelle von Anfangs- und Endklammern
 * es ist vorab keine Deklaration von Variablen oder Argumenten notwendig

Python ist *erweiterbar*: Wenn man weiß, wie man in C programmiert, ist es einfach, dem Interpreter eine neue eingebaute Funktion oder ein Modul hinzuzufügen, entweder um kritische Operationen mit maximaler Geschwindigkeit auszuführen oder um Python-Programme mit Bibliotheken zu verknüpfen, die möglicherweise nur in binärer Form verfügbar sind (z. B. eine herstellerspezifische Grafikbibliothek). Wenn man erst einmal damit vertraut ist, kann man den Python-Interpreter in eine in C geschriebene Anwendung einbinden und ihn als Erweiterung oder Befehlssprache für diese Anwendung verwenden.

Die Sprache ist übrigens nach der BBC-Sendung [Monty Python's Flying Circus](https://de.wikipedia.org/wiki/Monty_Python%E2%80%99s_Flying_Circus) benannt und hat nichts mit dem [gleichnamigen Reptil](https://de.wikipedia.org/wiki/Pythons) zu tun. Anspielungen auf Monty-Python-Sketche in der Dokumentation sind nicht nur erlaubt, sondern erwünscht!

Jetzt, wo die Begeisterung für Python geweckt ist, ist es Zeit, die Sprache genauer unter die Lupe nehmen wollen. Da man eine Sprache am besten lernt, indem man sie anwendet, lädt das Tutorial dazu ein, während des Lesens mit dem Python-Interpreter zu spielen.

Im nächsten Kapitel wird die Installation des Python-Interpreters kurz erklärt und dessen Funktionsweise erläutert. Letzteres sind eher banale Informationen, die aber für das Ausprobieren der später gezeigten Beispiele wichtig sind.

Im weiteren Verlauf des Tutorials werden verschiedene Funktionen der Sprache und des Systems Python anhand von Beispielen vorgestellt, angefangen bei einfachen Ausdrücken, Anweisungen und Datentypen über Funktionen und Module bis hin zu fortgeschrittenen Konzepten wie Ausnahmen und benutzerdefinierten Klassen.

***

 * nächste Seite: [den Python-Interpreter nutzen](interpreter.md)
 * vorherige Seite: [Startseite](start.md)
