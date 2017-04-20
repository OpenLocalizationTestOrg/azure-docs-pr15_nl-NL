<properties 
    pageTitle="Zelfstudie: Werkdag configureren voor binnenkomende synchronisatie | Microsoft Azure" 
    description="Leer hoe u synchronisatie van binnenkomende verbindingen met Azure Active Directory gebruiken om in te schakelen voor eenmalige aanmelding, geautomatiseerd inrichting en meer!" 
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
    ms.date="04/06/2016" 
    ms.author="jeedes" />

#<a name="tutorial-configuring-workday-for-inbound-synchronization"></a>Zelfstudie: Werkdag configureren voor binnenkomende-synchronisatie
>[AZURE.NOTE]Azure Active Directory (AD) Premium is beschikbaar voor klanten in China die werken met de overal ter wereld exemplaar van Azure AD.    
Azure AD Premium wordt momenteel niet ondersteund in de Microsoft Azure-service beheerd door 21Vianet in China.    

Het doel van deze zelfstudie is om aan te geven de stappen die u uitvoeren in werkdag en Microsoft Azure AD wilt naar personen uit werkdag importeren naar Microsoft Azure AD.    
 Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:  

-   Een geldig abonnement op Azure  
-   Een tenant in werkdag  

Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:  

1.  De toepassingsintegratie van de voor werkdag inschakelen  
2.  Maken van een integratiegebruiker-systeem  
3.  Een beveiligingsgroep maken  
4.  De integratie systeemgebruiker toewijzen aan de beveiligingsgroep  
5.  Groep Beveiligingsopties configureren  
6.  Wijzigingen in beveiligingsbeleid activeren  
7.  Configureren gebruiker importeren in Microsoft Azure AD  

##<a name="enabling-the-application-integration-for-workday"></a>De toepassingsintegratie van de voor werkdag inschakelen

Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor televergaderingen.    

###<a name="to-enable-the-application-integration-for-workday-perform-the-following-steps"></a>Als u wilt de toepassingsintegratie van de voor werkdag inschakelt, moet u de volgende stappen uitvoeren:

1.  Klik in de beheerportal Azure, klik op het linker navigatiedeelvenster, op **Active Directory**.    

    ![Active Directory] (./media/active-directory-saas-inbound-synchronization-tutorial/IC700993.png "Active Directory")  

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .    

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .    

    ![Toepassingen] (./media/active-directory-saas-inbound-synchronization-tutorial/IC700994.png "Toepassingen")  

4.  Als u wilt openen in de **Galerie van toepassing**, klik op **Een App toevoegen**en klik op **toevoegen een toepassing voor mijn organisatie om te gebruiken**.    

    ![Wat wilt u doen?] (./media/active-directory-saas-inbound-synchronization-tutorial/IC700995.png "Wat wilt u doen?")  

5.  Typ in het **zoekvak** **werkdag**.    

    ![Werkdag] (./media/active-directory-saas-inbound-synchronization-tutorial/IC701021.png "Werkdag")  

6.  Selecteer **werkdag**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.    

    ![Werkdag] (./media/active-directory-saas-inbound-synchronization-tutorial/IC701022.png "Werkdag")  

##<a name="creating-an-integration-system-user"></a>Maken van een integratiegebruiker-systeem

1.  In de **Werkdag Workbench**, voert u de optie **gebruiker maken** in het zoekvak en klik op de koppeling, **Integratie systeem-gebruiker maken**.     

    ![gebruiker maken] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750979.png "gebruiker maken")  

2.  De taak Integration systeem-gebruiker maken door het opgeven van een gebruikersnaam en wachtwoord voor een nieuwe gebruiker van de integratie-systeem voltooien.  Laat de vereisen nieuwe wachtwoord bij volgende aanmelden optie uitgeschakeld, omdat deze gebruiker via programmacode aanmelden.    
    Laat de minuten van de time-sessie met de standaardwaarde van 0, voorkomen van de gebruiker sessies time-out voortijdig dat wordt.    

    ![Integratie systeemgebruiker maken] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750980.png "Integratie systeemgebruiker maken")  

