<properties
    pageTitle="Meld u aan analyses Veelgestelde vragen over | Microsoft Azure"
    description="Antwoorden op veelgestelde vragen over de Log Analytics-service."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="log-analytics-faq"></a>Meld u aan analyses Veelgestelde vragen

Deze FAQ Microsoft is een lijst met veelgestelde vragen over Log Analytics in Microsoft bewerkingen Management Suite Kantoorbeheersysteem. Als u extra vragen over Log Analytics hebt, gaat u naar het [discussieforum](https://social.msdn.microsoft.com/Forums/azure/home?forum=opinsights) en vragen stellen. Iemand uit onze community helpen u uw antwoorden kunt ophalen. Als u een vraag is meestal wordt gevraagd, wordt we dit toevoegen aan dit artikel zodat deze snel en eenvoudig kan worden gevonden.

## <a name="general"></a>Algemene

**V. welke controles worden uitgevoerd door de AD en SQL-Assessment oplossingen?**

A. De volgende query ziet u een beschrijving van alle controles die momenteel uitgevoerd:

```
(Type=SQLAssessmentRecommendation OR Type=ADAssessmentRecommendation) | dedup RecommendationId | select FocusArea, ActionArea, Recommendation, Description | sort Type, FocusArea,ActionArea, Recommendation
```

De resultaten kunnen wordt geëxporteerd naar Excel voor verdere controleren.

* *V: Waarom zie ik iets anders dan *OMS* in SCOM Administration? **

A: afhankelijk van welke Update Rollup van SCOM u bezig bent, ziet u mogelijk een knooppunt voor *System Center Advisor*, *Operationele inzichten*of *Log Analytics*.

De update van de tekenreeks tekst naar *OMS* is opgenomen in een management pack, moet handmatig worden geïmporteerd. Volg de instructies op de meest recente SCOM Update Rollup KB-artikel en vernieuw de OMS-console als u wilt zien van de meest recente updates in het knooppunt *OMS* .

* *V: Is er een *on-premises implementatie* versie van OMS? **

A: Nee. Log Analytics worden verwerkt en zeer grote hoeveelheden gegevens opgeslagen. Als een cloudservice kan Log Analytics schaal-up zo nodig en voorkomen dat een invloed op de prestaties in uw omgeving.

Ook wordt een cloud-service wordt niet moet u de Log Analytics-infrastructuur bijhouden en uitgevoerd en kunt ontvangen veelgebruikte functie-updates en correcties.

## <a name="configuration"></a>Configuratie
**V. kan ik de naam van de tabel/blob container gebruikt om te lezen van Azure diagnostische gegevens (af) wijzigen?**  

A.  Nee, dit is momenteel niet mogelijk, maar is gepland voor een toekomstige release.

**V. wat IP-adressen het gebruik van de services OMS doen? Hoe kan ik ervoor zorgen dat mijn firewall alleen toestaat dat verkeer naar de OMS-Services?**  

A. De Log Analytics-service is gebouwd Azure en de eindpunten ontvangt IP-adressen die in de [Microsoft Azure Datacenter IP-bereiken gebruikt](http://www.microsoft.com/download/details.aspx?id=41653).

Als de service-implementaties worden aangebracht, wordt de werkelijke IP-adressen van de services OMS wijzigen. De DNS-namen toe te staan dat via uw firewall worden beschreven aan [de proxy en firewall instellingen configureren in Log Analytics](log-analytics-proxy-firewall.md).

**V. ik ExpressRoute voor het verbinding maken met Azure gebruiken. Worden mijn Log Analytics-verkeer mijn verbinding ExpressRoute gebruiken?**  

A. De verschillende soorten ExpressRoute verkeer worden beschreven in de [documentatie van ExpressRoute](./expressroute/expressroute-faqs.md#supported-services).

Verkeer naar Log Analytics gebruikt de ExpressRoute openbare peering circuitlijnen.

**V. is er een eenvoudige en eenvoudige manier een bestaande Log Analytics-werkruimte verplaatsen naar een ander Log Analytics werkruimte/Azure-abonnement?**  We hebben verschillende klant OMS werkruimten die we zijn testen en trialing van onze Azure-abonnement en deze wilt verplaatsen naar productie, zodat we wilt verplaatsen naar hun eigen Azure/OMS-abonnement.  

A. De `Move-AzureRmResource` cmdlet kunt u een logboek Analytics-werkruimte en ook een account automatisering van Azure-abonnementen gaan naar de andere. Zie [Verplaatsen-AzureRmResource](http://msdn.microsoft.com/library/mt652516.aspx)voor meer informatie.

Deze wijziging kan ook worden aangebracht in de portal van Azure.

U geen gegevens uit één Log Analytics-werkruimte naar de andere verplaatsen, of de regio die Log Analytics-gegevens worden opgeslagen in wijzigen.

**V: hoe voeg ik OMS naar SCOM?**

A: bijgewerkt naar de meest recente update rollup en management packs importeren kunt u SCOM verbinden met Log Analytics.

Houd er rekening mee dat de verbinding SCOM met Log Analytics alleen beschikbaar voor SCOM 2012 SP1 en hoger is.

**V: hoe kan ik controleren of een agent communiceren met Log Analytics zich?**

A: om ervoor te zorgen dat de agent met OMS communiceren kan, gaat u naar: Configuratiescherm, beveiliging en **Microsoft Monitoring Agent**-instellingen.

Klik op het tabblad **Azure Log Analytics Kantoorbeheersysteem** zoekt u een groen vinkje. Een groen vinkje bevestigt dat de agent communiceren met de OMS-service is.

Het pictogram van een gele waarschuwing betekent dat de agent heeft problemen communicatie met OMS. Eén reden is de service Microsoft Monitoring Agent is gestopt en moet opnieuw worden gestart.

**V: hoe kan ik een agent communiceren met Log Analytics stoppen?**

A: In SCOM, de computer verwijderen uit de OMS beheerde lijst. Hiermee wordt alle verkeer via SCOM voor die medewerker. Voor agenten rechtstreeks verbonden met OMS, u kunt voorkomen dat ze communiceren met OMS via: Configuratiescherm, beveiliging en **Microsoft Monitoring Agent**-instellingen.
Verwijder alle werkruimten vermeld onder **Azure Log Analytics Kantoorbeheersysteem**.

## <a name="agent-data"></a>Agent gegevens

**V. hoeveel gegevens kan ik via de agent naar verzenden Log Analytics? Is er een maximale hoeveelheid gegevens per klant?**  
A. Het gratis abonnement Hiermee stelt u een dagelijkse initiaal van 500 MB per werkruimte. De abonnementen standard en premium hebben geen limiet van de hoeveelheid gegevens die wordt geüpload. Als een cloudservice, Log Analytics in OMS ontworpen automatisch schaalfactor snel aan de greep van het volume die afkomstig zijn uit een klant – zelfs als het TB per dag.

De Log Analytics-agent is ontworpen om ervoor te zorgen heeft een kleine footprint en enige gegevenscompressie eenvoudige bevat. Een van onze klanten daadwerkelijk een blog op de tests die ze uitgevoerd tegen onze agent en hoe onder de indruk dat deze zijn geschreven. Het gegevensvolume varieert afhankelijk van de oplossingen uw klanten kunnen zijn. U kunt gedetailleerde informatie over het gegevensvolume vinden en raadpleegt u de verdeling door oplossing onder de **Gebruik** tegel in de overzichtspagina OMS.

U kunt een [klant-blog](http://thoughtsonopsmgr.blogspot.com/2015/09/one-small-footprint-for-server-one.html) over de laag footprint van de OMS-agent lezen voor meer informatie.

**V. hoeveel netwerkbandbreedte wordt door de Microsoft Management Agent MMA () gebruikt bij het verzenden van gegevens naar Log Analytics?**

A. Bandbreedte is een functie van de hoeveelheid gegevens die worden verzonden. Gegevens worden gecomprimeerd als deze is verzonden via het netwerk.

**V. hoeveel gegevens per agent is verzonden?**

A. Dit is grotendeels afhankelijk:

- de oplossingen die u hebt ingeschakeld
- het aantal logboeken en van prestatiemeteritems die worden verzameld
- het volume van de gegevens in de logboeken

De gratis prijzen laag is een goede manier om ingebouwde verschillende servers en toelichtingen het gegevensvolume van de normale. Algemene wordt gebruik weergegeven op de pagina **Gebruik** .
Voor computers waarop mogen de WireData-agent uitvoeren, kunt u zien hoeveel gegevens wordt verzonden met behulp van de volgende query:

```
Type=WireData (ProcessName="C:\\Program Files\\Microsoft Monitoring Agent\\Agent\\MonitoringHost.exe") (Direction=Outbound) | measure Sum(TotalBytes) by Computer
```



## <a name="next-steps"></a>Volgende stappen

- [Aan de slag met Log Analytics](log-analytics-get-started.md) naar meer informatie over Log Analytics en get omhoog en worden uitgevoerd in minuten.
