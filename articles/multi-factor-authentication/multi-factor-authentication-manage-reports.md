<properties
    pageTitle="Azure meervoudige verificatie-rapporten"
    description="Hiermee wordt beschreven hoe u met de functie Azure meervoudige verificatie - rapporten."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="reports-in-azure-multi-factor-authentication"></a>Rapporten in Azure meervoudige verificatie

Azure meervoudige verificatie biedt verschillende rapporten die kunnen worden gebruikt door u en uw organisatie. Deze rapporten kunnen worden geopend via de meervoudige verificatie Management Portal, waarin moet u beschikken over een licentie Azure MFA Provider, of een Azure MFA, Azure AD Premium of Enterprise mobiliteit Suite. Hier volgt een lijst met de beschikbare rapporten.

U kunt rapporten openen via de portal Azure Management.

Naam| Beschrijving
:------------- | :------------- |
Gebruik | De weergave-informatie wordt op algehele gebruik, gebruiker samenvatting en gebruikersgegevens gebruiksrapporten.
Status van de server|Dit rapport toont de status van meervoudige verificatie-Servers die is gekoppeld aan uw account.
Geblokkeerde gebruikersgeschiedenis|De geschiedenis van aanvragen voor blokkeren of deblokkeren gebruikers weergeven deze rapporten
Gepasseerd gebruikersgeschiedenis|Ziet u de geschiedenis van aanvragen, worden omzeild meervoudige verificatie voor het telefoonnummer van een gebruiker.
Waarschuwing|Ziet u een geschiedenis van fraude meldingen verzonden tijdens het datumbereik dat u hebt opgegeven.
In de wachtrij|Lijsten rapporten in de wachtrij voor verwerking en hun status. Een koppeling naar downloaden of bekijken van het rapport weergegeven wanneer het rapport voltooid is.

## <a name="to-view-reports"></a>Rapporten weergeven

1.  Meld u aan bij http://azure.microsoft.com
2.  Selecteer aan de linkerkant, Active Directory.
3.  Selecteer een van de volgende opties:
    - **Optie 1**: klik op het tabblad meervoudige Auth Providers. Selecteer uw provider van MFA en klik op de knop beheren onder.
    - **Optie 2**: Selecteer de map en klik op het tabblad configureren. Selecteer onder de sectie meervoudige verificatie beheren service-instellingen. Klik onder aan de pagina Service-instellingen van MFA, op de koppeling Ga naar de portal.
4.  In de beheerportal Azure meervoudige verificatie ziet u de weergave de sectie van een rapport in het linker navigatiegedeelte. Hier kunt u de rapporten die hierboven is beschreven.

<center>![Cloud](./media/multi-factor-authentication-manage-reports/report.png)</center>


**Aanvullende informatie**

* [Voor gebruikers](./end-user/multi-factor-authentication-end-user.md)
* [Azure meervoudige verificatie op MSDN](https://msdn.microsoft.com/library/azure/dn249471.aspx)
