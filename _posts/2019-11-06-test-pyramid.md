---
title: Automatisierte Software Tests
toc: true
note: true
updated: 2019-07-25 23:37
teaser: Baut ein Ingenieur ein Haus, so müssen vor dem Bezug der neuen Wohneinheit mehrere Qualitätskontrollen durchgeführt werden. Allein schon die elektrische Installation erfordern eine Abnahme durch geschultes und qualifiziertes Personal. Refelktiert man die Computer Wissenschaften in dieser Hinsicht, so wird einem deutlich, dass wir in einer jungen Wissenschaft zu hause sind. Das Sicherstellen von Software Funktionalität ist (leider) noch zu oft ein Thema, welches nicht in der Arbeitswelt angekommen ist.
---

Die COmputer Wissenschaft gehört zu den neuen, mordernen WIssenschaften. Obwohl dies wohl kaum jemand bestreiten mag, so scheint es, dass klassische Ingenieurs-Disziplinen trotzdem nicht wirklich ANwenugnd in dieser modernen Branche finden. Gerad wenn es darum geht das neu gebaute zu verifizieren und auf ihre QUalität und Umsetzungsart zu prüfen. Obwohl automatisierte Softwre-Tests in den letzen Jahren enorm an Bedeutung gewonnen haben, wird noch zu Oft Software entwickelt, welche weder einen Beweis der entwickelten FUnktionalität bereitstellt, noch verifiziert ob das geliefterte Produkt überhaupt den formalen Anforderungen entspricht.

## Test Pyramide - Basics
Klaissisch, aber dennoch verankert und aktuell: Die Test Pytamide. Kaum jemwand kommt im Studium drum herum. Dennoch, wird ihr viel zu wenig beachtung geschenkt und oft als theroretisches Produkt abgestempelt. Oft liegt dies einzig und allein am Wirtschaftlichen Aspekten der Softwareentwicklung. Meine pesönlcihe Meinugn dazu: ** Wir entwickler können nicht erwarten dass wir Zeit verreschnen können um Tests zu schreiben. Gleichwohl kann eine Maurer keine Zeit verrechnen, welche er verwendet um zu prüfen ob die Mauer nun Snekrech und Rechtwinklig ist oder nicht.** Mit Agilen Methoden haben wir alle Werkzeuge in der Hand diese Arbeit transparent zu machen und diese auch entsprechend inklusive den Features zu schätzen.

Da wir dies un geklärt haben, lass und die Test-Payramde anschuaen:
![Wasserfall vs. Agil](/assets/images/test-pyramid.PNG)

Was auffällt, dass die Manuellen Tests eigentlich gar kein Teil der Pyramide sind. Dies mag vielleicht erschrekend wirken für diejenigen, welche bis anhin nur manuell getestet haben. WIllkommen in der digitalen, automatisierten Welt. Viele funktionale ANforderungen lassen sich sehr gut automatisiert testen, ohne jegliche manuellen AUfwand. Dies bedeuten nicht, dass keine manuellen Tester mehr benötigt werden, nur nicht im diesem Masse. Wir Softwareentwickler sollten selbst dazu fähig sein, die FUnktionalität der eignenen Entwicklungen mit eigenen Mitteln sicherzustellen, ohne externen AUfwand. Der Maurer muss schliesslich seine Mauer auch selbst mit der Wasserwaage überprüfen.

<div class="divider"></div>

## Referenzen

[^1]: Agiles Manifest, <a href="https://agilemanifesto.org/iso/de/manifesto.html" target="_blank">agilemanifesto.org</a>
[^2]: Domain-Driven Design, Eric Evans, <em><a href="https://amzn.to/3143VjB" target="_blank">Amazon</a></em>
[^3]: Der leidenschaftliche Programmierer, Chad Fowler, <em><a href="https://amzn.to/313gxYv" target="_blank">Amazon</a></em>
[^4]: Software Engineering, Volker Birk, <a href="https://www.youtube.com/watch?v=CdMjNviFdXk" target="_blank">YoutTube</a>
[^5]: Clean Coder, Robert C. Martin, <em><a href="https://amzn.to/2POyxnZ" target="_blank">Amazon</a></em>
[^6]: Extreme Programming Explained, Kent Beck, <em><a href="https://amzn.to/2PVNhSb" target="_blank">Amazon</a></em>
[^7]: Extreme Programming - Eine Übersicht, Rolf Dornberger & Thomas Habegge, Fachhochschule Solothurn Nordwestschweit, Reihe A: Paper 2005-05
[^8]: Management 3.0, Jurgen Appelo, <em><a href="https://amzn.to/315SuIa" target="_blank">Amazon</a></em>
