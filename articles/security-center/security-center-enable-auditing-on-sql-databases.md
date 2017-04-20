<properties
   pageTitle="Controle inschakelen in SQL-databases in Azure Beveiligingscentrum | Microsoft Azure"
   description="In dit document ziet u hoe u het implementeren van de Azure Beveiligingscentrum aanbeveling **controle inschakelen in SQL-databases**."
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

# <a name="enable-auditing-on-sql-databases-in-azure-security-center"></a>Controle inschakelen in SQL-databases in Azure Beveiligingscentrum

Azure Beveiligingscentrum wordt aangeraden dat u hebt ingeschakeld voor alle SQL-databases controle als controle niet al is ingeschakeld. Controle, kunt u voor het behoud van regelgeving, inzichtelijk activiteit in een database maken en inzicht in de verschillen en afwijkingen die bedrijven bezwaren kunnen aangeven of verdachte problemen met de beveiliging.

Zodra u hebt ingeschakeld controle kunt u de instellingen voor detectie van bedreiging en e-mailberichten om te ontvangen beveiligingsmeldingen kunt configureren. Bedreiging detectie vastgesteld activiteiten met een afwijkende database waarin wordt aangegeven dat mogelijke beveiligingsrisico's met de database. Hiermee kunt u detecteren en reageren op mogelijke threats terwijl ze voorkomen.

Deze aanbeveling is van toepassing op de SQL Azure-service. SQL waarop uw virtuele machines zijn niet opgenomen.

> [AZURE.NOTE] In dit document maakt u kennis met de service met behulp van een voorbeeld-implementatie.  Dit is niet een stapsgewijze handleiding.

## <a name="implement-the-recommendation"></a>De aanbeveling implementeren

1. Selecteer de **Controle op SQL-databases inschakelen**in het blad **aanbevelingen** .  Hiermee opent u het blad **Controle inschakelen op SQL-databases** .
![Controle inschakelen in SQL-databases][1]

2. Selecteer een SQL-database controle inschakelen in. Hiermee opent u het blad **controle & bedreiging detectie** .
![Controle- en bedreiging detectie][2]
3. Selecteer **aan** onder **controle**op het blad **controle & bedreiging detectie** .
![Controle- en bedreiging detectie inschakelen][3]


5. Volg de stappen in de [aan de slag met SQL Database bedreiging detectie](../sql-database/sql-database-threat-detection-get-started.md) inschakelen en configureren van bedreiging detectie en configureren van de lijst met e-mailberichten die beveiligingsmeldingen na detectie van afwijkende activiteiten ontvangt.

## <a name="see-also"></a>Zie ook

In dit artikel hebt u geleerd hoe u de aanbeveling Beveiligingscentrum implementeert "Controle inschakelen in SQL-databases." Zie de volgende onderwerpen voor meer informatie over het beveiligen van uw SQL-database:

- [Beveiliging van uw SQL-Database](../sql-database/sql-database-security.md)

Zie de volgende onderwerpen voor meer informatie over Beveiligingscentrum:

- [Beveiligingsbeleid voor apparaten in Azure Beveiligingscentrum instellen](security-center-policies.md) --meer informatie over het configureren van beveiligingsbeleid voor apparaten voor uw Azure abonnementen en resourcegroepen.
- [Aanbevelingen voor de beveiliging beheren in Azure Beveiligingscentrum](security-center-recommendations.md) --Kijk hoe aanbevelingen ervoor zorgen dat u uw Azure resources beveiligen.
- [Beveiliging statuscontrole in Azure Beveiligingscentrum](security-center-monitoring.md) --informatie over het controleren van de status van uw Azure resources.
- [Beheren en beantwoorden van beveiligingswaarschuwingen in Azure Beveiligingscentrum](security-center-managing-and-responding-alerts.md) --meer informatie over het beheren en reageren op beveiligingsmeldingen.
- [Partneroplossingen Monitoring met Azure Beveiligingscentrum](security-center-partner-solutions.md) --informatie over het controleren van de status van uw partneroplossingen.
- [Veelgestelde vragen over azure beveiliging beheercentrum](security-center-faq.md) --zoeken vaak Veelgestelde vragen over het gebruik van de service.
- [Azure-beveiliging blog](http://blogs.msdn.com/b/azuresecurity/) --het laatste nieuws van Azure beveiliging en informatie.

<!--Image references-->
[1]: ./media/security-center-enable-auditing-on-sql-databases/enable-auditing-on-sql-databases.png
[2]:./media/security-center-enable-auditing-on-sql-databases/auditing-threat-detection.png
[3]: ./media/security-center-enable-auditing-on-sql-databases/auditing-threat-detection-blade.png
