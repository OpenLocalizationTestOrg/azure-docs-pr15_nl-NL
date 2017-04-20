<properties 
    pageTitle="De PhoneFactor-Agent upgraden naar de Server Azure meervoudige verificatie"
    description="In dit document wordt beschreven hoe u aan de slag met Azure MFA Server en hoe u een upgrade uitvoert vanuit de oudere phonefactor-agent."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="upgrading-the-phonefactor-agent-to-azure-multi-factor-authentication-server"></a>De PhoneFactor-Agent upgraden naar de Server Azure meervoudige verificatie

Een upgrade van de v5.x PhoneFactor Agent of oudere op de Server Azure meervoudige verificatie moet PhoneFactor Agent en de verbonden onderdelen worden verwijderd voordat de meervoudige verificatie-Server en de verbonden onderdelen kunnen worden geïnstalleerd.

## <a name="to-upgrade-the-phonefactor-agent-to-azure-multi-factor-authentication-server"></a>De PhoneFactor Agent upgraden naar Azure meervoudige verificatieserver
<ol>
<li>Eerst een back-up het gegevensbestand PhoneFactor. De standaardlocatie is C:\Program Files\PhoneFactor\Data\Phonefactor.pfdata.


<li>Als de gebruiker-Portal is geïnstalleerd:</li>
<ol>
<li>Navigeer naar de map installeren en back-up van het bestand web.config. De standaardlocatie is C:\inetpub\wwwroot\PhoneFactor.</li>


<li>Als u aangepaste thema's hebt toegevoegd bij de portal, back-up van uw aangepaste map onder de map C:\inetpub\wwwroot\PhoneFactor\App_Themes.</li>


<li>Verwijder de Portal van de gebruiker via de PhoneFactor-Agent (alleen beschikbaar als geïnstalleerd op dezelfde server als de PhoneFactor-Agent) of via de Windows-programma's en onderdelen.</li></ol>




<li>Als de webservice voor Mobile-App is geïnstalleerd:
<ol>
<li>Ga naar de map installeren en back-up van het bestand web.config. De standaardlocatie is C:\inetpub\wwwroot\PhoneFactorPhoneAppWebService.</li>
<li>Verwijderen van de mobiele App-webservice tot en met Windows-programma's en onderdelen.</li></ol>

<li>Als de SDK van Web-Service is geïnstalleerd, moet u het verwijderen via de PhoneFactor-Agent of via de Windows-programma's en onderdelen.

<li>Verwijderen van de PhoneFactor-Agent tot en met Windows-programma's en onderdelen.

<li>Installeer de meervoudige verificatie-Server. Houd er rekening mee dat het installatiepad wordt opgehaald uit het register uit de vorige PhoneFactor Agent installatie zodat deze op dezelfde locatie (bijvoorbeeld C:\Program Files\PhoneFactor) moet installeren. Nieuwe installaties heeft een andere standaard installatiepad (zoals C:\Program Files\Multi-Factor verificatieserver). Het gegevensbestand opmerkingen van de vorige PhoneFactor Agent moet worden bijgewerkt tijdens de installatie, zodat uw gebruikers en de instellingen moeten nog wel werd weergegeven na de installatie van de nieuwe meervoudige verificatie-Server.

<li>Als u wordt gevraagd, de meervoudige verificatie-Server activeren en te zorgen dat deze is toegewezen aan de juiste replicatiegroep.

<li>Als de Web-Service SDK eerder is geïnstalleerd, installeert u de nieuwe Web Service SDK via de gebruikersinterface voor meervoudige verificatie-Server. Houd er rekening mee dat de standaardnaam virtuele map nu "MultiFactorAuthWebServiceSdk" in plaats van 'PhoneFactorWebServiceSdk is'. Als u de naam van de vorige gebruiken wilt, moet u de naam van de virtuele map wijzigen tijdens de installatie. Anders als u de installatie gebruik van de nieuwe standaardnaam toestaat, moet u de URL in alle toepassingen die verwijzen naar de Web-Service SDK zoals de Portal van de gebruiker en de mobiele App-webservice zodat deze verwijzen op de juiste locatie wijzigen.

