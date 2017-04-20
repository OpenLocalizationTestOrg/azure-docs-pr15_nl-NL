<properties
   pageTitle="Beveiligen van Azure SQL-service in Azure Beveiligingscentrum | Microsoft Azure"
   description="In dit document adressen aanbevelingen in Azure Beveiligingscentrum die u helpen beveiligen met een SQL Azure-service en het effectief overeenkomstig de beleidsregels van beveiligingsbeleid voor apparaten."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/04/2016"
   ms.author="terrylan"/>

# <a name="protecting-azure-sql-service-in-azure-security-center"></a>Azure SQL-service in Azure Beveiligingscentrum beveiligen

Azure Beveiligingscentrum analyseert de beveiligingsstatus van uw Azure resources. Wanneer Beveiligingscentrum worden aangegeven mogelijke beveiligingsrisico's, wordt u begeleid bij het proces van het configureren van de benodigde besturingselementen aanbevelingen.  Aanbevelingen hebben betrekking op Azure resourcetypen: virtuele machines (VMs), netwerken, SQL en toepassingen.

In dit artikel biedt aanbevelingen die van toepassing op SQL Azure-service.  Azure SQL aanbevelingen servicecentrum rond het inschakelen van controle voor Azure SQL-servers en databases en inschakelen van versleuteling voor SQL-databases.  De volgende tabel als een overzicht gebruiken om u te helpen u meer informatie over de beschikbare aanbevelingen voor SQL-service en wat elke doen als u deze hebt toegepast.

## <a name="available-sql-service-recommendations"></a>Beschikbare aanbevelingen voor SQL-service

|Aanbeveling|Beschrijving|
|-----|-----|
|[Controle van SQL server inschakelen](security-center-enable-auditing-on-sql-servers.md)|Wordt aanbevolen dat u hebt ingeschakeld controle voor Azure SQL-servers (Azure SQL-service alleen; niet zijn opgenomen in SQL waarop uw virtuele machines).|
|[Controle van de SQL-database activeert](security-center-enable-auditing-on-sql-databases.md)|Wordt aanbevolen dat u hebt ingeschakeld controle voor Azure SQL-databases (Azure SQL-service alleen; niet zijn opgenomen in SQL waarop uw virtuele machines).|
|[Transparante gegevensversleuteling inschakelen op SQL-databases](security-center-enable-transparent-data-encryption.md)|Aanbevolen versleuteling voor SQL-databases (alleen voor SQL Azure-service) in te schakelen.|

## <a name="see-also"></a>Zie ook

Zie de volgende onderwerpen voor meer informatie over aanbevelingen die van toepassing op andere resourcetypen Azure:

- [Beveiligen van uw virtuele machines in Azure Beveiligingscentrum](security-center-virtual-machine-recommendations.md)
- [Beveiligen van uw toepassingen in Azure Beveiligingscentrum](security-center-application-recommendations.md)
- [Beveiligen van uw netwerk in Azure Beveiligingscentrum](security-center-network-recommendations.md)

Zie de volgende onderwerpen voor meer informatie over Beveiligingscentrum:

- [Beveiligingsbeleid voor apparaten in Azure Beveiligingscentrum instellen](security-center-policies.md) --meer informatie over het configureren van beveiligingsbeleid voor apparaten voor uw Azure abonnementen en resourcegroepen.
- [Beheren en beantwoorden van beveiligingswaarschuwingen in Azure Beveiligingscentrum](security-center-managing-and-responding-alerts.md) --meer informatie over het beheren en reageren op beveiligingsmeldingen.
- [Veelgestelde vragen over azure beveiliging beheercentrum](security-center-faq.md) --zoeken vaak Veelgestelde vragen over het gebruik van de service.
