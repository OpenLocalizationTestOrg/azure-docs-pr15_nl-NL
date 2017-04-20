<properties
    pageTitle="Krijg Azure meervoudige Auth Provider gestart | Microsoft Azure"
    description="Informatie over het maken van een Azure meervoudige Auth-Provider."
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
    ms.date="10/14/2016"
    ms.author="kgremban"/>



# <a name="getting-started-with-an-azure-multi-factor-auth-provider"></a>Aan de slag met een Azure meervoudige Auth-Provider
Verificatie in twee stappen is standaard beschikbaar voor globale beheerders met Azure Active Directory en de Office 365-gebruikers. Echter aanschaffen als u wilt profiteren van [Geavanceerde functies](multi-factor-authentication-whats-next.md) vervolgens u moet de volledige versie van Azure meervoudige verificatie MFA ().

> [AZURE.NOTE]  Een Provider van Azure meervoudige verificatie wordt gebruikt om te profiteren van functies van de volledige versie van Azure MFA. Het is voor gebruikers die **geen licentie via Azure MFA, Azure AD Premium, of EMS hebben**.  Azure MFA, Azure AD Premium en EMS bevatten de volledige versie van Azure MFA al dan niet standaard.  Als u licenties hebt, klikt hoeft u niet een Azure meervoudige Auth-Provider.

Een provider Azure meervoudige verificatie is vereist voor het downloaden van de SDK.

> [AZURE.IMPORTANT]  Als u wilt downloaden van de SDK maken een Provider van Azure meervoudige Auth zelfs als er Azure MFA, AAD Premium of EMS licenties.  Als u een Provider van Azure meervoudige Auth voor dit doel maken en licenties al hebt, moet u de Provider maken met het model **Per gebruiker is ingeschakeld** . Koppeling vervolgens de Provider naar de map waarin de licenties Azure MFA, Azure AD Premium of EMS.  Dit zorgt ervoor dat u zijn alleen gefactureerd als er meer unieke gebruikers met de SDK dan het aantal licenties dat u eigenaar bent.


## <a name="to-create-a-multi-factor-auth-provider"></a>Om een meervoudige Auth-Provider te maken

Gebruik de volgende stappen uit om een Provider van Azure meervoudige Auth te maken.

1. Meld u als beheerder aan bij de [portal van Azure klassieke](https://manage.windowsazure.com) .
2. Selecteer aan de linkerkant, **Active Directory**.
3. Selecteer op de pagina Active Directory aan de bovenkant, **Meervoudige verificatieproviders**.
![Maken van een MFA-Provider](./media/multi-factor-authentication-get-started-auth-provider/authprovider1.png)
4. Klik onderaan op **Nieuw**.
![Maken van een MFA-Provider](./media/multi-factor-authentication-get-started-auth-provider/authprovider2.png)
5. Selecteer onder App-Services, **Meervoudige Auth Provider**
![maken van een MFA-Provider](./media/multi-factor-authentication-get-started-auth-provider/authprovider3.png)
6. Selecteer **snel tabellen maken**.
![Maken van een MFA-Provider](./media/multi-factor-authentication-get-started-auth-provider/authprovider4.png)
5. Vul de volgende velden en selecteer **maken**.
    1. **Naam** : de naam van de meervoudige Auth-Provider.
    2. **Gebruiksmodel** , of u wilt de individuele gebruikers in staat stellen of betalen per verificatie. Kies een van de twee opties:
        - Per verificatie – aanschaffen model dat per verificatie kosten. Meestal gebruikt voor scenario's met Azure meervoudige verificatie in een toepassing voor consumenten-omlaag.
        - Per gebruiker ingeschakeld – ingeschakeld aanschaffen model dat per kosten gebruiker. Meestal gebruikt voor toegang van werknemer tot toepassingen, zoals Office 365. Kies deze optie als er enkele gebruikers die al een licentie voor Azure MFA.
    2. **Directory** – de Azure Active Directory-tenant dat de meervoudige verificatie-Provider is gekoppeld aan. Let op van de volgende opties:
        - U hoeft niet een Azure AD-directory om een meervoudige Auth-Provider te maken. Laat het vak leeg als u alleen Azure meervoudige verificatieserver of SDK te gebruiken.
        - De meervoudige Auth Provider moeten worden gekoppeld aan een Azure AD-directory om te profiteren van de geavanceerde functies.
        - Azure AD Connect, AAD-synchronisatie of DirSync zijn alleen vereist als u uw on-premises Active Directory-omgeving synchroniseert met een Azure AD-directory.  Als u alleen een Azure AD-map die niet is gesynchroniseerd gebruikt, is dit niet vereist.
![Maken van een MFA-Provider](./media/multi-factor-authentication-get-started-auth-provider/authprovider5.png)
5. Wanneer u op maken, de meervoudige verificatie-Provider is gemaakt en u ziet een bericht ontvangt: **aspx-Provider voor meervoudige verificatie**. Klik op **Ok**.
![Maken van een MFA-Provider](./media/multi-factor-authentication-get-started-auth-provider/authprovider6.png)