##<a name="creating-a-security-group"></a>Een beveiligingsgroep maken

De scenario's beschreven in deze zelfstudie, moet u een onbeperkte integratie systeem-beveiligingsgroep maken en de gebruiker aan toewijzen.    

1.  Voer beveiligingsgroep maken in het zoekvak en klik op de koppeling, beveiligingsgroep maken.     

    ![Groep CreateSecurity] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750981.png "Groep CreateSecurity")  

2.  De beveiligingsgroep maken taak voltooien.  Beveiligingsgroep van de integratie-systeem Selecteer, zonder beperkingen in de vervolgkeuzelijst Type van verpachte beveiligingsgroep maken van een beveiligingsgroep waaraan leden expliciet, worden toegevoegd.     

    ![Groep CreateSecurity] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750982.png "Groep CreateSecurity")  

##<a name="assigning-the-integration-system-user-to-the-security-group"></a>De integratie systeemgebruiker toewijzen aan de beveiligingsgroep

1.  Voer de beveiligingsgroep bewerken in het zoekvak en klik op de koppeling, **Beveiligingsgroep bewerken**.     

    ![Beveiligingsgroep bewerken] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750983.png "Beveiligingsgroep bewerken")  

2.  Zoeken naar en selecteer de nieuwe beveiligingsgroep voor integratie met hun naam weergeven    

    ![Beveiligingsgroep bewerken] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750984.png "Beveiligingsgroep bewerken")  

3.  De nieuwe integratie systeemgebruiker toevoegen aan de nieuwe beveiligingsgroep.       

    ![Systeem-beveiligingsgroep] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750985.png "Systeem-beveiligingsgroep")  

##<a name="configuring-security-group-options"></a>Groep Beveiligingsopties configureren

In deze stap geeft verleent u aan de nieuwe groepsmachtigingen voor bewerkingen ophalen en plaatsen op de objecten die zijn beveiligd met de volgende domein beveiligingsbeleid voor apparaten:  

-   Externe Account inrichting  
-   Werknemer Data: Openbare werknemer rapporten  
-   Werknemersgegevens: Alle posities  
-   Werknemer Data: Huidige personeel informatie  
-   Werknemer Data: Titel van Business op werknemersprofiel  

&nbsp;  

1.  Domein beveiligingsbeleid voor apparaten invoeren in het zoekvak en klik op de koppeling, domein beveiligingsbeleid voor apparaten voor functioneel gebied.     

    ![Domein beveiligingsbeleid voor apparaten] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750986.png "Domein beveiligingsbeleid voor apparaten")  

2.  Systeem zoekt en selecteert u de module systeem.  Klik op de knop gekenmerkt, OK.     

    ![Domein beveiligingsbeleid voor apparaten] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750987.png "Domein beveiligingsbeleid voor apparaten")  

3.  In de lijst met beveiligingsbeleid voor apparaten voor de module systeem, beveiligingsbeheer uitvouwen en selecteer het domeinbeveiligingsbeleid, externe inrichting van Account.     

    ![Domein beveiligingsbeleid voor apparaten] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750988.png "Domein beveiligingsbeleid voor apparaten")  

4.  Klik op de knop Machtigingen bewerken en klik vervolgens in het scherm machtigingen bewerken de nieuwe beveiligingsgroep toevoegen aan de lijst met beveiligingsgroepen met machtigingen voor de integratie ophalen en plaatsen.     

    ![Machtigingen voor het bewerken] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750989.png "Machtigingen voor het bewerken")  

5.  Herhaal stap 1 hierboven, om terug te keren naar het scherm voor het selecteren van functiegebieden en schakel ditmaal zoeken naar personeel, selecteert u de module personeel en klik op de knop gekenmerkt, OK.    

    ![Domein beveiligingsbeleid voor apparaten] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750990.png "Domein beveiligingsbeleid voor apparaten")  

