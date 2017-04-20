<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met werkdag | Microsoft Azure" 
    description="Informatie over het gebruik van de werkdag met Azure Active Directory om te kunnen eenmalige aanmelding, geautomatiseerd inrichting en meer!." 
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
    ms.date="09/09/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-workday"></a>Zelfstudie: Azure Active Directory-integratie met werkdag
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en werkdag. Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een tenant in werkdag
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor werkdag inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Configuratie van de inrichting van gebruiker

![Scenario] (./media/active-directory-saas-workday-tutorial/IC782919.png "Scenario")

##<a name="enabling-the-application-integration-for-workday"></a>De toepassingsintegratie van de voor werkdag inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor televergaderingen.

###<a name="to-enable-the-application-integration-for-workday-perform-the-following-steps"></a>Als u wilt de toepassingsintegratie van de voor werkdag inschakelt, moet u de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-workday-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-workday-tutorial/IC700994.png "Toepassingen")

4.  Als u wilt openen in de **Galerie van toepassing**, klik op **Een App toevoegen**en klik op **toevoegen een toepassing voor mijn organisatie om te gebruiken**.

    ![Wat wilt u doen?] (./media/active-directory-saas-workday-tutorial/IC700995.png "Wat wilt u doen?")

5.  Typ in het **zoekvak** **werkdag**.

    ![Werkdag] (./media/active-directory-saas-workday-tutorial/IC701021.png "Werkdag")

6.  Selecteer **werkdag**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Werkdag] (./media/active-directory-saas-workday-tutorial/IC701022.png "Werkdag")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij werkdag met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Als onderdeel van deze procedure bent u verplicht voor het maken van een base 64 gecodeerd certificaat.  
Als u niet bekend met deze procedure bent, raadpleegt u [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik op de pagina **werkdag** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-workday-tutorial/IC782920.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij werkdag** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-workday-tutorial/IC782921.png "Eenmalige aanmelding configureren")

3.  Klik op de pagina **App-URL configureren** de volgende stappen uitvoeren en klik vervolgens op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-workday-tutorial/IC782957.png "URL van App configureren")

    een. Typ in het tekstvak **Aanmeldingsadres op URL** de URL die wordt gebruikt door uw gebruikers te melden bij een werkdag met het volgende patroon:`https://impl.workday.com/<tenant>/login-saml2.htmld`

    b.  Typ de werkdag antwoord-URL die met het volgende patroon in het tekstvak **Werkdag antwoord URL** :`https://impl.workday.com/<tenant>/login-saml.htmld`

    >[AZURE.NOTE]De URL van uw antwoord moet een subdomein (bijvoorbeeld: www, wd2, wd3, wd3-impl, wd5, wd5-impl). 
    >Werkt met iets zoals '*http://www.myworkday.com*' maar '*http://myworkday.com*' betekent niet. 
 
4.  Klik op de pagina **configureren eenmalige aanmelding bij werkdag** op **certificaat downloaden**als u wilt downloaden van het certificaat, en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-workday-tutorial/IC782922.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw werkdag bedrijfssite als beheerder.

6.  Ga naar **Menu \> Workbench**.

    ![Workbench] (./media/active-directory-saas-workday-tutorial/IC782923.png "Workbench")

7.  Ga naar **Accountbeheer van de**.

    ![Accountbeheer] (./media/active-directory-saas-workday-tutorial/IC782924.png "Accountbeheer")

8.  Ga naar het **bewerken van de Tenant Setup-beveiliging**.

    ![Beveiliging van de Tenant bewerken] (./media/active-directory-saas-workday-tutorial/IC782925.png "Beveiliging van de Tenant bewerken")

