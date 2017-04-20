<properties
    pageTitle="Instellen van waarschuwingen voor uw Microsoft Azure-abonnementen facturering | Microsoft Azure"
    description="Hierin wordt beschreven hoe u kunt waarschuwingen instellen op uw factuur Azure zodat u facturering verrassingen kunt voorkomen."
    services=""
    documentationCenter=""
    authors="vikdesai"
    manager="mbaldwin"
    editor=""
    tags="billing"
    />

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/18/2016"
    ms.author="vikdesai"/>

# <a name="set-up-billing-alerts-for-your-microsoft-azure-subscriptions"></a>Facturering waarschuwingen instellen voor uw Microsoft Azure-abonnementen

Gaat u over hoeveel u elke maand voor uw Azure-abonnement bent besteden? Als u de accountbeheerder van het voor een Azure-abonnement bent, kunt u de Azure facturering waarschuwing-Service te maken van aangepaste billing waarschuwingen die u helpen bewaken en facturering activiteit voor uw Azure-accounts beheren.

Deze service is een voorbeeld-service, dus het eerste wat dat u moet doen teken omhoog voor deze. Ga naar [de pagina onderdelen van de Preview-versie](https://account.windowsazure.com/PreviewFeatures) in de beheerportal Azure-account deze inschakelen.

## <a name="set-the-alert-threshold-and-email-recipients"></a>De ontvangers van de waarschuwing drempel en e-mail instellen

Nadat u de e-mail een bevestiging dat de facturering-service is ingeschakeld voor uw abonnement ontvangt, gaat u naar [de pagina abonnementen](https://account.windowsazure.com/Subscriptions) in de portal account. Klik op het abonnement dat u wilt controleren en klik vervolgens op **meldingen**.

![][Image1]

Klik vervolgens op **Melding toevoegen** om uw eerste account te maken: u kunt in totaal vijf billing meldingen per abonnement, met een ander drempel plus maximaal twee e-mailgeadresseerden voor elke melding instellen.

![][Image2]

Wanneer u een waarschuwing toevoegt, u hieraan een unieke naam, kiest u een drempelwaarde voor uitgaven en kies de e-mailadressen waar meldingen worden verzonden. Wanneer u de drempelwaarde voor instelt, kunt u een **Totaal facturering** of een **Monetaire punten** uit de lijst **Waarschuwing voor** . Voor een totaal facturering, wordt een melding wanneer abonnement besteden groter is dan de drempelwaarde voor verzonden. Voor een monetaire krediet, wordt een melding wanneer monetaire tegoeden onder de limiet plaatst verzonden. Monetaire tegoeden zijn meestal van toepassing op gratis experimenten en abonnementen die is gekoppeld aan MSDN-accounts.

![][Image3]

Azure ondersteuning biedt voor elk e-mailadres, maar niet bevestigen dat de e-mailadres werken, dus controleer of op typefouten.

## <a name="check-on-your-alerts"></a>Controleren of uw waarschuwingen

Nadat u meldingen hebt ingesteld, wordt het beheercentrum Account deze lijsten en ziet u hoeveel meer kunt u instellen. Elke melding ziet u de datum en tijd waarop die deze is verzonden, of u nu een waarschuwing voor totale facturering of monetaire creditnota en de limiet die u hebt ingesteld. De notatie datum en tijd is coördineren van 24-uurs-Universal Time (UTC) en de datum is jjjj-mm-dd-notatie. Klik op het plusteken (+) voor een melding in de lijst om deze te bewerken of klik op de Prullenbak om het te verwijderen.

[Image1]: ./media/azure-billing-set-up-alerts/billingalert1.png
[Image2]: ./media/azure-billing-set-up-alerts/billingalert2.png
[Image3]: ./media/azure-billing-set-up-alerts/billingalerts3.png
