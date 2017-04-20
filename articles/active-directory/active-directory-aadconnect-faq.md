<properties
    pageTitle="Azure AD Connect: Veelgestelde vragen over | Microsoft Azure"
    description="Deze pagina bevat veelgestelde vragen over Azure AD Connect."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-faq"></a>Azure AD Connect Veelgestelde vragen

## <a name="general-installation"></a>Algemene installatie
**V: installatie werken als de globale beheerder van Azure AD 2FA ingeschakeld heeft?**  
Met de versies van februari 2016, wordt dit ondersteund.

**V: is er een manier om te Azure AD Connect onbeheerd installeren?**  
Dit wordt alleen ondersteund als u wilt installeren Azure AD Connect met de wizard installeren. Een installatie zonder toezicht en stille wordt niet ondersteund.

**V: ik heb een bos waar een domein niet bereikbaar. Hoe installeer ik Azure AD Connect?**  
Met de versies van februari 2016, wordt dit ondersteund.

**V: de AD DS-systeemstatus-agent werkt op server core?**  
Ja. Na de installatie van de agent, kunt u het registratieproces met het volgende PowerShell-commandlet uitvoeren: 

`Register-AzureADConnectHealthADDSAgent -Credentials $cred`

## <a name="network"></a>Netwerk
**V: ik heb een firewall, netwerkapparaat of iets anders dat de maximale tijd die verbindingen beperkt geopend in mijn netwerk kunt blijven. Hoe lang moet mijn client kant time-out drempelwaarde zijn bij het gebruik van Azure AD Connect?**  
Alle netwerken software, fysieke apparaten of iets anders dat de maximale tijd die verbindingen kunnen open blijven beperkt moet een drempelwaarde voor ten minste 5 minuten (300 seconden) gebruiken voor de verbinding tussen de server waarop de Azure AD Connect-client is geïnstalleerd en Azure Active Directory. Dit geldt ook voor alle eerder uitgebrachte Microsoft Identity synchronisatie gereedschap.

**V: zijn niveaudomeinen (één etiket domeinen) ondersteund?**  
Nee, Azure AD Connect biedt geen ondersteuning voor on-premises implementatie forests/domeinen niveaudomeinen gebruiken.

**V: zijn 'gestippelde"NetBios benoemde ondersteund?**  
Nee, Azure AD Connect biedt geen ondersteuning voor on-premises implementatie forests/domeinen waarvan de naam van de NetBios een periode bevat '. ' in de naam.

## <a name="federation"></a>Federatie
**V: wat moet ik doen als ik een e-mailbericht ontvangt dat vragen om mijn Office 365-certificaat te vernieuwen**  
Gebruik de richtlijnen die wordt beschreven in het onderwerp [vernieuwen van certificaten](active-directory-aadconnect-o365-certs.md) voor het vernieuwen van het certificaat.

**V: ik heb 'automatisch bijwerken gebruikmakende partij"instellen voor O365 gebruikmakende partij. Moet ik doen als mijn token handtekeningcertificaat automatisch boven?**  
Gebruik de richtlijnen die wordt beschreven in het artikel [certificaten vernieuwen](active-directory-aadconnect-o365-certs.md).

## <a name="environment"></a>Omgeving
**V: is dit de naam van de server nadat Azure AD Connect is geïnstalleerd ondersteund?**  
Nee. De servernaam wijzigt, wordt de synchronisatie-engine mogen geen verbinding maken met de SQL-database en de service wordt niet kan worden gestart.

## <a name="identity-data"></a>Id-gegevens
**V: het kenmerk UPN (userPrincipalName) in Azure AD niet overeenkomen met de on-premises UPN - waarom?**  
Raadpleeg de volgende artikelen:

- [Gebruikersnamen in Office 365, Azure of Intune niet overeenkomen met de on-premises implementatie UPN of de alternatieve aanmeldings-ID](https://support.microsoft.com/en-us/kb/2523192)
- [Wijzigingen worden niet gesynchroniseerd door het hulpprogramma Azure Active Directory-synchronisatie nadat u de UPN van een gebruikersaccount als u wilt gebruiken, een ander federatieve domein wijzigen](https://support.microsoft.com/en-us/kb/2669550)

U kunt ook Azure AD zodat de synchronisatie-engine bij het userPrincipalName zoals is beschreven in [Azure AD Connect synchronisatie servicefuncties](active-directory-aadconnectsyncservice-features.md)configureren.

## <a name="custom-configuration"></a>Aangepaste configuratie
**V: waar worden de PowerShell-cmdlets voor Azure AD Connect beschreven?**  
Met uitzondering van de cmdlets beschreven op deze site, worden de andere PowerShell-cmdlets zijn gevonden in Azure AD Connect niet ondersteund voor klantgebruik.

* *V: kan ik gebruiken 'servers exporteren importeren' zijn gevonden in *synchronisatie servicebeheer* configuratie verplaatsen tussen servers? **  
Nee. Deze optie niet alle configuratie-instellingen worden opgehaald en mag niet worden gebruikt. U moet in plaats daarvan de wizard gebruiken om te maken van de basis configuratie op de tweede server en de synchronisatie-regel-editor gebruiken om te genereren PowerShell-scripts voor een aangepaste regel verplaatsen tussen servers. Zie [aangepaste configuratie van actief naar tijdelijk opslaan server verplaatsen](active-directory-aadconnect-upgrade-previous-version.md#move-custom-configuration-from-active-to-staging-server).

## <a name="troubleshooting"></a>Problemen oplossen
**V: hoe kan ik hulp bij Azure AD Connect krijgen?**

[Zoek het Microsoft Knowledge Base (KB)](https://www.microsoft.com/en-us/Search/result.aspx?q=azure%20active%20directory%20connect&form=mssupport)

- Zoek het Microsoft Knowledge Base (KB) voor technische oplossingen voor veelvoorkomende einde-problemen oplossen over de ondersteuning voor Azure AD Connect.

[Microsoft Azure Active Directory-Forums](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)

- U kunt zoeken en blader voor technische vragen en van de community antwoorden of uw eigen vraag door te klikken [hier](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).

[Azure AD Connect-klantondersteuning](https://manage.windowsazure.com/?getsupport=true)

- Gebruik deze koppeling naar ondersteuning krijgen via de portal van Azure.
