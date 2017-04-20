<properties
   pageTitle="Toevoegen en beheren van meerdere Azure Active Directory-mappen | Microsoft Azure"
   description="Instructies en aanbevolen procedures voor het toevoegen en beheren van uw Azure Active Directory-mappen, waarin wordt uitgelegd dat mappen als een volledig onafhankelijke resources"
   services="active-directory"
   documentationCenter=""
   authors="curtand"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="curtand"/>

# <a name="add-and-manage-multiple-azure-active-directory-directories"></a>Toevoegen en beheren van meerdere Azure Active Directory-mappen

In Azure Active Directory (Azure AD), wordt elke map een volledig onafhankelijke resource: een peer, volledig uitgerust en logisch onafhankelijk van andere mappen die u beheert. Er is geen bovenliggende en onderliggende relatie tussen mappen. Deze onafhankelijkheid tussen mappen bevat onafhankelijkheid van de resource, administratieve onafhankelijkheid en onafhankelijkheid van synchronisatie.

##<a name="resource-independence"></a>Resource-onafhankelijkheid

Als u maken of verwijderen van een resource in een bepaalde map, heeft dit geen invloed op elke resource in een andere map, met de gedeeltelijke uitzondering van externe gebruikers, hieronder beschreven. Als u een aangepast domein 'contoso.com' met een bepaalde map gebruikt, worden niet gebruikt voor een andere map.

##<a name="administrative-independence"></a>Administratieve onafhankelijkheid

Als een niet-beheerder van map 'Contoso' maakt vervolgens een testmap 'Test':
- Standaard is de gebruiker die een map maakt toegevoegd als een externe gebruiker in de nieuwe map en de rol van globale beheerder in die map toegewezen.
- De beheerders van de map 'Contoso' hebben geen directe beheerdersbevoegdheden naar map 'Test', tenzij een beheerder van 'Test' ze specifiek deze bevoegdheden verleent. Beheerders van 'Contoso' kunnen toegang tot directory 'Test' bepalen of ze het gebruikersaccount die 'Test.' bepalen
- Als u wijzigen (toevoegen of verwijderen) een beheerdersrol voor een gebruiker in een bepaalde map, de wijziging heeft geen invloed op de beheerdersrol die de gebruiker heeft mogelijk in een andere map.

##<a name="synchronization-independence"></a>Synchronisatie onafhankelijkheid

U kunt elke map Azure AD onafhankelijk gebruikt om toegang te krijgen van de gegevens die zijn gesynchroniseerd vanuit één exemplaar van een configureren:
  - Het hulpprogramma Directory Synchronization (DirSync) om gegevens te synchroniseren met een enkel AD-bos.
  - De Azure Active Directory Connector voor Forefront identiteit Manager, gegevens synchroniseren met een of meer on-premises implementatie forests en/of niet-Azure AD-gegevensbronnen.

##<a name="add-an-azure-ad-directory"></a>Een Azure AD-map toevoegen

U kunt een Azure AD-map toevoegen in de portal van Azure klassieke, selecteert u de Azure Active Directory-extensie aan de linkerkant en tikt u op **toevoegen**.

> [AZURE.NOTE]   In tegenstelling tot andere Azure bronnen zijn uw mappen niet onderliggende bronnen van een abonnement op Azure. Als u annuleren of toestaan dat uw Azure abonnement verloopt, kunt u nog steeds toegang tot uw directorygegevens met Azure PowerShell, de API Azure-grafiek of andere interfaces zoals het Office 365-beheercentrum. U kunt ook een ander abonnement koppelen aan de map.

Zie voor een globaal overzicht van Azure AD volumelicenties problemen en aanbevolen procedures, [Wat is Azure Active Directory licensing?](active-directory-licensing-what-is.md).
