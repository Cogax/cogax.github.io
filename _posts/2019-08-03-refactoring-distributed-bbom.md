---
title: Refactoring Distributed Big Balls of Mud
toc: true
note: true
updated: 2019-07-25 23:37
teaser: Der Microservice Hype hat viele Softwareentwickler dazu verleitet schlechte Softwarearchitekturen über das Netzwerk zu verteilen. Was daraus resultiert ist oft schlimmer als nur ein schlechter Monolith. Und trotzdem haben es auch solche Systeme verdient modernisiert, gewartet und weiter erweiter zu werden.
---
Der Begriff *Big Ball of Mud* [^ddd] bezeichnet die Struktur einer Software, welche keine erkennbare Architektur besitzt. Es gilt als schlimmes, aber doch häufig auftretendes Anti-Pattern. Schlimmer geht nicht, oder doch? *Distributed Big Balls of Mud* [^distributed-bbom] bezeichnet das selbe Phänomen, bei welchem die Architektur jedoch verteilte Systeme beinhaltet. Solche Software ist schlecht wartbar und wird oft als Legacy bezeichnet. Doch wie gehts damit weiter? Kürzlich hatte ich mit genau solcher Legacy Software zu tun. Dieser Artikel beschreibt ein rein Hypothetisches Vorgehen wie man aus dem (wortwörtlichen) Schlamassel rauskommen kann.

Der hier erwähnte Fall beschreibt ein System welches als Cloud basierte Webapplikation betrieben wurde. Es wurde versucht die Microservice Architektur einzusetzen. Gründe für eine solche Entscheidung waren im Nachhinein schwer zu identifizieren. Vielleicht hat man nur den Aspekt berücksichtigt, dass die Entwicklungen in einer Microservice Architektur an performance gewinnen *können*.

## Legacy Software
Die Definition von Legacy Software ist wohl für jeden eine andere. In diesem konkreten Fall waren folgende Punkte für mich Gründe genug, diese Software als Legacy und Umbauwürdig zu bezeichnen:

**Hoher Support- Aufwand:**
Das System war schwer verständlich. Fachliche Logik war durch die schnelle Entwicklungsphase über das ganze System vertreut. Zudem kamen diverse Workarounds- und Fehler hinzu. Fachliche Logik wurde mehrfach aber trotzdem unterschiedlich Implementiert.

**Hoher Operations- Aufwand:**
Das System wurde wie eine mechanische Maschine betrieben. Mal hat ein Entwickler da etwas in der Production-Datenbank korrigiert, mal musste man dort fehlerhafte Import-Dateien manuell bereinigen. Produktivstellungen (Deployments) waren sehr heikel und mühsam und demnach nicht automatisiert. Die Datenbanken (und damit auch Teile im Code) waren im "extend only" Mode, weil man Angst hatte etwas kaputt zu machen.

**Viele Defects / Bugs:**
Neue Anforderungen führen zu komischen Verhalten in anderen Teilen der Software. Oder, wenn man einen Defect fixt, entstehen drei andere. All dies sind indizien für legacy Software.

**Wartbarkeit:** [^wartbarkeit]
Die Wartbarkeit war nicht gegeben. Ich persönlich  finde eine gute Messgrösse der Software Wartbarkeit ist, zu schauen wie lange es dauert, bis neue Entwickler ein System erweitern, und im Fall von DevOps, selbständig betreiben können. Oft erkennt man dies zwar, die Reaktiv schlägt sich dann aber in einer ebenfalls nicht gewarteten Dokumentation nieder. Somit hat man noch mehr unwartbare Artefakte, welche neue Entwickler mehr vrwirren als helfen.

**Skalierbarkeit:**
In gewissen Teilen der Software wurde die nicht vorhandene Skalierbarkeit wohl erkannt. Statt diese Thematik jedoch aufzugreifen und zu diesem Zeitpunkt ein Refactoring anzustossen, wurde bewusst die Skalierbarkeit bewusst limitiert.

**Keine Tests:**
Leider waren überhaupt keine Tests implementiert. Automatisierte Tests waren von Beginn an kein Bestand der Lösung - dementsprechend war auch die Code Qualität (Stichwort Kopplung etc.). Das wirklich erschrekende war jedoch, dass überhaupt kein Testing Prozess vorhanden war. Bei Neuentwicklungen wurde nur der Nice-Case durchgespielt. Es gab keine Person neben den Entwicklern, welche das Ganze System kannte und hätte testen können.

