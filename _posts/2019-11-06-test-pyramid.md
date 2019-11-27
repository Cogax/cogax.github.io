---
title: Automatisierte Software Tests
toc: true
note: true
updated: 2019-07-25 23:37
teaser: Baut ein Ingenieur ein Haus, so müssen vor dem Bezug der neuen Wohneinheit mehrere Qualitätskontrollen durchgeführt werden. Allein schon die elektrische Installation erfordern eine Abnahme durch geschultes und qualifiziertes Personal. Refelktiert man die Computer Wissenschaften in dieser Hinsicht, so wird einem deutlich, dass wir in einer jungen Wissenschaft zu hause sind. Das Sicherstellen von Software Funktionalität ist (leider) noch zu oft ein Thema, welches nicht in der Arbeitswelt angekommen ist.
---

Die Computer Wissenschaft gehört zu den neuen, mordernen Wsssenschaften. Obwohl dies wohl kaum jemand bestreiten mag, so scheint es, dass klassische Ingenieurs-Disziplinen trotzdem nicht wirklich Anwendung in dieser modernen Branche finden. Gerade wenn es darum geht, das neu gebaute zu verifizieren und auf die Qualität und Umsetzungsart zu prüfen. Obwohl automatisierte Softwre-Tests in den letzen Jahren enorm an Bedeutung gewonnen haben, wird noch viel zu oft Software entwickelt, welche weder einen Beweis der entwickelten Funktionalität bereitstellt, noch verifiziert ob das geliefterte Produkt überhaupt den formalen Anforderungen entspricht.

## Test Pyramide - Basics
Klaissisch, aber dennoch verankert und aktuell: Die Test Pytamide. Kaum jemwand kommt im Studium drum herum. Dennoch, wird ihr viel zu wenig beachtung geschenkt und oft als theroretisches Produkt abgestempelt. Oft liegt dies einzig und allein am Wirtschaftlichen Aspekten der Softwareentwicklung. Meine pesönlcihe Meinugn dazu: ** Wir entwickler können nicht erwarten dass wir Zeit verreschnen können um Tests zu schreiben. Gleichwohl kann eine Maurer keine Zeit verrechnen, welche er verwendet um zu prüfen ob die Mauer nun Snekrech und Rechtwinklig ist oder nicht.** Mit Agilen Methoden haben wir alle Werkzeuge in der Hand diese Arbeit transparent zu machen und diese auch entsprechend inklusive den Features zu schätzen.

Da wir dies un geklärt haben, lass und die Test-Payramde anschuaen:
<img src="/assets/images/test-pyramid.PNG" alt="Test Pyramide" class="centered" style="max-width:400px;" />

Was auffällt, dass die Manuellen Tests eigentlich gar kein Teil der Pyramide sind. Dies mag vielleicht erschrekend wirken für diejenigen, welche bis anhin nur manuell getestet haben. WIllkommen in der digitalen, automatisierten Welt. Viele funktionale ANforderungen lassen sich sehr gut automatisiert testen, ohne jegliche manuellen AUfwand. Dies bedeuten nicht, dass keine manuellen Tester mehr benötigt werden, nur nicht im diesem Masse. Wir Softwareentwickler sollten selbst dazu fähig sein, die FUnktionalität der eignenen Entwicklungen mit eigenen Mitteln sicherzustellen, ohne externen AUfwand. Der Maurer muss schliesslich seine Mauer auch selbst mit der Wasserwaage überprüfen.

### Synonyme
<img src="/assets/images/testpyramid/num-of-executions.JPG" alt="Test Pyramide: Synonyme" class="centered" style="max-width:550px;" />

TODO:
* Explizite Anforderungsdokumentation im agilen Umfeld
* Was ist eine Unit?
* Synonyme
* Durchlaufpfade in einer Software

## Aspekte
Die Pyramide kann aus verschiedenen Perspektiven angeschaut werden. Ihre geometrische Form hat jedoch aus jeder einzelnen Perspektive ihre Begründung. Die folgende Aufführung verschiedener Aspekte und Sichtweisen zeigt, dass die Pyramide nicht nur für Entwickler und Tester von belangen ist. Die Aufzählung ist sicherlich nicht komplett.

<img src="/assets/images/testpyramid/speed.JPG" alt="Test Pyramide: Geschwindigkeit" class="centered" style="max-width:550px;" />

**Geschwindigkeit**
Kein manueller tester wird je auch nur die kleinste Funktionalität innerhalb von einigen Millisekunden testen. Muss er auch nicht, dazu gibt es ja die Unit-Tests. Unit-Tests testen nur eine sehr kleine einheit uns sind desshalb auch sehr performant.



