<properties 
    pageTitle="Hoe gebruikersaccounts beheren in Azure API Management | Microsoft Azure" 
    description="Informatie over het maken of gebruikers in Azure API Management uitnodigen" 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-manage-user-accounts-in-azure-api-management"></a>Hoe u gebruikersaccounts beheren in Azure API Management

Beheer van API zijn ontwikkelaars de gebruikers van de API's die u laten zien API te beheren. Deze handleiding geeft aan hoe u maakt en ontwikkelaars uitnodigen gebruik van de API's en producten die u beschikbaar zijn voor uw exemplaar van de API Management aanbrengt. Zie de documentatie [entiteit gebruiker](https://msdn.microsoft.com/library/azure/dn776330.aspx) in de verwijzing [API Management REST](https://msdn.microsoft.com/library/azure/dn776326.aspx) voor informatie over het beheer van gebruikersaccounts via een programma's.

## <a name="create-developer"> </a>Maken van een nieuwe ontwikkelaar

Klik op **beheren** in de klassieke Azure-Portal voor uw service API Management om een nieuwe ontwikkelaar. Hiermee gaat u naar de publisher-API-beheerportal. Als u een exemplaar van de service API Management nog niet hebt gemaakt, raadpleegt u [een exemplaar van de service API Management maken][] in deze zelfstudie [aan de slag met Azure API Management][] .

![Publisher-portal][api-management-management-console]

Klik op **gebruikers** in het menu **API beheer** aan de linkerkant en klik op **gebruiker toevoegen**.

![Ontwikkelaars maken][api-management-create-developer]

Voer het **e-mailadres**, **wachtwoord**en **naam** voor de nieuwe ontwikkelaar en klik op **Opslaan**.

![Ontwikkelaars maken][api-management-add-new-user]

Standaard gemaakte ontwikkelaars accounts **actief**zijn en dat is gekoppeld aan de groep **ontwikkelaars** .

![Nieuwe ontwikkelaars][api-management-new-developer]

Ontwikkelaars accounts met een **actieve** status kunnen worden gebruikt voor toegang tot alle van de API's waarvoor ze abonnementen hebben. Zie de zojuist gemaakte ontwikkelaar om aan te koppelen extra groepen, [groepen met ontwikkelaars koppelen][].

## <a name="invite-developer"> </a>Een ontwikkelaar uitnodigen

Een ontwikkelaar uitnodigen, klikt u op **gebruikers** in het menu **API beheer** aan de linkerkant en klik vervolgens op **Uitnodigen gebruiker**.

![Ontwikkelaars uitnodigen][api-management-invite-developer]

Voer de naam en e-mailadres van de ontwikkelaar en klik op **uitnodiging**.

![Ontwikkelaars uitnodigen][api-management-invite-developer-window]

Een bevestigingsbericht wordt weergegeven, maar de zojuist uitgenodigd ontwikkelaar niet wordt weergegeven in de lijst tot nadat ze de uitnodiging geaccepteerd. 

![Bevestiging uitnodigen][api-management-invite-developer-confirmation]

Als een ontwikkelaar wordt uitgenodigd, wordt een e-mailbericht verzonden naar de ontwikkelaar. Deze e-mail wordt gegenereerd op basis van een sjabloon en kan worden aangepast. Zie [e-mailsjablonen configureren][]voor meer informatie.

Zodra de uitnodiging wordt aangenomen, is het account wordt geactiveerd.

## <a name="block-developer"></a> Uitschakelen of opnieuw activeren op een ontwikkelaarsaccount

Nieuw gemaakte of uitgenodigd ontwikkelaars accounts zijn standaard **actief**. Als u wilt een ontwikkelaarsaccount hebt gedeactiveerd, klikt u op **Adresblok**. Als u wilt een ontwikkelaarsaccount geblokkeerde activeren, klikt u op **activeren**. Een geblokkeerde ontwikkelaarsaccount kan niet toegang tot de developer portal of een API's bellen. Als u wilt een gebruikersaccount verwijderen, klikt u op **verwijderen**.

![Blok ontwikkelaars][api-management-new-developer]

## <a name="reset-a-user-password"></a>Een gebruikerswachtwoord opnieuw instellen

Als u wilt het wachtwoord van een gebruiker opnieuw instellen, klikt u op de naam van het account.

![Wachtwoord opnieuw instellen][api-management-view-developer]

Klik op **wachtwoord opnieuw in** een koppeling verzenden aan de gebruiker het wachtwoord opnieuw in te stellen.

![Wachtwoord opnieuw instellen][api-management-reset-password]

Als u via programmacode met gebruikersaccounts werkt, raadpleegt u de documentatie [entiteit gebruiker](https://msdn.microsoft.com/library/azure/dn776330.aspx) in de verwijzing [API Management REST](https://msdn.microsoft.com/library/azure/dn776326.aspx) . Als u wilt een gebruikerswachtwoord opnieuw naar een bepaalde waarde, kunt u gebruikt de bewerking voor het [bijwerken van een gebruiker](https://msdn.microsoft.com/library/azure/dn776330.aspx#UpdateUser) en geef het gewenste wachtwoord.

## <a name="pending-verification"></a>In behandeling verificatie

![In behandeling verificatie][api-management-pending-verification]

## <a name="next-steps"> </a>Vervolgstappen

Nadat een ontwikkelaarsaccount is gemaakt, kunt u koppelen aan rollen en u hierop abonneren producten en API's. Lees [hoe u maken en hierin groepen wilt gebruiken][]voor meer informatie.


[api-management-management-console]: ./media/api-management-howto-create-or-invite-developers/api-management-management-console.png
[api-management-add-new-user]: ./media/api-management-howto-create-or-invite-developers/api-management-add-new-user.png
[api-management-create-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-create-developer.png
[api-management-invite-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer.png
[api-management-new-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-new-developer.png
[api-management-invite-developer-window]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-window.png
[api-management-invite-developer-confirmation]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-confirmation.png
[api-management-pending-verification]: ./media/api-management-howto-create-or-invite-developers/api-management-pending-verification.png
[api-management-view-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-view-developer.png
[api-management-reset-password]: ./media/api-management-howto-create-or-invite-developers/api-management-reset-password.png
[]: ./media/api-management-howto-create-or-invite-developers/.png



[Create a new developer]: #create-developer
[Invite a developer]: #invite-developer
[Deactivate or reactivate a developer account]: #block-developer
[Next steps]: #next-steps
[Het maken en hierin groepen wilt gebruiken]: api-management-howto-create-groups.md
[Het koppelen van groepen met ontwikkelaars]: api-management-howto-create-groups.md#associate-group-developer

[Aan de slag met Azure API Management]: api-management-get-started.md
[Een exemplaar van de service API Management maken]: api-management-get-started.md#create-service-instance
[E-mailsjablonen configureren]: api-management-howto-configure-notifications.md#email-templates