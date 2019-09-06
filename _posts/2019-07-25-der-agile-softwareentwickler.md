---
title: Der Agile Softwareentwickler
updated: 2019-07-25 23:37
teaser: Software entwickelt man heute agil. Agil ist in, agil hat sich bewährt. Alles dreht sich um agile Softwareentwicklung. Unternehmen krempeln ihre Softwareentwicklungs Abteilungen um und Informatikstudenten erlernen die inkrementelle, iterative und empirische herangehensweise bereits im Studium. In Schulungen und Kursen werden vermeindlich neue Begriffe wie Product Owner, Scrum Master, Sprint oder Daily vermittelt und erläutert. Was dabei leider sehr oft nicht berücksichtigt wird ist, dass sich die Rolle der Entwickler ebenfalls wesentlich ändert.
---
Will man von einem traditionellen (wohl Wasserfall orientierten-) Ansatz auf einen Agilen Softwareentwicklungs-Ansatz umstellen, so kommt man nicht um prominente Vertreter wie Scrum oder Kanban herum. Ehe man sich versieht, setzt man sich mit Rollenbeschreibungen wie dem Product Owner oder Scrum Master auseinandersetzen. Die Unmengen an Büchern, Kurse und natürlich Blog Posts helfen einem dabei. Was bei einer solchen "Agilen Transformation" meist ausser Acht gelassen wird ist, dass sich die Rolle des Entwicklers sehr stark ändert. Erkennt man dies nicht, droht die Transformation zu scheitern.

In diesem Artikel möchte im meine Gedanken zur Rolle eines agilen Softwareentwicklers teilen.

## Wasserfall vs. Agil

In einem typisch Wasserfall orientiertem Vorgehen ist der Softwareentwickler primär in der Entwicklungs-Phase involviert. Im Gegensatz dazu ist er bei einem Agilen Vorgehen in allen Projektphasen vertreten. Natürlich bedeutet dies nicht, dass er auf einmal die Anforderungen für das Produkt selbst erhebt und damit die "Requirement Engineers" Arbeitslos macht. Es geht dabei vielmehr darum, dass bei einem Agilen Vorgehen die verschiedenen Diszplinen sehr eng miteinander zusammenarbeiten und dadurch alle daran beteiligten Personen ihr Spektrum erweitern müssen.

![Minion](/assets/images/waterfall-vs-agile.png)

Das Entwickler-Team wird also bereits bei der Anforderungserhebung miteinbezogen. Dies is sehr entscheidend, da nur sie das Know-How über die Implementierung der Software haben. Sie sind die einzigen, welche beurteilen können wie eine neue Anforderung implementiert werden soll, ob es eine einfachere und güstigere Alternative für das Problem gibt, oder ob die Architektur für kommende Anforderungen bereits gewappnet ist oder nicht.

Die Agile Vorgehensweise sagt meiner Meinung nämlich vorallem eines aus:
**Diejenigen, die die Arbeit machen, entscheiden auch wie sie sie machen. Dies betrifft sowohl die technische Umsetzung, wie auch die Vorgehensweise, mit welcher die Software entwickelt wird.**

Im folgenden will ich genauer auf die (meiner Meinung nach) relevantesten Eigenschaften und Fähigkeiten eines Entwicklers im Agilen Umfeld eingehen.

## Teamfähigkeit

Die Fähigkeit im Team arbeiten zu können muss jeder beteiligte einer agilen Umgebung besitzen, nicht nur der Entwickler. Sieht man sich das Agile Manifest [^1] genauer an, so sieht man, dass gleiche mehrere der dort definierten Werte und Prinzipien darauf abzielen:

> **Individuen und Interaktionen** mehr als Prozesse und Werkzeuge <br />
> **Zusammenarbeit mit** dem Kunden mehr als Vertragsverhandlung <br />
> *[..]* <br />
> Die besten Architekturen, Anforderungen und Entwürfe entstehen durch selbstorganisierte Teams. 

