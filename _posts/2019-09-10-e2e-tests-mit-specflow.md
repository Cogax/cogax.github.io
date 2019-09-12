---
title: E2E Tests für ASP.NET Core mit Specflow
updated: 2019-09-10 17:53
teaser: In diesem Artikel beschreibe ich wie man End-to-end (E2E) Tests so schreibt, damit diese auch von Business Personen wie Product Owner oder Domänenexperten gelesen werden können. Dieser Ansatz kann gerade bei der Arbeit mit Legacy Projekten welche bisher ohne jegliche Tests entwickelt wurden von Vorteil sein.
---

Kürzlich hatte ich mit einem Legacy Projekt zu tun und wollte gewisse Teile umbauen (Refactoring). Natürlich ist ein Refactoring unter einem "Sicherheitsnetz" aus automatisierten Tests wesentlich einfacher. Leider gab es für dieses Projekt weder Unit- noch Integrations- oder System- Tests. Ich entschied mich dazu, vor meinem Refactoring die wichtigsten Use-Cases mit End-to-End Tests abzusichern. Da ich bei anderen Projekten sehr gute Erfahrungen mit Specflow gemacht hatte setzte ich dies gleich ein.

# Test Pyramide & Test Quadranten
Die Test Pyramide [^7] sollte jedem Entwickler ein Begriff sein. Sie sagt im wesentlichen aus, dass es drei Stufen von Tests gibt: Unit-, Integrations- und System- Tests. Unit Tests sind die Tests, welche die kleinste Unit einer Software testen. Diese Tests laufen  wobei die Unit Tests eine sehr schnEs ist klar, dass E2E Tests nicht die performantesten sind, jedoch sind es genau diese, welche dem Entwickler ein sicheres Gefühl geben, dass er nicht kaput gemacht hat. Sieht man sich die Test Quadranten[^1] an, so sehe ich diese Tests im 2. Quadranten.

> End-to-end tests (also called Broad Stack Tests) give you the biggest confidence when you need to decide if your software is working or not. [^2]

Grunsätzlich bin ich der Meinung, man soltle bei einem Refactoring (und auch generell) die Test-Pyramide von unten hoch arbeiten. Zuerst also Unit- dann Integrations- und am Ende End-to-End- Tests schreiben. In diesem konkreten Fall war die Architektur schlicht (noch) nicht ausgelegt, dass man in absehbarer ZEit sinnvole Unit- oder Integrationstest geschrieben hätte.

In diesem Artikel verwende ich E2E Tests als synonym für Akzeptanztests. Zum Thema Akzeptanztests, ATDD, BDD, etc. findet man in Google genug nützliche Links, unter anderem diesen[^3] und diesen[^4]. Durch den Einsatz der Gherkin Syntax [^5] werden werden die Tests für alle involvierten Personen lesbar gemacht. Somit könnten sogar konkrete Anforderungen als Gherkin Feature formuliert werden. Cucumber, das Tool welche Gherkin interpretiert ist in .NET als Specflow [^6] bekannt.

TODO: Hier das Feature zeigen und beschreiben, dann erst die implementation!!!

# Let's go
Das Projekt war ein .NET Framework Projekt welches .NET Core 2.0 MVC als Nuget Package referenziert hat:
```csharp
<Project Sdk="Microsoft.NET.Sdk.Web">
    <TargetFramework>net47</TargetFramework>
    ...
    <PackageReference Include="Microsoft.AspNetCore" Version="2.0.1" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="2.0.1" />
    ...
```




<div class="divider"></div>

## Referenzen

[^1]: <a href="http://tryqa.com/what-are-test-pyramid-and-testing-quadrants-in-agile-testing-methodology/" target="_blank">What are Test Pyramid and Testing Quadrants in Agile Testing Methodology?</a>
[^2]: <a href="https://martinfowler.com/articles/practical-test-pyramid.html" target="_blank">The Practical Test Pyramid</a>
[^3]: <a href="http://tryqa.com/what-are-the-different-agile-testing-methodology-test-driven-development-behavior-driven-development/#behavior_driven_development" target="_blank">BDD</a>
[^4]: <a href="https://martinfowler.com/articles/practical-test-pyramid.html#acceptance" target="_blank">Akzeptanz Tests</a>
[^5]: <a href="https://cucumber.io/docs/gherkin/reference/" target="_blank">Gherkin Reference</a>
[^6]: <a href="https://specflow.org/" target="_blank">Specflow</a>
[^7]: <a href="https://martinfowler.com/bliki/TestPyramid.html" target="_blank">TestPyramid</a>
