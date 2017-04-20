<properties
   pageTitle="De map voor uw Office 365-abonnement in Azure beheren | Microsoft Azure"
   description="Een Office 365-abonnement-directory met Azure Active Directory en de Azure klassieke-portal beheren"
   services="active-directory"
   documentationCenter=""
   authors="curtand"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="curtand"/>

# <a name="manage-the-directory-for-your-office-365-subscription-in-azure"></a>De map voor uw Office 365-abonnement in Azure beheren

In dit artikel wordt beschreven hoe voor het beheren van een map die is gemaakt voor een Office 365-abonnement met behulp van de Azure klassieke portal. U moet de Service-beheerder of de beheerder van een collega van een abonnement op Azure te melden bij de portal van Azure klassieke. Als u een abonnement op Azure nog niet hebt, kunt u vandaag registreren voor een [gratis proefabonnement van 30 dagen](https://azure.microsoft.com/trial/get-started-active-directory/) en uw eerste cloud-oplossing implementeert in onder 5 minuten, via deze koppeling. Zorg ervoor dat het werk of school-account waarmee u zich aanmelden bij Office 365 gebruiken.

Nadat u het Azure abonnement hebt voltooid, kunt u aanmelden bij de portal van Azure klassieke en Azure services gebruiken. Klik op de Active Directory-extensie als u wilt dezelfde map die wordt geverifieerd door uw Office 365-gebruikers beheren.

Als u een Azure-abonnement hebt nog, is het proces voor het beheren van een map voor extra ook eenvoudig. Stel, hebt Michael Smith Office 365-abonnement voor Contoso.com. Hij heeft ook een Azure-abonnement dat hij zich hebt geregistreerd met behulp van zijn Microsoft-account, msmith@hotmail.com. In dit geval beheert hij twee mappen.

  Abonnement |  Office 365  |  Azure
  -------------- | ------------- | -------------------------------
  Weergavenaam |  Contoso  |     Standaardmap Azure Active Directory (Azure AD)
  Domeinnaam  |  Contoso.com  | msmithhotmail.onmicrosoft.com

Hij wil voor het beheren van de identiteit van de gebruikers in de directory van Contoso tijdens hij is aangemeld bij Azure met zijn Microsoft-account, zodat hij Azure AD-functies zoals meervoudige verificatie kunt inschakelen. In het volgende diagram mogelijk handiger om u te illustreren het proces.

![Diagram voor het beheren van twee onafhankelijke mappen](./media/active-directory-manage-o365-subscription/AAD_O365_03.png)

In dit geval zijn de twee mappen onafhankelijk van elkaar.

## <a name="to-manage-two-independent-directories"></a>Voor het beheren van twee onafhankelijke mappen
In de volgorde voor Michael Smith voor het beheren van beide mappen tijdens hij is aangemeld bij Azure als msmith@hotmail.com, hij moet de volgende stappen uitvoeren:

> [AZURE.NOTE]
> Deze stappen kunnen worden voltooid alleen wanneer een gebruiker is aangemeld met een Microsoft-account. **Gebruik bestaande map** niet beschikbaar als de gebruiker is aangemeld met een werk- of schoolaccount, de optie voor is. Een werk- of schoolaccount-account kan worden geverifieerd alleen door de basismap (dat wil zeggen de map waar het account voor werk- of schoolaccount is opgeslagen, en het bedrijf of school toe).

1.  Meld u aan bij de [portal van Azure klassieke](https://manage.windowsazure.com) als msmith@hotmail.com.
2.  Klik op **Nieuw** > **App services** > **Active Directory** > **Directory** > **Aangepaste maken**.
3.  Klik op de bestaande map gebruiken en schakel het selectievakje **ik ben klaar om te worden nu afgemeld** .
4.  Meld u aan bij de Azure klassieke-portal als globale beheerder van Contoso.onmicrosoft.com (bijvoorbeeld msmith@contoso.com).
5.  Wanneer u wordt gevraagd naar **de directory van Contoso gebruiken met Azure?**, klikt u op **Doorgaan**.
6.  Klik op **nu afmelden**.
7.  Meld u aan bij de portal van Azure klassieke als msmith@hotmail.com. De directory van Contoso en de standaardmap weergegeven in de Active Directory-extensie.

Nadat u deze stappen uitvoert, msmith@hotmail.com is van een globale beheerder in de directory van Contoso.

## <a name="to-administer-resources-as-the-global-admin"></a>Resources beheren als de globale beheerder
Nu Stel dat Jane Doe moet beheren websites en database resources die zijn gekoppeld aan het Azure abonnement voor msmith@hotmail.com. Voordat zij die doen kunt, moet de Michael Smith deze extra stappen uitvoeren:

1.  Meld u aan bij de [portal van Azure klassieke](https://manage.windowsazure.com) met de Service-beheerder-account voor het abonnement dat Azure (in dit voorbeeld msmith@hotmail.com).
2.  Het abonnement overbrengen naar de map Contoso: klik op **Instellingen** > **abonnementen** > Selecteer het abonnement > **Adreslijst bewerken** > selecteren **Contoso (Contoso.com)**. Als onderdeel van de overdracht, werk of school worden accounts die CO-beheerders van het abonnement zijn verwijderd.
3.  Jane Doe toevoegen als collega beheerder van het abonnement: klik op **Instellingen** > **beheerders** > Selecteer het abonnement > **toevoegen** > type **JohnDoe@Contoso.com**.

## <a name="next-steps"></a>Volgende stappen
Zie [hoe een abonnement is gekoppeld aan een map](active-directory-how-subscriptions-associated-directory.md)voor meer informatie over de relatie tussen abonnementen en -mappen.
