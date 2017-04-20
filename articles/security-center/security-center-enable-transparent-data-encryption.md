<properties
   pageTitle="Transparante gegevenscodering in Azure Beveiligingscentrum inschakelen | Microsoft Azure"
   description="In dit document ziet u hoe u het implementeren van de aanbeveling Azure Beveiligingscentrum **Transparante gegevensversleuteling inschakelen**."
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
   ms.date="07/29/2016"
   ms.author="terrylan"/>

# <a name="enable-transparent-data-encryption-in-azure-security-center"></a>Transparante gegevenscodering in Azure Beveiligingscentrum inschakelen

Azure Beveiligingscentrum wordt aangeraden dat u doorzichtige gegevens versleuteling (TDE) in SQL-databases, inschakelen als TDE niet al is ingeschakeld. TDE uw gegevens beschermen en helpt u voldoet aan de verschillende nalevingsvereisten door te coderen van uw database, gekoppeld back-ups en transactie-logbestanden in rust, zonder wijzigingen aan uw toepassing. Voor meer dat informatie raadpleegt u [Doorzichtige gegevensversleuteling met Azure SQL-Database](https://msdn.microsoft.com/library/dn948096).

Deze aanbeveling is van toepassing op de SQL Azure-service. SQL waarop uw virtuele machines zijn niet opgenomen.

> [AZURE.NOTE] In dit document maakt u kennis met de service met behulp van een voorbeeld-implementatie.  Dit is niet een stapsgewijze handleiding.

## <a name="implement-the-recommendation"></a>De aanbeveling implementeren

1. Selecteer in het blad **aanbevelingen** **Transparante gegevensversleuteling inschakelen**.
![Transparante gegevenscodering inschakelen][1]

2. Hiermee opent u het blad **Transparante gegevens-versleuteling voor SQL-databases inschakelen** . Selecteer een SQL-database TDE inschakelen.
![Selecteer SQL DB TDE inschakelen][2]
3. Klik op het blad **transparante gegevensversleuteling** Selecteer **aan** onder gegevensversleuteling en selecteert u **Opslaan** in het bovenste lint van het blad.
![TDE inschakelen][3]

  Zodra TDE op de geselecteerde SQL-database is ingeschakeld, wordt de **versleuteling status** wordt gewijzigd in **versleutelde**.    

  ![Status versleuteling][4]

## <a name="see-also"></a>Zie ook

In dit artikel hebt u geleerd hoe u de aanbeveling Beveiligingscentrum implementeert "Schakel transparante gegevenscodering." Zie de volgende onderwerpen voor meer informatie over SQL TDE:

- [Transparante gegevensversleuteling met Azure SQL-Database](https://msdn.microsoft.com/library/dn948096)
- [Aan de slag met transparante gegevens versleuteling (TDE)](../sql-data-warehouse/sql-data-warehouse-encryption-tde.md)

Zie de volgende onderwerpen voor meer informatie over Beveiligingscentrum:

- [Beveiligingsbeleid voor apparaten in Azure Beveiligingscentrum instellen](security-center-policies.md) --meer informatie over het configureren van beveiligingsbeleid voor apparaten voor uw Azure abonnementen en resourcegroepen.
- [Aanbevelingen voor de beveiliging beheren in Azure Beveiligingscentrum](security-center-recommendations.md) --Kijk hoe aanbevelingen ervoor zorgen dat u uw Azure resources beveiligen.
- [Beveiliging statuscontrole in Azure Beveiligingscentrum](security-center-monitoring.md) --informatie over het controleren van de status van uw Azure resources.
- [Beheren en beantwoorden van beveiligingswaarschuwingen in Azure Beveiligingscentrum](security-center-managing-and-responding-alerts.md) --meer informatie over het beheren en reageren op beveiligingsmeldingen.
- [Partneroplossingen Monitoring met Azure Beveiligingscentrum](security-center-partner-solutions.md) --informatie over het controleren van de status van uw partneroplossingen.
- [Veelgestelde vragen over azure beveiliging beheercentrum](security-center-faq.md) --zoeken vaak Veelgestelde vragen over het gebruik van de service.
- [Azure-beveiliging blog](http://blogs.msdn.com/b/azuresecurity/) --het laatste nieuws van Azure beveiliging en informatie.

<!--Image references-->
[1]: ./media/security-center-enable-tde-on-sql-databases/enable-tde.png
[2]:./media/security-center-enable-tde-on-sql-databases/transparent-data-encryption-blade.png
[3]: ./media/security-center-enable-tde-on-sql-databases/turn-on-tde.png
[4]: ./media/security-center-enable-tde-on-sql-databases/encrypted.png