### Was Teamwork nicht ist

Teamfähigkeit bzw. Teamwork wird leider sehr oft missverstanden. Vielleicht erkennt man sowas erst wenn man selbst einige Zeit in einem selbstorganisierten Team mitgearbeitet hat. Ich habe mich schwergetan, eine einheitliche Definition für Teamwork auszuarbeiten. Stattdessen habe ich einige Punkte aufgelistet, was für mich Teamwork beduetet, bzw. eben nicht:

* Teamwork bedeutet nicht, Aufgaben direkt an Teammitglieder zu vergeben.
  * Alle Teammitglieder sollen stets informiert sein wer was macht. Aufgaben sollen also ebenfalls im Team verteilt werden. Im Idealfall organisiert sich dabei das Team selbst und die Aufgaben werden vom Team selbst verteilt.
* Teamwork bedeutet nicht, dass nur ein Mitglied das Wissen über eine Aufgabe hat.
* Teamwork beudeutet, dass Probleme im Team besprochen und gelöst werden. Tritt also bei einer Aufgabe ein Problem auf, so wird dieses im Team diskutiert und die Lösung, welche für das Team am besten passt wird ausgewählt.
* Teamwork beduetet, dass alle Mitglieder über den Stand der aktuellen Arbeiten informiert sind. Fällt ein Teammitglied aus (Bspw. Durch Krankheit), so ist der akutelle Arbeitsstand immer präsent.
* Teamwork beudetet, dass alle Mitglieder miteinander und für einander arbeiten. Die Arbeit ist erst getan wenn das ganze Team fertig ist. Die Leistungen der Individuen hat keine Relevanz, es zählt ausschieslich die Teamleistung.
  * Teamwork beduetet nicht, dass Egebnis aufgrund einer Person gut oder schlecht ist. Es ist immer das ganze Team verantwortlich für Erfolge und Niederlagen.
* Teamwork bedeutet, dass es keine Hierarchien gibt. 
  * Keine Hierarchien impliziert nicht, dass auch keine Strukturen vorhanden sind.
  * In einem guten Team gibt es erfahrene wie auch weniger erfahrene Mitglieder. Anstatt das Erfahrene Mitglieder Hierarchisch hervorgehoben werden, sind in einem Team vielmehr Leader Qualitäten gefragt. Mitglieder mit weniger Erfahrung müssen die Qalitäten aufweissen sich das Wissen schnell von anderen Mitgliedern "abzusaugen".
* Teamwork beduetet, dass man auch weniger erfreuliche Arbeiten zugunsten des Teams erledigt.
* Team ist Interdisziplinär
  * Technisch
  * Fachlich nicht umbeingt
* Teamwork bedeutet, dass ständig eine Ansprechsperson vorhanden ist.
* Teamwork beduetet, dass Kritik immer konstruktiv plaziert wird.
  * Teamwork beduetet, dass man Froh um konstruktive Kritiv und immer offen dafür ist.
* Teamwork beduetet ehrlichkeit in allen belangen. Führt zu Vertrauen.
* PairProgramming
* Teamwork bedeutet, dass eine aktive Feedbackkultuir zwischen den Mitgliedern lebt. Jeder kann mit Kritik umgehen uns weiss diese zu verarbeiten. Jeder ist Einsichtig und nimmt Kritik entgegen. Jeder kann seine Meinung äussern und vertreten. Niemand ist rechthaberisch.
* Teamwork beduetet nicht, alle arbeit auf sich zu ziehen und diese dann zu delegieren. Ein oft beobachtetes Phänomen wenn man neu in einem Team zusammenarbeiten soll. Falls man durch mehrwissen gewisse Arbeiten auf sich zieht muss man damit uzugehen wissen. Man muss als Leader agieren und andere Mitglieder dabei unterstützen diese Fähigkeiten ebenfalls zu erlernen. Man ist verpflichtet, seine eigene Kapazität im Griff zu haben.
  * Das Problem ist höufig, dass solche Personen sich bei der Aufwand (Story-Point) Schätzung durchsetzen. Da das Know-How bei ihnen höher ist, wird die Arbeit oft nach ihrem Masse geschötzt und nich nach dem Maas des gesamten Teams. Desshalb sind dann diese Personen innerhalb einer Iteration höufig unter Druck.
