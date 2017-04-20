<properties
    pageTitle="Wachtwoord verloopbeleid instellen in Azure Active Directory | Microsoft Azure"
    description="Meer informatie over het controleren van verloopbeleid en bijna verlopen wachtwoorden van gebruikers wijzigen afzonderlijk of bulksgewijs voor Azure Active directory-wachtwoorden"
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="curtand"/>


# <a name="set-password-expiration-policies-in-azure-active-directory"></a>Wachtwoord verloopbeleid instellen in Azure Active Directory

> [AZURE.IMPORTANT] **Bent u hier omdat u problemen hebt aanmelden?** Zo ja, [als volgt hoe u kunt wijzigen en uw eigen wachtwoord opnieuw instellen](active-directory-passwords-update-your-own-password.md).

Als een globale beheerder voor een Microsoft-cloudservice, kunt u de Microsoft Azure Active Directory-Module voor Windows PowerShell gebruiken voor het instellen van wachtwoorden van gebruikers niet te verlopen. U kunt ook Windows PowerShell-cmdlets gebruiken om te verwijderen de-verloopt nooit configuratie of om te zien welke gebruiker wachtwoorden zijn ingesteld op niet verlopen. In dit artikel bevat hulp bij van cloudservices, zoals Microsoft Intune en Office 365, die gebaseerd op Microsoft Azure Active Directory voor identiteits- en -services zijn.

  > [AZURE.NOTE] Alleen wachtwoorden opnieuw instellen voor gebruikersaccounts die niet zijn gesynchroniseerd via adreslijstsynchronisatie kunnen worden geconfigureerd niet verlopen. Zie voor meer informatie over directory-synchronisatie, de lijst met onderwerpen in [Active Directory-synchronisatie: roadmap](https://msdn.microsoft.com/library/azure/hh967642.aspx).

Als u wilt Windows PowerShell-cmdlets gebruiken, moet u eerst installeren ze.

## <a name="what-do-you-want-to-do"></a>Wat wilt u doen?

- [Hoe controleer verloopbeleid voor een wachtwoord](#how-to-check-expiration-policy-for-a-password)

- [Instellen dat een wachtwoord verloopt](#set-a-password-to-expire)

- [Een wachtwoord instellen zodat niet verloopt](#set-a-password-to-never-expire)

## <a name="how-to-check-expiration-policy-for-a-password"></a>Hoe controleer verloopbeleid voor een wachtwoord

1.  Verbinding maken met Windows PowerShell met de referenties van uw bedrijf-beheerder.

2.  Voer een van de volgende opties:

    - Om te zien of een enkel gebruikerswachtwoord is ingesteld dat het nooit verloopt, voert u de volgende cmdlet uit waarbij u de UPN (User Principal Name) (bijvoorbeeld aprilr@contoso.onmicrosoft.com) of de gebruikers-ID van de gebruiker die u wilt controleren:`Get-MSOLUser -UserPrincipalName <user ID> | Select PasswordNeverExpires`

    - Als u wilt zien van de instelling 'Wachtwoord verloopt nooit' voor alle gebruikers, moet u de volgende cmdlet uitvoeren:`Get-MSOLUser | Select UserPrincipalName, PasswordNeverExpires`

## <a name="set-a-password-to-expire"></a>Instellen dat een wachtwoord verloopt

1.  Verbinding maken met Windows PowerShell met de referenties van uw bedrijf-beheerder.

2.  Voer een van de volgende opties:

    - Als u wilt het wachtwoord van één gebruiker instellen dat het wachtwoord verloopt, kunt u de volgende cmdlet uitvoeren met behulp van de UPN (User Principal Name) of de gebruikers-ID van de gebruiker:`Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $false`

    - Als u wilt instellen de wachtwoorden van alle gebruikers in de organisatie, zodat ze verloopt, gebruikt u de volgende cmdlet:`Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $false`

## <a name="set-a-password-to-never-expire"></a>Instellen dat een wachtwoord nooit verloopt

1. Verbinding maken met Windows PowerShell met de referenties van uw bedrijf-beheerder.

2.  Voer een van de volgende opties:

    - Als u wilt instellen dat het wachtwoord van één gebruiker nooit verloopt, kunt u de volgende cmdlet uitvoeren met behulp van de UPN (User Principal Name) of de gebruikers-ID van de gebruiker:`Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $true`

    - Als u wilt instellen dat de wachtwoorden van alle gebruikers in een organisatie nooit verloopt, moet u de volgende cmdlet uitvoeren:`Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $true`

## <a name="next-steps"></a>Volgende stappen

* **Bent u hier omdat u problemen hebt aanmelden?** Zo ja, [als volgt hoe u kunt wijzigen en uw eigen wachtwoord opnieuw instellen](active-directory-passwords-update-your-own-password.md).
