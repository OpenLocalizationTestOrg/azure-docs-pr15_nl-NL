
<properties 
    pageTitle="Azure RemoteApp gebruiken met Office 365-gebruikersaccounts | Microsoft Azure"
    description="Informatie over het gebruik van Azure RemoteApp met mijn Office 365-gebruikersaccounts"
    services="remoteapp"
    documentationCenter="" 
    authors="piotrci" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="how-to-use-azure-remoteapp-with-office-365-user-accounts"></a>Azure RemoteApp gebruiken met Office 365-gebruikersaccounts

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Als u een Office 365-abonnement hebt u een Azure Active Directory die uw gebruikersnamen en wachtwoorden gebruikt voor toegang tot Office 365-services worden opgeslagen. Bijvoorbeeld wanneer uw gebruikers Office 365 ProPlus activeren ze worden geverifieerd ten opzichte van Azure AD wilt controleren op licenties. De meeste klanten willen gebruiken dezelfde map met Azure RemoteApp.

Als u Azure RemoteApp implementeert u waarschijnlijk een Azure-abonnement dat is gekoppeld aan een ander Azure AD gebruikt. Pas het telefoonboek van uw Office 365 gebruiken, moet u het abonnement dat Azure verplaatsen naar deze map.

Zie [het gebruik van uw Office 365-abonnement met Azure RemoteApp](remoteapp-officesubscription.md)voor informatie over het implementeren van Office 365-clienttoepassingen.
 
## <a name="phase-1-register-your-free-office-365-azure-active-directory-subscription"></a>Fase 1: Registreren uw gratis Office 365 Azure Active Directory-abonnement
Als u de Azure klassieke portal gebruikt, administratieve toegang krijgen tot uw Azure AD via de beheerportal Azure via de stappen in [uw gratis Azure Active Directory-abonnement hebt geregistreerd](https://technet.microsoft.com/library/dn832618.aspx) . Als het resultaat van deze procedure moet u het volgende kunnen Meld u aan bij de portal van Azure en uw adreslijst er – nu ziet u niet veel meer omdat het volledige Azure abonnement dat u met Azure RemoteApp gebruikt zich in een andere map.

Onthoud dat de naam en het wachtwoord van het beheerdersaccount die u hebt gemaakt in deze stap: ze nodig hebben in fase 2.

Als u de portal van Azure gebruikt, raadpleegt u [hoe u registreren en een gratis Azure Active Directory die met behulp van Office 365-portal activeren](http://azureblogger.com/2016/01/how-to-register-and-activate-a-free-azure-active-directory-using-office-365-portal/).

## <a name="phase-2-change-the-azure-ad-associated-with-your-azure-subscription"></a>Fase 2: Wijzig de Azure AD die is gekoppeld aan uw Azure-abonnement.
We gaan uw Azure-abonnement wijzigen van de huidige map naar de Office 365-map die wordt gewerkt met in fase 1.

Volg de instructies die wordt beschreven in [de tenant Azure Active Directory in Azure RemoteApp wijzigen](remoteapp-changetenant.md). Speciale aandacht besteden aan de volgende stappen uit:

- Stap #1: Als u hebt Azure RemoteApp (ARA) geïmplementeerd in dit abonnement, controleert u of dat u Verwijder alle Azure AD-gebruikersaccounts uit alle siteverzamelingen ARA eerst voordat u iets anders. U kunt ook overwegen verwijderen van een bestaande collecties.
- Stap #2: Dit is een kritieke stap. U moet gebruiken van een Microsoft-account (bijvoorbeeld @outlook.com) als een Service-beheerder het abonnement; dit is omdat we gebruikersaccounts vanuit de bestaande Azure AD die zijn gekoppeld aan het abonnement – kan hebben als we doet, is niet mogelijk om deze te verplaatsen naar een ander Azure AD.
- Stap #4: Bij het toevoegen van een bestaande map, het systeem wordt gevraagd u aan te melden met het beheerdersaccount voor die map. Zorg ervoor dat het beheerdersaccount van fase 1 gebruiken.
- Stap #5: Wijzig de bovenliggende map van het abonnement in het telefoonboek van uw Office 365. Het eindresultaat moet dat onder Instellingen -> abonnementen uw abonnement bevat de map Office 365. 
![Wijzigen van de bovenliggende map van het abonnement](./media/remoteapp-o365user/settings.png)
 

Nu is uw Azure RemoteApp-abonnement gekoppeld aan uw Office 365 Azure AD; u kunt de bestaande Office 365-gebruikersaccounts gebruiken met Azure RemoteApp!




