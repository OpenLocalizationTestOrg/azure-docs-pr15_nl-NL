<properties
    pageTitle="Het implementeren van de Access-extensie voor Configuratiescherm voor Internet Explorer met Groepsbeleid | Microsoft Azure"
    description="Klik hier voor meer informatie over het groepsbeleid gebruiken om te implementeren van de invoegtoepassing Internet Explorer voor de portal mijn Apps."
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>
<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/16/2016"
    ms.author="markvi"/>

#<a name="how-to-deploy-the-access-panel-extension-for-internet-explorer-using-group-policy"></a>Het implementeren van de Access-extensie voor Configuratiescherm voor Internet Explorer met Groepsbeleid

Deze zelfstudie wordt getoond hoe u de uitbreiding van Access Configuratiescherm extern installeren voor Internet Explorer op uw gebruikers computers met Groepsbeleid. Dit toestel is vereist voor Internet Explorer-gebruikers die u zich aanmeldt bij apps die zijn geconfigureerd met [wachtwoorden gebaseerde eenmalige aanmelding](active-directory-appssoaccess-whatis.md#password-based-single-sign-on)nodig hebben.

Het wordt aanbevolen dat beheerders de implementatie van dit toestel automatiseren. Anders hebben gebruikers downloaden en installeren van de extensie zelf, die is vatbaar voor gebruikersfout en beheerdersrechten voor nodig. Deze zelfstudie behandelt één methode om implementaties van de software te automatiseren via Groepsbeleid. [Meer informatie over Groepsbeleid.](https://technet.microsoft.com/windowsserver/bb310732.aspx)

De uitbreiding van Access Configuratiescherm is ook beschikbaar voor [Chrome](https://go.microsoft.com/fwLink/?LinkID=311859) en [Firefox](https://go.microsoft.com/fwLink/?LinkID=626998), geen van beide waarvan beheerdersmachtigingen vereisen te installeren.

##<a name="prerequisites"></a>Vereisten voor

- U hebt ingesteld met [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx)en u uw gebruikers machines aan uw domein hebt toegevoegd.
- U moet de 'Instellingen bewerken' gemachtigd om te bewerken GPO (GPO's). Standaard leden van de volgende beveiligingsgroepen over deze machtiging beschikken: beheerders van het domein, beheerders van het bedrijf en Maker Eigenaar Groepsbeleid. [Meer informatie.](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx)

##<a name="step-1-create-the-distribution-point"></a>Stap 1: Maak de komma verdeling

Plaats het installatiepakket moet u eerst op een netwerklocatie die toegankelijk is uit alle de machines die u de extensie wilt op afstand installeren. Ga hiervoor als volgt te werk:

1. Meld u aan bij de server als een beheerder

2. Klik in het venster **Server Manager** gaat u naar **bestanden en opslag-Services**.

    ![Bestanden openen en opslagservices](./media/active-directory-saas-ie-group-policy/files-services.png)

3. Ga naar het tabblad **waarden voor aandelen** . Klik op **taken** > **Nieuwe delen...**

    ![Bestanden openen en opslagservices](./media/active-directory-saas-ie-group-policy/shares.png)

4. Voltooi de **Wizard Nieuwe delen** en machtigingen om ervoor te zorgen dat deze zijn toegankelijk vanaf uw gebruikers machines instellen. [Meer informatie over waarden voor aandelen.](https://technet.microsoft.com/library/cc753175.aspx)

5. Het volgende pakket voor Microsoft Windows Installer (MSI-bestand) downloaden: [Access Configuratiescherm Extension.msi](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access Panel Extension.msi)

6. Kopieer het installatiepakket op de gewenste locatie op de optie voor delen.

    ![Kopie het MSI-bestand naar u bent de optie voor delen.](./media/active-directory-saas-ie-group-policy/copy-package.png)

8. Controleer of uw clientcomputers toegang tot het installatiepakket via de optie voor delen. 

##<a name="step-2-create-the-group-policy-object"></a>Stap 2: Het groepsobject beleid maken

1. Meld u aan bij de server waarop uw Active Directory Domain Services (AD DS)-installatie.

2. In de Serverbeheer, gaat u naar **Extra** > **Groepsbeleid beheren**.

    ![Ga naar Extra > beleid Managment groeperen](./media/active-directory-saas-ie-group-policy/tools-gpm.png)

3. De hiërarchie van uw organisatie-eenheid weergeven in het linkerdeelvenster van het venster **Groepsbeleid beheren** , en bepalen op welke bereik u wil het groepsbeleid toegepast. Bijvoorbeeld, wilt u misschien een kleine OU om te implementeren naar een paar gebruikers voor het testen van Kies of u mogelijk een op het hoogste niveau OU om te implementeren voor uw gehele organisatie kiezen.

    > [AZURE.NOTE] Als u wilt maken of bewerken van uw organisatie-eenheden, Ga terug naar de Server Manager en gaat u naar **Extra** > **Active Directory-gebruikers en Computers**.

4. Als u een organisatie-eenheid hebt geselecteerd, met de rechtermuisknop op dit en selecteer **een GPO in dit domein maken en hier een koppeling...**

    ![Een nieuw GPO maken](./media/active-directory-saas-ie-group-policy/create-gpo.png)

5. Typ een naam voor de nieuwe groepsbeleidobject in de prompt **Nieuwe GPO** .

    ![De naam van het nieuwe GPO](./media/active-directory-saas-ie-group-policy/name-gpo.png)

6. Met de rechtermuisknop op het groepsobject beleid die u zojuist hebt gemaakt en selecteer **bewerken**.

    ![Het nieuwe GPO bewerken](./media/active-directory-saas-ie-group-policy/edit-gpo.png)

##<a name="step-3-assign-the-installation-package"></a>Stap 3: Het installatiepakket toewijzen

1. Hiermee bepaalt u of u wilt implementeren van de extensie op basis van **De configuratie van de Computer** of **De configuratie van de gebruiker**. Wanneer u met [de Computerconfiguratie van de](https://technet.microsoft.com/library/cc736413%28v=ws.10%29.aspx), wordt de extensie op de computer ongeacht welke gebruikers zich aanmelden bij deze zijn geïnstalleerd. Met [de Gebruikersconfiguratie van de](https://technet.microsoft.com/library/cc781953%28v=ws.10%29.aspx)heeft gebruikers aan de andere kant de extensie geïnstalleerd voor hen ongeacht welke computers deze zich aanmelden.

2. In het linkerdeelvenster van het venster **Groepsbeleid Management-Editor** , gaat u naar een van de volgende mappaden, afhankelijk van het type configuratie die u hebt gekozen:
    - `Computer Configuration/Policies/Software Settings/`
    - `User Configuration/Policies/Software Settings/`

3. Met de rechtermuisknop op **Software-installatie**en selecteert u **Nieuw** > **pakket...**

    ![Een nieuwe software-installatiepakket maken](./media/active-directory-saas-ie-group-policy/new-package.png)

4. Ga naar de gedeelde map waarin het installatiepakket uit [stap 1: maken van de verdeling komma](#step-1-create-the-distribution-point), selecteert u het MSI-bestand en klik op **openen**.

    > [AZURE.IMPORTANT] Als de optie voor delen bevindt zich op deze dezelfde server, controleert u of dat u het MSI via het netwerk bestandspad, in plaats van het lokale bestandspad benadert.

    ![Selecteer het installatiepakket te gaan in de gedeelde map.](./media/active-directory-saas-ie-group-policy/select-package.png)

5. Selecteer in de prompt **Software distribueren** **toegewezen** voor uw implementatiemethode. Klik vervolgens op **OK**.

    ![Selecteer toegewezen en klik op OK.](./media/active-directory-saas-ie-group-policy/deployment-method.png)

De extensie is nu geïmplementeerd in de organisatie-eenheid die u hebt geselecteerd. [Meer informatie over Software-installatie.](https://technet.microsoft.com/library/cc738858%28v=ws.10%29.aspx)

##<a name="step-4-auto-enable-the-extension-for-internet-explorer"></a>Stap 4: Automatisch inschakelen de extensie voor Internet Explorer 

Naast het installatieprogramma uitvoert, moet elke-extensie voor Internet Explorer zijn expliciet ingeschakeld voordat deze kan worden gebruikt. Volg de onderstaande stappen voor het inschakelen van de extensie voor het Configuratiescherm van Access met Groepsbeleid:

1. Klik in het venster **Editor Management Groepsbeleid** gaat u naar een van de volgende paden, afhankelijk van het type configuratie u ervoor kiest in [stap 3: het installatiepakket toewijzen](#step-3-assign-the-installation-package):
    - `Computer Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`
    - `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`

2. Met de rechtermuisknop op de **Lijst met invoegtoepassingen**en selecteer **bewerken**.
    ![Bewerken lijst met invoegtoepassingen.](./media/active-directory-saas-ie-group-policy/edit-add-on-list.png)

3. Selecteer in het venster **Lijst met invoegtoepassingen** **ingeschakeld**. Klik vervolgens onder de sectie **Opties** op **... weergeven**.

    ![Klik op inschakelen en klik vervolgens op weergeven...](./media/active-directory-saas-ie-group-policy/edit-add-on-list-window.png)

4. Klik in het venster **Inhoud weergeven** kunt u de volgende stappen uitvoeren:

    1. Voor de eerste kolom (het veld **Waardenaam** ), kopieer en plak de volgende klasse-ID:`{030E9A3F-7B18-4122-9A60-B87235E4F59E}`

    2. Voor de tweede kolom **(het waardeveld)** , typt u in de volgende waarde:`1`

    3. Klik op **OK** om het venster **Inhoud weergeven** te sluiten.

    ![Vul de waarden zoals hierboven.](./media/active-directory-saas-ie-group-policy/show-contents.png)

5. Klik op **OK** om uw wijzigingen toepassen op en sluit het venster **Lijst met invoegtoepassingen** .

De extensie moet nu de machines in de geselecteerde OU zijn ingeschakeld. [Meer informatie over het gebruik van Groepsbeleid of Internet Explorer-invoegtoepassingen uit te schakelen.](https://technet.microsoft.com/library/dn454941.aspx)

##<a name="step-5-optional-disable-remember-password-prompt"></a>Stap 5 (optioneel): "Wachtwoord onthoudt," Prompt uitschakelen

Wanneer gebruikers zich aanmelden naar websites met de Access-extensie voor Configuratiescherm, kan Internet Explorer de volgende gevraagd 'Wilt u uw wachtwoord opslaan?' weergeven

![](./media/active-directory-saas-ie-group-policy/remember-password-prompt.png)

Als u voorkomen dat uw gebruikers wilt kunnen zien van deze prompt wordt weergegeven, volgt u de onderstaande stappen om te voorkomen dat automatisch aanvullen van het onthouden van wachtwoorden:

1. Ga naar het pad hieronder ziet u in het venster **Groepsbeleid Management-Editor** . Houd er rekening mee dat deze configuratieinstelling alleen beschikbaar wordt weergegeven onder **De configuratie van de gebruiker is**.
    - `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/`

2. Zoek de naam van **de functie automatisch aanvullen voor gebruikersnamen en wachtwoorden in formulieren inschakelen**instelling.

    > [AZURE.NOTE] Eerdere versies van Active Directory mogelijk een lijst met deze instelling met de naam **niet toestaan dat voor automatisch aanvullen wachtwoorden op te slaan**. De configuratie voor deze instelling verschilt van de instelling beschreven in deze zelfstudie.

    ![Vergeet niet te zoeken naar dit onder instellingen.](./media/active-directory-saas-ie-group-policy/disable-auto-complete.png)

3. Klik met de rechtermuisknop op de bovenstaande instelling en selecteer **bewerken**.

4. Klik in het venster getiteld **inschakelen van de functie automatisch aanvullen voor gebruikersnamen en wachtwoorden op formulieren**Selecteer **uitgeschakeld**.

    ![Selecteer uitschakelen](./media/active-directory-saas-ie-group-policy/disable-passwords.png)

5. Klik op **OK** als u wilt deze wijzigingen toepassen op en sluit het venster.

Gebruikers kunnen niet meer hun referenties of automatisch aanvullen gebruiken voor toegang tot eerder opgeslagen referenties op te slaan. Dit beleid staat echter gebruikers blijven automatisch aanvullen gebruiken voor andere soorten formuliervelden die zijn, zoals zoekvelden toe.

> [AZURE.WARNING] Als dit beleid is ingeschakeld nadat gebruikers hebt gekozen om op te slaan sommige referenties, dit beleid is *niet* schakelt u de referenties die al zijn opgeslagen.

##<a name="step-6-testing-the-deployment"></a>Stap 6: De implementatie testen

Volg de onderstaande stappen om te controleren of als de extensie-implementatie voltooid is:

1. Als u met de **Configuratie van de Computer**hebt geïmplementeerd, meld u aan bij een clientcomputer die bij de organisatie-eenheid die u hebt geselecteerd hoort in [stap 2: maken van het groepsobject beleid](#step-2-create-the-group-policy-object). Als u geïmplementeerd met **Configuratie van de gebruiker**, moet u zich aanmelden als een gebruiker met een organisatie-eenheid.

2. Het duurt enkele Aanmeldingsadres ins voor het groepsbeleid wordt gewijzigd in volledig bijwerken met deze computer. Open **een opdrachtpromptvenster** als u wilt dat de update, en voer de volgende opdracht:`gpupdate /force`

3. U moet het apparaat voor de installatie plaatsvindt opnieuw opstarten. Bootup kan aanzienlijk langer duren dan gebruikelijke terwijl de extensie is geïnstalleerd.

4. Start opnieuw op en open **Internet Explorer**. In de rechterbovenhoek van het venster, klik op **Extra** (tandwielpictogram) en selecteer vervolgens **Invoegtoepassingen beheren**.

    ![Ga naar Extra > invoegtoepassingen beheren](./media/active-directory-saas-ie-group-policy/manage-add-ons.png)

5. Controleer of de **Access-extensie voor Configuratiescherm** is geïnstalleerd en dat de **Status** is ingesteld op **ingeschakeld**in het venster **Invoegtoepassingen beheren** .

    ![Controleer of de Access-extensie voor Configuratiescherm is geïnstalleerd en is ingeschakeld.](./media/active-directory-saas-ie-group-policy/verify-install.png)

## <a name="related-articles"></a>Verwante artikelen

- [Artikel Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
- [Toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md)
- [De Access-extensie voor Configuratiescherm voor probleemoplossing voor Internet Explorer](active-directory-saas-ie-troubleshooting.md)
