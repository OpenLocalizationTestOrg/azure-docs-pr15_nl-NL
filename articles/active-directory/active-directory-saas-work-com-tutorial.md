<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Work.com | Microsoft Azure" 
    description="Informatie over het gebruiken van Work.com met Azure Active Directory om te schakelen eenmalige aanmelding, geautomatiseerde inrichting en meer!." 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-workcom"></a>Zelfstudie: Azure Active Directory-integratie met Work.com
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en Work.com.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een Work.com eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, toewijzen de AAD-gebruikers aan wie u hebt toegang worden Work.com kunnen eenmalige aanmelding in de toepassing op uw bedrijfssite Work.com (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor Work.com inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-work-com-tutorial/IC794105.png "Scenario")

##<a name="enabling-the-application-integration-for-workcom"></a>De toepassingsintegratie van de voor Work.com inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor Work.com.

###<a name="to-enable-the-application-integration-for-workcom-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor Work.com door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-work-com-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-work-com-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-work-com-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-work-com-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **Work.com**.

    ![Galerie van toepassing] (./media/active-directory-saas-work-com-tutorial/IC794106.png "Galerie van toepassing")

7.  Selecteer **Work.com**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Work.com] (./media/active-directory-saas-work-com-tutorial/IC794107.png "Work.com")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij Work.com met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
U moet een certificaat uploaden naar Work.com.com als onderdeel van deze procedure.

>[AZURE.NOTE] Als u wilt configureren eenmalige aanmelding, moet u nog een aangepaste domeinnaam van Work.com instellen. U moet ten minste een domeinnaam definiëren, uw domeinnaam testen en het dashboard implementeren naar uw hele organisatie.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Meld u aan bij uw tenant Work.com als beheerder.

2.  Ga naar **Instellingen**.

    ![Setup] (./media/active-directory-saas-work-com-tutorial/IC794108.png "Setup")

3.  Klik op het linker navigatiedeelvenster, in de sectie **beheren** op **Domain Management** om uit te vouwen van de gerelateerde sectie en klik vervolgens op **Mijn domein** om de pagina **Mijn domein** te openen. 

    ![Mijn domein] (./media/active-directory-saas-work-com-tutorial/IC767825.png "Mijn domein")

4.  Zorg ervoor dat het in "**Stap 4 geïmplementeerd in gebruikers**" is om te bevestigen dat uw domein instellen juist is, en uw "**Instellingen voor mijn domein**" te controleren.

    ![Domein geïmplementeerd in gebruiker] (./media/active-directory-saas-work-com-tutorial/IC784377.png "Domein geïmplementeerd in gebruiker")

5.  In een browservenster verschillende web en meld u aan bij uw Azure klassieke portal.

6.  Klik op de pagina **Work.com **toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-work-com-tutorial/IC794109.png "Eenmalige aanmelding configureren")

7.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Work.com** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-work-com-tutorial/IC794110.png "Eenmalige aanmelding configureren")

8.  Typ op de pagina **App-URL configureren** , in het tekstvak **Work.com aanmelding op URL** de URL die wordt gebruikt door uw gebruikers aanmelden bij uw Work.com-toepassing (bijvoorbeeld: " *http://company.my.salesforce.com*"), en klik op **volgende**: 

    ![URL van App configureren] (./media/active-directory-saas-work-com-tutorial/IC794111.png "URL van App configureren")

9.  Klik op de pagina **configureren eenmalige aanmelding bij Work.com** op **Download-certificaat**te downloaden van het certificaat, en sla het certificaatbestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-work-com-tutorial/IC794112.png "Eenmalige aanmelding configureren")

10. Meld u aan bij uw tenant Work.com.

11. Ga naar **Instellingen**.

    ![Setup] (./media/active-directory-saas-work-com-tutorial/IC794108.png "Setup")

12. Vouw het menu **Besturingselementen voor beveiliging** , en klik op **Instellingen voor eenmalige aanmelding**.

    ![Instellingen voor eenmalige aanmelding] (./media/active-directory-saas-work-com-tutorial/IC794113.png "Instellingen voor eenmalige aanmelding")

13. Klik op de pagina **Instellingen voor eenmalige aanmelding** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Op SAML ingeschakeld] (./media/active-directory-saas-work-com-tutorial/IC781026.png "Op SAML ingeschakeld")

    1.  Selecteer **SAML ingeschakeld**.
    2.  Klik op **Nieuw**.