<img src="/assets/images/testpyramid/costs.JPG" alt="Test Pyramide: Wirtschaftlichkeit" class="centered" style="max-width:550px;" />

**Wirtschaftlichkeit**
Unit-Tests sind schnell geschrieben und sehr performant. Sie kosten werder in der Entwicklung, noch in der Build-Pipeline viel Zeit. Im Gegansatz dazu sind komplette End-to-End Tests eher schwierig zu entwickeln und dauern eher länger, wesshalb sie auch kostspieliger sind. Übrigens: Schlecht designte Tests kosten immer viel, da der Wartungsaufwand hoch ist ;)



<img src="/assets/images/testpyramid/confidence.JPG" alt="Test Pyramide: Aussagekraft" class="centered" style="max-width:550px;" />

**Aussagekraft**
Wenn ein Unit-Test grün ist, so sagt dies dem Produktverantwortlichen nicht viel über die Funktionsweise seines Produktes aus. Wenn er hingegen von einem manuellen Tester ein positives Feedback bekommt, so hat diese Aussage weit mehr bedeutung. Test im oberen Teil der Pyramide sagen im wesentlichen aus, **ob** ein Use-Case funktioniert oder nicht. Tests im unteren Teil hingegen sollten konkret aussagen **was** funktioniert bzw. eben nicht.



<img src="/assets/images/testpyramid/num-of-tests.JPG" alt="Test Pyramide: Testanzahl" class="centered" style="max-width:550px;" />

**Anzahl der Tests**
Durch dass, das die Testkategorien im oberen Teil der Pyramide eher kostspieliger sind, sollte es auch weniger davon geben.



<img src="/assets/images/testpyramid/num-of-executions.JPG" alt="Test Pyramide: Testdurchläufe" class="centered" style="max-width:550px;" />

**Anzahl der Ausführungen**
Die Anzahl an durchführungen von manuellen Tests lassen sich während einer Iterration wohl an einer Hand abzählen. Im Gegensatz dazu sollten Unit-Tests so oft wie möglich, am besten nach jeder Code-Anpassung durchgeführt werden.



<img src="/assets/images/testpyramid/isolation.JPG" alt="Test Pyramide: Isolation" class="centered" style="max-width:550px;" />

**Isolation**
Mit einem Unit-Test kann eine Kompnente bzw. eine Unit ganz spezifisch getestet werden. SO kann eine Logik sehr genau unter Betrachtung aller Varianten getestet werden. Bei einem Test im oberen Teil der Pyramide ist dieser Grad an Isolierung schwer zu erreichen.



<img src="/assets/images/testpyramid/brittleness.JPG" alt="Test Pyramide: Stabilität" class="centered" style="max-width:550px;" />

**Stabilität**
Ein heikles Thema bei End-to-End Tests ist die Stabilität. Da diese Tests an viele Vorbedingungen geknüpft sind, ist es schwer diese stabil hinzubekommen und zu halten. Ein Unit Test darf nie rot sein, wenn die Funktionalität eigendlich Funktioniert. Bei einem End-to-End Test kann dies durch äussere Einflüsse jedoch vorkommen.



<img src="/assets/images/testpyramid/coverage.JPG" alt="Test Pyramide: Abdeckung" class="centered" style="max-width:550px;" />

**Abdeckung**
Wir ein manuelles Test-Szenario durchgespielt, wird im hinergrund nicht sehr viel Code durchloffen. Genauso auch bei einem End-to-End Test. Ein einzelner Unit-Test durchläuft natürlich noch weniger Code, jedoch decken diese durch die hohe Anzahl einen viel höheren Anteil ab.



<img src="/assets/images/testpyramid/stakeholders.JPG" alt="Test Pyramide: Stakeholder" class="centered" style="max-width:550px;" />

**Stakeholder**
Ein sehr wichtiger Punkt finde ich, dass Teste im oberen Teil der Pyramide ganz andere Stakeholder haben als im unteren Teil. Kaum ein Produktverantwortlicher will das wir Entwickler Unit Tests schreiben. Aber er will Qualität. Desshalb sind wir dafür verantwortlich. Anders sieht es Beispielsweise bei End-to-End Tests aus. Da kann es einen Produktverantwortlichen sehr wohl interessieren, welche Use-Cases ihm eine hohe Confidence geben sollen für einen anstehenden Release.

## Ablauf

<img src="/assets/images/e2e-path.PNG" alt="Test Pyramide: Geschwindigkeit" class="centered" style="max-width:550px;" />

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
