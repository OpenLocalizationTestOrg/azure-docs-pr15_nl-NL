<properties
   pageTitle="Een web application-firewall toevoegen in Azure Beveiligingscentrum | Microsoft Azure"
   description="In dit document ziet u hoe u het implementeren van de Azure Beveiligingscentrum aanbevelingen **toevoegen een firewall van de toepassing web** en **beveiliging voltooien**."
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

# <a name="add-a-web-application-firewall-in-azure-security-center"></a>Een web application-firewall in Beveiligingscentrum Azure toevoegen

Azure Beveiligingscentrum kan aanbevelen dat u een web-toepassing firewall (WAF) uit een Microsoft-partner teneinde uw webtoepassingen toevoegen. In dit document begeleidt u door een voorbeeld van hoe u dit doet.

> [AZURE.NOTE] In dit document maakt u kennis met de service met behulp van een voorbeeld-implementatie.  Dit is niet een stapsgewijze handleiding.

## <a name="implement-the-recommendation"></a>De aanbeveling implementeren

1. Selecteer **Secure webtoepassing met web application firewall**in het blad **aanbevelingen** .
![Secure web toepassing][1]

2. Selecteer in het blad **beveiligen van uw webtoepassingen web application firewall met** een webtoepassing. Hiermee opent u het blad **toevoegen een Web Application-Firewall** .
![Een web application-firewall toevoegen][2]
3. U kunt een bestaande web application-firewall gebruiken als deze functie beschikbaar of u een nieuwe record kunt maken. In dit voorbeeld zijn er geen bestaande WAFs beschikbaar zodat we een nieuwe WAF wilt maken.

4. Als u wilt een nieuwe WAF maken, selecteert u een oplossing in de lijst met geïntegreerde partners. In dit voorbeeld wordt we **Barracuda Web Application Firewall**selecteren.
5. Het blad **Barracuda Web Application Firewall** wordt geopend zodat u informatie over de partneroplossing. Selecteer **maken** in het blad informatie.
![Firewall informatie blade][3]

6. Het **Nieuwe Web Application Firewall** blad wordt geopend, kunt u **VM** configuratiestappen uitvoeren en geef **WAF informatie**. Selecteer **VM configuratie**.

7. In het blad **VM configuratie** voert u de gegevens die zijn vereist voor het draaien van de virtuele machine die de WAF kan worden uitgevoerd.
![VM configuratie][4]
8. Ga terug naar het **Nieuwe Web Application Firewall** blad en selecteer **WAF informatie**. In het blad **WAF informatie** configureert u de WAF zelf. Stap 7 kunt u de virtuele machine waarop de WAF wordt uitgevoerd en stap 8 kunt u voor het inrichten van de WAF zelf configureren.

## <a name="finalize-application-protection"></a>Beveiliging voltooien

1. Ga terug naar het blad **aanbevelingen** . Een nieuwe vermelding is gegenereerd nadat u de WAF, genaamd **Voltooien toepassing bescherming**hebt gemaakt. Dit item kunt u weet dat u het voltooien moet van daadwerkelijk bekabeling van de WAF binnen het Azure virtuele netwerk zodat deze de toepassing kunt beschermen.
![Beveiliging voltooien][5]

2. Selecteer **Voltooien Toepassingsbeveiliging**. Een nieuwe blade wordt geopend. U ziet dat er een webtoepassing die u moet de verkeer omgeleid hebben.
3. Selecteer de webtoepassing. Een blade wordt geopend waarmee u stappen voor het voltooien van de instelling webtoepassing firewall. Voer de stappen uit en selecteer **beperken-verkeer is toegestaan**. Beveiligingscentrum wordt vervolgens de bedradingsdiagrammen-ups van doen voor u.
![Verkeer beperken][6]

> [AZURE.NOTE] U kunt meerdere webtoepassingen in Beveiligingscentrum beveiligen door deze toepassingen toevoegen aan uw bestaande WAF-implementaties. WAF toestellen (gemaakt met het implementatiemodel resourcemanager-) moeten worden geïmplementeerd op een aparte virtueel netwerk. WAF toestellen (gemaakt met het klassieke implementatiemodel) zijn beperkt tot het gebruik van de beveiligingsgroep van een netwerk. In de toekomst wordt deze ondersteuning uitgebreid voor een volledig aangepast distributie van een toestel WAF (klassieke). Meer informatie over de [klassieke en resourcemanager implementatiemodellen](../azure-classic-rm.md) voor Azure resources.

De logboeken van die WAF zijn nu volledig geïntegreerd. Beveiligingscentrum kunt starten automatisch verzamelen en analyseren van de logboeken zodat deze kan belangrijke beveiligingsmeldingen oppervlak aan u.

## <a name="see-also"></a>Zie ook

In dit document hebt u geleerd hoe u het implementeren van de aanbeveling Beveiligingscentrum "Toevoegen een webtoepassing." Zie de volgende onderwerpen voor meer informatie over het configureren van een firewall van web-toepassing:

- [Een Web-toepassing Firewall (WAF) configureren voor App-Service-omgeving](../app-service-web/app-service-app-service-environment-web-application-firewall.md)

Zie de volgende onderwerpen voor meer informatie over Beveiligingscentrum:

- [Beveiligingsbeleid voor apparaten in Azure Beveiligingscentrum instellen](security-center-policies.md) --meer informatie over het configureren van beveiligingsbeleid voor apparaten voor uw Azure abonnementen en resourcegroepen.
- [Beveiliging statuscontrole in Azure Beveiligingscentrum](security-center-monitoring.md) --informatie over het controleren van de status van uw Azure resources.
- [Beheren en beantwoorden van beveiligingswaarschuwingen in Azure Beveiligingscentrum](security-center-managing-and-responding-alerts.md) --meer informatie over het beheren en reageren op beveiligingsmeldingen.
- [Aanbevelingen voor de beveiliging beheren in Azure Beveiligingscentrum](security-center-recommendations.md) --Kijk hoe aanbevelingen ervoor zorgen dat u uw Azure resources beveiligen.
- [Veelgestelde vragen over azure beveiliging beheercentrum](security-center-faq.md) --zoeken vaak Veelgestelde vragen over het gebruik van de service.
- [Azure-beveiliging blog](http://blogs.msdn.com/b/azuresecurity/) --zoeken blog over Azure beveiliging en naleving in berichten.

<!--Image references-->
[1]: ./media/security-center-add-web-application-firewall/secure-web-application.png
[2]:./media/security-center-add-web-application-firewall/add-a-waf.png
[3]: ./media/security-center-add-web-application-firewall/info-blade.png
[4]: ./media/security-center-add-web-application-firewall/select-vm-config.png
[5]: ./media/security-center-add-web-application-firewall/finalize-waf.png
[6]: ./media/security-center-add-web-application-firewall/restrict-traffic.png
