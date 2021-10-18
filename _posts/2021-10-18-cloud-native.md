---
title: Wieso Cloud Native?
toc: true
note: false
updated: 2021-10-18 20:27
teaser: Der Begriff "Cloud Native" gewinnt immer mehr an Popularität. Was steckt dahinter und wieso ist Cloud Native != Cloud?
---

Cloud Native ist ein Ansatz, welcher versucht, Applikationen, die in der Cloud betrieben werden, unabhängig vom Cloud Provider zu definieren. Durch die Cloud wurden sehr viele "Managed Services" (sogenannte Plattformservices) von verschiedensten Cloud-Providern angeboten. Jeder Service löst dabei meist ein spezifisches Problem sehr gut, büsst dafür jedoch meist bei den Punkten Konfigurationsmöglichkeiten, Portabilität, Interoperabilität und Wartung ein. Durch Plattformservices bindet man sich nicht nur an den Vendor ("Vendor Lock-In"), sondern auch an dessen Release Zyklus. Vermutlich muss man seine Applikation stets up-to Date halten - was bei grösseren Enterprise Applikationen oft schlichtweg nicht möglich ist.

Hat man eine Applikation in der Cloud Betrieben, so hat man aufgrund der spezifischen Plattform Services sehr oft eine Microservice-Architektur angewandt. Nur so konnte man die Stärken der einzelnen Services optimal ausnutzen und kombinieren. Microservice Architekturen bringen jedoch auf der Infrastruktur und Betrieb Seite viele Herausforderungen mit sich. So sind beispielsweise die Integrationspunkte zwischen den Services ein zentraler Punkt, welche oft zu Problemen führt. Diese Probleme führen oft dazu, dass man vermeintliche Infrastruktur Themen in der Applikation selbst und somit in jeden einzelnen Service implementiert, nur um die Kommunikationsschwierigkeiten der Services zu korrigieren. Retry Logiken sind nur ein Beispiel davon.

Der Cloud Native Ansatz versucht sich beim Cloud-Computing aufs Wesentliche zu konzentrieren. Als Basis wird quasi nur die Infrastruktur genommen, welche jeder Cloud Provider mit Leichtigkeit bieten kann. Darauf baut ein ganzes Ökosystem auf, welches mit Kubernetes als Flaggschiff versucht, einen generischen Ansatz in das Cloud-Computing zu bringen.

## Es geht immer um Abstraktionen

Abstraktion ist beim Design und dem Architekturentwurf einer Applikation ein zentrales Element. Man nutzt Clean-, Layer-, Hexagonalarchitektur Patterns, um die Businesslogik einer Applikation von der Infrastruktur zu abstrahieren. Man nutzt das Dependency-Inversion Principle um Module sinnvoll voneinander zu entkoppeln und baut generische Schnittstellen, welche einem von der konkreten Implementation abstrahieren.

### Abstraktion durch Container

Um eine Software ohne Wissen der Laufzeitumgebung bereitzustellen, sollte man Container verwenden. Nur so kann man sich sinnvoll vom Implementierungsdetail der Laufzeitumgebung abstrahieren. Ein Container beinhaltet jeweils die Runtime der Software, egal ob die Software in Java, Python oder C# .NET geschrieben ist, schlussendlich wird ein lauffähiger Container geliefert. Der Container kann nur auf alles Plattformen betrieben werden. Es ist nicht nötig, dass eine bestimmte JRE oder .NET Version installiert ist. Durch diese Abstraktion bekommt man viele Vorteile:

**Einheitlicher, generischer DevOps Prozess**

Deployment Pipelines können einheitlich aufgebaut werden, da das Artefakt immer im selben Format daher kommt. Dadurch kann man generische Schritte im Prozess definieren, welche für ein ganzes Unternehmen identisch sind.

**Portabilität**

Ein Container kann überall laufen gelassen werden. Wenn Teams verschiedene Applikationen entwickeln und diese Abhängigkeiten untereinander haben, so kann auf Container Ebene integriert werden. Der Container bietet dabei eine ideale Abstraktion. So können Teams trotz Abhängigkeit entkoppelt arbeiten und sind weder von der Programmiersprache noch von irgendwelchen Systemvoraussetzungen aneinandergebunden.

**Agilität und Flexibilität**

