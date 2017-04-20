<properties
    pageTitle="Krijg Azure MFA gestart in de cloud | Microsoft Azure"
    description="Dit is de pagina in een Microsoft Azure meervoudige verificatie die wordt beschreven hoe u aan de slag met Azure MFA in de cloud."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/17/2016"
    ms.author="kgremban"/>

# <a name="getting-started-with-azure-multi-factor-authentication-in-the-cloud"></a>Aan de slag met Azure meervoudige verificatie in de cloud
In dit artikel leert hoe u aan de slag met Azure meervoudige verificatie in de cloud.

> [AZURE.NOTE]  De volgende documentatie vindt u informatie over het inschakelen van gebruikers met behulp van de **Azure klassieke Portal**. Als u voor meer informatie over het instellen van Azure meervoudige verificatie voor O365-gebruikers zoeken wilt, raadpleegt u [meervoudige verificatie voor Office 365 instellen.](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6?ui=en-US&rs=en-US&ad=US)

![MFA in de Cloud](./media/multi-factor-authentication-get-started-cloud/mfa_in_cloud.png)

## <a name="prerequisites"></a>Vereisten voor
De volgende vereisten zijn vereist voordat u Azure meervoudige verificatie voor uw gebruikers inschakelen kunt.


1. [Registreren voor een abonnement op Azure](https://azure.microsoft.com/pricing/free-trial/) - als u nog niet een Azure-abonnement hebt, moet u eerst registreren voor een. Als u net begint en het gebruik van Azure MFA kunt u een proefabonnement
2. [Een meervoudige Auth Provider maken](multi-factor-authentication-get-started-auth-provider.md) en toewijzen aan de map of het [toewijzen van licenties aan gebruikers](multi-factor-authentication-get-started-assign-licenses.md)

> [AZURE.NOTE]  Licenties zijn beschikbaar voor gebruikers met Azure MFA, Azure AD Premium of Enterprise mobiliteit Suite (EMS).  MFA deel uitmaakt van Azure AD Premium en de EMS. Als u voldoende licenties hebt, hoeft u niet om een Auth Provider te maken.


## <a name="turn-on-two-step-verification-for-users"></a>Verificatie in twee stappen voor gebruikers inschakelen
Als u wilt beginnen met het vereisen van twee begin-verificatie in voor een gebruiker, status van de gebruiker te wijzigen van uitgeschakeld in ingeschakeld.  Zie voor meer informatie over gebruiker Staten, [Lidstaten van de gebruiker in Azure meervoudige verificatie](multi-factor-authentication-get-started-user-states.md)

Gebruik de volgende procedure om in te schakelen MFA voor uw gebruikers.

### <a name="to-turn-on-multi-factor-authentication"></a>Meervoudige verificatie inschakelen

1.  Meld u als beheerder aan bij de [portal van Azure klassieke](https://manage.windowsazure.com) .
2.  Aan de linkerkant, klikt u op **Active Directory**.
3.  Selecteer onder Directory, de map voor de gebruiker die u wilt inschakelen.
![Klik op map](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4.  Aan de bovenkant, klikt u op **gebruikers**.
5.  Klik onder aan de pagina, op **Meervoudige Auth beheren**. Een nieuw browsertabblad wordt geopend.
![Klik op map](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6.  Zoek de gebruiker die u wilt inschakelen voor verificatie in twee stappen. Mogelijk moet u de weergave aan de bovenkant wijzigen. Zorg ervoor dat de status ervan is **uitgeschakeld.** 
 ![Gebruiker inschakelen](./media/multi-factor-authentication-get-started-cloud/enable1.png)
7.  Plaats een **controleren** in het vak naast de naam.
7.  Aan de rechterkant, klikt u op **inschakelen**.
![Gebruiker inschakelen](./media/multi-factor-authentication-get-started-cloud/user1.png)
8.  Klik op **meervoudige auth inschakelen**.
![Gebruiker inschakelen](./media/multi-factor-authentication-get-started-cloud/enable2.png)
9.  Zoals u ziet dat status van de gebruiker is gewijzigd van **uitgeschakeld** **ingeschakeld**.
![Gebruikers in staat stellen](./media/multi-factor-authentication-get-started-cloud/user.png)

Nadat u uw gebruikers hebt ingeschakeld, moet u deze via e-mail melden. De volgende keer dat ze wil aanmelden, wordt ze gevraagd om in te schrijven hun account voor verificatie in twee stappen. Als ze verificatie in twee stappen gebruiken, moet ze ook appwachtwoorden instellen om te voorkomen die niet-browser apps is vergrendeld.


## <a name="use-powershell-to-automate-turning-on-two-step-verification"></a>PowerShell gebruiken om te automatiseren verificatie in twee stappen inschakelen

Als u wilt wijzigen van de via [Azure AD PowerShell](../powershell-install-configure.md) [staat](multi-factor-authentication-whats-next.md) , kunt u het volgende.  U kunt wijzigen `$st.State` gelijk is aan een van de volgende statussen:

- Ingeschakeld
- Afgedwongen
- Uitgeschakeld  

> [AZURE.IMPORTANT]  We ontmoedigen ten opzichte van gebruikers rechtstreeks vanuit de stand uitschakelen verplaatsen naar de stand afgedwongen. Niet-browser gebaseerde apps werkt niet meer omdat de gebruiker heeft geen MFA registratie doorlopen en een [appwachtwoord](multi-factor-authentication-whats-next.md#app-passwords)verkregen. Als u niet op browser gebaseerde apps en app wachtwoorden, wordt u aangeraden dat u uit de status uitgeschakeld naar gaan ingeschakeld. Hiermee kan gebruikers hebt geregistreerd en hun appwachtwoorden verkrijgen. Daarna kunt u deze verplaatsen naar afgedwongen.

Via PowerShell, zou een optie voor het bulksgewijs gebruikers inschakelen. Op dit moment er is geen bulksgewijs inschakelen-functie in de portal van Azure en moet u elke gebruiker afzonderlijk selecteren. Dit is heel taak als u veel gebruikers hebt. Door te maken van een PowerShell-scripts gebruik van de volgende, kunt u een lijst met gebruikers doorlopen en deze inschakelen.

        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "\*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta

Hier volgt een voorbeeld:

    $users = "bsimon@contoso.com","jsmith@contoso.com","ljacobson@contoso.com"
    foreach ($user in $users)
    {
        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "\*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName $user -StrongAuthenticationRequirements $sta
    }


Zie voor meer informatie [gebruiker Staten in Azure meervoudige verificatie](multi-factor-authentication-get-started-user-states.md)

## <a name="next-steps"></a>Volgende stappen
U kunt nu dat u Azure meervoudige verificatie in de cloud instellen hebt, configureren en instellen van uw implementatie. Zie [Azure meervoudige verificatie configureren](multi-factor-authentication-whats-next.md) voor meer informatie.
