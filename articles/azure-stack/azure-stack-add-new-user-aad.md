<properties
    pageTitle="Een nieuw Azure stapel tenant-account toevoegen in Azure Active Directory | Microsoft Azure"
    description="Na de implementatie van Microsoft Azure stapel Haalbaarheidstest, moet u ten minste één tenant-gebruikersaccount maken zodat u kunt de tenant-portal verkennen."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="erikje"/>

# <a name="add-a-new-azure-stack-tenant-account-in-azure-active-directory"></a>Een nieuw Azure stapel tenant-account toevoegen in Azure Active Directory

Na het [implementeren van de Azure stapel Haalbaarheidstest](azure-stack-run-powershell-script.md)moet u een gebruikersaccount tenant zodat u kunt de tenant-portal verkennen en uw aanbiedingen en plannen test. U kunt een tenantaccount maken met [behulp van de Azure-portal](#create-an-azure-stack-tenant-account-using-the-azure-portal) of via [PowerShell](#create-an-azure-stack-tenant-account-using-powershell).

## <a name="create-an-azure-stack-tenant-account-using-the-azure-portal"></a>Een Azure stapel tenantaccount met behulp van de Azure portal maken

U moet een Azure-abonnement voor het gebruik van de Azure-portal.

1. Meld u aan bij [Azure](http://manage.windowsazure.com).

2.  Klik in Microsoft Azure linkernavigatiebalk op **Active Directory**.

3.  Klik in de lijst directory klikt u op de map die u wilt gebruiken voor Azure-Stack of een nieuw account te maken.

4.  Klik op **gebruikers**op deze pagina directory.

5.  Klik op **gebruiker toevoegen**.

6.  Kies **nieuwe gebruiker in uw organisatie**in de wizard **gebruiker toevoegen** in de lijst **Type gebruiker** .

7.  Typ een naam voor de gebruiker in het vak **gebruikersnaam in te voeren** .

8.  In de **@** Kies de gewenste vermelding.

9.  Klik op de pijl-rechts.

10.  Typ op de pagina **gebruikersprofiel** van de wizard een **Voornaam**, **Achternaam**en **naam weer te geven**.

11. Kies in de lijst **functie** **gebruiker**.

12. Klik op de pijl-rechts.

13. Klik op **maken**op de pagina **krijgen tijdelijke wachtwoord** .

14. Kopieer het **nieuwe wachtwoord**.

15. Meld u aan bij Microsoft Azure met het nieuwe account. Wijzig het wachtwoord in als u hierom wordt gevraagd.

16. Meld u aan bij `https://portal.azurestack.local` met het nieuwe account om te zien van de tenant-portal.

## <a name="create-an-azure-stack-tenant-account-using-powershell"></a>Maak een Azure stapel tenant-account via PowerShell

Als u geen een Azure-abonnement hebt, kunt u de Azure-portal niet gebruiken om toe te voegen een tenant-gebruikersaccount. In dit geval kunt u in plaats daarvan de Azure Active Directory-Module voor Windows PowerShell.

> [AZURE.NOTE] Als u via Microsoft-Account (Live ID) Azure stapel Haalbaarheidstest implementeren, kunt u niet AAD PowerShell gebruiken tenantaccount maken. 

1.  Installeer de [Microsoft Online Services - aanmeldhulp voor IT-Professionals RTW](https://www.microsoft.com/en-us/download/details.aspx?id=41950).

2.  Installeren van de [Azure Active Directory-Module voor Windows PowerShell (64-bits versie)](http://go.microsoft.com/fwlink/p/?linkid=236297) en open dit.

3.  Voer de volgende cmdlets uit:




    ```powershell
    # Provide the AAD credential you use to deploy Azure Stack PoC
   
            $msolcred = get-credential
    
    # Add a tenant account "Tenant Admin <username>@<yourdomainname>" with the initial password "<password>".
    
            connect-msolservice -credential $msolcred
            $user = new-msoluser -DisplayName "Tenant Admin" -UserPrincipalName <username>@<yourdomainname> -Password <password>
            Add-MsolRoleMember -RoleName "Company Administrator" -RoleMemberType User -RoleMemberObjectId $user.ObjectId
    
    ```

4.  Meld u aan bij Microsoft Azure met het nieuwe account. Wijzig het wachtwoord in als u hierom wordt gevraagd.

5.  Meld u aan bij `https://portal.azurestack.local` met het nieuwe account om te zien van de tenant-portal.