* Wenn ein Mitglied mit seiner Arbeit fertig ist dann soll er den anderen helfen bevor er tech stories für sich umsetzt

### Was Teamwork ist

### Teamfähigkeit ist eine untypische Eigenschaft für Programmierer

Die Zeiten, als ein einzelner Programmierer das Know-How über die gesammte Software hat und diese alleine nach seinen Vorstellungen von Architektur und Design entwickelt ist vorbei. Dies ist nicht nur nicht Business-Orientiert, es skaliert nicht und ist kein langfristiger Ansatz. Solche Software wird früher oder später neu geschrieben werden müssen.

>Vielleicht sind wir nicht deswegen in das Metier der Programmierer gelangt, um mit anderen zu kollaborieren. Tja, Überraschung! Beim Programmieren geht es vor allem um die Zusammenarbeit mit anderen. Wir müssen mit unserem Business zusammenarbeiten - genauso wie auch mit unseren Programmierkollegen *[5]*

Im Paper [^7] wurde dies mithilfe von Extreme Programming ziemlich gut zusammengefasst:
> Die Entwickler und Entwicklerinnen müssen in einem hohen Mass teamfähig sein, da gerade das Entwickeln in Paaren eine hohe Sozialkompetenz voraussetzt. Dies ist insbesondere wichtig, da in XP der Programmierer neben der eigentlichen Programmierung auch für die Aufwandschätzung und für das Erstellen der Tests verantwortlich ist, also Aktivitäten, die immer auch mit den anderen Personen koordiniert werden müssen. Einzelgänger, die nicht über ihre Arbeit diskutieren können oder nicht kritikfähig sind, eignen sich nicht für ein XP-Team. Der Teamerfolg muss in jedem Moment über dem eigenen Erfolg stehen. Ängste sich zu blamieren, veraltete Ansichten zu vertreteten, nutzlos zu werden oder den hohen Anforderungen nicht zu genügen, müssen überwunden werden, um erfolgreich in einem XP-Team mitarbeiten zu können. Dies ist gerade für die Programmierer, die in der Regel nicht besonders kommunikativ sind, eine besondere Herausforderung (vgl. Beck 2003: 141).

## Kommunikation

Die Anforderungen an die Kommunikationsskills eines Softwareentwicklers verändern sich bei einer Umstellung auf Agil enorm. Während ein Softwareentwickler im klassischen Wasserfall modell quasi "Stupid" die vorgegebenen Anforderungen und Designs implementiert muss er im Agilen Umfeld mit diversen anderen Rollen kommunizieren können. Ob Stakeholder, Auftraggeber, End-User, Product Owner, Scrum Master, Tester, UI-/UX Spezialisten oder anderen Entwicklern - er muss mit ihnen allen Kommunizieren können. Viele dieser Rollen sprechen eine ganz andere Sprache und verstehen oft unter selbem Begriff etwas komplett anderes. Der Entwickler muss mit ihnen allen kommunizieren können, natürlich bekommt er da Untersützung von anderen Rollen (wie Beispielsweise dem Product Owner), trotzdem ist diese Fähigkeit sehr eintscheidend.

> Vielleicht sind Sie "nur ein Programmierer", aber die Fähigkeit, sich mit einem Geschäftskunden in der Sprache seine Branche zu unterhalten, ist für Ihren beruflichen Erfolg lebenswichtig. *[3]*

