<properties
    pageTitle="Een Windows 10-apparaat met Azure AD instellen Klik op instellingen | Microsoft Azure"
    description="Dit artikel wordt uitgelegd hoe gebruikers kunnen lid worden naar Azure AD via het menu instellingen."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="set-up-a-windows-10-device-with-azure-ad-from-settings"></a>Een Windows 10-apparaat met Azure AD instellen Klik op instellingen
Als u Windows 7 of Windows 8 al gebruikt en uw computer of apparaat is bijgewerkt naar Windows 10, kunt u deelnemen aan met Azure Active Directory (Azure AD) via het menu instellingen.

## <a name="to-join-to-azure-ad-from-the-settings-menu"></a>Om te koppelen aan Azure AD via het menu instellingen


1. Klik in het menu **Start** op de charm **Instellingen** .
2. **Instellingen**en selecteer **systeem**->**over**->**Azure AD deelnemen**.
<center>
![Deelnemen aan Azure AD via het menu instellingen](./media/active-directory-azureadjoin/active-directory-azureadjoin-settings.png)</center>

3. Klik op **Doorgaan** in het berichtvenster Azure AD deelnemen.
<center>
![Berichtvenster join Azure AD](./media/active-directory-azureadjoin/active-directory-azureadjoin-message.png)</center>
4. Geef uw aanmeldingsgegevens. Deze aanmeldervaring bevat de stappen die vereist voor volledige verificatie zijn. Als u deel van een federatieve tenant uitmaken, krijgt uw beheerder u de Federatie-ervaring, die wordt gehost door uw organisatie.
<center>
![Aanmeldingsgegevens bieden](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)</center>
5. Als uw organisatie heeft Azure meervoudige verificatie geconfigureerd voor het deelnemen aan aan Azure AD, vindt u de tweede factor voordat u verdergaat.
6. Klik op **accepteren** in het scherm **Dit apparaat te beheren** .
7. Ziet u het bericht "uw apparaat is nu gekoppeld aan uw organisatie in Azure AD".


## <a name="additional-information"></a>Aanvullende informatie
* [Meer informatie over het gebruik scenario's voor het deelnemen aan Azure AD](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Een domein behoren apparaten verbinden met Azure AD voor Windows 10 ervaringen](active-directory-azureadjoin-devices-group-policy.md)
* [Deelnemen aan Azure AD instellen](active-directory-azureadjoin-setup.md)
* [Identiteiten zonder wachtwoorden via Microsoft Passport verifiëren](active-directory-azureadjoin-passport.md)
