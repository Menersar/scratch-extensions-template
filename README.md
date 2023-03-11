# Vorwort und Credits

Da ich bei allen Anleitungen und Dokumentationen zur Erstellung von Scratch-3-Erweiterungen auf diverse Probleme und Error, meist aufgrund diverser Paket-Inkompatibilitäten, gestoßen bin habe ich nach langer Eigenrecherche die folgende Anleitung zur Erstellung von Scratch-3-Erweiterungen zusammengetragen.

Informationen zur Erstellung der Anleitung sind zum Großteil der folgenden Seite entnommen.
https://medium.com/@hiroyuki.osaki/how-to-develop-your-own-block-for-scratch-3-0-1b5892026421

Als Grundlage für die Projekte scratch-vm und scratch-gui sowie für Tests mit den enthaltenen Scratch-Erweiterungen hat das folgende GitHub-Repository gedient.
https://github.com/MrYsLab/s3onegpio

# Nutzung

## 1 Entwicklungsumgebung vorbereiten

## Raspberry Pi OS
	
nvm-Verion-Manager installieren.
	curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash

NodeJS vom Sytem entfernen.
	sudo npm cache clean -f
	sudo apt remove nodejs npm

Node-Version 16.0.0 neu installieren.
	sudo npm install -g n
	sudo n 16.0.0

Node-Version überprüfen (Ausgabe: v16.0.0).
	node -v

yarn installieren.
(Oft wird npm, anstelle von yarn, verwendet aber mit yarn hat es bei mir ohne Error funktioniert.)
	sudo npm uninstall -g yarn
	sudo npm install --global yarn

GitHub-Repository über folgenden Link herunterladen und entpacken.
	https://github.com/Menersar/scratch-extensions

## 2 Scratch-Projekte installieren
		
Die Projekte scratch-vm und scratch-gui müssen zusammen modifiziert und kompiliert werden, deshalb sollten sie, über folgende Terminal-Befehle, verbunden werden.
(Das Projekt scratch-gui wird als Parent-Project festgelegt, scratch-vm wird mit dem Parent gelinked)
	cd scratch-extensions
	cd scratch-vm 
	cd yarn install 
	cd yarn link
	cd ..
	cd scratch-gui 
	yarn link scratch-vm 
	yarn install
		
## 3 GUI starten -
		
Die Scratch-GUI wird über folgende Terminal-Befehle gestartet.
	cd scratch-gui
	yarn start

Nach erfolgreichem Kompilieren wird die folgende Ausgabe im Terminal angezeigt und der Scratch-Service ist startet.
	Compiled successfully.

Dann ist die Scratch-Oberfläche über folgende Adresse über einen Browser erreichbar.
(Aus der Terminal-Ausgabe Project is running at http://0.0.0.0:8601/ entnehmbar.)
	http://localhost:8601
		
Werden Änderungen in den Projekten scratch-vm oder scratch-gui vorgenommen und gespeichert, und während der Scratch-Service auf http://0.0.0.0:8601/ läuft, wird der Kompilierungsvorgang automatisch ausgeführt.
(Bei erfolgreichem Kompilieren ist werden Änderungen, wie beispielsweise neue Erweiterungen, in der Scratch-GUI übernommen und dargestellt.

## 4 Scratch-Block implementieren	
		
Jede Extension kann einen oder mehrere Blöcke haben
	
1. Einen Ordner in folgendem Pfad hinzufügen.
(Den Ordner scratch3_EXTENSION-NAME benennen; statt EXTENSION-NAME den Namen der neuen Erweiterung angeben.)
	scratch-vm/src/extensions/scratch3_EXTENSION-NAME

2. In dem Ordner eine neue Datei, wie folgt, anlegen.
(Die Datei index.js benennen.)
	scratch-vm/src/extensions/scratch3_EXTENSION-NAME/index.js

3. In der Datei werden die Blöcke der Erweiterung angegeben und definiert.

4. Die Datei, die der Implementierung des Erweiterungsmenüs dient, zu finden unter folgendem Pfad, öffnen.
	scratch-vm/src/extension-support/extension-manager.js

5. In der Datei die neue Erweiterung, wie folgt, angeben und so dem Projekt als Erweiterung hinzufügen.
(Die Zeile EXTENSION-ID: () => require ('EXTENSION-RELATIVE-PATH') in der Datei hinzufügen; statt EXTENSION-ID (aus index.js) und RELATIVER-PFAD-ZUR-NEUEN-ERWEITERUNG entsprechend die ID und den Pfad (zu scratch3_EXTENSION-NAME) angeben.)
	newblocks: () => require('../extensions/scratch3_EXTENSION-NAME')

## 5 Die GUI implementieren 

Der neu implementierte Scratch-Block muss noch in die Erweiterungsbibliothek von Scratch hinzugefügt werden.
	
1. Um die Erweiterung mit einem Bild in der Erweiterungsbibliothek darzustellen einen Ordner wie folget, in dem angegebenen Pfad, hinzufügen.
(Den Ordner EXTENSION-NAME benennen (keine Pflicht, aber einfacher die Erweiterung zu finden); statt EXTENSION-NAME den Namen der neuen Erweiterung angeben.)
	scratch-gui/src/lib/libraries/extensions/EXTENSION-NAME

2. In den Ordner zwei Bilder, für die Darstellung der Erweiterung in der Scratch-Bibliothek, wie folgt platzieren.
(Die Größe der Bilddatei für den Hintergrund des Eintrags beträgt 600 x 372, die Größe des Icons 180 x 180.)
(Die Bilddatei für den Hintergrund EXTENSION-NAME benennen, für das Icon EXTENSION-NAME-small (keine Pflicht, aber einfacher die Assets der Erweiterung zu finden); statt EXTENSION-NAME den Namen der neuen Erweiterung angeben.)
(Als Format habe ich png, jpg und svg auf korrekte Funktionsweise getestet; statt IMAGE-FORMAT das entsprechende Format der jeweiligen Bilddatei der neuen Erweiterung angeben.)
	scratch-gui/src/lib/libraries/extensions/EXTENSION-NAME/EXTENSION-NAME-small.IMAGE-FORMAT
	scratch-gui/src/lib/libraries/extensions/EXTENSION-NAME/EXTENSION-NAME.IMAGE-FORMAT

3. Die Datei index.jsx, zu finden unter folgendem Pfad, öffnen.
	scratch-gui/src/lib/libraries/extensions/index.jsx

4. In der Datei alle notwendigen Informationen und Referenzen für die Darstellung der neuen Erweiterung in der Scratch-Bibliothek angeben.
	
5. Die Scratch-GUI starten, siehe dazu 3 GUI starten.