Die Kommunikation mit anderen Rollen erfordert "Soft-Skills" wie Epathie. Es gehört Beispielsweise zu den Aufgaben eines Agilen Softwareentwicklern gewisse Anforderungen zu hinterfragen und nach dem wahren Problem dahinter zu Fragen. Dies kann von der Gegenseite oft falsch aufgefasst werden.
Er muss seinen Standpunkt in gewissen Diskussionen klar vertreten können. Gerade im täglichen Krieg zwischen Business- und Technischen Anforderungen muss er die technischen Eigenheiten erläutern und auf den Punkt bringen können. In grösseren Agilen Umgebungen kann dies zum Teil etwas politisch werden - auch diese Fähigkeit muss er beweisen um gewisse "Lobying" Arbeit zu leisten.

> Extreme Programming recognizes the importance of design decisions, but it strongly resists upfront design. Instead, it puts an admirable effort into communication and improving the project's ability to change course rapidly. *[2]*

> When the customer says, "You must do this and this and this," you have to be prepared to discuss which of those items is really necessary and how much of each. *[6]*

## Technische Exzellenz

Dass ein Softwareentwickler in seiner Programmiersprache fit sein muss wird niemand bestreiten. Natürlich ist dies im Agilen Umfeld nicht anders. Jedoch kommen einige Aspekte hinzu. Da der Entwickler keine fest definierten Vorgaben hat sind plötzlich Kenntnisse und Fähigkeiten in Software- Design und Architektur Pflicht. Es gibt keinen fixen Plan den der Entwickler "nur noch" in Code umwandeln muss[^4]. Im Agilen Manifest stehen ebenfalls einige wichtige Sätze zu dieser Thematik:

> Ständiges Augenmerk auf technische Exzellenz und gutes Design fördert Agilität. <br />
> Einfachheit - die Kunst, die Menge nicht getaner Arbeit zu maximieren - ist essenziell. <br />
> Die besten Architekturen, Anforderungen und Entwürfe entstehen durch selbstorganisierte Teams. <br /> *[1]*

Ein Agiler Entwickler hat ein Produktorientiertes Denken, ein traditioneller Entwickler eher ein Projektorientiertes. Damit meine ich, dass das Ziel eines Agilen Softwareentwicklers ist, eine geile Lösung für das Problem des Anwenders zu entwickeln. Dabei legt er natürlich Wert auf Qualität, Wartbarkeit sowie Erweiterbarkeit, weil das Produkt und nicht das Projekt im Augenmek liegt.

> *[..]* developers without solid design principles will produce a code base that is hard to understand or change - the oppisite of agility *[2]*

Die Technische Exzellenz manifestiert sich jedoch nicht nur im Sourcecode. Es gehört zu den Aufgaben eines Entwicklers Aufwände sowie Risikos zu anfallenden Anforderungen abzuschätzen. Er muss technische Anforderungen vertreten, begrunden und dafür werben können. Er muss sich zudem mit Prinzipien wie Continuous Integration sowie Deployment auskennen, da diese ein Enabler für kurze Feedback-Zyklen und wirkliche Agilität sind.

## Domänenwissen

Der Beruf Softwareentwickler trägt mit sich, dass man sich immer in zwei Fachbereichen auskennen muss. Der eine ist natürlich der IT- bzw. Software- Fachbereich. Dieser alleine gehört zu den komplexeren. Der jeweils andere ist der Fachbereich, wofür die Software-Lösung entwickelt wird. Da der agile Softwareentwickler sehr nahe mit Rollen des Requirement-Engineering zusammenarbeitet muss er sich auch mehr in die Domäne einarbeiten, als wenn man die konkreten Anforderungen vorgesetzt bekommt. Dies erfordert den Willen sich in eine zum Teil fremde Domäne einzuarbeiten und diese versuchen zu verstehen.