**Fehlendes Entwickler Know-How:**
Dieser Punkt geht nicht zwingend unter Legacy Software, dennoch beschleunigt er dessen Prozess. In diesem konkreten Fall wies sich dies in diversen Gebieten aus. Es gab zum Beispiel keine Entwicklungsumgebung. Ein lokal ausgeführter Service war immer mit der Datenbank sowie anderen Services aus dem Testsystem verknüpft. Dies machte nicht nur ein lokales entwickeln unmöglich sondern sorgte auch dazu, dass die Testumgebung nicht isoliert war. Noch schlimmer war, dass die Services auf Datenbankebene miteinander integriert waren.

## Refactoring: Grobes Vorgehen
Als allererstes muss natürlich erkannt werden, dass das System legacy ist. Dies ist für aussenstehende sicherlich einfacher als für Personen welche von Tag eins an dabei waren. Vielleicht wird erst versucht, bei den oben genannten Punkten nicht die Ursache sondern die Symptome zu bekämpfen. Man regelt den internen Support, man trennt Entwicklung und Operations auf (nicht DevOps sondern Dev vs. Ops), etc. Alles keine langfritigen Lösungen.

Als Entwickler muss man regelrecht dafür kämpfen, dass die subjektive Ansicht auf das bestehende System abgelegt wird und all die Probleme rein objektiv betrachtet werden. Man braucht viel Geduld, muss Lobbyarbeit für einen Umbau betreiben. Dabei ist ein gewisses Mass an Geduld, Kommunikationsfähigkeit und leider oft auch Politikwissenschaften ;) hilfreich.

Im folgenden will ich ein Vorgehen beschreiben welches meiner Ansicht nach im konkreten Fall Sinn macht. Und ja, ich bin mit bewusst, dass allein einzelne dieser Punkte in einigen Unternehmen bereits Jahre dauern können ;).

---------------
Ab hier die "würde" Form???

### 1. Why?
Als allererstes würde ich die Frage stellen, wieso der Code jetzt Legacy ist? Was hat man falsch gemacht, dass man jetzt vor so vielen Problem steht? Wieso wurde nicht rechtzeitig reagiert? Dies muss zwingend beantortet werden, da ansonten nicht die Ursache sondern nur die Symptome (schlecht Software) angegangen werden. Kann man diese Ursache nicht bekämpfen, so lohnt sich ein Refactoring schlicht weg nicht.

Im konkreten Fall hatte das "Wieso" verschiedene Gründe. 
* Ein Grund war, dass man das Produkt schnell am Markt platzieren wollte. Dies an sich ist nicht schlecht. Wenn dabei jedoch Kompromisse sowhol in technischer, wie auch fachlicher Hinsicht eingegangen werden, und diese Schulden (technical Dept LINK) nicht rechtzeitig abgearbeitet werden, so ist das Fenster eingebrochen (Clean Code analogie).
* Ein weiterer Grund war, dass Verschiedene Personen mit verschiedenen Ansichten ihre Anforderungen an die Entwicker heran trugen. Es gab keine zentrale Anlaufstelle, welche Konzepte genüberstellte, verglich und frühzeitig die Bremse bei unsinnigen Anforderungen zog. Diese Anforderungen kamen (natürlich) alle mit höchster Priorität.
* Die Entwickler gaben dem Druck nach. Leider arbeiteten die Entwickler nicht wirklich als Team zusammen. Es herrschte ein unterschiedliches technisches Niveau, trotzdem implementierte meist jeder einzelne Entwickler eine seperate Anforderungen. Dies meistens einfachheitshalber in einem eigenen Service (mit geteilten Datenbanken). Dieses Problem ist sicherlich Organisationell bedingt und hätte z.B. mit einem Agilen Ansatz verbessert werden können. (LIIIINK Agiler Softwareentwickler)
* Schlechte Architektur. Die Architektonischen Entscheidungen waren schlicht weg falsch. Man wollte von Beginn an Microservices implementieren, ohne deren Umfang zu kennen. Weder hatte man Erfahrungen noch das technische Know How um so eine Architektur richtig umzusetzen. Auch die Anforderungen haben eine Verteilte Architektur in diesem Masse nicht gerechtfertigt. Von diesem Standpunkt aus muss man den Entwicklern ein Komplement machen, da sie trotzdem ein lauffähiges System entwickelt haben. Praktisch alle Architekten welche die Microservice Architektur meistern erklären, dass ein Microservice Ansatz von Beginn an der falsche Weg ist. Sie empfehlen einen sauber modularen, Monolithen Ansatz zu Beginn, welcher sich später immer noch auftrennen lässt.

