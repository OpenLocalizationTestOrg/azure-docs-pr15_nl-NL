<properties
    pageTitle="Beleid en MDM groepsinstellingen | Microsoft Azure"
    description="Vindt u informatie over Groepsbeleid en mobiele apparaat management (MDM)-instellingen die moeten worden gebruikt op apparaten bedrijfs-eigendom. Dit beleid worden toegepast op de hele apparaat van de gebruiker."
    services="active-directory"
    keywords="Wat zijn groep beleid en MDM-instellingen voor Enterprise staat Roaming, Enterprise staat Roaming, windows cloud"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor="curtand"/>

<tags
    ms.service="active-directory"  
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="group-policy-and-mdm-settings"></a>Groepsbeleid en MDM-instellingen

Gebruik deze Groepsbeleid en het mobiele apparaat management (MDM) instellingen alleen op apparaten bedrijfs-eigendom omdat dit beleid worden toegepast op de hele apparaat van de gebruiker. Voor het toepassen van een beleid van MDM om uit te schakelen van de synchronisatie van de instellingen voor een persoonlijk apparaat gebruiker eigendom wordt een negatieve invloed hebben het gebruik van het apparaat. Andere gebruikersaccounts op het apparaat wordt Daarnaast kunnen ook worden beïnvloed door het beleid.

Ondernemingen die wilt laten beheren voor persoonlijke (onbeheerde) apparaten roaming de beschikking over de portal van Azure-of uitschakelen van roaming, in plaats van via Groepsbeleid of MDM.
De volgende tabellen worden de beschikbare beleidsinstellingen beschreven.

## <a name="mdm-settings"></a>MDM-instellingen
De instellingen voor het beleid van MDM toepassing zijn op Windows 10 en Windows 10 Mobile.  Ondersteuning voor Windows 10 Mobile bestaat alleen voor Microsoft-account op basis van roaming via de OneDrive-account van de gebruiker.  Raadpleeg 'Apparaten en eindpunten' sectie voor meer informatie over welke apparaten worden ondersteund voor het synchroniseren van Azure AD gebaseerd.

| Naam                               | Beschrijving                                                          |
|------------------------------------|----------------------------------------------------------------------|
| Microsoft-Account verbinding toestaan | Kunnen gebruikers om te verifiëren met een Microsoft-account op het apparaat |
| Synchronisatie van Mijn instellingen toestaan             | Kunnen gebruikers in staat Windows-instellingen en gegevens van de app; Uitschakelen van dit beleid wordt uitgeschakeld synchroniseren, evenals een back-ups op mobiele apparaten                  |

## <a name="group-policy-settings"></a>Groepsbeleidsinstellingen van
De groepsbeleidsinstellingen van toepassing op Windows 10-apparaten die zijn gekoppeld aan een Active Directory-domein. De tabel bevat ook oudere instellingen die voor het beheren van de synchronisatie-instellingen wilt weergegeven, maar die niet werken voor Enterprise staat Roaming voor Windows 10, die worden aangegeven met 'Do not use' in de beschrijving.

| Naam                                | Beschrijving |
|-------------------------------------|-------------|
| Accounts: Blok Microsoft-Accounts  |Deze instelling wordt voorkomen dat gebruikers nieuwe Microsoft-accounts toevoegen op deze computer|
| Synchroniseer niet                         |Hiermee voorkomt u dat gebruikers Windows-instellingen en gegevens van app verplaatsen|
| Synchroniseer niet personaliseren             |Hiermee schakelt u synchronisatie van de groep thema 's|
| Synchroniseer niet browserinstellingen        |Hiermee schakelt u synchronisatie van de groep Internet Explorer|
| Synchroniseer geen wachtwoorden               |Hiermee schakelt u synchronisatie van wachtwoorden groep|
| Synchroniseer geen andere instellingen voor Windows  |Hiermee schakelt u synchronisatie van groep van andere Windows-instellingen|
| Synchroniseer niet bureaublad aanpassen |Gebruik geen; heeft geen effect|
| Synchroniseer niet op verbindingen met datalimiet  |Hiermee schakelt u roaming op netwerken met een datalimiet verbindingen, zoals mobiele 3 G|
| Synchroniseer niet apps                    |Gebruik geen; heeft geen effect|
|Synchroniseer niet app-instellingen             |Hiermee schakelt u roaming van app-gegevens|
|Synchroniseer niet Startinstellingen           |Gebruik geen; heeft geen effect|


## <a name="related-topics"></a>Verwante onderwerpen
- [Overzicht Enterprise staat Roaming](active-directory-windows-enterprise-state-roaming-overview.md)
- [Enterprise staat roaming in Azure Active Directory inschakelen](active-directory-windows-enterprise-state-roaming-enable.md)
- [Instellingen en gegevens roaming Veelgestelde vragen](active-directory-windows-enterprise-state-roaming-faqs.md)
- [Windows 10 roaming instellingen verwijzing](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