14. In de sectie **SAML eenmalige aanmelding instellingen voor** de volgende stappen uitvoeren:

    ![Op SAML eenmalige aanmelding-instelling] (./media/active-directory-saas-work-com-tutorial/IC794114.png "Op SAML eenmalige aanmelding-instelling")

    1.  Typ een naam voor uw configuratie in **het tekstvak** .  

        >[AZURE.NOTE] Een waarde op te geven **in te voeren** , het tekstvak **API** automatisch ingevuld.

    2.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Work.com** de waarde **Uitgever URL** kopiëren en plak deze in het tekstvak **uitgever** .
    3.  Klik op **Bladeren**om het gedownloade certificaat.
    4.  Typ in het tekstvak **Entiteit-Id** **https://salesforce-work.com**.
    5.  Als het **SAML identiteitstype**, selecteer **bevestiging bevat de ID met Federatie uit het gebruikersobject**.
    6.  Als **SAML identiteit locatie**, selecteer **identiteit wordt in het element NameIdentfier van de instructie onderwerp**.
    7.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Work.com** de waarde van de **Externe aanmeldings-URL** kopiëren en plak deze in het tekstvak **Identiteit Provider aanmeldings-URL** .
    8.  Kopieer de waarde van de **URL van externe Meld u af** en plak deze in het tekstvak **Identiteit Provider afmelden URL** in de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij Work.com** .
    9.  Als de **Service Provider gestart aanvragen binden**en selecteert u **HTTP-bericht**.
    10. Klik op **Opslaan**.

15. Uw Work.com klassieke Portal en klik op het linker navigatiedeelvenster **Domain Management** om uit te vouwen van de sectie gerelateerde op en klik vervolgens op **Mijn domein** om de pagina **Mijn domein** te openen. 

    ![Mijn domein] (./media/active-directory-saas-work-com-tutorial/IC794115.png "Mijn domein")

16. Klik op de pagina **Mijn domein** in de sectie **Login pagina huisstijl** op **bewerken**.

    ![Aanmeldingspagina huisstijl] (./media/active-directory-saas-work-com-tutorial/IC767826.png "Aanmeldingspagina huisstijl")

17. Klik op de pagina **Login pagina huisstijl** in de sectie **Verificatieservice** wordt de naam van uw **Instellingen voor eenmalige aanmelding SAML** weergegeven. Selecteer deze en klik vervolgens op **Opslaan**.

    ![Aanmeldingspagina huisstijl] (./media/active-directory-saas-work-com-tutorial/IC784366.png "Aanmeldingspagina huisstijl")

18. In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-work-com-tutorial/IC794116.png "Eenmalige aanmelding configureren")

##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Azure Active Directory-gebruikers moeten kunnen aanmelden, als ze worden ingericht naar Work.com.  
Bij Work.com, met de inrichting van is een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Als u wilt configureren de inrichting van de gebruiker, moet u de volgende stappen uitvoeren:

1.  Meld u bij uw bedrijfssite Work.com aan als beheerder.

2.  Ga naar **Instellingen**.

    ![Setup] (./media/active-directory-saas-work-com-tutorial/IC794108.png "Setup")

3.  Ga naar **gebruikers beheren \> gebruikers**.

    ![Gebruikers beheren] (./media/active-directory-saas-work-com-tutorial/IC784369.png "Gebruikers beheren")

4.  Klik op **nieuwe gebruiker**.

    ![Alle gebruikers] (./media/active-directory-saas-work-com-tutorial/IC794117.png "Alle gebruikers")

5.  In de sectie gebruiker bewerken, moet u de volgende stappen uitvoeren:

    ![Gebruiker bewerken] (./media/active-directory-saas-work-com-tutorial/IC794118.png "Gebruiker bewerken")

    1.  Typ de **Achternaam**, **Alias**, **E-mail**, **gebruikersnaam** en **bijnaam** kenmerken van een geldige Azure Active Directory-account inrichten in de gerelateerde tekstvakken gewenste.
    2.  Selecteer de **rol**, **gebruikerslicentie** en **profiel**.
    3.  Klik op **Opslaan**.  

        >[AZURE.NOTE] De eigenaar van een account Azure Active Directory krijgt een e-mailbericht met inbegrip van een koppeling om te bevestigen van het account voordat deze geactiveerd wordt.

>[AZURE.NOTE] U kunt een andere Work.com gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door Work.com aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-workcom-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan Work.com, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina Work.com toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-work-com-tutorial/IC794119.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-work-com-tutorial/IC767830.png "Ja")
  
U moet nu 10 minuten wachten en controleer of dat het account is gesynchroniseerd met Work.com.com.
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.