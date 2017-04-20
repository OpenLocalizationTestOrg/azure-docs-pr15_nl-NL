<properties
    pageTitle="Niet kunt aanmelden bij Azure abonnement | Microsoft Azure"
    description="Beschreven hoe u enkele algemene problemen Azure abonnement login."
    services=""
    documentationCenter=""
    authors="genlin"
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
    ms.date="10/25/2016"
    ms.author="genli"/>

# <a name="i-cant-sign-in-to-manage-my-azure-subscription"></a>Ik kan niet aanmelden bij mijn abonnement op Azure beheren

In dit artikel begeleidt u bij enkele van de meest voorkomende methoden voor het oplossen van problemen met aanmelden.

## <a name="page-hangs-in-the-loading-status"></a>Pagina loopt vast in de status laden

Als uw browserpagina internet vastloopt, probeert u elk van de volgende stappen totdat u toegang hebt tot de [portal van Azure](https://portal.azure.com).

-   Vernieuw de pagina.
-   Gebruik een andere internetbrowser.
-   Als u Microsoft Internet Explorer gebruikt, gaat u naar de Azure-portal met behulp van de modus InPrivate-navigatie. 

    A.  Klik op **Extra** ![knop Extra](./media/billing-cannot-login-subscription/Toolsbutton.png) > **veiligheid** > **InPrivate-navigatie**.

    B TE DRUKKEN.  Blader naar de [Azure-portal](https://portal.azure.com)en meld u aan bij de portal.

## <a name="error-message-no-subscriptions-found"></a>Foutbericht 'Geen abonnement gevonden'

Als uw account niet voldoende machtigingen hebt, ziet u mogelijk een foutbericht **geen abonnement gevonden** . Alleen de accountbeheerder van een kunt u aan het [Account Center](https://account.windowsazure.com/), niet de servicebeheerders (SA) of CO-beheerders (CA).

**Scenario 1: Het foutbericht wordt ontvangen in de [portal van Azure](https://portal.azure.com)**

Dit probleem, [de rol collega beheerder of eigenaar toevoegen](billing-add-change-azure-subscription-administrator.md) voor het account kunt oplossen.

**Scenario 2: Het foutbericht wordt ontvangen in het [Beheercentrum van Azure-Account](https://account.windowsazure.com/Subscriptions)**

Controleer of het account dat u gebruikt de accountbeheerder van het. Als u wilt controleren wie de accountbeheerder van het is, als volgt te werk:

1.  Meld u aan bij de [portal van Azure](https://portal.azure.com).
2.  Selecteer in het menu Hub **abonnement**.
3.  Selecteer het abonnement waaraan u wilt controleren en selecteer **Instellingen**.
4.  Selecteer **Eigenschappen**. De accountbeheerder van het van het abonnement dat wordt weergegeven in het vak **Account-beheerder** .

## <a name="you-are-automatically-signed-in-as-a-different-user"></a>U worden automatisch aangemeld als een andere gebruiker

Dit probleem kan zich voordoen als u meer dan één gebruikersaccount in een webbrowser gebruikt.

Dit probleem, voert u een van de volgende methoden:

-   De cache wissen en verwijderen van cookies van Internet. Klik in Internet Explorer op **Extra** ![knop Extra](./media/billing-cannot-login-subscription/Toolsbutton.png) > **Internetopties** > **verwijderen**. Zorg ervoor dat de selectievakjes uit voor tijdelijke bestanden, cookies, wachtwoord en browsegeschiedenis zijn geselecteerd en klik vervolgens op verwijderen.

-   De instellingen van Internet Explorer te herstellen van alle persoonlijke instellingen die u hebt gemaakt opnieuw. Klik op **Extra** ![knop Extra](./media/billing-cannot-login-subscription/Toolsbutton.png)> **Internetopties** > **Geavanceerd** > schakelt u het **verwijderen van persoonlijke instellingen** > **opnieuw instellen**.

-   Blader naar de Azure-portal in de modus InPrivate-navigatie. Klik op **Extra** ![knop Extra](./media/billing-cannot-login-subscription/Toolsbutton.png) > **veiligheid** > **InPrivate-navigatie**.

## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning. 

Als u nog steeds hulp nodig hebt, [contact opnemen met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel opgelost. 