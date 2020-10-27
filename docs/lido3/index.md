---
title: LiDO-Kurzanleitung
date: 25-Sep-2020
author: Jona Lilienthal
---

# LiDO-Kurzanleitung

Diese Anleitung ist dazu gedacht Mitarbeitern der Fakultät Statistik eine Kurzeinführung in die Welt der R-Berechnung auf dem Rechencluster LiDO3 des ITMC zu geben. 
Hierbei bezieht sich diese auf den Stand vom März 2020. 
Da es immer zu System-Änderungen kommen kann (durch eine Neugestaltung von LiDO oder auch nur durch Systemaktualisierungen) können einzelne Abschnitte obsolet werden. 
Außerdem stellt alles Geschriebene lediglich persönliches (Halb-)wissen dar, das ich mir zur Bedienung von LiDO erarbeitet habe. 
Vieles ist daher unter Umständen nicht vollumfänglich korrekt oder verkürzt dargestellt. 
Sollten sich Änderungen ergeben haben, bzw. können bessere Informationen eingepflegt werden, so wird darum gebeten dies zu tun. 
Dieses Dokument selbst ist in einfachem Markdown geschrieben, welches mit `pandoc` in eine PDF umgewandelt wird. 

Diese Kurzanleitung setzt sich aus drei Abschnitten zusammen. 
Im ersten Teil werden Grundlagen zu Linux und der Linux-Kommandozeile beschrieben. 
Dies soll ermöglichen sich auf LiDO (ein Linux-System) sicher zu bewegen. 
Grundsätzlich ist es möglich mit minimalen Linux-Kenntnissen R-Skripte berechnen zu lassen. 
Es ist aber empfehlenswert sich zumindest Grundkenntnisse anzueignen um zu wissen wie das Dateisystem aufgebaut ist und wie Dateien verschoben, kopiert, gelöscht, und bearbeitet werden können. 

Der zweite Teil beschäftigt sich mit LiDO selbst. 
Der Aufbau der Dateistruktur, das Modulsystem, und der Scheduler Slurm werden beschrieben und erklärt, wie R im interaktiven Modus oder im Batch-Modus genutzt werden kann. 

Im dritten Teil sollen häufig auftretende Probleme gesammelt und Hilfestellung dazu gegeben werden. 

# Linux-Grundlagen

## Dateisystem

Linux und Unix-Systeme verwenden eine hierarchische Dateistruktur. 
Im Gegensatz zu Windows-Systemen, bei denen sich die Dateistruktur ausgehend einzelner Festplatten/Partitionen entwickelt (typischerweise von C: aus), startet die Dateistruktur unter Linux immer vom Grundverzeichnis 

    /

aus. 
Von dort aus verästeln sich Ordner und Dateien, wobei dies im Dateipfad getrennt durch `/` angegeben wird. 
Der Pfad 

    /home/benutzername/datei.R

beschreibt also die Datei `datei.R`, welche im Ordner `benutzername` liegt, welcher ein Unterordner des Ordners `home` ist. 

Das für den Benutzer wichtigste Verzeichnis ist für gewöhnlich das Home-Verzeichnis unter `/home/benutzername`. 
Hier sollten (können) eigene Dateien abgespeichert werden. 
Unter Lido gibt es zusätzlich das Arbeitsverzeichnis `/work/benutzername`, dazu später mehr. 

Werden Pfade zu Dateien/Ordnern angegeben, so werden diese absolut oder relativ referenziert. 
Absolute Pfade beginnen beim Grundverzeichnis und starten daher mit `/`, z.B. `/home/benutzername/simulation/version-1`. 
Relative Pfade beginnen ohne `/` und beziehen sich auf den Pfad vom aktuellen Ordner aus: Im Fall dass wir uns im Home-Verzeichnis (`/home/benutzername/`) befinden also z.B. `simulation/version-1`. 

In relativen Pfaden muss gelegentlich aufwärts gegangen werden. 
Das jeweilige Oberverzeichnisse erreicht man mit `..`, d.h. befinden wir uns in `/home/benutzername/simulation` so wird durch `../datei.R` die Datei unter `/home/benutzername/datei.R` referenziert. 

Durch `~` verweist man für gewöhnlich auf das eigene Homeverzeichnis, d.h. anstatt `/home/benutzername/simulation/version-1` kann auch `~/simulation/version-1` geschrieben werden (vorausgesetzt bei `benutzername` handelt es sich um den eigenen Account). 

Versteckte Dateien oder Ordner beginnen ihren Namen mit einem Punkt: ".". 
Solche Dateien werden in Dateimanagern oder in Auflistungen nicht automatisch angezeigt. 
Oftmals sind in solchen Dateien Konfigurationen/Einstellungen abgespeichert. 


## Shell, Terminal, Bash

Eine Shell bezeichnet die Schnittstelle, die es uns ermöglicht mit dem Computersystem zu interagieren. 
Dies können grafische Systeme sein, wie die Oberfläche von Windows oder Gnome/KDE unter Linux, oder kommando-basierte Systeme, wie die Eingabeaufforderung unter Windows oder ein Terminal unter Linux/Mac. 
Ein Terminal ist also eine kommando-basierte Shell. 
Unter Unix-Systemen (und damit unter Linux) gibt es eine Vielzahl von möglichen Terminals, die unterschiedliche Befehle und Syntaxen mitbringen. 
Das "Standard"-Terminal, und das einzige auf das sich hier bezogen wird, ist die Bash, da sie auf LiDO zum Einsatz kommt. 
Sofern nachfolgend der Begriff Terminal verwendet wird, sei hiermit immer die Bash gemeint. 

