<properties
    pageTitle="Meer informatie over de kosten van uw Azure externe service | Microsoft Azure"
    description="Meer informatie over facturering van externe services, voorheen bekend als Marketplace, kosten in Azure wordt aangegeven."
    services=""
    documentationCenter=""
    authors="adpick"
    manager="felixwu"
    editor=""
    tags="billing"
    />

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="adpick"/>

# <a name="understand-your-azure-external-service-charges"></a>Meer informatie over de kosten van uw Azure externe service

In dit artikel wordt uitgelegd dat de facturering van externe services in Azure wordt aangegeven. Externe services gebruikt om te worden aangeroepen Marketplace orders. Externe Services worden verstrekt door onafhankelijke serviceleveranciers, maar zijn volledig geïntegreerd binnen het Azure-systeem. Leer hoe u:

- Externe Services identificeren
- Meer informatie over hoe de facturering verschilt van andere Azure resources
- Weergeven en bijhouden van de kosten die u toenemen van het gebruik van externe services
- Externe serviceorders en hoe u betaalt voor hen beheren

## <a name="what-are-azure-external-services"></a>Wat zijn Azure externe services?

Externe services gebruikt om te worden aangeroepen Azure Marketplace. In het algemeen zijn de services die zijn gepubliceerd door derden beschikbaar voor Azure. ClearDB en SendGrid zijn bijvoorbeeld externe services die u kunt kopen in Azure wordt aangegeven, maar niet worden gepubliceerd door Microsoft.

### <a name="identify-external-services"></a>Externe services identificeren

Wanneer u een nieuwe externe service of de resource inrichten, wordt een beveiligingswaarschuwing weergegeven:

![Marketplace aanschaffen waarschuwing](./media/billing-understand-your-azure-marketplace-charges/marketplace-warning.PNG)

>[AZURE.NOTE] Externe services worden uitgegeven door bedrijven die niet Microsoft zijn, maar soms ook Microsoft-producten zijn gecategoriseerd als externe services.

### <a name="external-services-are-billed-separately"></a>Externe services worden afzonderlijk gefactureerd

Externe services worden behandeld als afzonderlijke orders binnen uw Azure-abonnement. De betalingsperiode voor elke service wordt ingesteld wanneer u de service kopen. Niet te verwarren met de betalingsperiode van het abonnement waaronder u deze hebt gekocht. Daarnaast ontvangt u afzonderlijke facturen en uw creditcard is afzonderlijk in rekening gebracht.

### <a name="each-external-service-has-a-different-billing-model"></a>Elke externe service heeft een ander billing model

Sommige services zijn gefactureerd op pay-as-you-go wijze terwijl de anderen een maandkalender gebaseerd betaling model gebruiken. U hebt een creditcard voor Azure externe services, u kunt geen externe services met factuur betalen aanschaffen.

### <a name="you-cant-use-monthly-free-credits-for-external-services"></a>U kunt de maandelijkse gratis tegoeden niet gebruiken voor externe services

Als u een Azure abonnement met [gratis tegoeden](https://azure.microsoft.com/pricing/spending-limits/)gebruikt, kan ze kunnen niet worden toegepast op externe service facturen. Gebruik een creditcard naar externe services aanschaffen.

## <a name="view-external-service-spending-and-history"></a>Weergave externe service uitgaven en geschiedenis

U kunt een lijst met de externe services die zich op elk abonnement binnen de [portal van Azure](https://portal.azure.com/)weergeven: 

1. Meld u aan bij de [Azure-portal](https://portal.azure.com/) en [Ga naar het blad **Facturering** ](https://portal.azure.com/?flight=1#blade/Microsoft_Azure_Billing/BillingBlade).

    ![Selecteer facturering in het menu Hub](./media/billing-understand-your-azure-marketplace-charges/billing-button.png) 
  
2. Selecteer het abonnement dat u wilt weergeven in de sectie **abonnement kosten** . 
   
    ![Selecteer een abonnement in het blad facturering](./media/billing-understand-your-azure-marketplace-charges/select-sub.png)

3. Klik op **externe services**.

    ![Klik op externe Services in het blad abonnement](./media/billing-understand-your-azure-marketplace-charges/external-service-blade.png)

4. Ziet u elk van uw externe serviceorders publisher, servicelaag die u hebt gekocht, de naam die u hebt opgegeven de resource en de status van de huidige. Selecteer een externe service om eerdere facturen weer te geven.

    ![Selecteer een externe service](./media/billing-understand-your-azure-marketplace-charges/external-service-blade2.png)

5. Van hieruit kunt u eerdere factuur bedragen, met inbegrip van de verdeling van de belasting weergeven.

    ![Weergave externe services factureringsgeschiedenis](./media/billing-understand-your-azure-marketplace-charges/billing-overview-blade.png)

## <a name="manage-payment-methods-for-external-service-orders"></a>Betalingsmethoden voor het externe serviceorders beheren

Werk uw betalingsmethode voor externe serviceorders vanuit het [Beheercentrum van Account](https://account.windowsazure.com/).

> [AZURE.NOTE] Als u uw abonnement met een account voor werk of School hebt gekocht, moet u [contact opnemen met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw betalingswijze te wijzigen.

1. Meld u aan bij het [Beheercentrum Account](https://account.windowsazure.com/) en [Ga naar het tabblad **marketplace** ](https://account.windowsazure.com/Store)

    ![Marketplace selecteert in het account-beheercentrum](./media/billing-understand-your-azure-marketplace-charges/select-marketplace.png)

2. Selecteer de externe service die u wilt beheren

    ![Selecteer de externe service die u wilt beheren](./media/billing-understand-your-azure-marketplace-charges/select-ext-service.png)

3. Klik op **betalingsmethode wijzigen** aan de rechterkant van de pagina. Deze koppeling keert u terug naar een ander portal voor het beheren van uw betalingsmethode.
    
    ![Overzicht van de volgorde](./media/billing-understand-your-azure-marketplace-charges/change-payment.PNG)

4. Klik op **gegevens bewerken** en volg de instructies voor het bijwerken van uw betalingsgegevens.

    ![Selecteer Bewerken info](./media/billing-understand-your-azure-marketplace-charges/edit-info.png)
    
## <a name="cancel-an-external-service-order"></a>De volgorde van een externe service annuleren

Als u annuleren van uw bestelling externe service wilt, moet u de resource in de [portal van Azure](https://portal.azure.com)verwijderen.

![Resource verwijderen](./media/billing-understand-your-azure-marketplace-charges/deleteMarketplaceOrder.PNG)

## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning.

Als u nog verdere vragen hebben, neemt [contact opnemen met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om het probleem opgelost snel.