6.  Vouw in de lijst met beveiligingsbeleid voor apparaten voor de module personeel, werknemersgegevens: personeel en Herhaal stap 4 hierboven voor elk van deze resterende beveiligingsbeleid voor apparaten:    

    -   Werknemer Data: Openbare werknemer rapporten  
    -   Werknemersgegevens: Alle posities  
    -   Werknemer Data: Huidige personeel informatie  
    -   Werknemer Data: Titel van Business op werknemersprofiel    

    ![Domein beveiligingsbeleid voor apparaten] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750991.png "Domein beveiligingsbeleid voor apparaten")  

##<a name="activating-security-policy-changes"></a>Wijzigingen in beveiligingsbeleid activeren

1.  Voer activeren in het zoekvak en klik op de koppeling, in behandeling beveiliging beleidswijzigingen activeren.    

    ![Activeren] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750992.png "Activeren")  

2.   Eerst de taak in behandeling beveiliging beleidswijzigingen activeren invoeren van een opmerking ter controle en klik vervolgens op de knop gekenmerkt, OK te klikken.      

    ![In afwachting van beveiliging activeren] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750993.png "In afwachting van beveiliging activeren")  

3.  De taak in het volgende scherm voltooien door te schakelen van het selectievakje gekenmerkt bevestigen en te klikken op de knop gekenmerkt, OK.     

    ![In afwachting van beveiliging activeren] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750994.png "In afwachting van beveiliging activeren")  

##<a name="configuring-user-import-in-microsoft-azure-ad"></a>Configureren gebruiker importeren in Microsoft Azure AD

Het doel van dit gedeelte is overzicht maken van het configureren van Microsoft Azure AD personen uit werkdag importeren.    

###<a name="to-configure-user-import-in-microsoft-azure-ad-perform-the-following-steps"></a>Als u wilt importeren van de gebruiker in Microsoft Azure AD configureren, moet u de volgende stappen uitvoeren:

1.  Klik op **configureren gebruiker importeren** om het dialoogvenster **Inrichting configureren** op de pagina **werkdag** toepassing integratie.    

2.  Klik op de pagina **instellingen en beheerdersreferenties** de volgende stappen uitvoeren en klik vervolgens op volgende:    

    ![Instellingen en beheerdersreferenties] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750995.png "Instellingen en beheerdersreferenties")    

    1.  Typ de naam van de gebruiker die u hebt gemaakt in de sectie voor het [maken van een gebruiker met integratie systeem](https://msdn.microsoft.com/library/azure/Dn762434.aspx#BKMK_CreateUser) in het tekstvak **werkdag beheerder gebruikersnaam in te voeren** .    
    2.  Typ het wachtwoord van de gebruiker die u hebt gemaakt in de sectie voor het [maken van een gebruiker met integratie systeem](https://msdn.microsoft.com/library/azure/Dn762434.aspx#BKMK_CreateUser) in het tekstvak **beheerderswachtwoord werkdag** .    
    3.  Typ in het tekstvak **werkdag tenant URL** de URL of de tenant van uw werkdag.    

3.  Klik op de pagina **verbinding testen** op **test starten** om te bevestigen connectivity en klik vervolgens op **volgende**.    

    ![Verbinding testen] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750996.png "Verbinding testen")  

4.  Klik op de pagina met **Opties voor Provisioning** op **volgende**.    

    ![Configuratieopties] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750997.png "Configuratieopties")  

5.  Klik in het dialoogvenster **starten inrichting** klikt u op **Voltooien**.    

    ![Start inrichting] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750998.png "Start inrichting")  

U kunt nu gaat u naar de sectie **gebruikers** en controleer of de gebruiker van uw werkdag is geïmporteerd.    