### Kommandozeile

Nachdem wir uns erfolgreich in ein Terminal eingeloggt haben (z.B. auf LiDO), befinden wir uns in einer Eingabezeile. 
Häufig werden zu Beginn der Zeile Informationen über den Benutzer, Rechner, und aktuellen Ordner angegeben. 
In meiner lokalen Linux-Installation erscheint z.B. 

    lilienthal@ThinkPad-X260:~$

was bedeutet, dass der Account `lilienthal` auf dem Rechner `ThinkPad-X260` eingeloggt ist und wir uns im Ordner `~` (also dem Home-Ordner) befinden.  
Unter LiDO erhalte ich dagegen eine etwas andere Darstellung: 

    [smjolili@gw02 ~]$ 

In diesem Fall ist der Account `smjolili` am Rechner `gw02` (einer der beiden LiDO-Gatewayserver) eingeloggt und wir befinden uns im Home-Ordner. 

Anhand dieser Angaben kann man sichergehen, dass der korrekte Benutzer am richtigen Rechner eingeloggt ist und was der aktuelle Ordner ist. 
Hinter dieser Angabe beginnt dann der Bereich in dem eigene Eingaben gemacht werden können. 
Dies können einfache Bash-Kommandos und -Befehle sein oder das Starten von Skripten und Programmen. 
Eine Eingabe wird durch \[Enter\] abgeschickt und die Kommandozeile erst nach Beendigung des Befehls wieder freigegeben. 

#### Beispiel: 
Ein einfacher Befehl ist z.B. `echo`, welcher Zeichenketten ausgibt. 
Durch die Eingabe von 

    echo "Hallo"

erhält man daher eine Ausgabezeile in der Hallo steht. 

#### Beispiel: 
Innerhalb der Bash können Variablen gesetzt werden, indem die Zuweisung 

    var="Welt"

abgeschickt wird. 
Anschließend wird auf den Variablenwert mittels `$variablenname` zugegriffen, z.B. also durch 

    echo "Hallo $var"

#### Beispiel: 
Die Bash kennt auch Bedingungen und Kontrollstrukturen. 
Durch 

    for i in {1..3}; do echo $i; done

werden die Zahlen 1, 2, 3 ausgegeben. 


### Befehlsyntax

Die Struktur von Bash-Befehlen gestaltet sich wie folgt: 

    Befehlname [Optionen] [Argumente]

Auf den Befehlsname folgen keine, einzelne, oder mehrere Optionen und Argumente. 
Optionen starten dabei mit zwei Minussen gefolgt der Optionsbezeichnung: `--option`. 
Für viele häufig verwendete Optionen gibt es Kurzschreibweisen mit einem Minus und einem Zeichen: `-o`. 
Argumente folgen durch Leerzeichen getrennt am Ende. 

#### Beispiel: 
Der Befehl `ls` listet alle Ordner und Dateien eines Verzeichnisses auf. 
Er benötigt keine Optionen und Argumente. Die Ausführung von `ls` bezieht sich dann auf den aktuellen Ordner. 
Fügt man dagegen einen Pfad als Argument hinzu, 

	ls ordner 

so bezieht sich die Funktion auf diesen Ordner. 
Mehrere Optionen sind verfügbar: Durch z.B. `-l` erhält man die Ausgabe in einer Listenform mit mehr Details. 
Durch `-h` werden Dateigrößen besser lesbar dargestellt. 
Mittels `-a` werden alle Dateien, auch versteckte, angezeigt. 
Mehrere Kurz-Optionen können auch kombiniert werden, sodass nur ein Minus benötigt wird: `ls -lha`. 

#### Beispiel: 
Der Befehl `cp` kopiert eine Datei oder Verzeichnis. 
Hierzu werden zwei Argumente benötigt, der Quellpfad und der Zielpfad. 
Der Befehl 

	cp datei.r copy.r 

erzeugt eine Kopie der Datei `datei.r` mit dem Namen `copy.r`. 
Hierbei werden möglicherweise existierende Dateien überschrieben. 
Wünscht man dies nicht, kann dies durch die Option `-n` verhindert werden. 
Durch die Option `-u` wird eine eventuell existierende Datei nur dann ersetzt, wenn die Quelldatei neuer ist. 


### Wichtige Befehle

Befehl | Funktion 
:-----:|----------------------------------------------------------------------------
`ls`   | Anzeigen von Dateien/Ordnern im aktuellen oder in einem anderen Verzeichnis
`cd`   | Wechsel des aktuellen Verzeichnisses
`pwm`  | Anzeige des Pfads des aktuellen Verzeichnisses
`exit` | Verlassen des aktuellen Terminals
`cp`   | Kopieren von Dateien
`mv`   | Verschieben von Dateien
`rm`   | Löschen von Dateien
`mkdir`| Erstellen eines Ordners
`more` | Darstellung einer Textdatei
`less` | Wie `more`, aber mit mehr Optionen (Lesemodus beenden mit `q`)
`man`  | Manual eines Befehls anzeigen (Beenden mit `q`)


### Hilfeseiten

Es gibt zwei Möglichkeiten Hilfe zu Befehlen zu erhalten (neben einer Internet-Recherche). 
Durch den Aufruf 

    man befehl

