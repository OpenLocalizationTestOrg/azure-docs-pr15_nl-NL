<properties
    pageTitle="Het beperken van toegang tot de inhoud van uw Azure CDN per land | Microsoft Azure"
    description="Informatie over het beperken van toegang tot uw Azure CDN-inhoud met de functie geografische filteren."
    services="cdn"
    documentationCenter=""
    authors="camsoper, rli"
    manager="akucer"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/14/2016"
    ms.author="Lichard"/>

#<a name="restrict-access-to-your-content-by-country---akamai"></a>Toegang tot uw inhoud per land - Akamai beperken

> [AZURE.SELECTOR]
- [Verizon](cdn-restrict-access-by-country.md)
- [Akamai standaard](cdn-restrict-access-by-country-akamai.md)

##<a name="overview"></a>Overzicht

Wanneer een gebruiker uw inhoud, al dan niet standaard aanvraagt, wordt de inhoud wordt verwerkt ongeacht waar de gebruiker dit verzoek om van toegevoegd. In sommige gevallen wilt u mogelijk het beperken van toegang tot uw inhoud per land. In dit onderwerp wordt uitgelegd hoe u het gebruiken van de functie voor het **Geografische filteren** om te kunnen de service toe te staan dat configureren of toegang weigeren per land.

> [AZURE.IMPORTANT] De producten Verizon en Akamai bieden dezelfde geografische filtering functionaliteit, maar verschilt van de gebruikersinterface. In dit document beschreven hoe de interface voor **Azure CDN standaard uit Akamai**. Zie [toegang tot uw inhoud per land - Verizon beperken](cdn-restrict-access-by-country.md)voor geografische-filteren met **Azure CDN standaard/Premium uit Verizon**.

Zie de sectie [overwegingen](cdn-restrict-access-by-country.md#considerations) aan het einde van het onderwerp voor informatie over aandachtspunten voor het configureren van dit type beperking.  

![Land filteren](./media/cdn-filtering/cdn-country-filtering-akamai.png)

##<a name="step-1-define-the-directory-path"></a>Stap 1: Het pad van de definiëren

Selecteer uw eindpunt in de portal en het tabblad geografische Filtering zoeken op de linkernavigatiebalk voor deze functie vinden.

Tijdens het configureren van een filter land, moet u het relatieve pad naar de locatie waaraan gebruikers worden wel of geen toegang. U kunt voor al uw bestanden met geografische filteren toepassen "/" of mappen geselecteerd door het opgeven van directory paden "/ afbeeldingen /". U kunt ook een geografische filteren in één bestand toepassen door het opgeven van het bestand en daarbij de afsluitende slash "/ pictures/city.png".

Voorbeeld directory pad filteren:

    /                                 
    /Photos/
    /Photos/Strasbourg/
    /Photos/Strasbourg/city.png

##<a name="step-2-define-the-action-block-or-allow"></a>Stap 2: Definiëren welke actie: blokkeren of toestaan

**Blokkeren:** Gebruikers uit de opgegeven landen wordt de toegang geweigerd aan activa bij dat pad recursieve aangevraagd. Als er geen andere land-filteropties zijn geconfigureerd voor die locatie, worden klikt u vervolgens alle andere gebruikers kunnen access.

**Toestaan:** Alleen uit de opgegeven landen kunnen gebruikers toegang tot de activa van dat pad recursieve gevraagd.

##<a name="step-3-define-the-countries"></a>Stap 3: De landen definiëren

Selecteer de landen die u wilt blokkeren of toestaan voor het pad. Zie [Azure CDN van Akamai landcodes](https://msdn.microsoft.com/library/mt761717.aspx)voor meer informatie.

De regel van het blokkeren van /Photos/Straatsburg/wordt bijvoorbeeld bestanden, met inbegrip filteren:

    http://<endpoint>.azureedge.net/Photos/Strasbourg/1000.jpg
    http://<endpoint>.azureedge.net/Photos/Strasbourg/Cathedral/1000.jpg


##<a name="country-codes"></a>Landcodes

De functie **Geografische Filtering** gebruikt landcodes definiëren van de landen waaruit een verzoek om te worden toegestaan of geblokkeerde voor een beveiligde map. U vindt de landcodes in [Azure CDN van Akamai landcodes](https://msdn.microsoft.com/library/mt761717.aspx). 

##<a id="considerations"></a>Overwegingen

- Dit kan duren een paar minuten totdat de wijzigingen in uw land filteren configuratie zijn doorgevoerd.
- Deze functie ondersteunt geen jokertekens (bijvoorbeeld ' *').
- De configuratie geografische filteren dat is gekoppeld aan het relatieve pad worden toegepast naar het opgegeven pad.
- Slechts één regel kan worden toegepast op hetzelfde relatieve pad (u kunt meerdere land-filters die naar hetzelfde relatieve pad verwijzen geen maken. Een map kan echter meerdere land filters hebben. Dit is vanwege de aard recursieve van land filters. Een ander land filter kan met andere woorden, aan een submap van een eerder geconfigureerde map worden toegewezen.

