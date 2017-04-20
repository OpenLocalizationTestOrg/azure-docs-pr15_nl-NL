<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Jobscience | Microsoft Azure" 
    description="Meer informatie over het gebruiken van Jobscience met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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
    ms.date="09/29/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-jobscience"></a>Zelfstudie: Azure Active Directory-integratie met Jobscience
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en Jobscience.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een abonnement Jobscience eenmalige aanmelding ingeschakeld
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan Jobscience één Meld u aan bij de toepassing op uw bedrijfssite Jobscience (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor Jobscience inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-jobscience-tutorial/IC784341.png "Scenario")
##<a name="enabling-the-application-integration-for-jobscience"></a>De toepassingsintegratie van de voor Jobscience inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor Jobscience.

###<a name="to-enable-the-application-integration-for-jobscience-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor Jobscience door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-jobscience-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-jobscience-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-jobscience-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-jobscience-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **jobscience**.

    ![Galerie van toepassing] (./media/active-directory-saas-jobscience-tutorial/IC784342.png "Galerie van toepassing")

7.  Selecteer **Jobscience**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Jobscience] (./media/active-directory-saas-jobscience-tutorial/IC784357.png "Jobscience")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij Jobscience met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Eenmalige aanmelding voor Jobscience configureren, moet u een vingerafdrukwaarde ophalen uit een certificaat.  
Als u niet bekend met deze procedure bent, raadpleegt u [het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Meld u als beheerder aan bij uw bedrijfssite Jobscience.

2.  Ga naar **Instellingen**.

    ![Setup] (./media/active-directory-saas-jobscience-tutorial/IC784358.png "Setup")

3.  Klik op het linker navigatiedeelvenster, in de sectie **beheren** op **Domain Management** om uit te vouwen van de gerelateerde sectie en klik vervolgens op **Mijn domein** om de pagina **Mijn domein** te openen. 

    ![Mijn domein] (./media/active-directory-saas-jobscience-tutorial/IC767825.png "Mijn domein")

4.  Zorg ervoor dat het in "**Stap 4 geïmplementeerd in gebruikers**" is om te bevestigen dat uw domein setup juist is, en uw "**Instellingen voor mijn domein**" te controleren.

    ![Domein geïmplementeerd in gebruiker] (./media/active-directory-saas-jobscience-tutorial/IC784377.png "Domein geïmplementeerd in gebruiker")

5.  In een browservenster verschillende web en meld u aan bij uw Azure klassieke portal.

6.  Klik op de pagina **Jobscience** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-jobscience-tutorial/IC784360.png "Eenmalige aanmelding configureren")

7.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Jobscience** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-jobscience-tutorial/IC784361.png "Eenmalige aanmelding configureren")

8.  Typ de URL voor het gebruik van de volgende patroon "*http://company.my.salesforce.com*" op de pagina **App-URL configureren** in het tekstvak **Jobscience Sign In URL** en klik vervolgens op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-jobscience-tutorial/IC784362.png "URL van App configureren")

9.  Klik op de pagina **configureren eenmalige aanmelding bij Jobscience** op **certificaat downloaden**als u wilt downloaden van het certificaat, en sla het certificaatbestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-jobscience-tutorial/IC784363.png "Eenmalige aanmelding configureren")

10. Klik op de site Jobscience bedrijf op **Besturingselementen voor beveiliging**en klik vervolgens op **Instellingen voor eenmalige aanmelding**.

    ![Besturingselementen voor beveiliging] (./media/active-directory-saas-jobscience-tutorial/IC784364.png "Besturingselementen voor beveiliging")

11. In de sectie **Instellingen voor eenmalige aanmelding** , moet u de volgende stappen uitvoeren:

    ![Instellingen voor eenmalige aanmelding] (./media/active-directory-saas-jobscience-tutorial/IC781026.png "Instellingen voor eenmalige aanmelding")

    1.  Selecteer **SAML ingeschakeld**.
    2.  Klik op **Nieuw**.

