# Docker Mini-Projekt – Apache Webserver

In diesem Mini-Projekt wird ein kleiner Apache-Webserver mit Docker erstellt. Die Webseite soll später über Port 8080 erreichbar sein. Zusätzlich sollen die Website-Dateien und die Logdateien ausserhalb des Containers gespeichert werden, damit sie direkt zugänglich sind.

## 1. Verzeichnis für das Projekt erstellen

Zuerst wird ein neuer Ordner für das Projekt erstellt und in diesen Ordner gewechselt:

bash
Kopieren
Bearbeiten
mkdir webserver_mini_projekt
cd webserver_mini_projekt

## 2. Dockerfile erstellen

Im Projektverzeichnis wird eine Datei namens "Dockerfile" erstellt. Dieses Dockerfile beschreibt, wie das Docker-Image aufgebaut werden soll:

bash
Kopieren
Bearbeiten
touch Dockerfile
Danach wird folgender Inhalt in das Dockerfile eingefügt:

FROM httpd:2.4
COPY ./public-html/ /usr/local/apache2/htdocs/
LABEL maintainer="dein name"
EXPOSE 80
CMD ["httpd-foreground"]

Das bedeutet: Es wird das offizielle Apache-Image verwendet, die HTML-Dateien werden ins Standard-Webverzeichnis kopiert und der Apache-Server wird im Vordergrund gestartet.

## 3. Docker-Image bauen

Nun wird das Docker-Image mit dem Namen "mein-projekt" erstellt:

bash
Kopieren
Bearbeiten
docker build -t mein-projekt .

## 4. Container starten

Der Container wird mit folgendem Befehl gestartet:

bash
Kopieren
Bearbeiten
docker run -d --name mein_webserver -p 8080:80 \
-v /home/vmadmin/webserver_mini_projekt/public-html:/usr/local/apache2/htdocs/ \
-v /home/vmadmin/webserver_mini_projekt/logs:/var/log/apache2/ \
mein-projekt
Dabei wird der Container "mein_webserver" genannt. Port 8080 des Hostsystems wird mit Port 80 im Container verbunden. Ausserdem werden zwei lokale Verzeichnisse eingebunden: Eines für die Webseite (public-html) und eines für die Apache-Logdateien (logs).

## 5. HTML-Webseite erstellen

Wenn die Datei "index.html" noch nicht existiert, kann sie mit folgendem Befehl schnell erstellt werden:

bash
Kopieren
Bearbeiten
mkdir -p /home/vmadmin/webserver_mini_projekt/public-html
echo "<h1>Hallo, das ist meine Webseite 🤘</h1>" > /home/vmadmin/webserver_mini_projekt/public-html/index.html

## 6. Test im Browser

Zum Testen wird im Browser die folgende Adresse aufgerufen:

http://localhost:8080

Wenn alles korrekt eingerichtet ist, sollte die HTML-Seite angezeigt werden, die sich ausserhalb des Containers im Verzeichnis public-html befindet.

## 7. Erkenntnisse aus dem Projekt

Durch dieses Projekt habe ich gelernt, wie man ein Dockerfile erstellt und ein Docker-Image baut, wie man Apache im Container zum Laufen bringt, wie man lokale Verzeichnisse in den Container einbindet (Volumes), und wie man über Portweiterleitung eine Webseite von aussen erreichbar macht.

