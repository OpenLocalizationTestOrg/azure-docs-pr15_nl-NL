<properties
    pageTitle="Cloud resources met Azure MFA en AD FS verzamelen"
    description="Dit is de pagina van de Azure meervoudige verificatie die wordt beschreven hoe u aan de slag met Azure MFA en AD FS in de cloud."
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

# <a name="securing-cloud-resources-with-azure-multi-factor-authentication-and-ad-fs"></a>Cloud resources met Azure meervoudige verificatie en AD FS beveiligen

Als uw organisatie wordt gekoppeld aan Azure Active Directory, gebruikt u Azure meervoudige verificatie of Active Directory Federation Services voor het beveiligen bronnen die worden gebruikt door Azure AD. Gebruik de volgende procedures voor het beveiligen Azure Active Directory-bronnen met Azure meervoudige verificatie of Active Directory Federation Services.

## <a name="secure-azure-ad-resources-using-ad-fs"></a>Azure AD-resources met AD FS verzamelen

Als u wilt beveiligen uw resource cloud, eerst een account voor gebruikers inschakelen en vervolgens een regel claims instellen. Volg deze procedure om de stappen uit te voeren:

1. Gebruik de stappen in [Turn-on meervoudige verificatie voor gebruikers](active-directory/multi-factor-authentication-get-started-cloud.md#turn-on-multi-factor-authentication-for-users) een account in te schakelen.
2. Start de AD FS-beheerconsole.
![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/adfs1.png)
3. Ga naar het **Te vertrouwen partijen vertrouwensrelaties** en met de rechtermuisknop op de te vertrouwen partijen vertrouwen. Selecteer **bewerken claimen regels...**
4. Klik op **regel... toevoegen**
5. In de vervolgkeuzelijst, selecteert u **verzenden van een aangepaste regel op Claims** en klik op **volgende**.
6. Voer een naam voor de regel claimen.
7. Klik onder aangepast regel: Voeg de volgende tekst toe:

    ```
    => issue(Type = "http://schemas.microsoft.com/claims/authnmethodsreferences", Value = "http://schemas.microsoft.com/claims/multipleauthn");
    ```

    Corresponderende claim:

    ```
    <saml:Attribute AttributeName="authnmethodsreferences" AttributeNamespace="http://schemas.microsoft.com/claims">
    <saml:AttributeValue>http://schemas.microsoft.com/claims/multipleauthn</saml:AttributeValue>
    </saml:Attribute>
    ```

8. Klik op **OK** en kies vervolgens **Voltooien**. Sluit de AD FS-beheerconsole.

Gebruikers kunnen Voltooi aanmelden met behulp van de on-premises implementatie-methode (zoals smartcard).

## <a name="trusted-ips-for-federated-users"></a>Vertrouwde IP-adressen voor federatieve gebruikers
Vertrouwde IP's kunnen beheerders omloopleiding verificatie in twee stappen voor specifieke IP-adressen of voor federatieve gebruikers aan wie een aanvragen die afkomstig zijn van binnen hun eigen intranet. De volgende secties wordt beschreven hoe Azure meervoudige verificatie vertrouwde IP-adressen configureren met federatieve gebruikers en verificatie in twee stappen omloopleiding, wanneer een nieuw vergaderverzoek afkomstig van binnen een intranet federatieve gebruikers is. Dit is bereikt door het configureren van AD FS als u wilt een Pass Through-query gebruiken of een binnenkomende claim-sjabloon met het bedrijfsnetwerk bevinden binnen claimtype filteren.

In dit voorbeeld gebruikmaakt van Office 365 voor onze te vertrouwen partijen vertrouwt.

### <a name="configure-the-ad-fs-claims-rules"></a>Configureer de AD FS claims regels

Het eerste wat u moet doen is voor het configureren van de AD FS-claims. We gaan maken twee claims regels, één voor het type van de claim binnen bedrijfsnetwerk en een extra eigenschap voor het behouden van onze gebruikers aangemeld.

1. Open de AD FS-Management.
2. Aan de linkerkant, selecteer **Partijen vertrouwensrelaties te vertrouwen**.
3. Klik met de rechtermuisknop op **Microsoft Office 365-identiteit Platform** en selecteer **Claimen regels bewerken...** 
 ![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip1.png)
4. Klik op regels voor uitgifte transformeren, op **regel toevoegen.** 
 ![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip2.png)
5. Klik op de transformeren claimen regel Wizard toevoegen, selecteer **doorheen of Filter een binnenkomende Claim** in de vervolgkeuzelijst en klik op **volgende**.
![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip3.png)
6. Geef een naam op de regel in het vak naast de naam van de regel claimen. Bijvoorbeeld: InsideCorpNet.
7. In de vervolgkeuzelijst naast inkomende claimen type, selecteert u **Binnen bedrijfsnetwerk**.
![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip4.png)
8. Klik op **Voltooien**.
9. Klik op uitgifte transformeren regels, klikt u op **Regel toevoegen**.
10. De transformeren claimen regel Wizard toevoegen, selecteert u **verzenden Claims een aangepaste regel** in de vervolgkeuzelijst en klik op **volgende**.
11. In het vak onder de naam van de regel Claim: Voer *Behouden gebruikers die zijn aangemeld In*.
12. Voer in het vak aangepaste regel:

        c:[Type == "http://schemas.microsoft.com/2014/03/psso"]
            => issue(claim = c);
![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip5.png)
13. Klik op **Voltooien**.
14. Klik op **toepassen**.
15. Klik op **Ok**.
16. Sluit de AD FS-Management.



### <a name="configure-azure-multi-factor-authentication-trusted-ips-with-federated-users"></a>Configureren van Azure meervoudige verificatie vertrouwde IP-adressen met federatieve gebruikers
Nu dat de claims uitgevoerd worden, kunnen we vertrouwde IP-adressen configureren.

1. Meld u aan bij de [portal van Azure klassieke](https://manage.windowsazure.com).
2. Aan de linkerkant, klikt u op **Active Directory**.
3. Selecteer onder Directory, de map waar u wilt instellen vertrouwde IP-adressen.
4. Klik op de map die u hebt geselecteerd, klikt u op **configureren**.
5. Klik op **service-instellingen beheren**in de sectie meervoudige verificatie.
6. Selecteer op de pagina Service-instellingen onder vertrouwde IP-adressen, **meerdere-factor-verificatie voor aanvragen van federatieve gebruikers op mijn intranet overgeslagen.** 
 ![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip6.png)
7. Klik op **Opslaan**.
8. Nadat de updates zijn toegepast, klikt u op **sluiten**.


Dat is alles. Federatieve Office 365-gebruikers mag op dit moment alleen wilt MFA gebruiken wanneer u een claim afkomstig zijn van buiten het bedrijfsintranet hebben.
