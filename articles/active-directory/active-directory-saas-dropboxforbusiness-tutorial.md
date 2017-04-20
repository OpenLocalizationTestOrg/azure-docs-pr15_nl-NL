<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Dropbox voor bedrijven | Microsoft Azure" 
    description="Meer informatie over het gebruik van Dropbox voor bedrijven met Azure Active Directory om te kunnen eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-dropbox-for-business"></a>Zelfstudie: Azure Active Directory-integratie met Dropbox voor bedrijven
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en Dropbox voor bedrijven.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een test-tenant in Dropbox voor bedrijven
  
Na het voltooien van deze zelfstudie, bedrijf de Azure AD-gebruikers die u hebt toegewezen aan Dropbox voor bedrijven één Meld u aan bij de toepassing op uw Dropbox voor bedrijven kunnen (serviceprovider gestart Meld u aan op)-site of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  Inschakelen van de toepassingsintegratie van de voor Dropbox voor bedrijven
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769508.png "Scenario")



##<a name="enabling-the-application-integration-for-dropbox-for-business"></a>Inschakelen van de toepassingsintegratie van de voor Dropbox voor bedrijven
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor Dropbox voor bedrijven.

###<a name="to-enable-the-application-integration-for-dropbox-for-business-perform-the-following-steps"></a>Als u wilt dat de toepassingsintegratie van de voor Dropbox voor bedrijven, kunt u de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **Dropbox voor bedrijven**.

    ![Galerie van toepassing] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC701010.png "Galerie van toepassing")

7.  Selecteer **Dropbox voor bedrijven**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Dropbox voor bedrijven] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC701011.png "Dropbox voor bedrijven")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers om te verifiëren in Dropbox voor bedrijven met hun account in Azure AD met Federatie op basis van het SAML-protocol.

Als onderdeel van deze procedure bent u vereist een base 64 gecodeerde certificaat uploaden naar uw Dropbox voor bedrijven-tenant. Als u niet bekend met deze procedure bent, raadpleegt u [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de Integratiepagina van **Dropbox voor bedrijven** -toepassing op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749323.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Dropbox voor bedrijven** **Eenmalige aanmelding Microsoft Azure AD**selecteren en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749327.png "Eenmalige aanmelding configureren")

3.  Klik op de pagina **App-URL configureren** , moet u de volgende stappen uitvoeren:

    een. Aanmelding voor toegang tot uw Dropbox voor bedrijven-tenant. 

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769509.png "Eenmalige aanmelding configureren")

    b. Klik in het navigatiedeelvenster aan de linkerkant op **Beheerconsole**. 

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769510.png "Eenmalige aanmelding configureren")

    c. Klik op **verificatie via** in het linkernavigatiedeelvenster op de **Beheerconsole**. 

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769511.png "Eenmalige aanmelding configureren")

    d. In de sectie **eenmalige aanmelding** , selecteert u **eenmalige aanmelding inschakelen**en klik vervolgens op **meer** om uit te vouwen in deze sectie.  

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769512.png "Eenmalige aanmelding configureren")

    e. Kopieer de URL naast **gebruikers zich kunnen aanmelden door in te voeren hun e-mailadres of ze rechtstreeks naar kunnen gaan**. 

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769513.png "Eenmalige aanmelding configureren")

    f. Plak de URL in het tekstvak URL **DropBox voor bedrijven aanmelden** op de Azure klassieke portal. 

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769514.png "Eenmalige aanmelding configureren")  



4. Klik op de pagina **configureren eenmalige aanmelding bij Dropbox voor bedrijven** op **certificaat downloaden**en sla het certificaatbestand op uw computer.  

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769515.png "Eenmalige aanmelding configureren")


5. Klik op uw Dropbox voor bedrijven-tenant, in de sectie **eenmalige aanmelding** van de pagina **verificatie** , moet u de volgende stappen uitvoeren: 

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769516.png "Eenmalige aanmelding configureren")

    een. Klik op **vereist**.

    b. De waarde **aanmeldingsproblemen pagina URL** kopiëren en plak deze in het tekstvak **URL aanmelden** in de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij Dropbox voor bedrijven** .


    c. Een **basis-64-codering** -bestand uit uw gedownloade certificaat maken. 

    > [AZURE.TIP] Zie [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)voor meer informatie.


    d. Klik op de knop **'Certificaat als kiezen'** en bladert u naar uw **base 64 codering certificaatbestand**.


    e. Klik op de knop **'Wijzigingen opslaan'** om de configuratie op uw DropBox voor bedrijven-tenant te voltooien.


6. In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten. 

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749329.png "Eenmalige aanmelding configureren")



##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de gebruiker de inrichting van Active Directory-gebruikersaccounts in Dropbox voor bedrijven.


### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Als u wilt configureren de inrichting van de gebruiker, moet u de volgende stappen uitvoeren:

1. Klik op van de **inrichting van de gebruiker configureren** om het dialoogvenster **Gebruiker inrichting configureren** in de klassieke Azure Portal, klik op de Integratiepagina van **Dropbox voor bedrijven** -toepassing.

2. Klik op de inschakelen gebruiker is geïnstalleerd in DropBox voor bedrijven, klikt u op inschakelen gebruiker inrichting als u wilt de Sign in to Dropbox wilt koppelen met Azure AD-dialoogvenster openen.  

    ![De inrichting van gebruiker] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769517.png "De inrichting van gebruiker")

3. Klik in het dialoogvenster **aanmelden bij Dropbox wilt koppelen met Azure AD** Meld u aan bij uw Dropbox voor bedrijven-tenant. 

    ![De inrichting van gebruiker] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769518.png "De inrichting van gebruiker")



4. Klik op **toestaan** als u wilt verlenen Azure AD voor toegang tot Dropbox. 

    ![De inrichting van gebruiker] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769519.png "De inrichting van gebruiker")



5. Klik op de knop **voltooid** om de configuratie.  

    ![De inrichting van gebruiker] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769520.png "De inrichting van gebruiker")




##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-dropbox-for-business-perform-the-following-steps"></a>Als u wilt toewijzen aan gebruikers in Dropbox voor bedrijven, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina van de integratie in de **Dropbox voor bedrijven **-toepassing, klikt u op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769521.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC767830.png "Ja")
  


U moet nu 10 minuten wachten en controleer of dat het account is gesynchroniseerd met Dropbox voor bedrijven.

Als eerste stap verificatie, kunt u de inrichten status controleren door te klikken op **Dashboard** op de pagina **Dropbox voor bedrijven** -toepassing-integratie in de klassieke Azure-Portal.

![Toewijzen aan gebruikers] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769522.png "Toewijzen aan gebruikers")


De inrichting van cyclus van een met succes zijn gebruiker wordt aangegeven door een gerelateerde status.

![Toewijzen aan gebruikers] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769523.png "Toewijzen aan gebruikers")


Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access.
Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.




## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)