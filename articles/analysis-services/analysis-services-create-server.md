<properties
   pageTitle="Een Analysis Services-server maken in Azure | Microsoft Azure"
   description="Informatie over het maken van een exemplaar van Analysis Services-server in Azure wordt aangegeven."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="owend"/>

# <a name="create-an-analysis-services-server"></a>Maak een Analysis Services-server
In dit artikel begeleidt u bij het maken van een nieuwe resource van de Analysis Services-server in uw Azure-abonnement.

## <a name="before-you-begin"></a>Voordat u begint
Als u wilt beginnen, hebt u het volgende nodig:

- **Azure-abonnement**: Bezoek [Azure gratis proefversie](https://azure.microsoft.com/offers/ms-azr-0044p/) om een account te maken.
- **Resourcegroep**: gebruik een resourcegroep die u al hebt of [een nieuw account te maken](../azure-resource-manager/resource-group-overview.md).

> [AZURE.NOTE] Maken van een Analysis Services-server kan leiden tot een nieuwe factureerbare service. Meer informatie raadpleegt u Analysis Services prijzen.

## <a name="create-an-analysis-services-server"></a>Maak een Analysis Services-server

1. Meld u aan bij de [portal van Azure](https://portal.azure.com).

2. Klik op **+ Nieuw** > **Intelligence + analytics** > **Analysis Services**.

3. Vul de vereiste velden in het blad **Analysis Services** , en druk vervolgens op **maken**.

    ![Server maken](./media/analysis-services-create-server/aas-create-server-blade.png)

    - **Servernaam**: Typ een unieke naam gebruikt om te verwijzen naar de server.

    - **Abonnement**: Selecteer het abonnement dat deze server stuklijsten aan.

    - **Resourcegroep**: dit zijn containers zo ontworpen dat u een verzameling Azure bronnen beheren. Zie voor meer informatie, [resourcegroepen](../resource-group-overview.md).

    - **Locatie**: deze Azure datacenter locatie fungeert als server. Kies een locatie op het dichtstbijzijnde uw grootste grondtal van de gebruiker.

    - **Prijzen laag**: Selecteer een prijzen laag. Maximaal 100 GB een Tabellair model worden ondersteund. U kunt uw prijzen laag later altijd wijzigen.

4. Klik op **maken**.

Maak gewoonlijk duurt onder een minuut; vaak slechts een paar seconden. Als u **toevoegen aan de Portal**hebt geselecteerd, gaat u naar uw portal om uw nieuwe server weer te geven. Of, Ga naar **meer services** > **Analysis Services** om te kijken of uw server gereed is. Als deze niet wordt weergegeven vernieuwt u de lijst.

 ![Dashboard](./media/analysis-services-create-server/aas-create-server-dashboard.png)


## <a name="next-steps"></a>Volgende stappen
Nadat u uw server hebt gemaakt, kunt u [een model implementeert](analysis-services-deploy.md) ernaar toevoegen met behulp van SSDT of met SSMS.

Als een model dat u dashboard naar uw server implementeren verbinding on-premises gegevensbronnen maakt, moet u een [On-premises gegevensgateway](analysis-services-gateway.md) installeren op een computer in uw netwerk.
