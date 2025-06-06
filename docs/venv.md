# Virtuelle Umgebungen und Paketverwaltung mit pip

## Einführung

Python-Anwendungen verwenden oft Pakete und Module, die nicht Teil der Standardbibliothek sind. Manchmal benötigen Anwendungen eine bestimmte Version einer Bibliothek, weil diese mit anderen (neueren) Versionen nicht funktionieren.

Das bedeutet, dass eine Python-Installation möglicherweise nicht die Anforderungen aller Anwendungen erfüllen kann. Wenn Anwendung A die Version 1.0 eines bestimmten Moduls benötigt, Anwendung B aber die Version 2.0, dann stehen die Anforderungen in Konflikt, und die Installation von Version 1.0 oder 2.0 führt dazu, dass eine der Anwendung nicht laufen kann.

Die Lösung für dieses Problem besteht darin, eine "virtuelle Umgebung" (auf Englisch: "virtual environment", kurz: venv) zu erstellen, einen in sich geschlossenen Verzeichnisbaum, der eine Python-Installation für eine bestimmte Python-Version sowie eine Reihe von zusätzlichen Paketen enthält. "virtuelle Umgebung" ist hier *nicht* mit einer virtuellen Maschine (wie z.B. mit VirtualBox, Hyper-V, KVM/Qemu oder VMware erstellt) zu verwechseln! In einer virtuellen Umgebung ist lediglich der Python-Interpreter sowie die Python-Module von denen des Systems isoliert, auf allen anderen Systemressourcen wie das eigene Homeverzeichnis, Systemverzeichnisse etc. hat man den normalen / gewohnten Zugriff.

Verschiedene Anwendungen können dann unterschiedliche virtuelle Umgebungen verwenden. Um das frühere Beispiel der widersprüchlichen Anforderungen zu lösen, kann Anwendung A eine eigene virtuelle Umgebung mit der Version 1.0 installiert haben, während Anwendung B eine andere virtuelle Umgebung mit der Version 2.0 nutzt. Wenn Anwendung B ein Upgrade einer Bibliothek auf Version 3.0 benötigt, hat dies keine Auswirkungen auf die Umgebung von Anwendung A. Virtuelle Umgebungen eignen sich auch sehr gut, um neue, instabile oder experimentelle Versionen eines Pakets zu testen, ohne mit einer ggf. bereits installierten stabilen Version zu kollidieren.

Ein weiterer, damit einhergehender Vorteil ist, dass innerhalb einer virtuellen Umgebung installierten Python-Module nicht mit denen kollidieren, die das Betriebssystem mitbringt und benötigt. Wie gesagt sind die Module in der virtuellen Umgebung isoliert, d.h. solange man in einer virtuellen Umgebung arbeitet und installiert, kann man nicht aus Versehen das System in eine inkonsistenten, fehlerhaften oder gar unbrauchbaren Zustand bringen.

Von daher gilt es im Allgemeinen als "best practice", in einer virtuellen Python-Umgebung zu arbeiten, sobald man Module aus externen Quellen nachinstallieren möchte oder muss. Außerdem kann es je nach verwendetem Betriebssystem und genutzter pip-Version (Informationen zu pip siehe Abschnitt *"Python Pakete mit pip verwalten"* weiter unten) vorkommen, dass pip die Nutzung einer virtuellen Umgebung forciert.

## Ein Virtual Environment anlegen

