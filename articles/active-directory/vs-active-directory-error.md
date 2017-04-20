<properties 
    pageTitle="Fout tijdens de verificatie-detectie" 
    description="De wizard van de verbinding active directory een niet-compatibele verificatietype gedetecteerd" 
    services="active-directory" 
    documentationCenter="" 
    authors="TomArcher" 
    manager="douge" 
    editor=""/>
  
<tags 
    ms.service="active-directory" 
    ms.workload="web" 
    ms.tgt_pltfrm="vs-getting-started" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="tarcher"/>

# <a name="error-during-authentication-detection"></a>Fout tijdens de verificatie-detectie

De wizard gedetecteerd tijdens detectie vorige verificatiecode, een niet-compatibele verificatietype.   

##<a name="what-is-being-checked"></a>Wat is ingeschakeld?

**Notitie:** Om te kunnen naar behoren herkent vorige verificatiecode in een project, moet het project worden geoptimaliseerd.  Als u deze fout opgetreden en er geen een vorige verificatiecode in uw project, bouw die tabellen opnieuw en probeer het opnieuw.

###<a name="project-types"></a>Projecttypen

De wizard controleert welk type project dat u ontwikkelt zodat deze de juiste verificatie-logica in het project invoeren kunt.  Als er een controller waar vandaan `ApiController` in het project, het project een project WebAPI wordt beschouwd.  Als er alleen domeincontrollers die zijn afgeleid van `MVC.Controller` in het project, het project een project MVC wordt beschouwd.  Iets anders wordt niet ondersteund door de wizard.  Webformulieren projecten worden momenteel niet ondersteund.

###<a name="compatible-authentication-code"></a>Compatibele verificatie-Code

De wizard controleert ook voor verificatie-instellingen die eerder zijn geconfigureerd met de wizard of compatibel zijn met de wizard.  Als alle instellingen aanwezig zijn, wordt een herhaalde hoofdletters/kleine letters en de wizard wordt geopend en de instellingen weer te geven.  Als er slechts enkele instellingen aanwezig zijn, wordt een fout hoofdletters/kleine letters.

De wizard Hiermee wordt gecontroleerd op een van de volgende instellingen, die uit het vorige gebruik van de wizard voortvloeien in een project MVC:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:AADInstance" value="" />
    <add key="ida:PostLogoutRedirectUri" value="" />

Bovendien de wizard Hiermee wordt gecontroleerd op een van de volgende instellingen in een project Web API die uit het vorige gebruik van de wizard voortvloeien:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:Audience" value="" />

###<a name="incompatible-authentication-code"></a>Niet-compatibele verificatiecode

Ten slotte probeert de wizard te detecteren versies van de verificatiecode die met eerdere versies van Visual Studio zijn geconfigureerd. Als u deze fout hebt ontvangen, betekent dit dat uw project bevat een niet-compatibele verificatietype. De wizard wordt gedetecteerd voor de volgende soorten verificatie uit eerdere versies van Visual Studio:

* Windows-verificatie 
* Afzonderlijke gebruikersaccounts 
* Organisatie-Accounts 
 

Om op te sporen Windows-verificatie in een project MVC, de wizard Hiermee wordt gezocht naar de `authentication` element uit uw **web.config** -bestand.

<pre>
    &lt;configuratie&gt;
        &lt;system.web&gt;
            <span style="background-color: yellow">&lt;verificatiemodus = "Windows" /&gt;</span>
        &lt;/system.web&gt;
    &lt;/Configuration&gt;
</pre>

Om op te sporen Windows-verificatie in een project Web API, de wizard Hiermee wordt gezocht naar de `IISExpressWindowsAuthentication` element uit van uw project **.csproj** bestand:

<pre>
    &lt;Project&gt;
&lt;PropertyGroup&gt;
            <span style="background-color: yellow">&lt;IISExpressWindowsAuthentication&gt;ingeschakeld&lt;/IISExpressWindowsAuthentication&gt;</span>
        &lt;/PropertyGroup > &lt;/Project        &gt;
</pre>

Om op te sporen afzonderlijke gebruikersaccounts verificatie, de wizard Hiermee wordt gezocht naar het element package uit uw **Packages.config** -bestand.

<pre>
    &lt;pakketten&gt;
        <span style="background-color: yellow">&lt;id="Microsoft.AspNet.Identity.EntityFramework als pakket inpakken" versie "2.1.0" = targetFramework = "net45" /&gt;</span>
    &lt;/pakketten&gt;
</pre>

Als u wilt een oud formulier Organisatieaccount verificatie kan worden gevonden, de wizard Hiermee wordt gezocht naar het volgende element van **web.config**:

<pre>
    &lt;configuratie&gt;
        &lt;appSettings&gt;
            <span style="background-color: yellow">&lt;sleutel toevoegen met = "ida: Realm" waarde = "***" /&gt;</span>
        &lt;/appSettings&gt;
    &lt;/Configuration&gt;
</pre>

Als u wilt wijzigen van het verificatietype, verwijdert u de niet-compatibele verificatietype en voer de wizard opnieuw uit.

Zie [Verificatie-scenario's voor Azure AD](active-directory-authentication-scenarios.md)voor meer informatie.