ruft man die Manual-Seite des Befehls auf. 
Hier sind oftmals ausführliche Erklärungen zur Funktionsweise, Optionen, und Argumenten dargestellt. 
Eine andere Möglichkeit ist es, die Hilfe-Option zu verwenden, 

    befehl --help 

die eine kürzere Funktionsübersicht enthält. 


### Bash-Features

Zuletzt sei noch kurz auf wichtige Features der Bash eingegangen, welche uns das Arbeiten an einer Kommandozeile erheblich erleichtern. 

#### History
Die zuletzt eingegebenen Befehle werden in einer History gespeichert (genauer: in der Datei `~/.bash_history`). 
Dies ermöglicht, dass wir diese Durchsuchen können. 

Befinden wir uns in der Kommandozeile, so kann (wie bei `R`) mittels der Pfeiltasten (\[hoch\] und \[runter\]) durch die letzten Befehle geblättert werden. 

Soll der zuletzt ausgeführte Befehl nochmal ausgeführt werden, so kann dies auch durch `!!` ausgelöst werden. 

Es kann auch in der History gesucht werden. 
Dazu drückt man gleichzeitig \[Strg+R\] und tippt dann die ersten Buchstaben des Befehls ein (genauso funktioniert es auch in `R`). 
Passende Treffer werden angezeigt und können mittels \[Enter\] direkt abgeschickt werden oder mittels Pfeiltaste \[rechts\] zur Bearbeitung ausgewählt werden. 

#### Tab-Vervollständigung

Wie in `R` lassen sich Befehle und Ordner/Dateien in der Bash durch \[Tab\] vervollständigen. 
Sofern die ersten Zeichen bereits ausreichend sind, wird der passende Befehl, Ordner, oder Datei eingesetzt. 
Gibt es noch mehrere Möglichkeiten, so wird bis zu dem Punkt vervollständigt ab dem sich die Möglichkeiten unterscheiden. 
Durch ein zweifaches \[Tab\] lassen sich dann diese Möglichkeiten anzeigen. 

### Wichtige Programme 

Weitere wichtige Programme zur Arbeit auf LiDO (oder anderen Linux-Systemen) sind ein Editor wie `vim` oder `nano` und ein Terminal-Multiplexer wie `tmux`. 
Diese Programme sind nicht zwingend notwendig, erleichtern aber die Arbeit da Umwege vermieden werden (Editierung der Dateien direkt auf LiDO anstatt Runterladen, Editieren, Hochladen) und weil eine bequemere Arbeitsumgebung geschaffen wird (mehrere Konsolen-"Fenster", Schutz gegen Verbindungsabbrüche). 

#### Editor

Auf LiDO sind die Editoren `nano`, `vim` und `emacs` installiert. 
Oftmals ist es sinnvoller kleine Änderungen an R-Skripten direkt auf LiDO vorzunehmen, daher ist es empfehlenswert sich die Funktionsweise solcher Terminal-Editoren anzuschauen. 

Anfängern sei dabei eher `nano` nahegelegt, der einfach in der Verwendung ist. 
Das Öffnen einer Datei erfolgt mittels

    nano dateiname

Im oberen Bereich kann man sich mittels Pfeiltasten bewegen und Änderungen direkt vornehmen. 
Im unteren Bereich befindet sich eine Art Menüleiste, in der mögliche Optionen angegeben werden. 
Hierbei wird jeweils angegeben, wie die Option aufgerufen wird. 
Das Zeichen `^` bedeutet dabei, dass die \[Strg\]-Taste, die Zeichen `M-`, dass die \[Alt\]-Taste zusätzlich gedrückt werden muss. 
Den Editor beenden kann man folglich mit \[Strg+X\], etwas rückgängig machen mit \[Alt+U\]. 

Die Editoren `vim` und `emacs` sind mächtiger, aber auch schwieriger zu erlernen. 
Eine gute Möglichkeit `vim` zu erlernen bietet der `vimtutor`, den man über den gleichnamigen Befehl in der Konsole starten kann. 

#### tmux

Wenn man über eine Fernverbindung an einem Computer arbeitet (wie auf dem Rechencluster der Statistik oder auf LiDO) begegnet man irgendwann dem Problem von Verbindungsabbrüchen. 
In so einem Fall verliert man den Zugriff auf die Konsole und muss sich erneut an dem Rechner einloggen, wodurch eine neue Sitzung gestartet wird. 
Dies ist insbesondere problematisch, wenn in einer interaktiven R-Sitzung gerechnet wurde, da man an diesen Inhalt nicht mehr gelangt. 

Eine Lösung für dieses Problem stellt das Programm `tmux` dar, ein sogenannter Terminal-Multiplexer, der es erlaubt mehrere Terminalsitzungen zu verwalten und zu speichern. 
Auf diese Weise kann eine Terminalsitzung verlassen (freiwillig oder unfreiwillig) und später fortgeführt werden. 
Außerdem ermöglicht `tmux` (unter anderem) das Aufteilen der Konsole in unterschiedliche Ausschnitte in denen jeweils separate Terminalsitzungen laufen. 

Da `tmux` sehr mächtig ist werden hier nur die absoluten Grundlagen beschrieben. 

Eine neue `tmux`-Sitzung startet man über den Befehl 

    tmux

