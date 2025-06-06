# Externe Python Module

Es gibt zehntausende externe Python-Module, die man für eigene Programmierprojekte nutzen kann. Die primäre Anlaufstelle ist der [Python Package Index](https://pypi.org/) (kurz: PyPI, aber auch als "Cheese Shop" bekannt). Es gibt Python-Module für alle möglichen Anwendungen.

Im Folgenden wird eine sehr kurze Übersicht über einige sehr gängige / viel genutzte Module für verschiedene Anwendungsgebiete geben. Die Übersicht erhebt keinerlei Anspruch auf Vollständigkeit. Eine weitere (leider nicht immer ganz aktuelle) Übersicht ist im [Python Wiki](https://wiki.python.org/moin/UsefulModules) zu finden.

## Mathematik / Rechnen / Datenanalyse
Ein sehr viel genutztes Python-Modul, sowohl direkt als auch als Basis für anderen Module, ist [numpy](https://numpy.org). numpy bietet im Kern die Datenstruktur "ndarray", welche sehr effizient große n-dimensionale Arrays (wie z.B. Vektoren und Matrizen) umgehen kann. Außerdem bietet numpy eine Reihe von mathematischen Funktionen, ein Submodul für Zufallszahlen, ein Submodul für lineare Algebra, eins für Fourier-Transformationen und vieles mehr. Für den Einstieg in numpy gibt es ein [Einsteiger-Tutorial](https://numpy.org/doc/stable/user/absolute_beginners.html).

[pandas](https://pandas.pydata.org) ist ein schnelles, leistungsstarkes, flexibles und einfach zu bedienendes Python-Modul. zur Datenanalyse und -manipulation. Besonders geeignet ist die Software für tabellarischen Daten und Zeitreihen, wobei pandas diverse Methoden zum Umgang mit fehlenden und fehlerhaften Daten hat. Die Kerndatenstruktur ist ein DataFrame (welche intern wiederrum auf numpy ndarrays aufsetzen). Ein DataFrame kann nach verschiedenen Kriterien gefiltert, sortiert und aggregiert werden. Damit kann man z.B. ähnliche Dinge programmieren, die man ansonsten mit einer Tabellenkalkulation (wie Excel oder LibreOffice Calc) machen würde. Auch für pandas gibt eine kompaktes Tutorial für Nutzer, die sich das Modul näher ansehen möchten: [10 minutes to pandas](https://pandas.pydata.org/docs/user_guide/10min.html)

[scipiy](https://scipy.org/) ist ein Python-Modul, welche fertige Submodule für diverse Felder der Mathematik und Ingenieurswissenschaften an Bord hat. scipiy ist ebenfalls umfassend dokumentiert, auf der [Startseite der Dokumentation](https://docs.scipy.org/doc/scipy/tutorial/index.html#user-guide) findet auch direkt eine Übersicht, für welche Anwendungsfelder Submodule enthalten sind.

## Webentwicklung
Webentwicklung mit Python in der Regel mit Hilfe eines Webframeworks - wovon es reichlich für alle Facetten von Anwendungen gibt. Der allgemeine Standard für Webapplikation mit Python ist [WSGI](https://wsgi.readthedocs.io/en/latest/what.html). WSGI steht für **W**eb **S**tandard **G**ateway **I**nterface und beschreibt den Standardweg, wie eine Applikation mit dem Server kommuniziert - was das Webframework für einen erledigt. Vereinfacht dargestellt ist der Ablauf so, dass man mit Hilfe des Frameworks eine Route / eine URL an eine Python-Funktion bindet. Beim Aufruf der Route wird die Funktion ausgeführt und liefert das Ergebnis wie eine generierte HTML-Seite zurück.

Die beiden bekanntesten - und wahrscheinlich auch meist genutzen - Webframeworks sind (wahscheinlich): Django und Flask

### Django
[Django](https://www.djangoproject.com/) ist ein umfangreiches Framework mit "batteries included" Philosophie, d.h. es bringt etliche Komponenten direkt mit. Dazu gehören eine Datenbankanbindung für relationale Datenbanken in Form des Django-ORM, eine eigene Template-Engine, ein Framework für HTML-Formulare, eine Sicherheitsframework, ein Framework zur Benutzerverwaltung und ein Admin-Backend. Django eignet sich für alle Arten von Anwendungen von klein bis sehr groß und umfangreich. Die besondere Stärke ist dabei - auch für Einsteiger und kleine Projekte - die hohe Integration der Djangokomponenten, wodurch man mit relativ wenig Code viel erreichen kann. Außerdem gibt es unzählige Erweiterungen für Django.

Django ist sehr umfangreich und detaillierte dokumentiert. Für den Beginn empfiehlt es sich, dass offizielle Django Tutorial unter [docs.djangoproject.com](https://docs.djangoproject.com/) durchzuarbeiten. Ein alternatives und ebenfalls gutes Tutorial ist [Django for Girls](https://tutorial.djangogirls.org/de). Dieses gibt es auch auf Deutsch und ist - trotz anderslautendem Namen - für alle Geschlechter geeignet.

### Flask
[Flask](https://flask.palletsprojects.com) ist ein Framework, mit dem sich mit sehr wenig Code bereits eine (sehr) einfache Webapplikation erstellen lässt. Nichtsdestotrotz eignet sich Flask auch für größere und große Projekte. Die Dokumentation von Flask enthält ein [Tutorial]( https://flask.palletsprojects.com/en/3.0.x/tutorial/) in dem gezeigt wird, wie man eine Webapplikation mit Flask erstellt.

Auch für Flask gibt es eine Vielzahl von [Erweiterungen]( https://flask.palletsprojects.com/en/3.0.x/extensions/), die sich in das eigene Projekt einbinden lassen.

## Systeminformationen
Zum Ermitteln von Systeminformationen wie der Systemauslastung und laufenden Prozessen ist das Modul [psutil](https://github.com/giampaolo/psutil) geeignet. Das Modul unterstützt alle gängigen Betriebssysteme wie Windows, MacOS und Linux, läuft aber auch auf weiteren Plattformen wie diversen BSD-Varianten. psutil ist sehr umfangreich dokumentiert: [Startseite der Dokumentation](https://psutil.readthedocs.io/en/latest/).

## Maschinelles Lernen
Für maschinelles Lernen gibt es unter anderem zwei populäre / viel genutzte Bibliotheken: [TensorFlow](https://www.tensorflow.org/), welches von Google stammt, und [PyTorch](http://pytorch.org/), welches ursprünglich aus dem Hause Facebook stammt.

[Keras](https://keras.io/) setzt auf wahlweise TensorFlow oder PyTorch auf und ist eine Bibliothek zur Erstellung neuronaler Netzwerke für das maschinelle Lernen.

Des Weiteren gibt es noch sehr viele Python-Module für maschinelles Lernen und KI, die auf bestimmten Felder aus diesem Themakomplex spezialisiert sind.

## Raspberry Pi
Nutzt man die GPIO Pins des Raspberry Pi zur Ansteuerung elektrische Schaltung, ist die Python Bibliothek [gpiozero](https://github.com/gpiozero/gpiozero) erste Wahl für Python. gpiozero hat viele High-Level Klassen z.B. einfachen Ansteuerung von LEDs, Abfragen von Tastern, Ansprechen von Distanzsensoren, Servos, und vieles mehr. gpiozero erlaubt bei Bedarf auch die low-level Programmierung des GPIO-Pins.

gpiozero ist ebenfalls umfangreich dokumentiert mit vielen Beispielen: [Startseite der Dokumentation](https://gpiozero.readthedocs.io/en/latest/index.html)

## GUI Programmierung
Es gibt für Python für alle gängigen GUI-Frameworks Module, um eine GUI (GUI = Graphical User Interface) zu erstellen. Die Standardinstallation von Python unterstützt direkt das [tkinter](https://docs.python.org/3/library/tkinter.html) Modul. Unter Linux muss man ggf. dafür das Paket **python3-tk** nachinstallieren. Mit tkinter erstellte graphische Programme sind optisch zwar etwas "altbacken", sind aber nichtsdestotrotz funktionell und gut nutzbar. Ein Vorteil von tkinter ist, dass es schon sehr lange in Python enthalten und entsprechend gut dokumentiert ist. Außerdem findet man für viele Probleme zu / mit tkinter Lösungen in Foren, Blogeinträgen etc.

Einige weitere gängige GUI-Framework sind:
 * [pyside](https://www.qt.io/qt-for-python) - Anbindung von Python an das [Qt Framework](https://www.qt.io/product/framework). Qt ist eines der umfangreichsten Frameworks mit einer breiten Unterstützung für viele Plattformen. Qt eignet sich für auch für große Anwendungen.
 * [python-gtk3](https://python-gtk-3-tutorial.readthedocs.io/en/latest/) - Anbindung von Python an das [GTK](https://docs.gtk.org/) Framework. Damit lassen sich ebenfalls komplexe grafische Benutzeroberflächen erzeugen, wobei man mehr GTK-basierte Anwendungen in der Linux-Welt als in der Windows-Welt vorfindet.
 * [Kivy](https://kivy.org/) - Kivy ist ein GUI-Framework, welches speziell für Python entwickelt wird. Kivy bietet ebenfalls eine breite Plattformunterstützung und bietet auch direkte Unterstützung für Touchscreens und Toucheingaben.

## Datenbankanbindung
Es gibt für so gut wie alle Datenbanken - egal relationale oder NoSQL - Python Anbindungen.

Python bringt in der Standardinstallation ein Modul zur Nutzung von [SQLite](https://docs.python.org/3/library/sqlite3.html). Für andere relationale Datenbank (wie PostgreSQL, MariaDB usw.) gibt es Python Module, die in der Regel - ebenso wie das in Python enthalten Modul für SQLite - der [DB-API 2.0](https://peps.python.org/pep-0249/) von Python folgen. Darin sind Standards festgelegt, wie eine Verbindung zur Datenbank hergestellt wird, wie ein Zeiger auf die Datenbank erstellt wird, wie SQL-Befehl übergeben werden usw.

Wenn die Nutzung einer relationalen Datenbank über sehr triviale Anwendungsfälle hinaus geht, ist der Einsatz von [SQLAlchemy](https://www.sqlalchemy.org/) empfehlenswert. SQLAlchemy bezeichnet sich selbst als "Python SQL toolkit and Object Relational Mapper", welches die zwei Hauptkomponenten von SQLAlchemy beschreibt: "Core" enthält Module zum Zugriff auf die Datenbank, Verbindungsmanagement, Anfrage der Datenbank und programmatischer Erstellung von Datenbankabfragen. Der ORM (ORM = Object Relational Mapper, auf Deutsch: Objektrelationale Abbildung) bietet alles, was man braucht, um via Python Klassen Datenmodelle darzustellen und in der Datenbank zu speichern. Der generelle Vorteil von SQLAlchemy ist, dass es einen vereinheitlichten Zugriff auf die gängigen relationalen Datenbanken bietet, d.h. beim Programmieren macht es keinen Unterschied, ob man z.B. SQLite oder PostgreSQL als Datenbank nutzt.

SQLAlchemy gibt es schon sehr lange, es ist sehr populär und entsprechend viel (und gerne) genutzt innerhalb der Python-Gemeinschaft. SQLAlchemy bietet ein [umfangreiches Tutorial](https://docs.sqlalchemy.org/en/20/tutorial/index.html).

*Randnotiz*: SQLAlchemy ist zusammen mit dem Django ORM sicherlich das (mit Abstand) meistgenutzte ORM für Python. Da das Django ORM voll in Django integriert ist (und außerhalb von Django nicht wirklich nutzbar ist), wird das Django ORM beim Erstellen von Webanwendungen mit Django genutzt und für alles andere bietet sich SQLAlchemy an.

## Weitere Tools
Wie schon Eingangs erwähnt gibt es noch unzählig mehr Python-Module. Zwei weitere, die relativ oft genutzt werden und nützlich sind, sollen hier noch Erwähnung finden.

Will man eine webbasierte / HTTP-basierte API abfragen, dann ist DAS Modul dafür [Requests](https://requests.readthedocs.io/en/latest/). Requests bietet eine sehr komfortable und nutzerfreundliche API zum Erstellen von HTTP-Requests sowie zum Auswerten der vom Server erhaltenen Antwort.

[more_itertools](https://more-itertools.readthedocs.io/en/stable/api.html) kann als eine Art Erweiterung für das itertools-Modul aus der Standardbibliothek gesehen werden. Es bietet dutzende weitere Funktionen zum Iterieren über Sequenzen, um diese aufzuteilen, filtern und gezielt daraus zu selektieren.

***

 * Vorheriges Kapitel: [Tipps und Anmerkungen](remarks.md)
 * Zurück zur [Startseite](index.md) 
