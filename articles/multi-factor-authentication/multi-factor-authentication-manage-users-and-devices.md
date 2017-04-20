<properties 
    pageTitle="Azure meervoudige verificatie-rapporten"
    description="Hiermee wordt beschreven hoe wijzigen van gebruikersinstellingen zoals dat de gebruiker het bewijs-up-proces opnieuw doen."
    documentationCenter=""
    services="multi-factor-authentication"
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

# <a name="managing-user-settings-with-azure-multi-factor-authentication-in-the-cloud"></a>Beheer van gebruikersinstellingen met Azure meervoudige verificatie in de cloud

Als beheerder kunt u de volgende instellingen voor de gebruiker en apparaat beheren.  

- [Geselecteerde gebruikers te leveren contactpersoonformulier verschillende opnieuw vereisen](#require-selected-users-to-provide-contact-methods-again)
- [Appwachtwoorden bestaande gebruikers verwijderen](#delete-users-existing-app-passwords)
- [MFA herstellen op alle geschorst apparaten voor een gebruiker](#restore-mfa-on-all-suspended-devices-for-a-user)






Dit is handig als een computer of apparaat wordt gestolen of u moet een gebruikers toegang krijgen tot verwijderen.


## <a name="require-selected-users-to-provide-contact-methods-again"></a>Geselecteerde gebruikers te leveren contactpersoonformulier verschillende opnieuw vereisen

In deze instelling wordt zorgt ervoor dat de gebruiker het registratieproces opnieuw uitvoeren wanneer hij of zij zich aanmeldt. Bedenk wel dat niet-browser apps nog steeds als de gebruiker appwachtwoorden deze heeft werkt.  U kunt de wachtwoorden van gebruikers app verwijderen door te ook selecteren **alle bestaande appwachtwoorden die zijn gegenereerd door de geselecteerde gebruikers verwijderen**.

### <a name="how-to-require-users-to-provide-contact-methods-again"></a>Hoe moeten gebruikers contactpersonen methoden opnieuw opgeven




1. Meld u aan bij de portal van Azure klassieke.
2. Aan de linkerkant, klikt u op Active Directory.
3. Klik onder op de map voor de map voor de gebruiker die u wilt dat hun contactmethode opnieuw bieden.
4. Aan de bovenkant, klikt u op gebruikers.
5. Klik onder aan de pagina, op meervoudige Auth. beheren Hiermee opent u de pagina meervoudige verificatie.
6. Zoek de gebruiker die u wilt beheren en schakel in het vakje naast de naam. Mogelijk moet u de weergave aan de bovenkant wijzigen.
7. U krijgt de koppeling **gebruikersinstellingen beheren** aan de rechterkant. Klikt u erop.
8. Schakel dit selectievakje in de **geselecteerde gebruikers te leveren contactpersoonformulier verschillende opnieuw vereisen**.
![Contactpersonen methoden bieden](./media/multi-factor-authentication-manage-users-and-devices/reproofup.png)
10. Klik op opslaan.
11. Klik op sluiten

## <a name="delete-users-existing-app-passwords"></a>Appwachtwoorden bestaande gebruikers verwijderen

Hiermee verwijdert u alle van de appwachtwoorden die een gebruiker heeft gemaakt. Niet-browser-apps die gekoppeld aan de appwachtwoorden van deze zijn niet meer werkt pas een nieuw appwachtwoord wordt gemaakt.

### <a name="how-to-delete-users-existing-app-passwords"></a>Hoe u gebruikers bestaande appwachtwoorden verwijderen

1. Meld u aan bij de portal van Azure klassieke.
2. Aan de linkerkant, klikt u op Active Directory.
3. Klik onder op de map voor de map voor de gebruiker die u wilt verwijderen van de app wachtwoorden opnieuw instellen voor.
4. Aan de bovenkant, klikt u op gebruikers.
5. Klik onder aan de pagina, op meervoudige Auth. beheren Hiermee opent u de pagina meervoudige verificatie.
6. Zoek de gebruiker die u wilt beheren en schakel in het vakje naast de naam. Mogelijk moet u de weergave aan de bovenkant wijzigen.
7. U krijgt de koppeling **gebruikersinstellingen beheren** aan de rechterkant. Klikt u erop.
8. Schakel dit selectievakje in **alle bestaande appwachtwoorden die zijn gegenereerd door de geselecteerde gebruikers verwijderen**.
![Appwachtwoorden verwijderen](./media/multi-factor-authentication-manage-users-and-devices/deleteapppasswords.png)
10. Klik op opslaan.
10. Klik op sluiten.

## <a name="restore-mfa-on-all-remembered-devices-for-a-user"></a>MFA herstellen op alle onthouden apparaten voor een gebruiker

Beheerders hebben de mogelijkheid om te herstellen meervoudige verificatie naar gebruikers apparaten en browsers. Wanneer u dit doet, dit het onthouden MFA worden verwijderd uit alle apparaten van de gebruiker en browsers en de gebruiker moet MFA gebruiken als u zich de volgende keer aanmeldt.

### <a name="how-to-restore-mfa-on-all-suspended-devices-for-a-user"></a>Het herstellen van MFA op alle geschorst apparaten voor een gebruiker

1. Meld u aan bij de portal van Azure klassieke.
2. Aan de linkerkant, klikt u op Active Directory.
3. Klik onder op de map voor de map voor de gebruiker die u terugzetten mfa wilt op.
4. Aan de bovenkant, klikt u op gebruikers.
5. Klik onder aan de pagina, op meervoudige Auth. beheren Hiermee opent u de pagina meervoudige verificatie.
6. Zoek de gebruiker die u wilt beheren en schakel in het vakje naast de naam. Mogelijk moet u de weergave aan de bovenkant wijzigen.
7. U krijgt de koppeling **gebruikersinstellingen beheren** aan de rechterkant. Klikt u erop.
8. Schakel dit selectievakje in het **herstellen van meervoudige verificatie op alle onthouden apparaten**
![appwachtwoorden verwijderen](./media/multi-factor-authentication-manage-users-and-devices/rememberdevices.png)
9. Klik op opslaan.
10. Klik op sluiten.