Es öffnet sich eine neue Bash-Sitzung, in welcher wir normal arbeiten können (Dateioperationen, Editoren benutzen, Skripte abschicken/starten, ...). 
Soll die Sitzung verlassen werden, ohne sie zu beenden, so wird dazu die Tastenkombination \[Strg+b\]+\[d\] (also Steuerung und b **gleichzeitig** drücken, **dann** d (detach) drücken) verwendet. 
Grundsätzlich funktionieren `tmux`-Befehle *innerhalb* der `tmux`-Sitzung immer so, dass zunächst \[Strg+b\] gedrückt wird und dann eine weitere Taste. 

Nach "detachen" der Sitzung befinden wir uns wieder in der Ursprungskonsole, von der wir `tmux` aufgerufen haben. 
Die aktuell laufenden Sitzung (mehrere können gleichzeitig verwaltet werden) können wir mittels 
   
    tmux ls

anzeigen, welches uns Informationen über die Anzahl der Fenster (innerhalb einer Sitzung können auch mehrere Fenster vorhanden sein) und den Zeitpunkt der Erstellung anzeigt. 
Wichtig ist die Kennzeichnung der Sitzung, die sich direkt zu Beginn der Zeile befindet. 
Standardmäßig werden die Sitzungen einfach durchnummeriert, aber auch Namen können vergeben werden. 

Um zurück in unsere Sitzung zu kommen wird der Befehl 

    tmux a[ttach] -t kennzeichnung

verwendet. 
Da unsere Sitzung die Kennzeichnung `0` hat, verwenden wir daher `tmux attach -t 0` (wir können das `attach` auch abkürzen und `tmux a -t 0` verwenden) und verbinden uns auf diese Weise zurück auf die gestartete Sitzung. 

Soll eine Sitzung mit einem Namen (anstatt der Durchnummerierung) erzeugt werden, so wird dafür der Befehl 

    tmux new -s name

verwendet. 
Eine bereits bestehende Sitzung kann mittels 

    tmux rename -t kennzeichnung neue-kennzeichnung

umbenannt werden. 