12. Klik in het dialoogvenster **SAML eenmalige aanmelding instelling bewerken** , moet u de volgende stappen uitvoeren:

    ![Op SAML eenmalige aanmelding-instelling] (./media/active-directory-saas-jobscience-tutorial/IC784365.png "Op SAML eenmalige aanmelding-instelling")

    1.  Typ een naam voor uw configuratie in **het tekstvak** .
    2.  In de klassieke Azure portal, klik op de dialoog pagina **configureren eenmalige aanmelding bij Jobscience** de waarde **Uitgever URL** kopiëren en plak deze in het tekstvak **uitgever**
    3.  Typ in het tekstvak **Entiteit-Id** **https://salesforce-jobscience.com**
    4.  Klik op **Bladeren** om te uploaden van uw Azure AD-certificaat.
    5.  Als het **SAML identiteitstype**, selecteer **bevestiging bevat de ID met Federatie uit het gebruikersobject**.
    6.  Als **SAML identiteit locatie**, selecteer **identiteit wordt in het element NameIdentfier van de instructie onderwerp**.
    7.  In de klassieke Azure portal, klik op de dialoog pagina **configureren eenmalige aanmelding bij Jobscience** de waarde van de **Externe aanmeldings-URL** kopiëren en plak deze in het tekstvak **Identiteit Provider aanmeldings-URL**
    8.  In de klassieke Azure portal, klik op de dialoog pagina **configureren eenmalige aanmelding bij Jobscience** de waarde van de **URL van externe afmelden** kopiëren en plak deze in het tekstvak **Identiteit Provider afmelden URL**
    9.  Klik op **Opslaan**.

13. Klik op het linker navigatiedeelvenster, in de sectie **beheren** op **Domain Management** om uit te vouwen van de gerelateerde sectie en klik vervolgens op **Mijn domein** om de pagina **Mijn domein** te openen. 

    ![Mijn domein] (./media/active-directory-saas-jobscience-tutorial/IC767825.png "Mijn domein")

14. Klik op de pagina **Mijn domein** in de sectie **Login pagina huisstijl** op **bewerken**.

    ![Aanmeldingspagina huisstijl] (./media/active-directory-saas-jobscience-tutorial/IC767826.png "Aanmeldingspagina huisstijl")

15. Klik op de pagina **Login pagina huisstijl** in de sectie **Verificatieservice** wordt de naam van uw **Instellingen voor eenmalige aanmelding SAML** weergegeven. Selecteer deze en klik vervolgens op **Opslaan**.

    ![Aanmeldingspagina huisstijl] (./media/active-directory-saas-jobscience-tutorial/IC784366.png "Aanmeldingspagina huisstijl")

16. In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-jobscience-tutorial/IC784367.png "Eenmalige aanmelding configureren")
  
Als u de SP klikt u op gestarte eenmalige aanmelding aanmeldings-URL van de **Instellingen voor eenmalige aanmelding** in de sectie **Beveiliging besturingselementen** menu.

![Besturingselementen voor beveiliging] (./media/active-directory-saas-jobscience-tutorial/IC784368.png "Besturingselementen voor beveiliging")
  
Klik op het SSO-profiel dat u hebt gemaakt in de stap hierboven.  
Deze pagina bevat de eenmalige op URL voor uw bedrijf (bijvoorbeeld *https://companyname.my.salesforce.com?so=companyid*).
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Als u wilt dat gebruikers Azure AD Meld u aan bij Jobscience, moeten ze worden ingericht in Jobscience.  
Bij Jobscience, met de inrichting van is een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Als u wilt configureren de inrichting van de gebruiker, moet u de volgende stappen uitvoeren:

1.  Meld u als beheerder aan bij uw bedrijfssite **Jobscience** .

2.  Ga naar configureren

    ![Setup] (./media/active-directory-saas-jobscience-tutorial/IC784358.png "Setup")

3.  Ga naar **gebruikers beheren \> gebruikers**.

    ![Gebruikers] (./media/active-directory-saas-jobscience-tutorial/IC784369.png "Gebruikers")

4.  Klik op **nieuwe gebruiker**.

    ![Alle gebruikers] (./media/active-directory-saas-jobscience-tutorial/IC784370.png "Alle gebruikers")

5.  Klik in het dialoogvenster **Gebruiker bewerken** , moet u de volgende stappen uitvoeren:

    ![Gebruiker bewerken] (./media/active-directory-saas-jobscience-tutorial/IC784371.png "Gebruiker bewerken")

    1.  Typ de voornaam, achternaam, alias, e-mail, de naam en bijnaam gebruikerseigenschappen van de Azure AD-gebruiker die u inrichten in de gerelateerde tekstvakken wilt.
    2.  Klik op **Opslaan**.

    >[AZURE.NOTE] De eigenaar van een account Azure AD krijgt een e-mailbericht met een koppeling om te bevestigen van het account voordat deze wordt geactiveerd.

>[AZURE.NOTE] U kunt een andere Jobscience gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door Jobscience aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-jobscience-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan Jobscience, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **Jobscience **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-jobscience-tutorial/IC784372.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-jobscience-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.