### 2. Conways Law (LIIIINK)
Ein Zitat, welches Personen auf Management Ebene im Zusammenhang mit existierender Legacy Software und schlechter Architektur wohl eher ungern lesen:

 > "Organizations which design systems […] are constrained to produce designs which are copies of the communication structures of these organizations." --Melvin E. Conway

 In diesem konkreten Fall hat es zugetroffen. Will man nun Umbauen, so sagt einem dieses Zitat jedoch auch was man besser machen kann. Ein erster Schritt ist also, ein Team aus Agilen Softwareentwicklern (LIIIIIINK) in einem Agilen Umfeld arbeiten zu lassen. Optimalerweise gibt es *einen* Produktverantwortlichen, welcher die fachlichen Anforderungen im Griff hat und die Konzepte ausarbeitet sowie auf Sinnhaftigkeit prüft. Diese Person soltle auch in der Lage sein bestehende Konzepte zu hinterfragen und nach sinnvolleren un einfacheren Lösungen zu suchen. 

### 3. Fokus auf Fachlichkeit
Denkt man an einen Umbau, so treffen oft verschiedene Meinungen der Entwickler aufeinander. Der eine will Event Sourcing versuchen, dem anderen reichen CQRS und sauberes Domain Driven Design. Der Punkt ist, dass all dies nicht von belangen ist. Bei einem Umbau sollte man sich auf das fachliche, sowie die bestehenden Problemen fokusieren. Man wird wohl nicht die ganz Grundlegenden Konzepte einfach so ändern können. Schrittweise ist dies sicherlich möglich, allerdings wird dies erst nach einigen Iterationen ersichtlich. Im Vorherein technische Visionen, Konzepte und Architekturen fix zu definieren macht bei einem solchen Umbau meiner Meinung nach kein Sinn.

Trotzdem wird der Moment kommen, wo man bei der Architektur in eine gewisse Richtung lenken muss. Meiner Meinung nach muss das Entwicklerteam muss nicht hochqualifiziert sein. Da sie jedoch Agile Softwareentwickler (LIIINK) in einem Agilen Umfeld sind, entscheiden sie allein wie sie arbeiten. So soll auch das Entwicklerteam die technische Architektur zusammen festlegen und ein Level am Komplexität finden, welchen alle Vertreten können.

Im angesprochenen Fall handelt es sich um ein System, welches in Production läuft und bei welchem noch laufend neue Feature Anforderungen reinkamen. Ein "Big Bang Umbau" oder Neu- bzw. Parallel Umbau stand von Beginn an nicht zur Diskussion. Der Umbau soll also in Iterativer manier von statten gehen, während das System in Betrieb bleibt.

### 4. Kopplung akzeptieren
Im hier angesprochen Fall war das System in mehrere Source Code Repositories aufgeteilt. Zudem gab es noch einige (gesharte) Datenbanken. Da diese Datenbanken jedoch als Integraitonspunkt zwischen den Services dienten gab es kein Source Code Repository welches der Owner der Datenbank war. Dies führte natürlich dazu, dass eine lokale Entwicklungsumgebung sowie automatisierte Datenbankmigrationen unmöglich waren. Ja, das System war weit weg von Continuous Deployment. Auch automatisierte Tests gegen eine Datenbank waren so schlichtweg unmöglich.

Den einzigen Ausweg sehe ich darin, das ganze System vorerst in einem Source Code Repository abzubilden. Somit akzeptiert man die Kopplung zwischen den zuvor vermeindlich unabhängig wrikenden Services. Dies bietet die Chance, das Deployment zentral zu regeln. Zudem hat mein ein Source Code Repository welches als Owner für die Datenbanken gilt. Dies führt dazu, dass eine lokale Entwicklungsumgebung sowie automatisierte Tests inkl. Datenbanken ermöglicht werden können. Dies natürlich nicht ohne grösseren Aufwand, aber zumindest besteht die Mlglichkeit dazu. Ist ist wichtig für die nächsten Punkte.

Diese Monolithisierung der Pseude-Microservice Architektur soll nur temporär von bestand sein. Wenn gewisse Code-Teile zuvor Mehrfach Implementiert wurden, kann es Sinn machen diese so zu belassen. Ein schwerwiegender Fehler wäre nun, eine weiter Kopplung auf Code-Ebene hinzuzufügen.

Ein weiterer Vorteil war im konkreten Fall, dass der noch nicht Vorhandene DevOps Prozess nun viel einfacher Implementiert werden konnte. Es mussten nicht x-Repositories entsprechend aufgerüstet werden, sondern es konnte alles Zentral gelöst werden.

### 5. Entwicklungsumgebung komplett isolieren
Da man zuvor den Schritt "zurück zum Teil-Monolith" gewagt hat sollte nun eine komplett isolierte Entwicklungsumgebung möglich sein. Konkret bedeutet dies, dass alle Datenbanken lokal lauffähig, sowie automatisiert initialisierbar sind. Auch Infrastructur Komponenten wie Message Bus sollte nun per InMemory (oder ähnlich) Lösung verwendbar sein.

