---
title: Anleitung für das Einloggen bei LiDo3 außerhalb des Uni-Netzes für Windows-Rechner
date: 27-May-2020
author: Christina Meschede
---

## I) PUTTY

Voraussetzungen:

* Installation von PUTTY
* Zugang zum Statistik-Server
* Eine LiDo Zugangsberechtigung (siehe <https://service.tu-dortmund.de/group/intra/lido3neuantrag>)
* Einen Private-Key (generiert mithilfe PUTTYgen und erstellt beim LiDo-Antrag (Punkt 3))

**Achtung:** In der Anleitung wird zwischen der Uni-Kennung und dem
Statistik-Username unterschieden.

Am Anfang das zu PUTTY gehörende Programm ‚Pageant' öffnen und den
Private Key laden.

Nun PUTTY öffnen und eine Verbindung zu *shell.statistik.tu-dortmund.de*
einrichten:

![](.//media/image1.png)

Die Einstellungen speichern, indem ein Name frei gewählt wird (hier
*shell.statistik -- gw02.lido*) und mit *save* speichern.

Nun in den Reiter *Connection -- SSH* wechseln. Unter ‚Remote command'
folgenden Code eingeben:

```
ssh -l \<Uni-Kennung\> gw02.lido.tu-dortmund.de
```

(alternativ kann man natürlich auch den gateway-server gw01 nutzen)

![](.//media/image2.png)

Nun in den Reiter *Connection -- SSH -- Auth* wechseln und die beiden
Häkchen unter *Authentication parameters* setzen. Sowie die private key
file laden:

![](.//media/image3.png)

Zum Schluss die Einstellungen speichern, in dem erneut im ersten Reiter
*Session* auf Save gedrückt wird. Nun kann die Verbindung geöffnet
werden.

Nun - wie normalerweise auch - den Statistik-Username und das
Statistik-Passwort eingeben. Dann sollte man direkt weiter zu LiDo
geführt werden.

**Wenn ihr in Zukunft nun über den shell server auf LiDo3 wollt, müsst
ihr immer zunächst den Private Key in Pageant laden. Dann könnt ihr euch
über PUTTY und der darin gespeicherten Session einwählen.**

## II) WinSCP

Voraussetzungen:

* Installation von PUTTY
* Installation von WinSCP
* Zugang zum Statistik-Server
* Eine LiDo Zugangsberechtigung (siehe <https://service.tu-dortmund.de/group/intra/lido3neuantrag>)
* Einen Private-Key (generiert mithilfe PUTTYgen und erstellt beim LiDo-Antrag (Punkt 1))

Wie im Bild zusehen unter Rechnername den gewünschten LiDo Gateway
setzen mit Portnummer 22 und unter Benutzername den persönlichen
Uni-Kennung schreiben.

![](.//media/image4.png)

Dann den Button *Erweiter...* drücken und den Reiter *Verbindung --
Tunnel* wählen:

![](.//media/image5.png)

Dort die Verbindungsdaten von *shell* eintragen sowie den
Statistik-Username.

Nun den Reiter SSH -- Authentifizierung wählen und die Private key file
laden:

![](.//media/image6.png)

Der private key kann lokal auf deinem Rechner gespeichert sein (ich weiß
nicht, ob es Probleme gibt, wenn er auf dem `//store` gespeichert ist).

Nun auf *OK* drücken und im ursprünglichen Anmeldungsfenster auf
*Speichern* drücken.

Nun kannst du dich *Anmelden*.

WinSCP fragt dich nun nach einem Passwort. Hier wird dein
Statistik-Passwort benötigt.

Nun solltest du Zugriff auf das deine LiDo Verzeichnisse `/work/sm..` und
`/home/sm..` haben.
