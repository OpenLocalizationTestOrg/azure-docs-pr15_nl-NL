<properties
    pageTitle="Wat zijn Appwachtwoorden in Azure MFA?"
    description="Deze pagina helpt gebruikers begrijpen wat appwachtwoorden zijn en wat ze worden gebruikt voor met betrekking tot Azure MFA."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>



# <a name="what-are-app-passwords-in-azure-multi-factor-authentication"></a>Wat zijn Appwachtwoorden in Azure meervoudige verificatie?

Bepaalde apps niet-browser, zoals de Apple systeemeigen e-mailclient die gebruikmaakt van Exchange Active Sync, ondersteuning momenteel geen voor meervoudige verificatie. Meervoudige verificatie is per gebruiker ingeschakeld. Dit betekent dat als een gebruiker is ingeschakeld voor meervoudige verificatie en ze probeert te gebruiken dan browsers apps, worden deze niet kan doen. Een appwachtwoord kunt dit gebeurt.

>[AZURE.NOTE] Moderne verificatie voor de Office 2013-Clients
>
> Office 2013-clients (inclusief Outlook) is nu ondersteuning voor nieuwe verificatieprotocollen en kunnen worden ingeschakeld voor de ondersteuning van meervoudige verificatie.  Dit betekent dat eenmaal is ingeschakeld, appwachtwoorden niet vereist is voor gebruik met Office 2013-clients zijn.  Zie [Office 2013 moderne verificatie openbare preview aangekondigd](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/)voor meer informatie.

## <a name="how-to-use-app-passwords"></a>Het gebruik van de appwachtwoorden

Hier volgen enkele dingen die u moet denken over het gebruik van appwachtwoorden.

- De werkelijke wachtwoord wordt automatisch gegenereerd en niet door de gebruiker worden verstrekt. Dit komt omdat de automatisch gegenereerde wachtwoord is moeilijker onbevoegden te raden en veiliger is.
- Er is momenteel een limiet van 40 wachtwoorden per gebruiker. Als u probeert u een account maakt nadat u de limiet hebt bereikt, wordt u gevraagd een van uw bestaande appwachtwoorden verwijderen om te maken van een nieuwe record.
- Het wordt aanbevolen dat appwachtwoorden per apparaat en niet per toepassing worden gemaakt. U kunt bijvoorbeeld een appwachtwoord maken voor uw laptop en die appwachtwoord gebruikt voor al uw toepassingen op die laptop.
- Krijgt u een appwachtwoord de eerste keer u aanmelden.  Als u aanvullende bestanden nodig hebt, kunt u ze kunt maken.

![Setup](./media/multi-factor-authentication-end-user-app-passwords/app.png)

Nadat u een appwachtwoord hebt, kunt u dit gebruiken in plaats van het oorspronkelijke wachtwoord met deze apps niet-browser.  Dus bijvoorbeeld, als u meervoudige verificatie en de Apple systeemeigen e-mailclient op uw telefoon gebruikt.  Gebruik het appwachtwoord zodat deze kan overslaan meervoudige verificatie en blijven werken.

## <a name="creating-and-deleting-app-passwords"></a>Maken en verwijderen van appwachtwoorden
Tijdens uw initiële aanmeldingsproblemen krijgt u een appwachtwoord die u kunt gebruiken.  Bovendien kunt u ook maken en appwachtwoorden later verwijderen.  Hoe u dit doet, is afhankelijk van hoe u meervoudige verificatie gebruiken.  Kies het account dat het grootste deel op u van toepassing.

