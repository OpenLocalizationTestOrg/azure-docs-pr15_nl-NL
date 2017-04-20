<properties
    pageTitle="Upgrade van DirSync en Azure AD-synchronisatie | Microsoft Azure"
    description="Beschreven hoe u een upgrade van DirSync en Azure AD-synchronisatie naar Azure AD Connect."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/27/2016"
    ms.author="billmath"/>


# <a name="upgrade-windows-azure-active-directory-sync-dirsync-and-azure-active-directory-sync-azure-ad-sync"></a>Upgrade van Windows Azure Active Directory-synchronisatie ("DirSync") en Azure Active Directory-synchronisatie ("Azure AD-synchronisatie")
Azure AD Connect is de beste manier om uw on-premises adreslijst verbinden met Azure AD en Office 365. Dit is een uitstekende upgrade uitvoeren naar Azure AD Connect uit Windows Azure Active Directory-synchronisatie (DirSync) of Azure AD-synchronisatie terwijl deze hulpmiddelen nu zijn afgeschaft en einde van support op 13 April 2017 wordt bereikt.

De twee identiteit synchronisatie-hulpprogramma's die zijn afgeschaft zijn aangeboden voor één bos klanten (DirSync) en voor meerdere bos en andere geavanceerde klanten (Azure AD-synchronisatie). Deze oudere hulpprogramma's zijn vervangen door een oplossing die is beschikbaar voor alle scenario's: Azure AD Connect. Deze optie biedt nieuwe functionaliteit, verbeteringen van de functie en ondersteuning voor nieuwe scenario's. Moeten kunnen doorgaan met het synchroniseren van uw identiteit on-premises gegevens naar Azure AD en Office 365, wordt aangeraden dat u naar Azure AD Connect bijwerkt.

De laatste versie van DirSync is uitgebracht in juli 2014 en de laatste versie van Azure AD-synchronisatie is uitgebracht in mei 2015.

## <a name="what-is-azure-ad-connect"></a>Wat is Azure AD Connect
Azure AD Connect is de opvolger DirSync en Azure AD-synchronisatie. Deze worden gecombineerd alle scenario's deze twee ondersteund. U kunt meer informatie over deze bij de [integratie van uw on-premises implementatie identiteiten met Azure Active Directory](active-directory-aadconnect.md).

## <a name="deprecation-schedule"></a>Afschrijving planning

Datum | Opmerking
 --- | ---
13 april 2016 | Windows Azure Active Directory-synchronisatie ("DirSync") en Microsoft Azure Active Directory-synchronisatie ("Azure AD-synchronisatie") zijn aangekondigd als verouderd.
13 april 2017 | Ondersteuning voor uiteinden. Klanten wordt niet meer mogelijk om een melding ondersteuning zonder eerst een upgrade naar Azure AD Connect.

## <a name="how-to-transition-to-azure-ad-connect"></a>Hoe u de overgang naar Azure AD Connect
Als u DirSync er zijn twee manieren kunt u een upgrade uitvoert: In-place upgrade en parallelle implementatie. Een in-place upgrade geschikt is voor de meeste klanten en als u een recente hebt besturingsomgeving systeem en minder dan 50.000 objecten. In andere gevallen wordt het aanbevolen te doen een parallelle implementatie waar uw DirSync-configuratie wordt verplaatst naar een nieuwe server met Azure AD Connect.

Als u Azure AD-synchronisatie een in-place upgrade wordt aanbevolen. Als u wilt, is het mogelijk voor het installeren van een nieuwe Azure AD Connect-server parallel en een swing-migratie van uw server Azure AD-synchronisatie aan Azure AD Connect uitvoeren.

Oplossing | Scenario
----- | -----
[Upgrade van DirSync](./connect/active-directory-aadconnect-dirsync-upgrade-get-started.md) | <li>Als u een bestaande DirSync-server is gebeurd.</li>
[Upgrade van Azure AD-synchronisatie](active-directory-aadconnect-upgrade-previous-version.md)| <li>Als u van Azure AD-synchronisatie verplaatsen wilt.</li>

Als u zien hoe wilt u een in-place upgrade van DirSync naar Azure AD Connect, klikt u vervolgens raadpleegt u dit kanaal 9-video:

> [AZURE.VIDEO azure-active-directory-connect-in-place-upgrade-from-legacy-tools]

## <a name="faq"></a>FAQ
**V: ik heb een e-mailbericht ontvangen van het Team van Azure en/of een bericht van het berichtencentrum voor Office 365, maar ik gebruik verbinding maken.**  
De melding is ook verzonden naar klanten die gebruikmaken van Azure AD Connect met een getal opbouwen 1.0. \*.0 (met een versie van de pre-1.1). Microsoft raadt klanten op de hoogte blijven van Azure AD Connect-versies. Met de functie [Automatische upgrade](active-directory-aadconnect-feature-automatic-upgrade.md) kunt u 1.1 is heel eenvoudig kunt tevens een recente versie van Azure AD Connect is geïnstalleerd.

**V: worden DirSync/Azure AD-synchronisatie stoppen 13 April 2017 gewerkt?**  
Nee. De datum voor wanneer deze niet meer kunnen communiceren met Azure AD wordt aangekondigd op een later tijdstip. Is mogelijk dat informatie zoeken in dit onderwerp indien beschikbaar.

**V: welke versies DirSync kan ik upgraden van?**  
Dit wordt ondersteund als u wilt een upgrade uitvoert vanuit een willekeurige DirSync release momenteel wordt gebruikt.

**V: hoe zit het Azure AD-Connector voor FIM/MIM?**  
De Azure AD-Connector voor FIM/MIM **niet** is aangekondigd als verouderd. Het is op het **blokkeren van de functie**; geen nieuwe functionaliteit wordt toegevoegd en deze geen correcties ontvangt. Microsoft raadt klanten deze gebruiken voor het plannen van dit naar Azure AD Connect te gaan. Het is raadzaam niet start een nieuwe implementaties gebruiken. Deze verbindingslijn wordt aangekondigd afgeschaft in de toekomst.

## <a name="additional-resources"></a>Aanvullende informatie

* [Uw on-premises implementatie identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)