Innerhalb einer `tmux`-Sitzung lässt sich die Konsole in unterschiedliche Bereiche aufteilen. 
Dies ermöglicht z.B. in einer Aufteilung einen Editor laufen zu haben und in einer anderen Dateioperationen durchzuführen oder `R` zu verwenden. 
Eine horizontale Aufteilung wird über \[Strg+b\]+\[%\], eine vertikale über \[Strg+b\]+\["\] erzeugt. 
Zwischen den Aufteilungen kann durch \[Strg+b\]+\[Pfeiltaste\] gewechselt werden. 
Die aktive Aufteilung erkennt man an den grünen Rändern. 
Eine Aufteilung wird über das Verlassen der Konsole, also `exit` geschlossen. 
Aufteilungsgrößen können über \[Strg+b\]+\[Strg+Pfeiltaste\] verändert werden. 

Komplett beendet kann eine Sitzung entweder dadurch, dass innerhalb der `tmux`-Sitzung alle Fenster oder Aufteilungen beendet werden (mittels `exit`) oder von außerhalb über 

    tmux kill-session -t kennzeichnung



# LiDO

LiDO3 ist der Linux Cluster Dortmund in der dritten Generation, der seit 2018 an der Uni zur Verfügung steht. 
Im Vergleich zum Rechencluster der Fakultät Statistik bietet er erheblich mehr Rechenpower (8160 CPU-Kerne, 30 TB RAM, >1280 TB Speicher). 

LiDO verwendet ein Modulsystem um Software nach Bedarf zur Verfügung zu stellen. 
Auf diese Weise stehen oftmals auch unterschiedliche Softwareversionen zur Verfügung (bei `R` z.B. 3.4.3, 3.5.1, 3.6.1, und 4.0.0). 
Zur Priorisierung der Rechenaufgaben wird SLURM als "Workload Manager" verwendet. 
Hierbei gelangen Rechenanfragen zunächst in eine Warteschlange und werden je nach angefragten Ressourcen (Kerne, Speicher, Zeit) priorisiert, auf Rechenknoten verteilt, und abgearbeitet. 

Die nachfolgenden Abschnitte enthalten nähere Informationen zu diesen Punkten. 


## Login / Gateway-Server

Auf LiDO gelangt man über einen der beiden Gateway-Server, 

    gw01.lido.tu-dortmund.de
    gw02.lido.tu-dortmund.de

indem man sich per SSH (unter Windows mittels Putty, unter Linux `ssh benutzername@gw01.lido.tu-dortmund.de` im Terminal) und mit dem persönlichen SSH-Key anmeldet. 
Mehr Informationen hierzu in der Lido-Einführung des ITMC (`https://www.lido.tu-dortmund.de/cms/de/LiDO3/lido3kurz.pdf`). 
Speziell zum Einloggen unter Windows und vom Homeoffice aus: Siehe die E-Mail von Christina Meschede am 19.03.20 an den Fakultätsverteiler. 

Auf den Gateway-Servern können Module geladen werden (also u.a. `R`), sie sollten aber nicht für resourcen-aufwendige Rechnungen verwendet werden. 
Die Gateways dienen lediglich zur Verwaltung der Dateien (Kopieren, Verschieben, Editieren) sowie zum Starten und Verwalten der laufenden Aufgaben (Rechen-Jobs). 


## Dateisystem

Auf LiDO gibt es drei unterschiedliche Verzeichnisse in denen sich Benutzer bewegen. 

Das erste Verzeichnis ist das Home-Verzeichnis, `/home/benutzername`. 
Jeder Nutzer hat hier ein Limit von **32 GB Speicher**, welcher **regelmäßig gesichert** wird. 
Auf Rechenknoten kann in dieses Verzeichnis nicht beschrieben, sondern lediglich gelesen werden. 

Im Arbeitsverzeichnis, `/work/benutzername`, stehen **1 TB** **ungesichert**er Speicher zur Verfügung. 

Werden *extrem viele*, *zeitintensive* Dateioperationen mit *kleinen Dateien* durchgeführt, sollen diese im Verzeichnis `/scratch` durchgeführt werden. 
Hierzu müssen die Dateien zunächst dorthin kopiert, dort verarbeitet, und anschließend nach `/work/benutzername` kopiert werden. 

Für gewöhnliche Rechnungen sollten die ersten beiden Verzeichnisse ausreichen. 
Hierbei ist darauf zu achten, dass das work-Verzeichnis innerhalb des Jobs verwendet wird (da home nicht beschreibbar ist). 
Ergebnisse können nach Berechnung in das home-Verzeichnis kopiert werden um das Risiko von Datenverlust zu minimieren. 


## Module

Software ist unter LiDO in Modulen organisiert, die bei Bedarf geladen werden können. 
Die Verwaltung der laufenden Module geschieht über den Befehl `module` und seinen Unterfunktionen. 
Nachfolgend sind die wichtigsten Funktionen aufgeführt. 

Befehl                  | Beschreibung
----------------------- | ------------
`module avail` 	     	| Ausgabe einer List der verfügbaren Module 
`module list`  		    | Ausgabe der aktuell geladenen Module
`module add modulename` | Hinzufügen/Laden des Moduls `modulename`
`module rm modulename`  | Entfernen/Entladen des Moduls `modulename`
`module purge` 	    	| Entfernen/Entladen aller geladenen Module

Module sind immer nur für die aktuelle Sitzung geladen. 
Wird das Terminal verlassen und neu gestartet (über `exit` und neuem Login) oder ein neues Terminal gestartet (über `tmux`), so sind keine Module aktive. 
Sollen Module in jeder Sitzung aktiv sein, so kann dies durch einen Eintrag in der Datei `~/.bash_profile` festgelegt werden. 
Diese Datei wird bei jedem Starten der Bash ausgeführt. 

### Beispiel: 

Ein Aufruf von `module avail` zeigt, dass von `R` aktuell fünf Versionen verfügbar sind: 

    R/3.4.3-gcc72-base
    R/3.5.1-gcc73-base
    R/3.6.1-gcc73-base
    R/3.6.3-gcc93-base
    R/4.0.0-gcc93-base

Die aktuellste Version kann daher mittels `module add R/4.0.0-gcc93-base` hinzugefügt werden. 


## Slurm

Slurm ist die Software, die auf LiDO die Ressourcen verwaltet und verteilt. 
Benötigen wir also Rechenkerne zur Bearbeitung eines R-Skriptes so fragen wir diese Ressourcen bei Slurm an. 
Slurm reiht diese dann in eine Warteschlange ein und weist sie je nach Kapazitäten zu. 
Es kommt daher vor (und zwar meistens), dass nicht alle Ressourcen sofort zur Verfügung gestellt werden, sondern, je nach Andrang, erst nach einer Wartezeit. 

Auf dem Rechencluster der Fakultät Statistik übernimmt ebenfalls Slurm diese Aufgabe. 
Einige der Befehle sind daher u. U. bereits bekannt. 
Im Gegensatz zu LiDO werden auf dem Statistik-Rechencluster aber vorallem die Skripte `submitR` und `interactive` zur Job-Generierung verwendet, welche auf LiDO nicht installiert sind. 


### Slurm-Befehle

Nachfolgende Tabelle enthält die wichtigsten Befehle zur Bedienung von Slurm. 

Befehl    | Beschreibung
--------- | -----------------------------------------------------------
`sbatch`  | Abschicken einer Job-Datei im Batch-Modus
`squeue`  | Anzeige der aktuellen Warteschlange. Wichtige Option `-u benutzer` zur Filterung der angezeigten Benutzer
`scancel` | Stoppen eines laufendes Jobs mittels seiner ID
`salloc`  | Anfragen von Ressourcen
`srun` 	  | Starten eines Befehls auf zur Verfügung gestellten Ressourcen

### Slurm-Optionen

Nachfolgende Tabelle enthält die wichtigsten Optionen, mit denen die benötigten Ressourcen spezifiziert werden können. 
Dies ist sowohl im interaktiven Modus (mittels `salloc`, `srun`) als auch im Batch-Modus (mittels `sbatch`) notwendig. 

Optionen          | Kurz | Beschreibung
----------------- | ---- | --------------------------------------------
`--partition`     | `-p` | Angabe der angefragten Partition/Warteschlange
`--nodes`         | `-N` | Anzahl angeforderter Rechenknoten
`--ntasks`        | `-n` | Anzahl an tasks
`--mem`           |      | Angeforderter Speicher in MB
`--mem-per-cpu`   |      | Angeforderter Speicher in MB **pro Kern**
`--time` 	      | `-t` | Angefragte Rechenzeit in Minuten oder in den Formaten "mm:ss", "hh:mm:ss", "d-hh", "d-hh:mm", "d-hh:mm:ss"
`--constraint`    | `-C` | Einschränkung bzgl. der möglichen Nodes

Bis Ende November 2020 konnte mittels `--cpus-per-task` bzw. kurz `-c` die Anzahl an benötigter Rechenkerne angegeben werden. 
Seitdem wurde das Verhalten von LiDo so verändert, dass immer der komplette Knoten reserviert wird (also 20 bzw. 48 Rechenkerne, siehe unten). 
Diese Angabe ist daher nicht mehr notwendig. 

#### Partitionen

LiDO hat vier (öffentliche) Partitionen, die sich hinsichtlich der maximalen Laufzeit unterscheiden: 

Partition   | Max. Laufzeit 
----------- | -------------:
`short`     | 02:00:00
`med`       | 08:00:00
`long`      | 48:00:00
`ultralong` | 672:00:00

Kurze Jobs auf `short` werden für gewöhnlich deutlich schneller ausgeführt als lang-laufende Jobs. 
Oftmals lohnt es sich, viele kurze Jobs zu generieren, als einzelne sehr lang-laufende. 

#### Tasks, Nodes, Cores

In den Optionen werden die Begriffe "task", "node", und "cpus/cores" verwendet. 

Ein "node"/Knoten ist ein Computer des Rechenclusters, der eine bestimmte Anzahl von "cores"/Kernen zur Verfügung hat. 
Auf LiDO haben die (öffentlichen) nodes entweder 20 oder 48 Kerne (siehe Constraints). 
Mehrere "tasks" werden verwendet, wenn innerhalb eines Skripts (Bash-Skript, nicht R-Skript) Befehle gleichzeitig ausgeführt werden sollen. 

Wir wollen für gewöhnlich ein R-Skript starten, in dem mehrere Kerne zur Berechnung verwendet werden (mittels `parallel` oder `future`). 
Diese Kerne müssen vom gleichen Computer/Knoten stammen, da sie ansonsten nicht den gleichen Speicher verwenden. 
Wir verwenden daher 1 Task, 1 Knoten, und z.B. 20 Kerne, also die Optionen

    -n 1 -N 1 -c 20

#### Constraints

Über Constraints kann eine Auswahl der möglichen Knoten getroffen werden, was manchmal sinnvoll ist, da sich die Knoten in ihren Features unterscheiden. 
Die folgende Tabelle (entnommen von der LiDO-Seite) stellt diese dar: 

Constraint | CPU Typ | Beschleuniger Typ | Max Cores | Max RAM/Knoten
---------- | ------- | ----------------- | --------- | --------------
`cstd01`   |    2x Intel Xeon E5-2640v4 |	- |	20 | 64 GB
`cstd02` oder `ib_1to1` | 2x Intel Xeon E5-2640v4 | - | 20 | 64 GB
`cquad01`  |	4x Intel Xeon E5-4640v4 |	- |	48 |	256 GB 
`cquad02`  |	4x Intel Xeon E5-4640v4 |	- |	48 |	1024 GB	
`cgpu01` oder `tesla_k40` |	2x Intel Xeon E5-2640v4 |	2x NVIDIA Tesla K40 (12 GB) |	20 | 	64 GB


### Interaktiver Modus

Im interaktiven Modus erhalten wir eine Kommandozeile, mit der wie R starten und beliebig Code ausführen können. 
Die Schritte sind dabei: 

1. **Anfragen der Ressourcen:** 
   Folgender Code beantragt für uns einen Knoten für 60 Minuten

    `salloc -n 1 -N 1 -p short -t 60`

1. **Starten des Terminals:** 
   Nach Ressourcen-Zuteilung werden wir darüber informiert. Nun befinden wir uns aber noch nicht auf dem angefragten Rechner, sondern müssen uns erst dorthin verbinden. Per `srun` können Befehle oder Skripte an den Computer geschickt werden. Da wir ein Terminal erhalten wollen (um `R` zu starten) schicken wir den `bash`-Befehl: 

    `srun --pty bash`

1. **Laden und Starten von R:** 
   Nun sollten wir uns auf den Rechen-Node befinden. Dies erkennen wir daran, dass am Anfang der Zeile nicht mehr `@gw01` oder `@gw02` steht, sondern z.B. `@cstd01`. Um `R` zu verwenden müssen wir es zunächst laden. Anschließend starten wir es mittels `R`: 

    `module add R/3.6.1-gcc73-base`

    `R`

1. **Durchführen der Berechnungen:** 
   Nun kann `R` wie gewohnt verwendet werden. Der Code wird dabei häufig in das Fenster reinkopiert (\[Strg+V\] funktioniert dabei häufig nicht -> rechte Maustaste). Ergebnisse sollten am Ende abgespeichert werden und `R` mit der Funktion `q()` geschlossen werden. 

1. **Beenden des Jobs:** 
   Zur Beendidung des Jobs kann `exit` verwendet werden. Wir sind dann auf dem Gateway zurück, die Ressourcen stehen aber noch zur Verfügung (sofern die Zeit noch nicht abgelaufen ist). Wollen wir die Ressourcen freigeben, so erreichen wir dies mit einem erneuten `exit`. 

Wird nur ein Task verwendet, so können die Schritte 1. und 2. auch zusammengefasst werden, indem direkt 

    srun -n 1 -N 1 -p short -t 60 --pty bash

ausgeführt wird. 


### Batch-Modus

Im Batch-Modus übergibt man Slurm eine Datei, in der die Ressourcen-Spezifikationen und der auszuführende Code enthalten ist. 
Diese läuft dann automatisch durch, sobald die Ressourcen zur Verfügung stehen. 
Der Vorteil ist, dass man selbst keine Eingaben mehr tätigen muss (und daher auch nicht am Computer warten muss bis die Ressourcen zur Verfügung stehen), der Nachteil, dass eine zusätzliche Batch-Datei vorbereitet werden muss. 
Nachfolgend ist solch ein Skript beispielhaft dargestellt und anschließend werden die einzelnen Schritte erklärt. 

    #!/bin/bash -l
    #SBATCH -p short
    #SBATCH -t 60
    #SBATCH -N 1
    #SBATCH --constraint="cstd01"
    #SBATCH --mem=16384
    #SBATCH --mail-user=lilienthal@statistik.tu-dortmund.de
    #SBATCH --mail-type=FAIL

    cd /work/smjolili

    module purge
    module add R/3.5.1-gcc73-base 

    Rscript filename.R

Die erste Zeile (die sogenannte Shebang) gibt an, dass es sich bei der Datei um ein ausführbares Bash-Skript handelt. 

Die Slurm-Optionen werden **direkt anschließend** definiert, indem sie mit `#SBATCH` beginnend angeführt werden. 
Neben den bereits bekannten Ressourcen-Optionen, verwenden wir hier zusätzlich `--mail-user` und `--mail-type`, welche uns ermöglichen E-Mails bei festgelegten Ereignissen zu erhalten. 
Hierbei informiert die Option `BEGIN` über Job-Beginn, `END` über Job-Beendigung, `FAIL` über Job-Abbrüche, `ALL` über all diese Ereignisse, und `NONE` über keine. 
Im interaktiven Modus ist dies logischerweise nicht notwendig, da wir Beginn, Beendigung, Fehler direkt mitkriegen. 

Anschließend können normale Bash-Befehle verwendet werden um Verzeichnisse zu wechseln, Module zu laden, und R zu starten:  
Wir wechseln mittels `cd` in das Arbeitsverzeichnis. 
Alle Module werden entladen (`module purge`) und R geladen `module add R/3.5.1-gcc73-base`. 
Auf diese Weise stellen wir sicher, dass keine anderen Module als die ausgewählten aktiv sind.  
`R` wird dann im nicht-interaktiven Modus gestartet, indem der Befehl `Rscript` verwendet wird. 
Dies sorgt dafür dass die Datei `filename.R` von oben nach unten ausgeführt wird. 

Ist diese Datei erstellt und auf LiDO gespeichert, so kann sie über 

    sbatch pfad/datei

an Slurm übergeben werden. 


### Skripte

Zur leichteren Ausführen von interaktiven Sitzungen und des Batch-Betriebs können die Skripte `interactive` und `submitR` verwendet werden, welche im Ordner [`skripte`](skripte/) im Verzeichnis dieser Kurzanleitung zu finden sind. 
Die Funktionsweise lehnt sich dabei an die gleichnamigen Skripte an, welche auf dem Rechencluster der Fakultät Statistik verfügbar sind. 
Diese Skripte sind **keine** Funktionen von LiDO oder Slurm und es ist nicht gewährleistet, dass sie funktionieren. 

Zur Installation müssen die beiden Dateien in den Ordner `~/bin` kopiert werden (Ordner gegebenfalls erstellen) und als ausführbar gekennzeichnet werden, z.B. indem in der Bash die Befehle 

    chmod +x ~/bin/interactive
    chmod +x ~/bin/submitR

ausgeführt werden. 

Anschließend können die Befehle in der Bash durch Ausführen von 

    interactive

oder 

    submitR filename.R

ausgeführt werden. 
Die angeforderte Rechenzeit und Speicher (pro Kern) werden dabei automatisch abgefragt. 
Standardeinstellungen können im oberen Teil des Skriptes eingetragen werden. 
Hier kann auch das verwendete R-Modul spezifiziert werden. 
Falls die Datei unter Windows bearbeitet wird beachte hierzu auch die Fehlerbeschreibung in Abschnitt 3.1.  
Bei `submitR` steht zusätzlich die Option `replicate` zur Verfügung, womit die Ausführung der R-Datei repliziert werden kann. 
Die Laufzahl der Wiederholung kann innerhalb von `R` über den Befehl 

    as.integer(Sys.getenv("PBS_ARRAYID"))

ausgelesen werden. 
Der Wert von `replicate` kann dabei ganzzahlig sein, was die Iterationen 1, 2, ..., WERT erzeugt (d.h. `replicate=10` erzeugt 10 Wiederholungen mit den Laufzahlen 1, 2, ..., 9, 10), oder das Intervall als `WERT1-WERT2` beschreiben, was die Iterationen `WERT1`, ..., `WERT2` erzeugt (d.h. `replicate=3-6` erzeugt 4 Wiederholungen mit den Laufzahlen 3, 4, 5, 6). 



#### Beispiel: 

Wir möchten ein R-Skript, das zeit-intensive Simulationsberechnungen durchführt, 100 mal wiederholt berechnen. 
Über die `replicate`-Option kann dies leicht realisiert werden. 
Innerhalb der `R`-Datei wird der Index der aktuellen Iteration ausgelesen und zur Generierung der Ergebnis-Datei verwendet. 
Die `R`-Datei `file.R` sieht z.B. folgendermaßen aus: 

    library(important_package)
    source("important_functions.R")
    id <- as.integer(Sys.getenv("PBS_ARRAYID"))

    data <- generate_data()
    results <- important_calculations(data)
    save(results, file = paste0("results-", id, ".RData"))

Zur Übersendung an Slurm verwenden wir dann den Befehl 

    submitR file.R replicate=100


### batchtools

Neben der Verwendung der SLURM-Befehle zum Starten interaktiver Sitzungen oder zum Abschicken von Batch-Dateien sowie der Skripte `interactive` und `submitR`, kann auch das `R`-Paket `batchtools` zur Verwaltung/Durchführung von Rechenjobs verwendet werden. 
Da ich nie damit gearbeitet habe, sei hier lediglich auf die Internetseite `https://github.com/mllg/batchtools` verwiesen, auf der weitere Informationen zu finden sind. 


## R-Pakete installieren

Grundsätzlich werden R-Pakete auf LiDO mit den gleichen Befehlen installiert wie lokal, also mittels 

	install.packages()

Hierzu kann auf dem Gateway-Server R gestartet werden (das Modul muss vorher geladen werden). 
Pakete werden dann in eine persönliche Bibliothek im Home-Verzeichnis installiert und können später in Rechenjobs verwendet werden. 

Sollen Pakete innerhalb eines Rechenjobs (also nicht direkt auf dem Gateway) installiert werden, so können die Pakete nicht im Home-Verzeichnis abgelegt werden (kein Schreibrecht). 
Dies kann umgangen werden, indem das Work-Verzeichnis als Alternativpfad angegeben wird. 
Hierzu geben wir bei der Paket-Installtion im `install.packages`-Aufruf das `lib`-Argument an und verwenden dann beim Laden der Pakete ebenfalls das Argument `lib.loc` in `library`/`require`. 
Sinnvoller ist aber unter Umständen, den Verzeichnispfad für installierte Pakete global zu modifizieren. 
Hierzu bearbeiten wir die Datei `.Rprofile` (eine versteckte Konfigurationsdatei) im Home-Verzeichnis `/home/benutzername` (ist die Datei nicht vorhanden, so muss sie erzeugt werden). 
Diese Datei wird jedesmal ausgeführt, wenn `R` gestartet wird. 
Wir fügen die Zeile 

    .libPaths("/work/benutzername/R")

hinzu und beenden die Datei mit einer leeren Zeile (Wichtig: Die letzte Zeile der Datei muss leer sein!). 
Dies fügt der Liste von Verzeichnissen für R-Pakete zusätzlich den Ordner `R` in unserem Work-Verzeichnis (für das wir immer Schreibrecht haben) hinzu. 
Da diese Einstellung bei jeder `R`-Sitzung automatisch geladen wird, müssen wir nichts weiter tun und können Pakete installieren und laden. 
Da diese Datei im Home-Verzeichnis liegt, kann die Modifikation nicht aus einer interaktiven Sitzung getätigt werden, sondern muss auf dem Gateway-Server erfolgen. 
Hierzu kann ein Editor wie `nano` oder `vim` verwendet werden, oder die Datei wird lokal erstellt und auf LiDO hochgeladen. 


# Häufige Probleme 

## `Bash script and /bin/bash^M: bad interpreter: No such file or directory`

Dieser Fehler kann auftreten, wenn ein Bash-Skript (z.B. die Skripte `submitR` oder `interactive` beschrieben in Abschnitt 2.4.5) unter Windows bearbeitet und dann zur Ausführung nach LiDo geladen wird. 
Die Ursache liegt darin, dass Windows und Unix-/Linux-Systeme Zeilenendungen unterschiedlich kodieren. 
Eine Windows-kodierte Datei kann daher auf LiDo zu Fehlern führen. 
Grundsätzlich sind hier drei Möglichkeiten gegeben: 

* Verwendung eines Editors, bei dem eingestellt werden kann, wie das Zeilenende kodiert wird. Mögliche Editoren sind z.B. **Notepad++** oder **Sublime Text**. Auch in **RStudio** lässt sich die Kodierung einstellen (Options > Code > Saving > Line ending conversion auf Posix ändern). 
* Wird WinSCP zum hochladen verwendet, so kann eingestellt werden, dass die Zeilenendungskodierung automatisch angepasst wird. Informationen hierzu unter <https://winscp.net/eng/docs/faq_line_breaks>
* Bearbeitung der Datei direkt auf LiDo (hierzu können u.a. `nano`, `vim`, oder `emacs` genutzt werden, siehe Abschnitt 1.2.6.1). 
* Nachträgliche Umkodierung der Datei auf LiDo mittels `sed`-Befehls (siehe dazu nachfolgenden Link)

Weitere und ausführlichere Informationen lassen sich unter  
<https://stackoverflow.com/questions/14219092/bash-script-and-bin-bashm-bad-interpreter-no-such-file-or-directory>  
finden. 

## `bash: warning: setlocale: LC_CTYPE: cannot change locale (UTF-8): No such file or directory Fix`

Loggt man sich an einem MacOS-Rechner mittels Terminal auf LiDo ein, kann dieser Fehler angezeigt werden. 
Die Lösung hierzu befindet sich in den Einstellungen der Terminal-App: 

    Terminal > 
    Preferences > 
    Select Terminal type such as Basic (default) > 
    Advanced tab

In diesem Fenster muss sichergestellt werden, dass "Set locale environment variables on startup" **nicht** aktiviert ist. 

Quelle und mehr Informationen unter:  
<https://www.cyberciti.biz/faq/os-x-terminal-bash-warning-setlocale-lc_ctype-cannot-change-locale>  