9.  Klik in de sectie **URL's voor omleiding van** de volgende stappen uitvoeren:

    ![Omleiding van URL 's] (./media/active-directory-saas-workday-tutorial/IC7829581.png "Omleiding van URL 's")

    een. Klik op **rij toevoegen**.

    b. Typ in het tekstvak **Omleiden aanmeldings-URL** en het tekstvak **Mobile Omleidings-URL** , de **Werkdag Tenant URL** die u hebt ingevoerd op de pagina **Configureren App-URL** van de Azure klassieke portal.
    
    c. Kopieer de **URL van de Service één Sign-Out**in de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij werkdag** en plak deze in het tekstvak **Afmelden Omleidings-URL** .

    d.  Typ de naam van de omgeving in **omgeving** tekstvak.  


    >[AZURE.NOTE] De waarde van de omgeving-kenmerk is gekoppeld aan de waarde van de tenant-URL:
    >
    >-   Als de domeinnaam van de tenant werkdag URL met impl begint (bijvoorbeeld: *https://impl.workday.com/\<tenant\>/login-saml2.htmld*), het kenmerk **omgeving** moet zijn ingesteld op implementatie.
    >-   Als de domeinnaam die met iets anders begint, moet u contact opnemen met werkdag om te krijgen van de overeenkomstige waarde van de **omgeving** .

10. In de sectie **SAML-instelling** , moet u de volgende stappen uitvoeren:

    ![Op SAML-instelling] (./media/active-directory-saas-workday-tutorial/IC782926.png "Op SAML-instelling")

    een.  Selecteer **SAML-verificatie inschakelen**.

    b.  Klik op **rij toevoegen**.

11. Klik in de sectie SAML-identiteitsprovider kunt u de volgende stappen uitvoeren:

    ![Op SAML-identiteitsproviders] (./media/active-directory-saas-workday-tutorial/IC7829271.png "Op SAML-identiteitsproviders")

    een. Typ een providernaam in het tekstvak naam van de Provider identiteit (bijvoorbeeld: *SPInitiatedSSO*).

    b. Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij werkdag** de **Identiteit Provider-ID** -waarde kopiëren en plak deze in het tekstvak **uitgever** .

    c. Selecteer **werkdag Initialted afmelden inschakelen**.

    d. Kopieer de **URL van de Service één Sign-Out** -waarde in de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij werkdag** en plak deze in het tekstvak **Afmelden aanvragen URL** .


    e. Klik op **Identiteit Provider openbare sleutelcertificaat**en klik vervolgens op **maken**. 

    ![Maken] (./media/active-directory-saas-workday-tutorial/IC782928.png "Maken")

    f. Klik op **x509 maken openbare sleutel**. 
        
    ![Maken] (./media/active-directory-saas-workday-tutorial/IC782929.png "Maken")


1. Klik in de sectie **Weergave x509 openbare sleutel** moet u de volgende stappen uitvoeren: 

    ![Weergave x509 openbare sleutel] (./media/active-directory-saas-workday-tutorial/IC782930.png "Weergave x509 openbare sleutel") 

    een. Typ in het tekstvak **naam** een naam voor uw certificaat (bijvoorbeeld: *beschermingsmiddelen\_SP*).
        
    b. Typ de geldig vanaf kenmerkwaarde van uw certificaat in het tekstvak **Geldig van** .
    
    c.  Typ de geldig tot kenmerkwaarde van uw certificaat in het tekstvak **Geldig tot** .
        
    >[AZURE.NOTE] U kunt de geldige krijgen van de datum en de geldige einddatum van het gedownloade certificaat door erop te dubbelklikken. De datums worden vermeld onder het tabblad **Details** .

    d. Een **basis-64-codering** -bestand uit uw gedownloade certificaat maken.  

    >[AZURE.TIP] Zie voor meer informatie, [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

    e.  Open uw base 64 gecodeerde certificaat in Kladblok en kopieer de inhoud ervan.
    
    f.  Plak de inhoud van het Klembord in het tekstvak **certificaat** .
    
    g.  Klik op **OK**.

12.  De volgende stappen uitvoeren: 

    ![Configuratie van eenmalige aanmelding] (./media/active-directory-saas-workday-tutorial/IC7829351111.png "Configuratie van eenmalige aanmelding")

    een.  Schakel de **x509 combinatie van persoonlijke sleutel**.

    b.  Typ in het tekstvak **Service Provider-ID** **http://www.workday.com**.

    c.  Selecteer **inschakelen gestart SP SAML-verificatie**.

    d.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij werkdag** de waarde voor **Eenmalige aanmelding Service URL** kopiëren en plak deze in het tekstvak **URL IdP SSO-Service** .
     
    e. Selecteer **SP gestart verificatieaanvraag niet doen verkleinen**.

    f. Als **Aanvragen handtekening verificatiemethode**selecteren **SHA256**. 
        
    ![Verificatie aanvragen handtekeningmethode] (./media/active-directory-saas-workday-tutorial/IC782932.png "Verificatie aanvragen handtekeningmethode") 
 
    g. Klik op **OK**. 
        
    ![OK] (./media/active-directory-saas-workday-tutorial/IC782933.png "OK")

12. Klik op **volgende**in de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij werkdag** . 

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-workday-tutorial/IC782934.png "Eenmalige aanmelding configureren")

13. Klik op de pagina **confirmation voor eenmalige aanmelding** op **voltooid**. 

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-workday-tutorial/IC782935111.png "Eenmalige aanmelding configureren")



##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Als u een testgebruiker deze is ingericht in werkdag, moet u contact opnemen met het ondersteuningsteam werkdag.  
Het ondersteuningsteam werkdag maakt de gebruiker voor u.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-workday-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan werkdag, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **werkdag **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-workday-tutorial/IC782935.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-workday-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.