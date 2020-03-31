---
title: Dependency Injection - Anwendung in der Praxis
toc: true
note: true
updated: 2019-12-20 17:37
teaser: Microsoft hat es uns mit der  ziemlich einfach gemacht, die "Macht" // Potenzial? von Dependency Injection in unseren .NET Core Projekten auf einfache Art und Weise zu benützen. Leider wird der Funktionsumfang jedoch viel zu wenig ausgereizt.
---
Ich hatte kürzlich mit einem Projekt zu tun, welches zwar Dependency Injection benützt hatte, jedoch hat man es nicht verwendet, um die Software Architektur zu verbessern. Vielmehr wurde es als eine Art Infrastruktur-Komponente für Web Projekte angesehen. Dabei hilft Dependency Injection sowie ein IoC Container enorm dabei die Softwarearchitektur im Sinne von Clean Code und den SOLID Prinzipien zu verbessern.

In dem Projekt wurde an sehr vielen Orten das Single Responsibility Prinzip verletzt. Es gab sehr viele Grosse Klassen, welche alle sehr viel gemacht haben. Da die Software schnell gewachsen ist, sind für Speziafälle jewils direkt tief verschachtelte if-Statements verwendet worden. Im folgenden will ich ein kleines Beispiel Vorstellen, welches an das Projekt angelehnt ist. Der Code zu diesem Artikel befindet sich auf Github [^githubrepo].

## Beispiel

Wir haben ein Konzept `Auftrag` welches verschiedene *Status*, konkret `AuftragsStatus` haben kann. Wie man sich allein aus dieser Ausgangslage sicherlich vorstellen kann war an diversen Orten genau dieser Status geprüft worden un je nach Status wurde etwas anderes ausgeführt oder angezeigt. In diesem Beispiel wollen wir und mal auf die Anzeige beschränken. Sagen wir, wir wollen je nach `AuftragsStatus` die Anzeige des Auftrags etwas anders darstellen.

```csharp
public class Auftrag
{
    public AuftragStatus Status { get; set; }

    // ...
}

public enum AuftragStatus
{
    Erfasst,
    Bearbeitet,
    Deaktiviert,
    Archiviert,
    Geloescht
}
```

Im angesprochenen Projekt wurden jeweils solche Verzweigungen direkt in der View gemacht. Es gab (wenn übrehaupt) ein ViewModel, welches alle Properties aller Spezialfälle beinhaltet. Dabei kann man mit hilfe von Patterns wie Factory, Strategy, etc (LIIIIIIIINK), sowie Dependency Injection und IoC einfachere, wartbarere Lösungen erstellen, welche Beispielweise das SIngle Responsibility- oder Open- CLosed Prinzip erfüllen. Ein Beispiel wäre, wenn man für jeden Status eine View und ViewModel erstellen würde.

```csharp
public abstract class AuftragViewModel
{
    // ...
}

public class AuftragDeaktiviertViewModel : AuftragViewModel
{
    // ...
}

public class AuftragErfasstViewModel : AuftragViewModel
{
    // ...
}
```

Diese ViewModels haben dann nur Properties drauf, welche auch wirklich benötigt und angezeigt werden. Da jedes dieser ViewModels vom abstrakten `AuftragViewModel` ableitet, können wir nun einen zentralen Ort definieren, welcher uns das richtige ViewModel zurückgibt. Dieser Ort gibt nicht nur das richtige ViewModel zurück, sondern ist eigentlich auch für dessen Instanzierung zuständig. Natürlich wollen wir das ViewModel aber nicht mit `new` instanzieren, da wir uns ansonten noch um die KOntruktor-Argumente kümmern müssen, für welche wir ja einen IoC COntainer haben. ALso eine typische Factory.

```csharp
public class AuftragViewModelFactory
{
    private readonly IServiceProvider _serviceProvider;

    public AuftragViewModelFactory(IServiceProvider serviceProvider)
    {
        _serviceProvider = serviceProvider;
    }

    public AuftragViewModel Create(Auftrag auftrag)
    {
        AuftragViewModel viewModel = null;

        if (auftrag.Status.Equals(AuftragStatus.Deaktiviert))
        {
            viewModel = _serviceProvider.GetService<AuftragDeaktiviertViewModel>();
        }
        else if (auftrag.Status.Equals(AuftragStatus.Erfasst))
        {
            viewModel = _serviceProvider.GetService<AuftragErfasstViewModel>();
        }

        if (viewModel == null)
        {
            throw new Exception($"Kein Binding für {nameof(BindingKeys.AuftragsStatus)} '{auftrag.Status}' vorhanden!");
        }

        return viewModel;
    }
}
```

TODO: Beschreiben
Da die Factory die ViewModel Instanzen per ServiceProvider instanziert, müssen diese zuerst registriert werden.