<li>Als de gebruiker-Portal eerder op de Server PhoneFactor-Agent is geïnstalleerd, installeert u de nieuwe meervoudige verificatie gebruiker Portal via de gebruikersinterface voor meervoudige verificatie-Server. Houd er rekening mee dat de standaardnaam virtuele map nu "MultiFactorAuth" in plaats van 'PhoneFactor is'. Als u de naam van de vorige gebruiken wilt, moet u de naam van de virtuele map wijzigen tijdens de installatie. Als u de installatie gebruik van de nieuwe standaardnaam toestaat, moet u Klik op het pictogram van de Portal van de gebruiker in de meervoudige verificatie-Server en bijwerken van de URL van de Portal gebruiker op het tabblad instellingen.

<li>Als de gebruiker-Portal en/of de mobiele App-webservice eerder was geïnstalleerd op een andere server dan de PhoneFactor-Agent:
<ol>
<li>Ga naar de installatielocatie (bijvoorbeeld C:\Program Files\PhoneFactor) en de juiste installer(s) kopiëren naar de andere server. Er zijn 32-bits en 64-bits installatieprogramma's voor de gebruiker-Portal en de mobiele App-webservice. Ze worden MultiFactorAuthenticationUserPortalSetupXX.msi en MultiFactorAuthenticationMobileAppWebServiceSetupXX.msi respectievelijk genoemd.</li>
<li>Als u wilt installeren de gebruiker-Portal op de webserver, open een opdrachtpromptvenster als beheerder en de MultiFactorAuthenticationUserPortalSetupXX.msi uitvoeren. Houd er rekening mee dat de standaardnaam virtuele map nu "MultiFactorAuth" in plaats van 'PhoneFactor is'. Als u de naam van de vorige gebruiken wilt, moet u de naam van de virtuele map wijzigen tijdens de installatie. Als u de installatie gebruik van de nieuwe standaardnaam toestaat, moet u Klik op het pictogram van de Portal van de gebruiker in de meervoudige verificatie-Server en bijwerken van de URL van de Portal gebruiker op het tabblad instellingen. Bestaande gebruikers moet worden geïnformeerd over de nieuwe URL.</li>
<li>Ga naar de Portal gebruiker installatielocatie (bijvoorbeeld C:\inetpub\wwwroot\MultiFactorAuth) en het bestand web.config bewerken. Kopieer de waarden in de secties appSettings en applicationSettings van uw oorspronkelijke bestand van web.config back-upgegevens vóór de upgrade in het nieuwe web.config-bestand. Als de nieuwe naam van de standaard-virtuele map is bewaard tijdens de installatie van de Web-Service SDK, moet u de URL in de sectie applicationSettings zodat deze verwijzen naar de juiste locatie wijzigen. Als eventuele andere standaardinstellingen in het vorige bestand web.config zijn gewijzigd, moet u deze dezelfde wijzigingen toepassen op het nieuwe web.config-bestand.</li>
<li>Als u wilt de webservice voor Mobile-App installeren op de webserver, open een opdrachtpromptvenster als beheerder en de MultiFactorAuthenticationMobileAppWebServiceSetupXX.msi uitvoeren. Houd er rekening mee dat de standaardnaam virtuele map nu "MultiFactorAuthMobileAppWebService" in plaats van 'PhoneFactorPhoneAppWebService is'. Als u de naam van de vorige gebruiken wilt, moet u de naam van de virtuele map wijzigen tijdens de installatie. U kunt kiezen een kortere naam waarmee u gemakkelijk voor eindgebruikers typen in op hun mobiele apparaten. Als u de installatie gebruik van de nieuwe standaardnaam toestaat, moet u Klik op het pictogram Mobile-App in de meervoudige verificatie-Server en bijwerken van de URL van de mobiele App webservice.</li>
<li>Ga naar de mobiele App-webservice installatielocatie (bijvoorbeeld C:\inetpub\wwwroot\MultiFactorAuthMobileAppWebService) en het bestand web.config bewerken. Kopieer de waarden in de secties appSettings en applicationSettings van uw oorspronkelijke bestand van web.config back-upgegevens vóór de upgrade in het nieuwe web.config-bestand. Als de nieuwe naam van de standaard-virtuele map is bewaard tijdens de installatie van de Web-Service SDK, moet u de URL in de sectie applicationSettings zodat deze verwijzen naar de juiste locatie wijzigen. Als eventuele andere standaardinstellingen in het vorige bestand web.config zijn gewijzigd, moet u deze dezelfde wijzigingen toepassen op het nieuwe web.config-bestand.</li></ol>
