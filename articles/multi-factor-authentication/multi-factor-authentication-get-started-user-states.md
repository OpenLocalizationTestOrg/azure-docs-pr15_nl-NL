<properties 
    pageTitle="Microsoft Azure meervoudige verificatie gebruiker Staten"
    description="Meer informatie over gebruiker Staten in Azure MFA."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="user-states-in-azure-multi-factor-authentication"></a>Gebruiker Staten in Azure meervoudige verificatie

Gebruikersaccounts in Azure meervoudige verificatie bestaan uit de volgende drie verschillende statussen:

De staat | Beschrijving |Niet-browser apps beïnvloed| Notities
:-------------: | :-------------: |:-------------: |:-------------: |
Uitgeschakeld | De standaardstatus voor een nieuwe gebruiker niet is geregistreerd in meervoudige verificatie.|Nee|Meervoudige verificatie is geen gebruik van de gebruiker.
Ingeschakeld |De gebruiker is geregistreerd in meervoudige verificatie.|Nee.  Deze blijven werken totdat het registratieproces is voltooid.|De gebruiker is ingeschakeld, maar niet het registratieproces is voltooid. Ze wordt gevraagd om het proces aan de volgende aanmelden te voltooien.
Afgedwongen|De gebruiker is geregistreerd en het registratieproces voor het gebruik van meervoudige verificatie is voltooid.|Ja.  Apps vereist app wachtwoord. | De gebruiker mogelijk of zijn mogelijk niet uitgevoerd registratie. Als ze het registratieproces hebt voltooid, worden ze meervoudige verificatie gebruiken. Anders wordt wordt de gebruiker gevraagd om het proces aan de volgende aanmelden te voltooien.

## <a name="changing-a-user-state"></a>De gebruikersstatus van een wijzigen
Een gebruikers-status wordt gewijzigd afhankelijk van of deze is ingesteld voor MFA en of de gebruiker het proces is voltooid.  Wanneer u voor een gebruiker MFA inschakelt, wordt de status wordt gewijzigd van gebruikers uitgeschakeld ingeschakeld.  Nadat de gebruiker, waarvan de status is gewijzigd in ingeschakeld, zich aanmeldt en is het proces wordt voltooid, wordt hun status gewijzigd in afgedwongen.  

### <a name="to-view-a-users-state"></a>De status van een gebruiker weergeven
--------------------------------------------------------------------------------
1.  Meld u als beheerder aan bij de **portal van Azure klassieke** .
2.  Aan de linkerkant, klikt u op **Active Directory**.
3.  Klik onder op de **map** voor de map voor de gebruiker die u wilt inschakelen.
![Klik op map](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4.  Aan de bovenkant, klikt u op **gebruikers**.
5.  Klik onder aan de pagina, op **Meervoudige Auth beheren**.
![Klik op map](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6.  Hiermee opent u een nieuw browsertabblad.  U kunnen de status van gebruikers weergeven.
![Klik op map](./media/multi-factor-authentication-get-started-user-states/userstate1.png)

###<a name="to-change-the-state-from-disabled-to-enabled"></a>Als u wilt wijzigen van de stand van uitgeschakeld ingeschakeld
1.  Meld u als beheerder aan bij de **portal van Azure klassieke** .
2.  Aan de linkerkant, klikt u op **Active Directory**.
3.  Klik onder op de **map** voor de map voor de gebruiker die u wilt inschakelen.
![Klik op map](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4.  Aan de bovenkant, klikt u op **gebruikers**.
5.  Klik onder aan de pagina, op **Meervoudige Auth beheren**.
![Klik op map](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6.  Hiermee opent u een nieuw browsertabblad.  Zoek de gebruiker die u wilt inschakelen voor meervoudige verificatie. Mogelijk moet u de weergave aan de bovenkant wijzigen. Zorg ervoor dat de status ervan is **uitgeschakeld.** 
 ![Gebruiker inschakelen](./media/multi-factor-authentication-get-started-cloud/enable1.png)
7.  Plaats een **controleren** in het vak naast de naam.
7.  Aan de rechterkant, klikt u op **inschakelen**.
![Gebruiker inschakelen](./media/multi-factor-authentication-get-started-cloud/user1.png)
8.  Klik op **meervoudige auth inschakelen**.
![Gebruiker inschakelen](./media/multi-factor-authentication-get-started-cloud/enable2.png)
9.  U moet ervaart dat status van de gebruiker is gewijzigd van **uitgeschakeld** **ingeschakeld**.
![Gebruikers in staat stellen](./media/multi-factor-authentication-get-started-cloud/user.png)
10.  Nadat u uw gebruikers hebt ingeschakeld, is het aanbevolen dat u ze via e-mail melden.  Hij moet ook kennis ze hoe ze hun browser-apps kunnen gebruiken om te voorkomen dat wordt uitgesloten.

### <a name="to-change-the-state-from-enabledenforced-to-disabled"></a>Wijzigen van de stand van ingeschakeld/afgedwongen in uitgeschakeld
1.  Meld u als beheerder aan bij de **portal van Azure klassieke** .
2.  Aan de linkerkant, klikt u op **Active Directory**.
3.  Klik onder op de **map** voor de map voor de gebruiker die u wilt inschakelen.
![Klik op map](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4.  Aan de bovenkant, klikt u op **gebruikers**.
5.  Klik onder aan de pagina, op **Meervoudige Auth beheren**.
![Klik op map](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6.  Hiermee opent u een nieuw browsertabblad.  Zoek de gebruiker die u wilt uitschakelen. Mogelijk moet u de weergave aan de bovenkant wijzigen. Zorg ervoor dat de status ervan een van beide **ingeschakeld is** of **afgedwongen.**
7.  Plaats een **controleren** in het vak naast de naam.
7.  Klik op **uitschakelen**aan de rechterkant.
![Gebruiker uitschakelen](./media/multi-factor-authentication-get-started-user-states/userstate2.png)
8.  U wordt gevraagd om te dit bevestigen.  Klik op **Ja**.
![Gebruiker uitschakelen](./media/multi-factor-authentication-get-started-user-states/userstate3.png)
9.  Vervolgens ziet u dat geslaagd is.  Klik op **sluit.** 
 ![Gebruikers uitschakelen](./media/multi-factor-authentication-get-started-user-states/userstate4.png)