```csharp
public class AuftragModule
{
    public static void Register(IServiceCollection container)
    {
        container.AddTransient<AuftragDeaktiviertViewModel>();
        container.AddTransient<AuftragErfasstViewModel>();
    }
}
```

Die Anwendung ist nun denkbar einfach. Das ViewModel kann einfach erzeugt werden, Da ein VM normalerweise die View kennt, sollte auch das rendern die View kein problem mehr sein

```csharp
[TestMethod]
public void Create_WennBindingsAufgesetzt_DannWirdKorrektesViewModelErzeugt()
{
    // Arrange
    IServiceCollection container = new ServiceCollection();
    AuftragModule.Register(container);
    IServiceProvider serviceProvider = container.BuildServiceProvider();

    Auftrag auftrag = new Auftrag { Status = AuftragStatus.Deaktiviert };
    AuftragViewModelFactory auftragViewModelFactory = new AuftragViewModelFactory(serviceProvider);

    // Act
    AuftragViewModel viewModel = auftragViewModelFactory.Create(auftrag);

    // Assert
    Assert.IsInstanceOfType(viewModel, typeof(AuftragDeaktiviertViewModel));
}
```

## Erweiterung mit Contextual Bindings
Wie man anhand des obigen Beispiels vielleicht erraten kann, ist der `AuftragStatus` ziemlich zentral. AN unterschiedlichsten stellen wird aufgrund dieses Status eine andere Logik aufgerufen. Dies führt dazu, dass es wohl diverse Factory-Implementationen geben wird, welche alle ähnliche Logik beinhalten.

Es wäre schön, wenn man beim registrieren eines Typs auch gleich Informationen definieren könnten, wann das entsprechende Binding aufgelöst werden soll. Wenn es also für einen Service mehrere Implementationen gibt, so soll mann beim registrieren der entsprechenden Implementation definieren können, unter welchen Bedingungen das Binding relevant ist. Im NInject Framework [^ninject] gibt es dazu "Contextual Bindings" [^ninjectcontextual]. Im Microsoft DI Framework existiert so eine Lösung meines Wissens nach nicht, aber man kann es selber bauen [^githubrepo].

```csharp
public class AuftragViewModelFactory
{
    private readonly IServiceProvider _serviceProvider;

    public AuftragViewModelFactory(IServiceProvider serviceProvider)
    {
        _serviceProvider = serviceProvider;
    }

    public AuftragViewModel Create(Auftrag auftrag)
    {
        AuftragViewModel viewModel = _serviceProvider.Resolve<AuftragViewModel>(BindingKeys.AuftragsStatus, auftrag.Status);

        if (viewModel == null)
        {
            throw new Exception($"Kein Binding für {nameof(BindingKeys.AuftragsStatus)} '{auftrag.Status}' vorhanden!");
        }

        return viewModel;
    }
}
```

Diese Factory Implementation ruft eine neue Extension Methode `Resolve` auf, welche ich geschrieben habe. Mitgegeben werden können Metadaten und zwar ein key sowie ein value. Dies ist stark vom NInject Metadata Binding abgeschaut.

```csharp
public class AuftragModule
{
    public static void Register(IServiceCollection container)
    {
        container.Bind<AuftragViewModel, AuftragDeaktiviertViewModel>()
            .WithMetadata(BindingKeys.AuftragsStatus, AuftragStatus.Deaktiviert);

        container.Bind<AuftragViewModel, AuftragErfasstViewModel>()
            .WithMetadata(BindingKeys.AuftragsStatus, AuftragStatus.Erfasst);
    }
}

public class BindingKeys
{
    public const string AuftragsStatus = "AuftragsStatus";
}
```

Beim registrieren einer Service-Implementation können neu Metadaten mitgegeben werden. Dass AuftragDeaktiviertViewModel wird so Beispielsweise an den `AuftragStatus.Deaktiviert` geknüpft. Somit ist diese Logik als Art "Konfiguration" ausserhalb der Factory definiert.

<div class="divider"></div>

## Referenzen & Informationsquellen
[^dinetcore]: <a href="https://docs.microsoft.com/en-us/aspnet/core/fundamentals/dependency-injection" target="_blank">Dependency injection in ASP.NET Core</a>
[^ninject]: <a href="http://www.ninject.org/" target="_blank">NInject</a>
[^ninjectcontextual]: <a href="https://github.com/ninject/Ninject/wiki/Contextual-Binding" target="_blank">NInject - Contextual Bindings</a>
[^githubrepo]: <a href="https://github.com/Cogax/DependencyInjectionExtensions" target="_blank">Github: Extenstions und Beispiele</a>


https://www.tutorialspoint.com/design_pattern/factory_pattern.htm