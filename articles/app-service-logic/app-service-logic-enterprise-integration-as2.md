<properties 
    pageTitle="Leer hoe u een overeenkomst AS2 maken voor de Enterprise-integratie Pack" 
    description="Leer hoe u een overeenkomst AS2 maken voor de Enterprise-integratie Pack | Microsoft Azure-App-Service" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="msftman" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/29/2016" 
    ms.author="deonhe"/>

# <a name="enterprise-integration-with-as2"></a>Enterprise-integratie met AS2

## <a name="create-an-as2-agreement"></a>Een overeenkomst AS2 maken
Pas de enterprise-functies gebruiken in logica-apps, moet u eerst overeenkomsten maken. 

### <a name="heres-what-you-need-before-you-get-started"></a>Hier ziet u wat u nodig hebt voordat u begint
- Een [account van de integratie](./app-service-logic-enterprise-integration-accounts.md) gedefinieerd in uw Azure-abonnement  
- Ten minste twee [partners](./app-service-logic-enterprise-integration-partners.md) al zijn gedefinieerd in uw account integratie  

>[AZURE.NOTE]Wanneer u een overeenkomst maakt, kan de inhoud in het bestand overeenkomst moet overeenkomen met het Overeenkomstsoort.    


Nadat u [gemaakt van een account integratie](./app-service-logic-enterprise-integration-accounts.md) en [partners toegevoegd hebt](./app-service-logic-enterprise-integration-partners.md), kunt u een overeenkomst maken door deze stappen uit:  

### <a name="from-the-azure-portal-home-page"></a>Vanaf de startpagina voor Azure portal

Nadat u zich aanmelden bij de [portal van Azure](http://portal.azure.com "Azure-portal"):  
1. Selecteer **Bladeren** in het menu aan de linkerkant.  

>[AZURE.TIP]Als u de koppeling **Bladeren** niet ziet, moet u mogelijk eerst het optiemenu uitvouwen. Dit doen door het selecteren van de koppeling voor **menu weergeven** die zich bevindt op links boven in het menu samengevouwen.  

![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    
2. *Integratie* Typ in het zoekvak van het filter en selecteer **Integratie Accounts** in de lijst met resultaten.       
 ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  
3. Selecteer in het blad **Integratie Accounts** dat wordt geopend, de integratie-account waarin u de overeenkomst wilt maken. Als u niet ziet een integratie accounts lijsten, [maken een eerste](./app-service-logic-enterprise-integration-accounts.md "All about integration accounts").  
![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  
4.  Selecteer de tegel **overeenkomsten** . Als u de tegel overeenkomsten niet ziet, moet u dit eerst toevoegen.   
![](./media/app-service-logic-enterprise-integration-agreements/agreement-1.png)   
5. Selecteer de knop **toevoegen** in het blad overeenkomsten dat wordt geopend.  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-2.png)  
6. Voer een **naam** voor uw overeenkomst en selecteer de **Host-Partner**, **Host identiteit**, **Gast Partner**, **Gast identiteit**, in het blad overeenkomsten dat wordt geopend.  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-3.png)  

Hier volgen enkele details die u mogelijk nuttig zijn bij het configureren van de instellingen voor uw overeenkomst: 
  
|Eigenschap|Beschrijving|
|----|----|
|Host Partner|Een overeenkomst moet zowel een host en als gast partner. De partner host vertegenwoordigt de organisatie dat de overeenkomst is configureert.|
|Host-identiteit|Een id voor de host-partner. |
|Gast Partner|Een overeenkomst moet zowel een host en als gast partner. De partner Gast vertegenwoordigt de organisatie die bedrijf met de partner host doet.|
|Gast-identiteit|Een id voor de partner Gast.|
|Ontvangstinstellingen|Deze eigenschappen toepassen op alle berichten die zijn ontvangen door een overeenkomst|
|Instellingen voor verzenden|Deze eigenschappen toepassen op alle berichten die zijn verzonden door een overeenkomst|  
Gaat u verder:  
7. Selecteer **Ontvangen instellingen** te configureren hoe berichten ontvangen via deze overeenkomst moeten worden verwerkt.  
 
 - U kunt desgewenst de eigenschappen in het binnenkomende bericht negeren. Als u wilt dat doet, schakelt u het selectievakje **berichteigenschappen van het negeren** .
  - Schakel het selectievakje **bericht moet worden ondertekend** als u vereisen alle inkomende berichten wilt moeten worden ondertekend. Als u deze optie selecteert, moet u ook het **certificaat** dat wordt gebruikt voor het valideren van de handtekening op de berichten selecteren.
  - Desgewenst kunt u berichten ook worden versleuteld vereist. Als u wilt dat doet, schakelt u het selectievakje **bericht moet worden gecodeerd** . Vervolgens moet u het **certificaat** dat wordt gebruikt voor de binnenkomende berichten decoderen selecteren.
  - U kunt ook de berichten worden gecomprimeerd vereisen. Als u wilt dat doet, schakelt u het selectievakje **bericht moet worden gecomprimeerd** .  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-4.png)  