Der Begriff "Domain Driven Design" welcher vom gleichnamigen Buch von Eric Evans stammt impliziert genau dies. Nicht per Zufall wird bereits im Vorwort das agile Entwicklungsvorgehen betont und der Zusammenhang zwischen Domänengetriebenem Softwaredesign und Agiler Vorgehensweise erläutert.

## Empirisches Mindset

Ein agiler Softwareentwickler entwickelt nicht nur die Software im Sinne eines empirischen Ansatzes sondern sollte auch einen solchen Mindset haben. Er will das Produkt stets verbessern, dies macht er in kleinen Schritten und reflektiert diese zwischendurch. Genau diese Einstellung sollte er auch zu seinem Handwerk haben. Er will sich stets verbessern, stets neue Sachen und Herangehensweisen lernen. Er kann sein erlerntes Wissen weitergeben aber auch neue Sachen von anderen annehmen. 

### Entwicklermentalitäten

Erfahrene Entwickler äussern ihre Erfahrung auf verschiedene Arten. Das eine extrem ist, die "sich tendenziell überschätzende" Art und Weise. Für diese Entwickler ist alles relativ einfach umzusetzen. Probleme gibt es nicht nur Lösungen und kaum ist ein Problem andiskutiert wissen diese Entwickler bereits wie man sie am besten angehen und Lösen sollte. Die andere Art und Weise ist die "sich tendentiell unterschätzende". Diese Programmieren sehen überall Herausforderungen. Werden Probleme andiskutiert, so sind für diese Entwickler erstmal nur eine Menge an Problemen zu sehen. Generell denke ich, dass die zweite Art eher für ein agiles Entwicklungsteam geeognet ist. Sie nehmen Probleme ernst, was dazu führt dass sie sich damit auseinandersetzen. Sie hinterfragen die Dinge eher als die Entwickler welche schon die vermeindliche Lösung im Kopf haben. Entwickler die sich in solchen Situationen tendenziell überschätzen neigen dazu, Aufwandschätzungen etc. zu beinflussen. Diskussionen mit Product Owner oder Stakeholder sind zudem nicht so konstruktiv, da viele Lücken in den Anforderungen erst durch nachfragen der Entwickler auftauchen. Das Nachfragen und hinterfragen macht man jedoch nur, wenn man ein Problem sieht, nicht wenn man schon die geeignete Lösung dafür im Kopf hat.

## Schlussfolgerung

Ein Agile Entwickler denkt nicht in Projekten oder Meilensteinen. Ein Agiler Entwickler hat stets das Produkt im Fokus und will gute Lösungen für den Anwender entwickeln. Er ist Teamfähig, kann mit verschiedensten Rollen kommunizieren, ist technisch sehr versiert und arbeitet sich in den Fachbereich der Software hinein. All dies macht den Agilen Softwareentwickler zu einem der wertvollsten Personen in einem Unternehmen. Für Unternehmen stellt sich die Frage, wie kommt man an solche Agile Softwareentwickler. Volker Birk erläutert dies in seinem Talk ganz gut "Du musst sie ausbilden, ab und zu bekommst du einen geschenkt"[^4]. 

<div class="divider"></div>

## Referenzen

[^1]: Agiles Manifest, <a href="https://agilemanifesto.org/iso/de/manifesto.html" target="_blank">agilemanifesto.org</a>
[^2]: Domain Driven Design, Eric Evans
[^3]: Der leidenschaftliche Programmierer, Chad Fowler
[^4]: Software Engineering, Volker Birk, <a href="https://www.youtube.com/watch?v=CdMjNviFdXk" target="_blank">YoutTube</a>
[^5]: Clean Coder, Robert C. Martin
[^6]: Extreme Programming Explained, Kent Beck
[^7]: Extreme Programming - Eine Übersicht, Rolf Dornberger & Thomas Habegge, Fachhochschule Solothurn Nordwestschweit, Reihe A: Paper 2005-05
[^8]: Management 3.0, Jurgen Appelo