Hoe u meervoudige authentiation gebruiken|Beschrijving
:------------- | :------------- |
[Ik gebruiken met Office 365](#creating-and-deleting-app-passwords-with-office-365)|  Dit betekent dat u wilt appwachtwoorden via de Office 365-beheerportal maken.
[Ik weet het niet](#creating-and-deleting-app-passwords-with-myapps-portal)|Dit betekent dat u later appwachtwoorden tot en met [https://myapps.microsoft.com](https://myapps.microsoft.com) maken
[Ik gebruik deze met Microsoft Azure](#create-app-passwords-in-the-azure-portal)| Dit betekent dat u later appwachtwoorden via de portal van Azure maken.

## <a name="creating-and-deleting-app-passwords-with-office-365"></a>Maken en verwijderen van appwachtwoorden met Office 365

Als u meervoudige verificatie met Office 365 die u wilt maken en verwijderen van appwachtwoorden via de Office 365-beheerportal gebruiken.

### <a name="to-create-app-passwords-in-the-office-365-portal"></a>Appwachtwoorden maken in de Office 365-portal
--------------------------------------------------------------------------------

1. Meld u aan bij de [Office 365-portal](https://login.microsoftonline.com/).
2. Selecteer het object en kies Office 365-instellingen in de rechterbovenhoek.
3. Klik op extra beveiliging verificatie.
4. Klik op de koppeling met de mededeling dat aan de rechterkant **bijwerken van Mijn telefoonnummers die wordt gebruikt voor accountbeveiliging.** 
 ![Instellen](./media/multi-factor-authentication-end-user-manage/o365a.png)
5. Hiermee gaat u naar de pagina waarmee u uw instellingen wijzigen.
![Cloud](./media/multi-factor-authentication-end-user-manage/o365b.png)
6. Aan de bovenkant, naast extra beveiliging verificatie, klikt u op **appwachtwoorden.**
7. Klik op **maken**.
![Cloud](./media/multi-factor-authentication-end-user-app-passwords-create-o365/apppass.png)
8. Voer een naam voor het appwachtwoord en klik op **volgende**.
![Een appwachtwoord maken](./media/multi-factor-authentication-end-user-app-passwords/create1.png)
9. Het appwachtwoord naar het Klembord kopiëren en plak deze in uw app.
![Een appwachtwoord maken](./media/multi-factor-authentication-end-user-app-passwords/create2.png)


### <a name="to-delete-app-passwords-using-the-office-365-portal"></a>Verwijderen van appwachtwoorden met behulp van de Office 365-portal
--------------------------------------------------------------------------------


1. Meld u aan bij de [Office 365-portal](https://login.microsoftonline.com/).
2. Selecteer het object en kies Office 365-instellingen in de rechterbovenhoek.
3. Klik op extra beveiliging verificatie.
4. Klik op de koppeling met de mededeling dat aan de rechterkant **bijwerken van Mijn telefoonnummers die wordt gebruikt voor accountbeveiliging.** 
 ![Instellen](./media/multi-factor-authentication-end-user-manage/o365a.png)
5. Hiermee gaat u naar de pagina waarmee u uw instellingen wijzigen.
![Cloud](./media/multi-factor-authentication-end-user-manage/o365b.png)
6. Aan de bovenkant, naast extra beveiliging verificatie, klikt u op **appwachtwoorden.**
7. Naast het appwachtwoord die u wilt verwijderen, klikt u op **verwijderen**.
![Een appwachtwoord verwijderen](./media/multi-factor-authentication-end-user-app-passwords/delete1.png)
8. Bevestig de verwijdering door te klikken op **Ja**.
![Bevestig het verwijderen](./media/multi-factor-authentication-end-user-app-passwords/delete2.png)
9. Zodra de appwachtwoord wordt verwijderd kunt u klikken op **sluiten**.
![Sluiten](./media/multi-factor-authentication-end-user-app-passwords/delete3.png)


## <a name="creating-and-deleting-app-passwords-with-myapps-portal"></a>Maken en verwijderen van appwachtwoorden met Apps de portal.
Als u niet zeker weet hoe u meervoudige verificatie gebruiken, kunt klikt u vervolgens u altijd maken en verwijderen van appwachtwoorden via de portal Apps.

### <a name="to-create-an-app-password-using-the-myapps-portal"></a>Een appwachtwoord met behulp van de portal Apps maken

1. Aanmelden bij [https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Aan de bovenkant, selecteer profiel.
3. Selecteer extra beveiliging verificatie.
![Cloud](./media/multi-factor-authentication-end-user-manage/myapps1.png)
4. Hiermee gaat u naar de pagina waarmee u uw instellingen wijzigen.
![Setup](./media/multi-factor-authentication-end-user-manage-myapps/proofup.png)
5. Aan de bovenkant, naast extra beveiliging verificatie, klikt u op **appwachtwoorden.**
6. Klik op **maken**.
![Een appwachtwoord maken](./media/multi-factor-authentication-end-user-app-passwords/create3.png)
7. Voer een naam voor het appwachtwoord en klik op **volgende**.
![Een appwachtwoord maken](./media/multi-factor-authentication-end-user-app-passwords/create1.png)
8. Het appwachtwoord naar het Klembord kopiëren en plak deze in uw app.
![Een appwachtwoord maken](./media/multi-factor-authentication-end-user-app-passwords/create2.png)

### <a name="to-delete-an-app-password-using-the-myapps-portal"></a>Een appwachtwoord met behulp van de portal Apps verwijderen

1. Aanmelden bij [https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Aan de bovenkant, selecteer profiel.
3. Selecteer extra beveiliging verificatie.
![Cloud](./media/multi-factor-authentication-end-user-manage/myapps1.png)
4. Hiermee gaat u naar de pagina waarmee u uw instellingen wijzigen.
![Setup](./media/multi-factor-authentication-end-user-manage-myapps/proofup.png)
5. Aan de bovenkant, naast extra beveiliging verificatie, klikt u op **appwachtwoorden.**
6. Naast het appwachtwoord die u wilt verwijderen, klikt u op **verwijderen**.
![Een appwachtwoord verwijderen](./media/multi-factor-authentication-end-user-app-passwords/delete1.png)
7. Bevestig de verwijdering door te klikken op **Ja**.
![Bevestig het verwijderen](./media/multi-factor-authentication-end-user-app-passwords/delete2.png)
8. Zodra de appwachtwoord wordt verwijderd kunt u klikken op **sluiten**.
![Sluiten](./media/multi-factor-authentication-end-user-app-passwords/delete3.png)


## <a name="create-app-passwords-in-the-azure-portal"></a>Appwachtwoorden maken in de portal van Azure

Als u meervoudige verificatie met Azure die u wilt maken van appwachtwoorden via de portal van Azure gebruikt.

### <a name="to-create-app-passwords-in-the-azure-portal"></a>Appwachtwoorden maken in de portal van Azure

1. Meld u aan bij de portal Azure Management.
2. Aan de bovenkant, met de rechtermuisknop op uw gebruikersnaam in te voeren en selecteer extra beveiliging verificatie.
3. Selecteer op de pagina proofup aan de bovenkant, appwachtwoorden
4. Klik op **maken**.
5. Voer een naam voor het appwachtwoord en klik op **volgende**
6. Het appwachtwoord naar het Klembord kopiëren en plak deze in uw app.
![Cloud](./media/multi-factor-authentication-end-user-app-passwords-create-azure/app2.png)

### <a name="to-delete-app-passwords-in-the-azure-portal"></a>Verwijderen van appwachtwoorden in de portal van Azure

1. Meld u aan bij de portal Azure Management.
2. Aan de bovenkant, met de rechtermuisknop op uw gebruikersnaam in te voeren en selecteer extra beveiliging verificatie.
3. Aan de bovenkant, naast extra beveiliging verificatie, klikt u op **appwachtwoorden.**
4. Naast het appwachtwoord die u wilt verwijderen, klikt u op **verwijderen**.
5. Bevestig de verwijdering door te klikken op **Ja**.
6. Zodra de appwachtwoord wordt verwijderd kunt u klikken op **sluiten**.
![Sluiten](./media/multi-factor-authentication-end-user-app-passwords/delete3.png)