Das Modul, mit dem virtuelle Umgebungen erstellt und verwaltet werden, heißt [venv](https://docs.python.org/3/library/venv.html). `venv` ist normalerweise in der Python Standardinstallation enthalten. Bei manchen Linuxdistributionen kann es notwendig sein, das Paket **python3-venv** über die Paketverwaltung des Systems nachzuinstallieren.

Um eine virtuelle Umgebung zu erstellen, entscheidet man sich für ein Verzeichnis, in dem man de venv ablegen will, und führen das Modul `venv` als Skript mit dem Zielverzeichnispfad aus:

```shell
python3 -m venv tutorial-env
```

Dadurch wird im aktuellen Verzeichnis das Verzeichnis `tutorial-env` erstellt, falls es noch nicht existiert, und auch diverse Verzeichnisse innerhalb dieses Verzeichnisses erstellt. Diese enthalten eine Kopie des Python-Interpreters und verschiedene unterstützende Dateien. Der Python-Interpreter, der im venv verwendet wird, ist der selbe, dann man auch bei einem Aufruf von `python3` im System startet.

Ein üblicher Verzeichnisort für eine virtuelle Umgebung ist `.venv`. Dieser Name sorgt dafür, dass das Verzeichnis in der Regel in der Shell versteckt bleibt und somit nicht auffällt, während der Name erklärt, warum das Verzeichnis existiert. Er verhindert auch Überschneidungen mit ``.env`` Umgebungsvariablendefinitionsdateien, die von einigen Werkzeugen unterstützt werden. Prinzipiell hat man beim Name der virtuellen Umgebung und ob meine seine virtuellen Umgebungen in ein oder mehrere Verzeichnisse legt, frei Wahl.

Sobald man eine virtuelle Umgebung erstellt hat, kann man diese aktivieren.

Unter Windows wiefolgt:

```bash
tutorial-env\Scripts\activate
```

Unter Linux, MacOS und anderen unixoiden Betriebssystemen:

```bash
source tutorial-env/bin/activate
```

Das Skript **activate** ist für die Bash-Shell geschrieben. Wenn man die csh oder fish Shell benutzen, gibt es die alternativen **activate.csh** und **activate.fish** Skripte, die man stattdessen verwenden sollte.

Wenn man die virtuelle Umgebung aktiviert, wird die Eingabeaufforderung der Shell so geändert, dass sie anzeigt, welche virtuelle Umgebung man benutzt. Und die Umgebung wird so geändert, dass der Aufruf von `python3` die Version und Installation aus der virtuellen Umgebung startet. Zum Beispiel:

```bash
$ source ~/envs/tutorial-env/bin/activate
(tutorial-env) $ python
Python 3.10.11 (tags/v3.10.11:7d4cc5a, Apr  5 2023, 00:38:17) [MSC v.1929 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> import sys
>>> sys.pyth
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: module 'sys' has no attribute 'pyth'. Did you mean: 'path'?
>>> sys.path
['', 'C:\\Program Files\\WindowsApps\\PythonSoftwareFoundation.Python.3.10_3.10.3056.0_x64__qbz5n2kfra8p0\\python310.zip',  ..., 'C:\\Users\\joche\\code\\rechnen', 'C:\\Users\\noisefloor\\code\\rechnen\\lib\\site-packages']
>>>
```

Zum Deaktivieren des venv führt man folgenden Befehl im Terminal aus:

```bash
deactivate
```

## Python Pakete mit pip verwalten

Der Standardweg (und gängigste Weg) zur Installation von Python-Pakete ist [pip](https://docs.python.org/3/installing/index.html). pip dient ebenfalls zum Aktualisieren und Entfernen von Paketen.  Standardmäßig installiert pip Pakete aus dem [Python Package Index](https://pypi.org). Man kann den Python Package Index durchsuchen, indem man ihn in Ihrem Webbrowser aufruft.

pip ist normalerweise in der Standardinstallation von Python enthalten. Bei manchen Linuxdistributionen kann es notwendig sein, das Paket **python3-pip** über die Paketverwaltung des Systems nachzuinstallieren.

pip hat eine ganze Reihe von Unterbefehlen: "install", "uninstall", "freeze", usw.  Mehr dazu ist in der [Dokumentation von pip](https://pip.pypa.io/en/stable/cli/) zu finden.

Man kann die neueste Version eines Pakets installieren, indem man den Namen des Pakets angibt:

```bash
(tutorial-env) $ python3 -m pip install novas
Collecting novas
Downloading novas-3.1.1.3.tar.gz (136kB)
Installing collected packages: novas
Running setup.py install for novas
Successfully installed novas-3.1.1.3
```

Man kann auch eine bestimmte Version eines Pakets installieren, indem man den Paketnamen gefolgt von ``==`` und der Versionsnummer angibt:

```bash
(tutorial-env) $ python3 -m pip install requests==2.6.0
Collecting requests==2.6.0
Using cached requests-2.6.0-py2.py3-none-any.whl
Installing collected packages: requests
Successfully installed requests-2.6.0
```

Wenn man diesen Befehl erneut ausführen, wird pip feststellen, dass die angeforderte Version bereits installiert ist und tut nichts. Man kann eine andere Versionsnummer angeben, um diese Version zu erhalten.

Man kann `python3 -m pip install --upgrade` ausführen, um das Paket auf die neueste Version zu aktualisieren:

```bash
(tutorial-env) $ python3 -m pip install --upgrade requests
Collecting requests
Installing collected packages: requests
Found existing installation: requests 2.6.0
Uninstalling requests-2.6.0:
Successfully uninstalled requests-2.6.0
Successfully installed requests-2.7.0
```

Wie in der Beispielausgabe oben zu sehen ist wird bei einem Upgrade erst die vorherige Version deinstalliert und dann die neuere installiert.

*Hinweis*: `python3 -m pip install --upgrade name_des_pakets` macht keinen Unterschied zwischen Bugfix Versionen, neuen Minor-Versionen und neuen Haupt-Versionen, es wird immer die neuste Version installiert. Dies ist eventuell nicht immer wünschenswert, wenn man z.B. aufgrund von größeren Änderungen zwischen Version X und Y eines Pakets lieber bei Version X bleiben würde. In diesem Fall darf man pip nicht mit der Option `--upgrade` ausführen, sondern muss z.B. auf der Projektseite nachschauen, welches die neuste Version der Hauptversion X ist. Um beim obigen Beispiel zu bleiben: wenn man z.B. bei `requests` in der Version 2.6.x bleiben möchte, anstatt 2.7.0 zu installieren und es gäbe eine Version 2.6.1, dann würde man diese über den Befehl `python3 -m pip install requests==2.6.1` installieren (und dabei würde die Version 2.6.0 deinstalliert).

`python -m pip uninstall` gefolgt von ein oder mehreren Paketnamen entfernt diese Pakete aus der virtuellen Umgebung.

`python -m pip show` zeigt Informationen zu einem Paket an:

```bash
(tutorial-env) $ python -m pip show requests
---
Metadata-Version: 2.0
Name: requests
Version: 2.7.0
Summary: Python HTTP for Humans.
Home-page: http://python-requests.org
Author: Kenneth Reitz
Author-email: me@kennethreitz.com
License: Apache 2.0
Location: /Users/akuchling/envs/tutorial-env/lib/python3.4/site-packages
Requires:
```

`python -m pip list` zeigt alle Pakete, die in der aktuell aktivierten virtuellen Umgebung installiert sind:

```bash
(tutorial-env) $ python -m pip list
novas (3.1.1.3)
numpy (1.9.2)
pip (7.0.3)
requests (2.7.0)
setuptools (16.0)
```

`python -m pip freeze` erzeugt eine ähnliche Liste der installierten Pakete, aber die Ausgabe verwendet das Format, das `python -m pip install` erwartet. Eine übliche Konvention ist es, diese Liste in eine Datei **requirements.txt** zu schreiben:

```bash
tutorial-env) $ python -m pip freeze > requirements.txt
(tutorial-env) $ cat requirements.txt
novas==3.1.1.3
numpy==1.9.2
requests==2.7.0
```

Die **requirements.txt** kann dann der Versionskontrolle übergeben und als Teil einer Anwendung ausgeliefert werden.  Die Benutzer können dann alle erforderlichen Pakete mit `install -r` installieren:

```bash
(tutorial-env) $ python -m pip install -r requirements.txt
Collecting novas==3.1.1.3 (from -r requirements.txt (line 1))
...
Collecting numpy==1.9.2 (from -r requirements.txt (line 2))
...
Collecting requests==2.7.0 (from -r requirements.txt (line 3))
...
Installing collected packages: novas, numpy, requests
Running setup.py install for novas
Successfully installed novas-3.1.1.3 numpy-1.9.2 requests-2.7.0
```

`pip` kennt noch mehr Optionen. Weitere Beispiel sin in [Installing Python Modules](https://docs.python.org/3/installing/index.html) in der Python-Dokumentation gezeigt.

Wenn man selber ein Python Modul im Python Packge Index veröffentlichen möchte findet man auf der Seite [Packaging Python Projects](https://packaging.python.org/en/latest/tutorials/packaging-projects/) in der Python-Dokumentation.

***

 * Nächstes Kapitel: [Was kommt als Nächstes?](whatnow.md)
 * Vorheriges Kapitel: [Eine (kurze) Tour durch die Standardbibliothek](stdlib.md)