Fremdsysteme, eingekaufte Software oder Lecagy Applikationen können in containerisierter Form betrieben werden. Dies bringt einem Unternehmen die Flexibilität, gewisse Komponenten auszutauschen oder Schrittweise abzulösen. Ein Unternehmen kann somit viel schneller auf Marktsituationen reagieren oder mit Unternehmen zusammenarbeiten, welche andere Technologien verwenden.

**Interoperabilität**

Dadurch, dass jeder Container wie ein Legobaustein von aussen identisch aussieht, können die Container beliebig angeordnet werden. Man kann eine Systemlandschaft aus Legobausteinen aufbauen und dabei optimale Technologien für jede Problemstellung verwenden.

### Abstraktion im Betrieb

Durch den Einsatz von Container wir der Betrieb generisch. Statt spezifische Applikationen auf entsprechend vorbereitete Umgebungen zu deployen werden nur noch Container deployt und orchestriert.

**Abstraktion der Infrastruktur**

Sehr oft ist im Infrastrukturlayer der Applikation sehr viel Code implementiert, der in die Infrastruktur ausgelagert werden sollte. Ein Beispiel sind Retries und das Circuit-Breaker Pattern bei synchronen Integrationen via Http Aufrufe. Sehr oft werden in jeder Applikation Policies definiert, welche regeln, welcher Endpunkt welche Charakteristiken hat. Dies sollte in die Infrastruktur ausgelagert werden und in einem Service Mesh systemweit definiert sein. Mit Proxy-Containern kann die Kommunikation ausserhalb der Applikation geregelt werden. Somit können Applikationsentwickler einfacher arbeiten ohne Implementierungsdetails der Infrastruktur.

Weitere Beispiele:

- File Zugriffe: Sollten via Filesystem Zugriffe innerhalb des Containers gemacht werden. Die Infrastruktur hat dann die Aufgabe, entsprechende Volume-Mounts für den Container aufzusetzen.
- Konfigurationen: Oft werden Konfigurationen in der Startup Phase via Fremdsystem geladen. Diese sollten ausschliesslich via File Zugriffe (siehe oben) oder Umgebungsvariablen gemacht werden.

**Generisches Monitoring**

Monitoring ist vielfach etwas, was pro Applikation separat eingerichtet wird. Mit Container hat man jedoch die Möglichkeit, praktisch alle wichtigen Metriken generisch aus allen Containern zu ziehen. Auch Netzwerkverkehr kann mit Proxy Implementierung bzw. Service Meshes optimal überwacht werden. Es ist nicht notwendig, ein Monitoring Tool zu finden, welches SDK's für alle eingesetzten Technologien zu Verfügung stellt.

### Abstraktionen von Kubernetes

Kubernets versucht viele Probleme sehr generisch zu lösen. Die Ressource Typen, welche Kubernetes von Haus aus liefert, sind so definiert, dass sich ziemlich einfach eine Grenze zwischen DevOps Teams und Infrastruktur Teams ziehen lässt. Somit können Infrastruktur Teams Ressourcen zur Verfügung stellen, welche DevOps Teams beziehen können.

Des Weiteren gibt es harte Abstraktionen, welche in der Kubernetes Implementierung vorhanden sind:

- **CRI - Container Runtime Interface** Diese Abstraktion definiert die Container Runtime. Das bekannteste Tool für Container ist Docker. Durch diese Abstraktion ist es jedoch möglich, eine Vielzahl weiterer Container Runtimes zu unterstützen. Mittlerweile gehört Docker nicht mehr zur Standard Runtime.
- **CNI - Container Network Interface** Diese Abstraktion ermöglicht es, das Networking innerhalb des Kubernetes Clusters zu definieren.
- **CSI - Container Storage Interface** Diese Abstraktion definiert den Zugriff auf Storage Medien. Es gibt viele Implementationen für verschiedenste Speichermedien.

## Worauf achten bei Cloud Native?

- Keine Infrastruktur im Code implementieren. Der Infrastruktur Layer sollte dadurch schmäler werden.
- Kubernetes ist eine ideale Grenze zwischen DevOps und Infrastruktur. Entwicklungsteams sollten ihre Services selbst in Kubernetes deployen und Monitoren. Die Infrastruktur (Service Mesh, Storage, Networking, etc.) kann vom Infrastruktur zur Verfügung gestellt werden. Kubernetes bietet optimale Abstrahierungen dieser Konzepte an.

<div class="divider"></div>