Zie de onderstaande tabel als u wilt meer informatie over wat de ontvangen instellingen inschakelen.  

|Eigenschap|Beschrijving|
|----|----|
|Berichteigenschappen van het negeren|Selecteer deze optie om aan te geven dat de eigenschappen van ontvangen berichten kunnen worden overschreven |
|Bericht moet worden ondertekend.|Hiermee kunt u berichten digitaal wilt ondertekenen vereisen inschakelen|
|Bericht moet worden gecodeerd|Inschakelen Hiermee kunt u berichten moeten worden gecodeerd vereisen. Niet-versleutelde berichten geweigerd.|
|Bericht moet worden gecomprimeerd|Inschakelen Hiermee kunt u berichten worden gecomprimeerd vereisen. Berichten niet gecomprimeerd geweigerd.|
|MDN tekst|Dit is een standaard MDN naar de afzender van het bericht wordt verzonden|
|MDN verzenden|Schakel dit toe te staan dat MDNs te kunnen verzenden.|
|Ondertekende MDN verzenden|Inschakelen Hiermee kunt u vereisen MDNs moet worden ondertekend.|
|Algoritme van de Microfoon||
|Asynchroon MDN verzenden|Inschakelen Hiermee kunt u vereisen berichten asynchroon worden verzonden.|
|URL|Dit is de URL waarop berichten worden verzonden.|
Nu gaat u verder:  
8. Selecteer **Instellingen voor verzenden** naar configureren hoe berichten die zijn verzonden via deze overeenkomst moeten worden verwerkt.  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-5.png)  

Zie de onderstaande tabel als u wilt meer informatie over wat de verzenden instellingen inschakelen.  

|Eigenschap|Beschrijving|
|----|----|
|Bericht ondertekenen in te schakelen|Schakel dit selectievakje inschakelen van alle verzonden berichten van de overeenkomst moet worden ondertekend.|
|Algoritme van de Microfoon|Selecteer het algoritme wordt gebruikt in het bericht ondertekenen|
|Certificaat|Selecteer het certificaat wilt gebruiken in het bericht ondertekenen|
|Berichtversleuteling inschakelen|Schakel dit selectievakje versleutelen van alle verzonden berichten van deze overeenkomst.|
|Versleutelingsalgoritme|Selecteer de versleutelingsalgoritme voor gebruik in berichtversleuteling|
|HTTP-headers uitgevouwen|Schakel dit selectievakje aan de HTTP-inhoudstype header uitgevouwen in een ononderbroken lijn.|
|MDN aanvragen|Dit selectievakje aanvragen van een MDN voor alle berichten van deze overeenkomst inschakelen|
|Ondertekende MDN aanvragen|Inschakelen om aan te vragen dat alle MDNs verstuurd naar deze overeenkomst bent aangemeld|
|Asynchroon MDN aanvragen|Asynchroon MDN worden verzonden naar deze overeenkomst aanvragen inschakelen|
|URL|De URL waaraan MDNs wordt verzonden|
|NRR inschakelen|Schakel dit selectievakje niet betrouwbaar ontvangstbevestiging inschakelen|
We bent bijna klaar.  
9. Selecteer de tegel **overeenkomsten** op het blad integratie-Account en ziet u de zojuist toegevoegde overeenkomst wordt vermeld.  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-6.png)