Nun müssen alle Schnittpunkte mit externen Systemen umgebaut werden. Im Vorliegenden Fall waren eine ganze Menge an Fremdsystemen an das System angebunden. Leider war die Architektur nicht auf austauschbarkeit ausgelegt (dies wäre wohl anders gewesen, wenn Tests von Beginn an implementiert worden wären). Das Ziel ist, dass für jedes Fremdsystem eine Fake-Implementation (oder Mocks etc.) gebaut und benutzt werden kann.

### 6. Sicherheitsnetz um bestehende Funktionalität
Da weder automatisierte Tests noch Test Personen mit genügend Know-How vorhanden sind soll im ersten Schritt eine Art Sicherheitsnetz aus automatisierten Tests erstellt werden. Diese sollen während dem Umbau als art Regressionstests dienen sollen. Diese Tests sollen nicht an die Implementierung gekoppelt sein, da ansonsten beim Umbau auch die Tests angepasst werden müssen. Somit kann der Betrieb der Software auch während dem Umbau sichergestellt werden. Die Entwickler merken wenn sie etwas an einem Ecken der Software ändern und dies eine andere Ecke kaputt macht sofort. Im schlimmsten Fall kann dies auch eine manuelle test Person sein, welche sich der Aufgabe annehmen will. Diese muss dann quasi in der Lage sein auf Knopfdruck die Funktionalität des gesamten Systems sicherzustellen - und das in kurzer Zeit.

Natürlich wären Tests auf Unit- bzw. Integrationsebene sehr hilfreich. Diese für bestehende legacy Funktionalität zu Implementieren wäre zu diesem zeitpunkt jedoch reine Zeitverschwendung. Bei den parallel neu zu entwickelnden Anforderungen sollen diese aber sehr wohl Implementiert werden. Im vorliegenden Fall war allerdings auch das Team zu Begin schlicht weg nicht fähig dazu. Es musste Schulungsaufwand betrieben werden um Themen wie automatisiertes Testing überhaupt den Entwicklern nahe zu bringen.

### 7. Fachlich orientierter Umbau
Zusammen mit Fachspezialisten (Domänenexperten) muss das bestehende System analysiert und kritisch hinterfragt werden. Dabei sollen die Fehler- und Lückenhafte Konzepte angesprochen werden und Langfritige Lösungen ausgearbeitet werden. Dabei soll allen beteiligten klar werden wohin man will und wie man dort hin will. Spätestens hier sollte man die Fachexperten und das Business hinter sich haben. Im Intertiven Vorgehen soll dann Schritt für Schritt nebst neuentwicklungen von Features umgebaut werden. Zwischenstände sollen den Interessierten stets präsentiert und demonstriert werden, damit diese stets im Bilde sind und reagieren können.

Es gibt genügend Ansätze wie man Refactoring durchführen kann. Man kann Neuentwicklungen komplett auslagern, temporäre Parallelentwicklungen machen, etc. (LIIINks Strangler Pattern, etc.). Auch dabei sollte versucht werden kleinere "Big Bangs" zu vermeiden. Technisch ist dies für das Team sicherlich eine herausforderung. Es wird teilweise etwas chaotisch im Code aussehen, Daten- sowie Code Replikationen werden entstehen, etc. Wichtig ist, dass das Team konstant und konzentriert daran arbeiten kann. Es sollen stets alle Informiert sein und nicht die ganze Verantwortung und Übersicht bei bspw. einer Person liegen.

Während dem Umbau werden die zuvor entwickelten Tests als Regressionstests verwendet. Dies bietet allen beteiligten Zuversicht, dass trotz dem Umbau die Funktionalität gewährleiset ist. Es soll versucht werden so oft wie möglich zu releasen.

Die Fachschnitte der Services müssen je nach Anforderungen neu gemacht werden. Dazu ist es vielleicht Notwendig zwei oder mehrere Services kurzfristig zusammenzulegen.Durch das Zusammenlegen kann der Grundsötzliche Aufbau geändert werden und der Fachschnitt neu angesetzt werden. 

Aus den gemachten Fehlern muss natürlich gelernt werden. Datenhoheiten müssen den Fachspezifische Services zugeteilt werden. Synchrone Integration sollte vermieden werden, etc.

## Schlussfolgerung
TODO

<div class="divider"></div>

## Referenzen

[^wartbarkeit]: <a href="https://de.wikipedia.org/wiki/Wartbarkeit" target="_blank">Wikipedia: Wartbarkeit</a>
[^ddd]: Domain-Driven Design, Eric Evans, <em><a href="https://amzn.to/3143VjB" target="_blank">Amazon</a></em>
[^distributed-bbom]: <a href="http://www.codingthearchitecture.com/2014/07/06/distributed_big_balls_of_mud.html" target="_blank">Distributed Big Balls of Mud</a>
