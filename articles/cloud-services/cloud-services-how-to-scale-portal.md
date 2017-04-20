<properties
    pageTitle="Automatisch de schaal van een cloudservice in de portal | Microsoft Azure"
    description="Informatie over het gebruik van de portal voor het configureren van regels voor automatisch schaal voor een cloud-service Webrol of werknemer rol in Azure wordt aangegeven."
    services="cloud-services"
    documentationCenter=""
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="adegeo"/>


# <a name="how-to-auto-scale-a-cloud-service"></a>Hoe u automatisch schalen een cloudservice

> [AZURE.SELECTOR]
- [Azure-portal](cloud-services-how-to-scale-portal.md)
- [Azure klassieke portal](cloud-services-how-to-scale.md)

Voorwaarden kunnen worden ingesteld voor een cloud-service werknemer rol die door een schaal in- of uitzoomen bewerking wordt geactiveerd. De voorwaarden voor de rol kunnen worden gebaseerd op de CPU, schijf of netwerk laden van de rol. U kunt ook een conditation op basis van een berichtenwachtrij of de meetwaarde van enkele andere Azure resource die is gekoppeld aan uw abonnement instellen.

>[AZURE.NOTE] In dit artikel ligt de nadruk op het web en werknemer rollen Cloudservice. Wanneer u een virtuele machine (klassieke) rechtstreeks maakt, wordt deze gehost in een cloudservice. U kunt de schaal van een standaard virtuele machine aanpassen door deze te koppelen met een [beschikbaarheid instellen](../virtual-machines/virtual-machines-windows-classic-configure-availability.md) en handmatig inschakelen of uitschakelen.

## <a name="considerations"></a>Overwegingen

Voordat u de schaal voor uw toepassing configureren, moet u de volgende informatie overwegen:

- Schaalbaarheid wordt beïnvloed door core gebruik. Grotere rol exemplaren gebruiken meer cores. U kunt de schaal van een toepassing alleen binnen de limiet van cores voor uw abonnement. Als uw abonnement geldt een limiet van twintig gietkernen en u een toepassing met twee gemiddeld grootte uitvoeren bijvoorbeeld cloud services (een totaal van vier cores), kunt u alleen de schaal van de implementaties van andere cloud-service in uw abonnement door 16 cores. Zie [Cloud Service grootten](cloud-services-sizes-specs.md) voor meer informatie over grootte.

- U kunt schalen op basis van een wachtrij bericht drempel. Zie voor meer informatie over het gebruik van wachtrijen voor [het gebruik van de wachtrij Storage-Service](../storage/storage-dotnet-how-to-use-queues.md).

- U kunt ook andere resources die zijn gekoppeld aan uw abonnement schalen.

- Als u wilt de beschikbaarheid van de toepassing inschakelen, moet u ervoor zorgen dat deze wordt gedistribueerd met twee of meer exemplaren van de rol. Zie [Niveau serviceovereenkomsten](https://azure.microsoft.com/support/legal/sla/)voor meer informatie.

## <a name="where-scale-is-located"></a>Waar bevindt zich schaal

Nadat u uw cloudservice, kunt u het blad zichtbaar service van cloud nodig hebt.

1. Selecteer de naam van de cloudservice op het blad cloud-service, klik op de tegel **rollen en exemplaren** .   
**Belangrijk**: Zorg ervoor dat u de rol van de service cloud, niet het exemplaar rol die zich onder de rol.

    ![](./media/cloud-services-how-to-scale-portal/roles-instances.png)

2. Selecteer de tegel **schaal** .

    ![](./media/cloud-services-how-to-scale-portal/scale-tile.png)

## <a name="automatic-scale"></a>Automatische schaal

U kunt de schaalinstellingen voor een rol met twee modi **handmatig** of **automatisch**configureren. Handmatig is zoals u zou verwachten, u de absolute aantal exemplaren instellen. Automatische echter kunt u setregels die hoe en door hoe veel u bepalen moet schaal.

Stel de optie **schaal door** op **planning en prestaties regels**.

![Instellingen voor de cloud services-schaal met het profiel en de regel](./media/cloud-services-how-to-scale-portal/schedule-basics.png)

1. Een bestaand profiel.
2. Een regel voor het profiel van de bovenliggende toevoegen.
3. Een ander profiel toevoegen.

Selecteer **profiel toevoegen**. Het profiel bepaalt welke modus die u wilt gebruiken voor de schaal: **altijd**, **Terugkeerpatroon**, **vaste datum**.

Nadat u het profiel en de regels hebt geconfigureerd, selecteert u het pictogram **Opslaan** aan de bovenkant.

#### <a name="profile"></a>Profiel

Het profiel ingesteld minimum- en maximumwaarden exemplaren voor de schaal, en ook wanneer dit bereik schaal actief is.

* **Altijd**

    Bewaren in dit bereik van de exemplaren die beschikbaar.  

    ![Cloudservice die altijd schaal](./media/cloud-services-how-to-scale-portal/select-always.png)
    
* **Terugkeerpatroon**

    Kies een reeks dagen van de week aan de nieuwe schaal.

    ![Cloud service schalen met een schema voor terugkeerpatroon](./media/cloud-services-how-to-scale-portal/select-recurrence.png)
    
* **Vaste datum**

    Een vaste datumbereik naar de schaal van de rol aanpassen.

    ![CLoud service schalen met een vaste datum](./media/cloud-services-how-to-scale-portal/select-fixed.png)

Nadat u het profiel hebt geconfigureerd, selecteert u de knop **OK** onderaan in het blad profiel.

#### <a name="rule"></a>Regel

Regels worden toegevoegd aan een profiel en geven van een voorwaarde waarmee de schaal wordt geactiveerd. 

De regel-trigger is gebaseerd op een meting van de cloudservice (CPU-gebruik, schijfactiviteit of netwerkactiviteiten) waaraan u een waarde kunt toevoegen. U kunt ook de trigger op basis van een berichtenwachtrij of de meetwaarde van enkele andere Azure resource die is gekoppeld aan uw abonnement hebben.

![](./media/cloud-services-how-to-scale-portal/rule-settings.png)

Nadat u de regel hebt geconfigureerd, selecteert u de knop **OK** onderaan in het blad voor de regel.

## <a name="back-to-manual-scale"></a>Terug naar handmatige schaal

Ga naar de [schaalinstellingen](#where-scale-is-located) en stel de optie **schaal door** op **een exemplaar tellen die ik handmatig invoeren**.

![Instellingen voor de cloud services-schaal met het profiel en de regel](./media/cloud-services-how-to-scale-portal/manual-basics.png)

Hiermee verwijdert u geautomatiseerde schaalbaarheid van de rol en klik vervolgens kunt u het aantal exemplaar rechtstreeks instellen. 

1. De schaal-optie voor (handmatig of automatisch).
2. Een rol exemplaar schuifregelaar voor het instellen van de exemplaren wilt aanpassen aan.
3. Exemplaren van de rol aan de nieuwe schaal aan.

Nadat u de schaalinstellingen hebt geconfigureerd, selecteert u het pictogram **Opslaan** aan de bovenkant.

