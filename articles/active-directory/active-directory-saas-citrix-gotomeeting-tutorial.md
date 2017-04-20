<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Citrix GoToMeeting | Microsoft Azure" 
    description="Informatie over het gebruiken van Citrix GoToMeeting met Azure Active Directory om te schakelen eenmalige aanmelding, geautomatiseerde inrichting en meer!." 
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

#<a name="tutorial-azure-active-directory-integration-with-citrix-gotomeeting"></a>Zelfstudie: Azure Active Directory-integratie met Citrix GoToMeeting  
Van toepassing op: Azure

>[AZURE.TIP]Voor feedback, klikt u op [hier](http://go.microsoft.com/fwlink/?LinkId=522412).

Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en Citrix GoToMeeting. Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een tenant in Citrix GoToMeeting

Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor Citrix GoToMeeting inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Configuratie] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768996.png "Configuratie")



##<a name="enabling-the-application-integration-for-citrix-gotomeeting"></a>De toepassingsintegratie van de voor Citrix GoToMeeting inschakelen

Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor Citrix GoToMeeting.

###<a name="to-enable-the-application-integration-for-citrix-gotomeeting-perform-the-following-steps"></a>Als u wilt de toepassingsintegratie van de voor Citrix GoToMeeting inschakelt, moet u de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing uit galerie toevoegen] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC749322.png "Toepassing uit galerie toevoegen")

6.  Typ in het **zoekvak**, **Citrix GoToMeeting**.

    ![Citrix GoToMeeting] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC701006.png "Citrix GoToMeeting")

7.  Selecteer **Citrix GoToMeeting**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Citrix GoToMeeting] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC701012.png "Citrix GoToMeeting")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij Citrix GoToMeeting met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Als onderdeel van deze procedure bent u vereist een base 64 gecodeerde certificaat uploaden naar uw tenant Citrix GoToMeeting.  
Als u niet bekend met deze procedure bent, raadpleegt u [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik op de pagina **Citrix GoToMeeting** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Op EENMALIGE aanmelding configureren** .

    ![Eenmalige aanmelding inschakelen] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768997.png "Eenmalige aanmelding inschakelen")

2.  Selecteer op de pagina **Hoe wilt u dat gebruikers aanmelden bij Citrix GoToMeeting** **Eenmalige aanmelding Microsoft Azure AD**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768998.png "Eenmalige aanmelding configureren")


3. Klik op de pagina **App-instellingen configureren** op **volgende**. 

    ![Eenmalige aanmelding inschakelen] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC7689981.png "Eenmalige aanmelding inschakelen")

4.  Klik op de pagina **configureren eenmalige aanmelding bij Citrix GoToMeeting** op **certificaat downloaden**en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768999.png "Eenmalige aanmelding configureren")

5.  In een ander browservenster, meld u aan bij uw Citrix organisatie Center ([https://account.citrixonline.com/organization/administration/](https://account.citrixonline.com/organization/administration/)).

6. Klik op het tabblad **Identiteitsprovider** en voer de volgende stappen uit:  

    ![Op SAML-instelling] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC6892321.png "Op SAML-instelling")

    een. Selecteer **handmatig**

    
    b. Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Citrix GoToMeeting** de waarde **Aanmeldingsproblemen pagina URL** kopiëren en plak deze in het tekstvak **aanmeldingsproblemen URL voor de aanmeldingspagina** . 

    
    c. In de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij Citrix GoToMeeting** de waarde **Sign-Out pagina URL** kopiëren en plak deze in het tekstvak **URL voor de aanmeldingspagina op Afmelden** .

    
    d. Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Citrix GoToMeeting** de **Entiteit-ID** -waarde kopiëren en plak deze in het tekstvak **Identiteit Provider entiteit-ID** .

   
    e. Als u wilt uw gedownloade certificaat uploaden, klikt u op **Certificaat uploaden**.

    
    f. Klik op **Opslaan**.

6.  In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769000.png "Eenmalige aanmelding configureren")


7. Klik op de pagina **confirmation voor eenmalige aanmelding** op **voltooid**.

    ![Op SAML-instelling] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC7689982.png "Op SAML-instelling")





##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker

Het doel van dit gedeelte is overzicht maken van het inschakelen van de inrichting van Active Directory-gebruikersaccounts naar Citrix GoToMeeting.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Als u wilt configureren de inrichting van de gebruiker, moet u de volgende stappen uitvoeren:

1.  Klik op van de **inrichting van de gebruiker configureren** om het dialoogvenster **Gebruiker inrichting configureren** in de klassieke Azure portal, klik op de pagina **Citrix GoToMeeting** toepassing integratie.

    ![Inrichting van de gebruiker configureren] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769001.png "Inrichting van de gebruiker configureren")

2.  Klik op de pagina **instellingen en beheerdersreferenties** kunt u de volgende stappen uitvoeren:

    ![Inrichting van de gebruiker configureren] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769002.png "Inrichting van de gebruiker configureren")

    een. Typ de naam van een beheerder in het tekstvak **Citrix GoToMeeting beheerder gebruikersnaam in te voeren** .

    
    b. In het tekstvak **Citrix GoToMeeting beheerderswachtwoord** wachtwoord van de beheerder.

    
    c. Klik op **volgende**.

3.  Klik op **de bevestigingspagina** op het vinkje om op te slaan van uw configuratie.

4.  Klik op de knop **valideren** om te controleren of uw configuratie.


##<a name="assigning-users"></a>Gebruikers toewijzen

Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-citrix-gotomeeting-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan Citrix GoToMeeting, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **Citrix GoToMeeting** toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769003.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC767830.png "Ja")

U moet nu 10 minuten wachten en controleer of dat het account is gesynchroniseerd met Dropbox voor bedrijven.

Als eerste stap verificatie, kunt u de inrichten status controleren door te klikken op Dashboard op het tabblad D op de pagina **Citrix GoToMeeting** toepassing-integratie in de klassieke Azure-portal.

![Dashboard] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769004.png "Dashboard")

De inrichting van cyclus van een met succes zijn gebruiker wordt aangegeven door een gerelateerde status:

![Status van de integratie] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769005.png "Status van de integratie")

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access.

Zie [Inleiding tot het deelvenster Access](https://msdn.microsoft.com/library/dn308586)voor meer informatie over het Access-deelvenster.
