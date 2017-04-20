<properties
    pageTitle="Werken met Claims hoogte Apps in toepassingsproxy"
    description="Hoe kom ik aan de slag met Azure AD-toepassingsproxy behandelt."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/22/2016"
    ms.author="kgremban"/>



# <a name="working-with-claims-aware-apps-in-application-proxy"></a>Werken met claims hoogte apps in toepassingsproxy

Vorderingen op de hoogte apps uitvoeren een omleiding naar de beveiliging Token Service (STS), die op zijn beurt vraagt om referenties van de gebruiker voor een token voordat u de gebruiker omleiden naar de toepassing. Als u wilt inschakelen toepassingsproxy voor gebruik met deze omleidingen, moeten de volgende stappen uit te gaan.

## <a name="prerequisites"></a>Vereisten voor
Zorg ervoor dat de STS de hoogte claims-app wordt omgeleid naar buiten uw on-premises netwerk beschikbaar is voordat het uitvoeren van deze procedure.

## <a name="azure-classic-portal-configuration"></a>Configuratie van Azure klassieke portal

1. Uw toepassing volgens de instructies die worden beschreven in [de toepassingen publiceren met toepassingsproxy](active-directory-application-proxy-publish.md)publiceren.
2. In de lijst met toepassingen, selecteert u de hoogte claims-app en klik op **configureren**.
3. Als u ervoor hebt gekozen **indirecte** als **Vooraf-verificatie-methode**, zorg er dan voor dat **HTTPS** selecteert als het kleurenschema van uw **Externe URL** .
4. Als u ervoor hebt gekozen **Azure Active Directory** als **Vooraf-verificatie-methode**, selecteert u **geen** als uw **Interne verificatiemethode**.


## <a name="adfs-configuration"></a>ADFS-configuratie

1. Open de ADFS-Management.
2. Ga naar het **Partijen vertrouwensrelaties te vertrouwen**, klik met de rechtermuisknop op de app die u wilt publiceren met toepassingsproxy en kies **Eigenschappen**.  
  ![Gebruikmakende partij vertrouwensrelaties Klik met de rechtermuisknop op de naam van de app - screentshot](./media/active-directory-application-proxy-claims-aware-apps/appproxyrelyingpartytrust.png)  
3. Selecteer op het tabblad **eindpunten** onder **eindpunt type** **WS-Federatie**.
4. Voer onder **Vertrouwde URL** de URL die u hebt ingevoerd in de toepassingsproxy onder **Externe URL** en klik op **OK**.  
  ![Een eindpunt - toevoegen beginwaarde voor vertrouwde URL - schermafbeelding](./media/active-directory-application-proxy-claims-aware-apps/appproxyendpointtrustedurl.png)  

## <a name="see-also"></a>Zie ook

- [Toepassingen met toepassingsproxy publiceren](active-directory-application-proxy-publish.md)
- [Inschakelen voor eenmalige aanmelding](active-directory-application-proxy-sso-using-kcd.md)
- [Problemen die u met toepassingsproxy ondervindt](active-directory-application-proxy-troubleshoot.md)
- [Native client apps om te communiceren met proxy-toepassingen inschakelen](active-directory-application-proxy-native-client.md)

Voor het laatste nieuws en updates, raadpleegt u de [toepassingsproxy-blog](http://blogs.technet.com/b/applicationproxyblog/)
